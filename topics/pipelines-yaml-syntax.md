# Pipelines YAML

Below is the full YAML specification for TeamCity pipelines. Since pipelines are actively evolving, the specification may change in future releases.

```yaml
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Pipeline schema",
  "description": "Main pipeline definition.",
  "required": [
    "jobs"
  ],

  "definitions": {

      "dependencies": {
        "type": "array",
        "description": "A collection of dependencies that represent pipeline relations with external objects (classic build configurations or other pipelines).",
        "items": {
          "anyOf": [
            {
              "type": "string",
              "description": "A simple dependency that references a linked object via its external ID.",
              "pattern": "^[A-Za-z0-9_]+$"
            },
            {
              "type": "object",
              "description": "A dependency with additional properties that specify the behavior of linked objects.",
              "patternProperties": {
                "^[A-Za-z0-9_]+$": {
                  "properties": {
                    "reuse": {
                      "type": "string",
                      "enum": [
                        "none",
                        "successful",
                        "successful-or-failed"
                      ],
                      "description": "Specifies which of upstream dependency builds (runs) can be reused."
                    },
                    "enforce-revisions-synchronisation": {
                      "type": "boolean",
                      "description": "Specifies whether the code revisions should be explicitly synchronized."
                    },
                    "on-failed-dependency": {
                      "type": "string",
                      "enum": [
                        "run-add-problem",
                        "run-ignore-problem",
                        "mark-as-failed-to-start",
                        "cancel"
                      ],
                      "description": "Specifies the default action in case an upstream dependency fails."
                    },
                    "on-incomplete-dependency": {
                      "type": "string",
                      "enum": [
                        "run-add-problem",
                        "run-ignore-problem",
                        "mark-as-failed-to-start",
                        "cancel"
                      ],
                      "description": "Specifies the default action in case an upstream dependency fails to start."
                    }
                  },
                  "additionalProperties": false
                }
              }
            }
          ]
        }
    },

    "files": {
      "title": "Files publication",
      "description": "The collection of job file outputs, including both files published as artifacts and files shared to downstream jobs.",
      "type": "string",
      "enum": [
        ""
      ]
    },
    "runOn": {
      "title": "Agents",
      "description": "The collection of agents eligible to run this job.",
      "definitions": {
        "arch": {
          "type": "object",
          "title": "CPU Architecture",
          "properties": {
            "arch": {
              "type": "string",
              "description": "The architechture of the agent machine.",
              "enum": [
                "x86",
                "x86_64",
                "arm",
                "aarch64",
                "ia64",
                "sparc",
                "riscv64"
              ]
            }
          },
          "additionalProperties": false
        },
        "cpu": {
          "type": "object",
          "title": "CPU count",
          "properties": {
            "cpu": {
              "type": "integer",
              "description": "The number of CPU cores on the agent machine."
            }
          },
          "additionalProperties": false
        },
        "custom": {
          "type": "object",
          "title": "A user-defined expression that TeamCity evaluates for each authorized agent. Agents that fit the criteria are marked as eligible to run the job, others are labeled as incompatible.",
          "properties": {
            "name": {
              "description": "The display name of a custom requirement."
            },
            "requirement": {
              "type": "string",
              "description": "The logical operator used to compare the actual agent parameter value with the given one.",
              "enum": [
                "exists",
                "not-exists",
                "contains",
                "matches",
                "equals",
                "not-equals",
                "more-than",
                "less-than",
                "more-than-or-equals",
                "less-than-or-equals"
              ]
            },
            "parameter": {
              "description": "A parameter reported by the agent machine whose value must match the required criteria."
            },
            "value": {
              "description": "A custom value to compare against the agent's parameter value. Not required for the 'exists' operator."
            }
          },
          "required": ["requirement", "parameter", "name"],
          "additionalProperties": false
        },
        "os-version": {
          "type": "object",
          "title": "Operating system version",
          "description": "The operating system version of the agent machine.",
          "properties": {
            "os-version": {
              "type": "string"
            }
          },
          "additionalProperties": false
        },
        "os-family": {
          "type": "object",
          "title": "Operating system family",
          "description": "The operating system family.",
          "properties": {
            "os-family": {
              "type": "string"
            }
          },
          "additionalProperties": false
        },
        "ram": {
          "type": "object",
          "title": "RAM size",
          "description": "The amount of memory installed on the agent machine.",
          "properties": {
            "ram": {
              "type": "string",
              "pattern": "^\\d+GB$"
            }
          },
          "additionalProperties": false
        }
      },
      "anyOf": [
        {
          "type": "string",
          "title": "JetBrains-hosted agents",
          "description": "Build agents hosted and maintained by JetBrains.",
          "enum": [
            "Windows-Small",
            "Mac-Medium",
            "Linux-XLarge",
            "Linux-Large",
            "Linux-Medium",
            "Linux-Small",
            "Windows-Medium"
          ]
        },
        {
          "type": "string",
          "title": "Self-hosted agents",
          "description": "Self-hosted build agents maintained by your agent administrators.",
          "enum": [
            "self-hosted"
          ]
        },
        {
          "type": "object",
          "title": "Self-hosted agents",
          "description": "Self-hosted build agents maintained by your agent administrators.",
          "properties": {
            "self-hosted": {
              "type": "array",
              "items": {
                "anyOf": [
                  {"$ref": "#/definitions/runOn/definitions/arch"},
                  {"$ref": "#/definitions/runOn/definitions/cpu"},
                  {"$ref": "#/definitions/runOn/definitions/os-family"},
                  {"$ref": "#/definitions/runOn/definitions/os-version"},
                  {"$ref": "#/definitions/runOn/definitions/ram"},
                  {"$ref": "#/definitions/runOn/definitions/custom"}
                ]
              }
            }
          },
          "additionalProperties": false
        }
      ]
    },
    "repositories": {
      "type": "object",
      "description": "A collection of remote repositories this job can check out when it runs.",
      "properties": {
        "main": {
          "type": "object",
          "description": "The main repository that cannot be removed.",
          "properties": {
            "enabled": {
              "type": "boolean"
            },
            "path": {
              "type": "string"
            }
          },
          "additionalProperties": false
        }
      }
    },
    "job": {
      "type": "object",
      "description": "An individual job that specifies the sequence of basic build steps, defines its own input and output parameters, and has its own build agent requirements.",
      "properties": {
        "name": {
          "type": "string"
        },
        "parameters": {
          "type": "object",
          "title": "Job input parameters",
          "description": "The list of name/value pairs designed to be used only within its parent job. Can be referenced directly by name (%env.GIT_HOME%).",
          "additionalProperties": {
            "type": "string"
          }
        },
        "output-parameters": {
          "type": "object",
          "title": "Job output parameters",
          "description": "The list of name/value pairs designed to share values with dependent downstream jobs. Can be referenced via the '%job.jobId.parameterName%' syntax.",
          "additionalProperties": {
            "type": "string"
          }
        },
        "steps": {
          "title": "Step",
          "description": "The collection of build steps in the order of their execution.",
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "type": {
                "type": "string",
                "enum": [
                  "script",
                  "maven",
                  "gradle",
                  "node-js",
                  "unity"
                ]
              },
              "working-directory": { "type": "string" },
              "goals": { "type": "string" },
              "pom-location": { "type": "string" },
              "runner-arguments": { "type": "string" },
              "local-repo-scope": { "type": "string" },
              "jdk-home": { "type": "string" },
              "jvm-args": { "type": "string" },
              "tasks": { "type": "string" },
              "gradle-home": { "type": "string" },
              "use-gradle-wrapper": { "type": "string" },
              "gradle-wrapper-path": { "type": "string" },
              "gradle-params": { "type": "string" },
              "enable-debug": { "type": "string" },
              "enable-stacktrace": { "type": "string" },
              "build-file": { "type": "string" },
              "execute-method": { "type": "string" },
              "run-editor-tests": { "type": "string" },
              "project-path": { "type": "string" },
              "build-target": { "type": "string" },
              "build-player": { "type": "string" },
              "no-graphics": { "type": "string" },
              "silent-crashes": { "type": "string" },
              "no-quit": { "type": "string" },
              "arguments": { "type": "string" },
              "build-player-path": { "type": "string" },
              "detection-mode": { "type": "string" },
              "is-incremental": { "type": "string" },
              "unity-root": { "type": "string" },
              "script-content": { "type": "string" },
              "shell-script": { "type": "string" },
              "docker-image": { "type": "string" },
              "dockerfile-source": { "type": "string" },
              "dockerfile-path": { "type": "string" }
            }
          }
        },
        "files-publication": {
          "type": "array",
          "title": "Files publication",
          "description": "The collection of job file outputs, including both files published as artifacts and files shared to downstream jobs.",
          "items": {
            "anyOf": [
              {
                "type": "string"
              },
              {
                "type": "object",
                "properties": {
                  "path": {
                    "type": "string"
                  },
                  "share-with-jobs": {
                    "type": "boolean",
                    "description": "true if dependent downstream jobs can access this file; otherwise, false."
                  },
                  "publish-artifact": {
                    "type": "boolean",
                    "description": "true if this file should be visible on the Artifacts tab of the run results page; otherwise, false."
                  }
                },
                "required": ["path"],
                "additionalProperties": false
              }
            ]
          }
        },
        "runs-on": {
          "$ref": "#/definitions/runOn"
        },
        "files": {
          "$ref": "#/definitions/files"
        },
        "dependencies": {
          "type": "array",
          "items": {
            "anyOf": [
              {
                "type": "string"
              },
              {
                "type": "object",
                "properties": {}
              }
            ]
          }
        },
        "integrations": {
          "type": "array",
          "description": "The list of credentials required to access external resources, such as Docker or NPM registries.",
          "items": {
            "anyOf": [
              {
                "type": "string"
              },
              {
                "type": "object",
                "properties": {}
              }
            ]
          }
        },
        "repositories": {
          "type": "array",
          "items": {
            "$ref": "#/definitions/repositories"
          }
        },
        "parallelism": {
          "type": "number",
          "description": "The number of batches in which TeamCity should attempt to split the tests."
        },
        "checkout-working-directories-only": {
          "type": "boolean",
          "deprecated": true
        },
        "enable-dependency-cache": {
          "type": "boolean"
        },
        "allow-reuse": {
          "type": "boolean",
          "description": "true, if TeamCity should reuse the previous run if neither the job nor a remote repository has any new changes; false, if a job should run anew every time it's triggered."
        }
      },
      "additionalProperties": false
    }
  },
  "properties": {
    "name": {
      "title": "Name of the pipeline",
      "description": "The public pipeline name and ID.",
      "type": "string",
      "deprecated": true
    },
    "parameters": {
      "type": "object",
      "description": "The list of name/value pairs that each job can access via the '%parameter-name%' syntax.",
      "additionalProperties": {
        "type": "string"
      }
    },
    "secrets": {
      "type": "object",
      "description": "The list parameters that store sensitive values and never expose them via the UI or public API.",
      "additionalProperties": {
        "type": "string",
        "pattern": "^credentialsJSON:.+"
      }
    },
    "jobs": {
      "type": "object",
      "description": "The list of jobs owned by this pipeline",
      "properties": {},
      "patternProperties": {
        "^[^\\s]+$": {
          "$ref": "#/definitions/job"
        }
      },
      "additionalProperties": {
        "$ref": "#/definitions/job"
      }
    },
    "dependencies": {
      "$ref": "#/definitions/dependencies"
    },
    "importParameters": {
      "type": "array",
      "title": "Import Parameters",
      "description": "The list of parameters that should be imported from parent TeamCity projects.",
      "items": {
        "type": "string"
      }
    }
  },
  "additionalProperties": false
}
```