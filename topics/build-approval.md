[//]: # (title: Build Approval)
[//]: # (auxiliary-id: Build Approval)

The _Build Approval_ [build feature](adding-build-features.md) allows users to control starting a build manually, using approvals. 
This build feature ensures that newly queued builds will not be assigned to agents 
unless they are approved by individual users or groups defined in the approval rules.

This feature can be especially useful for processes requiring approval of more than one person, 
for example deployments, resource consuming builds, resource removing operations, etc. 
Approvals also prevent users from triggering a build accidentally. 

If a build is not approved within the specified period of time, it will be cancelled.


## Build Approval Settings

[Add Build Approval](adding-build-features.md) to your build configuration.
 
All branches that are used in this feature __must__ be present in the repository and included into the [Branch Specification](working-with-feature-branches.md#configuring-branches) of the current build configuration.

<table>

<tr><td>

Option

</td>
<td>

Description

</td>
</tr><tr>
<td>

Approval rules

</td><td>

* user-based rules require an approval from a specific [user](creating-and-managing-users.md); they follow the `user:<username>` syntax;

* group-based rules require a certain number of approvals from members of a specific [group](creating-and-managing-user-groups.md); they follow the `group:<groupKey>:<approvalCount>` syntax.

For example, the rules below will allow a build to start only if it is approved by the `teamlead` user and by at least two members of the`QA` group:

```
user:teamlead
group:QA:2
```

You can specify multiple rules in a single feature; in this case, __all__ of the rules must be met for the build to start. 
If a user matches several rules (e.g. a user is a part of multiple groups referenced in the rule), 
the approval from that user will count towards each rule. 
In the example above, if the `teamlead` user is a member of the `QA` group, 
then, when a build is approved by `teamlead`, only one more approval from the `QA` group is needed for the build to start.

</td>
</tr><tr>
<td>

Timeout in

</td><td>

Period of time (in minutes) before the build will be automatically canceled if not approved. Defaults to 360 minutes (6 hours).

</td>
</tr><tr>
<td>

Treat manually started build as approval

</td><td>

If this option is enabled, and a build is started by a user who is eligible to approve build, the feature will automatically add an approval from this user to this build.

</td>
</tr>
</table>

## Notifications & Audit

The notification event `Queued build requires approval` is supported by the [notification rules](adding-notification-rules.md) and by the [Notifications](notifications.md) build feature. It will fire only if build has Build Approval feature enabled.

This event overrides the *Builds with my changes only* option in the notification rules.

When a build is approved by a user, a corresponding [audit entry](tracking-user-actions.md) is created.
