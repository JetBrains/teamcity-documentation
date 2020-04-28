[//]: # (title: Setting Up TeamCity for VMware vSphere and vCenter)
[//]: # (auxiliary-id: Setting Up TeamCity for VMware vSphere and vCenter)
TeamCity vSphere integration allows using TeamCity [agent cloud features](teamcity-integration-with-cloud-solutions.md) with VMware __vSphere__ and __vCenter__ installation. It requires configuring TeamCity with your VMware vSphere/vCenter account and then handles automatic creation/starting/stopping/deleting of the virtual machines with TeamCity agents on\-demand, based on the queued builds.

The functionality is implemented as a plugin bundled with TeamCity.

## Requirements

<note>

 The integration requires a write access for vSphere/vCenter via API, which is available in the licensed versions; the API support of free versions is limited to read-only  access.
</note>

## Features

The TeamCity vSphere integration allows you to:
* select the type of behavior:
  * either to start/stop an existing virtual machine
  * or clone a virtual machine or a template, deleting the clone once it becomes idle (when the build is finished or an idle timeout elapses, depending on the profile settings)
* select a virtual machine snapshot to start
* specify the folder for clones and the resource pool where your machine will be allocated
* configure the maximum number of started instances

## Usage

The following steps are required to set up TeamCity-VMware vSphere agent cloud integration:

1. Create a virtual machine where your builds will run. Refer to the VMware vSphere website for details on creating [virtual machines](https://pubs.vmware.com/vsphere-51/index.jsp#com.vmware.vsphere.vm_admin.doc/GUID-7834894B-DD17-4D59-A9BF-A33D02478521.html).
2. Make sure VMware Tools or Open VM Tools are installed. See [VMware documentation](https://docs.vmware.com/en/VMware-Tools/10.1.0/com.vmware.vsphere.vmwaretools.doc/GUID-28C39A00-743B-4222-B697-6632E94A8E72.html).
3. Install a TeamCity agent, configure and test the machine as described in [this section](teamcity-integration-with-cloud-solutions.md#Preparing+a+virtual+machine).
   * If you want TeamCity to start/stop this machine on demand or to clone it, proceed to configuring the VMware [cloud profile](agent-cloud-profile.md) on the TeamCity server.  When the profile is modified, TeamCity detects the changes immediately, and forces shutdown of the agents started prior to these changes once the agents finish the current build.
   * If you want to create a template of this machine and clone it, refer to the VMware vSphere web site for details on creating [templates](https://pubs.vmware.com/vsphere-51/index.jsp#com.vmware.vsphere.vm_admin.doc/GUID-F40130B0-0194-4A41-91FA-1A967721924B.html) and proceed to configuring the VMware [cloud profile](agent-cloud-profile.md) on the TeamCity server. Make sure to specify the valid [vCenter SDK URL](https://pubs.vmware.com/vsphere-51/topic/com.vmware.vsphere.install.doc/GUID-191D86C8-EEF4-4198-9C11-2E0F25D2AB89.html).

<note>

When configuring the TeamCity build agent, be sure to specify the valid TeamCity server URL in the build agent properties.
</note>

### Notes on configuring Agent cloud profile

* You can limit a number of instances across all images and/or set the limit per image. 
* It is possible to specify unique hostnames for cloud agents: when adding an image, choose a customization spec from the corresponding field. The option is available for Windows and Linux virtual machines.
* When using the same image in different cloud profiles, to avoid possible conflicts, it is possible to use a custom agent image name when configuring a cloud profile in TeamCity. This can be also useful with naming patterns for agents. When a custom agent image name is specified, the names of cloud agent instances cloned from the image will be based on this name.
 
__ __