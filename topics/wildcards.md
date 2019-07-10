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

 matches any text in the file or directory name excluding directory separator ("/" or "\")


</td>
</tr>
<tr>


<td>

 `?`     


</td>


<td>

 matches single symbol in the file or directory name excluding directory separator 


</td>
</tr>
<tr>


<td>

 `**`  


</td>


<td>

 matches any symbols including the directory separator 


</td>
</tr>
</table>




You can read more on Ant wildcards in the corresponding [section](http://ant.apache.org/manual/dirtasks.html#patterns) of Ant documentation.



Examples

For a directory structure:

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

 all the files          


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

 all log files      

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

 all files in a/b directory incl. those in subfolders 


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

 files directly in the a/b directory 

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

