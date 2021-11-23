[//]: # (title: Difference Viewer)
[//]: # (auxiliary-id: Difference Viewer)

TeamCity Difference Viewer allows reviewing the differences between two versions of a file modified in the source control and navigating between these differences. You can access the viewer from almost any place in the TeamCity UI where the changes' lists appear, for example, the __Projects__ page, the __Build Configuration Home__ page, or the __[Changes](working-with-build-results.md#Changes)__ tab of the build results page. Comparing images in the GIF, PNG, or JPG file formats are also supported.

Clicking the name of a modified file opens the viewer (the left part shows the file in its former state, and the right one — the modified file):

<img src="diff-view.png" alt="Difference Viewer" width="750"/>

The window heading displays the file modifications summary:
* the filename along with its status,
* the changes author,
* the comment on the changes list.   

To move between changes, use the next and previous change arrows and the red and green bars on the versions' separator.

If you want to switch to your IDE and explore a change in detail, click __Open in the IDE__ in the upper-right corner or select it in the pop-up menu next to the filename. The file opens, and you will be navigated to this particular change.

 <seealso>
        <category ref="concepts">
            <a href="project.md">Project</a>
            <a href="build-configuration.md">Build Configuration</a>
        </category>
</seealso>