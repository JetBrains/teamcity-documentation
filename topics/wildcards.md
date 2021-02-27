[//]: # (title: Wildcards)
[//]: # (auxiliary-id: Wildcards)

TeamCity supports wildcards in different configuration options.

## Antlike Wildcards

<table>
<tr>

<td>

Wildcard 

</td>

<td>

Description 

</td>
</tr>
<tr>


<td>

`*`

</td>


<td>

Matches any text in the file or directory name excluding directory separator (`/` or `\`).

</td>
</tr>
<tr>


<td>

`?`

</td>


<td>

Matches single symbol in the file or directory name excluding directory separator.

</td>
</tr>
<tr>

<td>

`**`

</td>

<td>

Matches any symbols including the directory separator.

</td>
</tr>
</table>

You can read more on Ant wildcards in the corresponding [section](http://ant.apache.org/manual/dirtasks.html#patterns) of the Ant documentation.

### Examples

See the pattern examples for the following file structure inside the current directory:

```Shell

\a
 -\b
   -\c
     -file1.txt
   -file2.txt
   -file3.log
 -\d
   -file4.log
 -file5.log
```

Examples:

<table>
<tr>


<td width="250">

Description

</td>

<td>

Pattern

</td>

<td>

Matching files

</td>
</tr>
<tr>

<td>

All files inside the current directory

</td>

<td>

`**`

</td>

<td>

```Shell
\a
 -\b
   -\c
     -file1.txt
   -file2.txt
   -file3.log
 -\d
   -file4.log
 -file5.log
 
```

</td>
</tr>
<tr>

<td>

All log files inside the current directory

</td>


<td>

`**/*.log`

</td>

<td>

```Shell

\a
 -\b
   -file3.log
 -\d
   -file4.log
 -file5.log
 ```

</td>
</tr>
<tr>


<td>

All files inside the `a/b` directory including those in subdirectories

</td>

<td>

`a/b/**`

</td>

<td>

```Shell
\b
 -\c
   -file1.txt
 -file2.txt
 -file3.log
 
```

</td>
</tr>
<tr>

<td>

All files inside the `a/b` directory

</td>

<td>

`a/b/*`

</td>

<td>

```Shell
\b
 -file2.txt
 -file3.log
```

</td>
</tr>
</table>
