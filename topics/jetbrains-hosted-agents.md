[//]: # (title: JetBrains-hosted Agents)

TeamCity Cloud offers a wide selection of ready-to-use build agents that can be used for a variety of building tasks. This article explains what hardware and software is available on these cloud machines.

## Agent Hardware

### x86_64 Agents

<tabs>


<tab title="Linux agents">


<table>
<tr>
<td>Agent Name</td>
<td>vCPU<sup>*</sup></td>
<td>RAM</td>
<td>Storage</td>
</tr>

<tr>
<td>Ubuntu-20.04-Small<br/>Ubuntu-22.04-Small</td>
<td>2</td>
<td>4 GB</td>
<td>50 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>

<tr>
<td>Ubuntu-20.04-Medium<br/>Medium-22.04-Small</td>
<td>4</td>
<td>8 GB</td>
<td>100 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>

<tr>
<td>Ubuntu-20.04-Large<br/>Ubuntu-22.04-Large</td>
<td>8</td>
<td>16 GB</td>
<td>200 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>

<tr>
<td>Ubuntu-20.04-XLarge<br/>Ubuntu-22.04-XLarge</td>
<td>16</td>
<td>32 GB</td>
<td>400 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>

</table>

</tab>



<tab title="Windows agents">

<table>
<tr>
<td>Agent Name</td>
<td>vCPU<sup>*</sup></td>
<td>RAM</td>
<td>Storage</td>
</tr>

<tr>
<td>Windows-Server-2019-Small<br/>Windows-Server-2022-Small</td>
<td>2</td>
<td>8 GB</td>
<td>100 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>

<tr>
<td>Windows-Server-2019-Medium<br/>Windows-Server-2022-Medium</td>
<td>4</td>
<td>8 GB</td>
<td>100 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>

<tr>
<td>Windows-Server-2019-Large<br/>Windows-Server-2022-Large</td>
<td>8</td>
<td>16 GB</td>
<td>200 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>

<tr>
<td>Windows-Server-2019-XLarge<br/>Windows-Server-2022-XLarge</td>
<td>16</td>
<td>32 GB</td>
<td>400 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>

</table>

</tab>

<tab title="macOS agents">

<table>
<tr>
<td>Agent Name</td>
<td>vCPU<sup>**</sup></td>
<td>RAM</td>
<td>Storage</td>
</tr>

<tr>
<td>macOS (Medium)</td>
<td>6</td>
<td>15 GB</td>
<td>150 GB storage for preinstalled software and running builds</td>
</tr>

</table>

</tab>

</tabs>

<sup>*</sup> — Intel Xeon Cascade Lake vCPUs<br/>
<sup>**</sup> — Intel i7 vCPUs

### ARM Agents


<table>

<tr>
<td>Agent Name</td>
<td>vCPU<sup>*</sup></td>
<td>RAM</td>
<td>Storage</td>
</tr>

<tr>
<td>Ubuntu-20.04-Small-Arm64<br/>Ubuntu-22.04-Small-Arm64</td>
<td>2</td>
<td>4 GB</td>
<td>118 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>

<tr>
<td>Ubuntu-20.04-Medium-Arm64<br/>Ubuntu-22.04-Medium-Arm64</td>
<td>4</td>
<td>8 GB</td>
<td>237 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>

<tr>
<td>Ubuntu-20.04-Large-Arm64<br/>Ubuntu-22.04-Large-Arm64</td>
<td>8</td>
<td>16 GB</td>
<td>474 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>

<tr>
<td>Ubuntu-20.04-XLarge-Arm64<br/>Ubuntu-22.04-XLarge-Arm64</td>
<td>16</td>
<td>32 GB</td>
<td>950 GB SSD for running builds<br/>100 GB root EBS volume</td>
</tr>
</table>

<sup>*</sup> — AWS Graviton3 vCPUs


## Agent Software

Each JetBrains-hosted agent comes with a set of preinstalled software. If none of the agents have a required software installed, you can [set up your own agents](install-and-start-teamcity-agents.md) or contact TeamCity support to file a request for this software to be installed on your JetBrains-hosted agents.

### Windows Agents

<include from="preinstalled-software-on-teamcity-cloud-windows-agents.md"
element-id="windows-jb-agents"/>

### Ubuntu Agents

<include from="preinstalled-software-on-teamcity-cloud-ubuntu-agents.md"
element-id="ubuntu-jb-agents"/>
