[//]: # (title: Mapping TeamCity Concepts to Other CI Terms)
[//]: # (auxiliary-id: Mapping TeamCity Concepts to Other CI Terms)
This information can be used when migrating to TeamCity from other Continuous Integration tools.

## Jenkins mapping

<include src="jenkins-to-teamcity-migration-guidelines.md" include-id="jenkins-mapping-to-teamcity"/>

## Bamboo mapping

<table><tr>

<td>

Bamboo


</td>

<td>

TeamCity


</td></tr><tr>

<td>

Project


</td>

<td>

Project


</td></tr><tr>

<td>

Plan


</td>

<td>

Build Configuration


</td></tr><tr>

<td>

Stage


</td>

<td>

Build Step


</td></tr><tr>

<td>

Job


</td>

<td>

Build


</td></tr><tr>

<td>

Task \- a part of the Job, often making use of an executable


</td>

<td>

a part of Build Step


</td></tr><tr>

<td>

Builder task


</td>

<td>

Build runner step


</td></tr><tr>

<td>

Agent


</td>

<td>

Build Agent


</td></tr><tr>

<td>

(Agent) capability


</td>

<td>

(Agent) Parameter


</td></tr><tr>

<td>

(Job) requirement


</td>

<td>

Agent Requirement


</td></tr></table>

__ __