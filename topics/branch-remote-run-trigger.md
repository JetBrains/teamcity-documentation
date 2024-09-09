[//]: # (title: Branch Remote Run Trigger)
[//]: # (auxiliary-id: Branch Remote Run Trigger)

The _branch remote run trigger_ automatically starts a new [personal build](personal-build.md) each time TeamCity detects changes in particular branches of the VCS roots of the build configuration. Finished personal builds are listed in the [build history](build-results-page.md#Build+History+in+Classic+UI), but only for the users who initiated them.

At the moment, the branch remote run trigger supports only Git and Mercurial VCSs.

For non-personal builds off branches, see [Working with Feature Branches](working-with-feature-branches.md). When a branch specification is configured for a VCS root, the branch remote run trigger only processes branches not matched and not excluded by the specification.

## Triggering Settings

A trigger monitors branches with names that match specific patterns.   
Default patterns are:
* for Git repositories: `refs/heads/remote-run/*`
* for Mercurial repositories: `remote-run/*`

These branches are regular version control branches and TeamCity does not manage them (that is if you no longer need the branch, you would need to delete the branch using regular version control methods).

By default, TeamCity triggers a personal build for the user detected in the last commit of the branch. You might also specify a TeamCity user in the name of the branch. To do that, use a placeholder `TEAMCITY_USERNAME` in the pattern and your TeamCity username in the name of the branch. For example, the pattern `remote-run/TEAMCITY_USERNAME/*` will match a branch `remote-run/joe/my_feature` and start a personal build for the TeamCity user `joe` (if such user exists).

<note>

At the moment there is no UI to show what's going on inside the trigger, so the only way to troubleshoot it is to look inside `teamcity-remote-run.log`. To see a more detailed log, enable `debug-vcs` logging preset on the __Administration | Diagnostics__ page.
</note>

To be able to trigger, a build branch should have at least one new commit comparing to the main branch.

### Example: Run a personal build from a command line

#### Git

```Shell

% cd <your_local_git_repo>
% git branch
* master
% git checkout -b my_feature
Switched to a new branch 'my_feature'
//code, commit; code, commit
% git push origin +HEAD:remote-run/my_feature
```

With the default pattern (`refs/heads/remote-run/*`), the `git branch -r` command will list your personal branches. If you want to hide them, change the pattern to `refs/remote-run/*` and push your changes to branches like `refs/remote-run/my_feature`. In this case your branches are not listed by the above command, although you can see them anyway using `git ls-remote <url_of_git_repository>`.

#### Mercurial

```Shell
% cd <your_local_hg_repo>
% hg branch
default
% hg branch remote-run/my_feature
marked working directory as branch remote-run/my_feature
//code, commit; code, commit
% hg push -b remote-run/my_feature --new-branch

```

## Limitations

If your build configuration has more than one VCS root which support branch remote run, and you push changes to all of them, TeamCity will start one personal build with changes per each VCS root.

## Triggered Build Customization

<include from="configuring-vcs-triggers.md" element-id="triggered-build-customization"/>

 <seealso>
        <category ref="admin-guide">
            <a href="git.md">Git</a>
            <a href="mercurial.md">Mercurial</a>
        </category>
</seealso>
