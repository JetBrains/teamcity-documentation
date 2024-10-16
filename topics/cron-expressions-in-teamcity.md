[//]: # (title: Cron Expressions)
[//]: # (auxiliary-id: Cron Expressions in TeamCity)

TeamCity allows you to flexibly schedule regular operations using the [cron](https://en.wikipedia.org/wiki/Cron#Operators) format. Currently, cron-like expressions are supported for [schedule triggers](configuring-schedule-triggers.md) and [server clean-up](teamcity-data-clean-up.md#Server+Clean-up+Settings).
{instance="tc"}

TeamCity allows you to flexibly schedule regular operations using the [cron](https://en.wikipedia.org/wiki/Cron#Operators) format. Currently, cron-like expressions are supported for [schedule triggers](configuring-schedule-triggers.md).
{instance="tcc"}

TeamCity uses [Quartz](https://www.quartz-scheduler.org/) for working with cron expressions. See the examples below or consider using the [CronMaker](http://www.cronmaker.com/) utility to generate expressions based on the Quartz cron format.

## Cron format in TeamCity

Cron expressions comprise six fields and one optional field separated with a white space. The fields are respectively described as follows:

<table><tr>

<td>

Field Name


</td>

<td>

Values


</td>

<td>

Special Characters


</td></tr><tr>

<td>

Seconds


</td>

<td>

0\-59


</td>

<td>

, \- \* /


</td></tr><tr>

<td>

Minutes


</td>

<td>

0\-59


</td>

<td>

, \- \* /


</td></tr><tr>

<td>

Hours


</td>

<td>

0\-23


</td>

<td>

, \- \* /


</td></tr><tr>

<td>

Day\-of\-month


</td>

<td>

1\-31


</td>

<td>

, \- \* ? / L W


</td></tr><tr>

<td>

Month


</td>

<td>

1\-12 of JAN\-DEC


</td>

<td>

, \- \* /


</td>

</tr>


<tr>

<td>

Day\-of\-week


</td>

<td>

1\-7 or SUN\-SAT


</td>

<td>

, \- \* ? / L #


</td></tr><tr>

<td>

Year (Optional)


</td>

<td>

empty, 1970\-2099


</td>

<td>

, \- \* /


</td></tr></table>

For the description of special characters, refer to [Quartz CronTrigger Tutorial](https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html#special-characters).

## Examples

<table><tr>
      
<td>

Schedule

</td>

<td>

Cron Expression

</td>

</tr>


<tr>

<td>

Each 2 hours at :30


</td>

<td>

<code>
0 30 0/2 \* \* ? \*
</code>

</td>

</tr>


<tr>

<td>

Every day at 11:45PM


</td>

<td>

<code>
0 45 23 \* \* ? \*
</code>

</td>

</tr>

<tr>

<td>

Every Sunday at 1:00AM

</td>

<td>

<code>
0 0 1 ? \* 1 \*
</code>

</td>

</tr>

<tr>

<td>

Every last day of month at 10:00AM and 10:00PM

</td>

<td>

<code>
0 0 10,22 L \* ? \*
</code>

</td>


</tr>

<tr>

<td>

Every 2 hours on weekdays, but not on weekends

</td>

<td>

<code>
0 0 0/2 ? \* 2-6 \*
</code>

</td>


</tr>

</table>

Where the cron expressions in this table have the following format:

```
Sec Min Hour Day-of-month Month Day-of-week Year
```

See also [other examples](https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/tutorial-lesson-06.html).

### Kotlin DSL

Using Kotlin DSL to configure a [schedule trigger](configuring-schedule-triggers.md) with the `0 0 0/2 ? * 2-6 *` cron expression:

```Kotlin
object Build : BuildType({
    triggers {
        schedule {
            schedulingPolicy = cron {
                seconds = "0"
                minutes = "0"
                hours = "0/2"
                dayOfMonth = "?"
                month = "*"
                dayOfWeek = "2-6"
                year = "*"
            }
            triggerBuild = always()
        }
    }
})
```