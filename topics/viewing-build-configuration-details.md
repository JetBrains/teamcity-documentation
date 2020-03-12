[//]: # (title: Viewing Build Configuration Details)
[//]: # (auxiliary-id: Viewing Build Configuration Details)
The __Build Configuration Home__ page provides the configuration details and enables you to:
	
* [run a custom build](triggering-a-custom-build.md) using the __Run__ button
* using the __Actions__ menu			
  * [pause triggers](build-configuration.md#Build+Configuration+State)		
  * check for [pending changes](change-state.md)		
  * enforce [clean checkout](clean-checkout.md)		
  * [assign an investigation](investigating-and-muting-build-failures.md)
* [edit the configuration settings](creating-and-editing-build-configurations.md#Configuring+Settings)

The build configuration details are separated into several tabs whose number may vary depending on your server or project configuration, for example, [dependencies](dependent-build.md), [TeamCity integration with other tools](integrating-teamcity-with-other-tools.md), and so on. 

## Overview

Provides information on:
	
* __Pending Changes__ also listed as a separate tab, see the details [below](#Pending+Changes)	
* __Current Status__ of the build configuration, and, if applicable:			
  * number of [queued builds](build-queue.md)		
  * for a running build – the progress details with the __Stop__ option to terminate the build		
  * for a [failed build](build-state.md) – the number and [agent](build-agent.md), and so on	
* the [Build History](build-history.md) section lists builds of the current build configuration

## History

Displays [Build History](build-history.md) on a separate page and allows filtering builds by build agents, [tagging builds](build-tag.md) and filtering them by tags (if available).

## Change Log

By default, lists changes from builds finished during the last 14 active days. Use the show all link to view the complete change log.

The page shows the change log with its graph of commits to the [monitored branches](working-with-feature-branches.md#Changes) of all VCS repositories used by the current build configurations and the repositories used by the [dependencies and dependent configurations](dependent-build.md) of the current configuration.

## Statistics

Displays the collected statistical data as [visual charts](statistic-charts.md#Build+Configuration+Statistics).

## Compatible Agents

Lists all authorized agents. Agents meeting [Agent Requirements](agent-requirements.md) are listed as compatible. For each incompatible agent, the reason is provided.   
The agents belonging to the [pool(s)](agent-pools.md) associated with the current project are listed first. 

## Pending Changes

Lists [changes](change-state.md) waiting to be included in the next build on a separate page.

## Settings

Lists the current [build configuration settings](creating-and-editing-build-configurations.md#Configuring+Settings) on a separate page.

__ __