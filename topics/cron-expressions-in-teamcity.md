[//]: # (title: Cron Expressions)
[//]: # (auxiliary-id: Cron Expressions in TeamCity)

TeamCity allows you to flexibly schedule regular operations using the [cron](https://en.wikipedia.org/wiki/Cron#Operators) format. Currently, cron-like expressions are supported for [schedule triggers](configuring-schedule-triggers.md) and [server clean-up](clean-up.md#Server+Clean-up+Settings).
{product="tc"}

TeamCity allows you to flexibly schedule regular operations using the [cron](https://en.wikipedia.org/wiki/Cron#Operators) format. Currently, cron-like expressions are supported for [schedule triggers](configuring-schedule-triggers.md).
{product="tcc"}

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


</td></tr><tr>

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


</td>

<td>

Each 2 hours at :30


</td>

<td>

Every day at 11:45PM


</td>

<td>

Every Sunday at 1:00AM


</td>

<td>

Every last day of month at 10:00AM and 10:00PM


</td></tr><tr>

<td>

Seconds


</td>

<td>

0


</td>

<td>

0


</td>

<td>

0


</td>

<td>

0


</td></tr><tr>

<td>

Minutes


</td>

<td>

30


</td>

<td>

45


</td>

<td>

0


</td>

<td>

0


</td></tr><tr>

<td>

Hours


</td>

<td>

0/2


</td>

<td>

23


</td>

<td>

1


</td>

<td>

10,22


</td></tr><tr>

<td>

Day\-of\-month


</td>

<td>

\*


</td>

<td>

\*


</td>

<td>

?


</td>

<td>

L


</td></tr><tr>

<td>

Month


</td>

<td>

\*


</td>

<td>

\*


</td>

<td>

\*


</td>

<td>

\*


</td></tr><tr>

<td>

Day\-of\-week


</td>

<td>

?


</td>

<td>

?


</td>

<td>

1


</td>

<td>

?


</td></tr><tr>

<td>

Year(Optional)


</td>

<td>

\*


</td>

<td>

\*


</td>

<td>

\*


</td>

<td>

\*


</td></tr></table>

See also [other examples](https://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/tutorial-lesson-06.html).
