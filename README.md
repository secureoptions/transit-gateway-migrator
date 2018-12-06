# TGW Migrator

- [Description](#description)
- [Important Limitations](#limitations)
- [Benefits of Using TGW Migrator](#benefits)
- [Pre-Deployment Requirements](#requirements)
- [General Deployment](#deployment)
- [Instructions to Migrate Transit VPC to Transit Gateway](#transitvpc)
- [Instructions to Attach Standalone VPCs to Transit Gateway](#taggedvpc)
- [Contributors](#contributors)
<br>

<br>
<a name="description"></a>

<b>Description</b><br>
The TGW Migrator is a tool which provides a seamless migration path from transit VPC solutions to AWS Transit Gateway. It can also be used to quickly attach and enable routing between VPCs through a common AWS Transit Gateway even if they are not part of a transit VPC.

You start by deploying a Cloudformation stack which in turn provisions a new dedicated VPC, subnet and EC2 which is used to run the TGW Migrator tool. You access the python-based tool by simply SSH'ing into the provisioned EC2 and running it per the instructions below.
<br>
<a name="limitations"></a>

## Important Limitations<br>
- This tool can only migrate VPCs to a Transit Gateway which are all in the same AWS region.
- It does not work across AWS accounts (but this will hopefully change over next couple of weeks)
- The tool is capable of migrating all "spoke VPCs" in a transit VPC to a Transit Gateway. However, it cannot automate the migration of on-prem VPNs to the Transit Gateway. This portion must be done manually.
<br>
<a name="benefits"></a>

## Benefits of Using TGW Migrator<br>
Aside from quickly and seamlessly migrating your VPCs to the Transit Gateway, this tool is also provides a quick option to roll back the migration if something went wrong. A process that could take minutes to hours to do manually (depending on how many VPCs you have) is easily accomplished within a few seconds.
<br>
<a name="requirements"></a>

## Pre-Deployment Requirements<br>
Make sure that each VPC that you intend to migrate to a Transit Gateway has at least one subnet in each Availability Zone (the subnet can be either public or private). If you don't do this the tool will ignore the VPC when it starts the migration to the Transit Gateway.
<br>
<a name="deployment">
  
## General Deployment
<ol>
  <li> <a href="https://console.aws.amazon.com/cloudformation/home?#/stacks/new?stackName=TGW-Migrator&templateURL=https://s3.amazonaws.com/secure-options/tgw-migrator-cf.json">Click Here</a> to launch the Cloudformation Stack
  </li> 
  <li>
    In the Cloudformation console wait for the stack to complete deployment and then click on the <b><i>Outputs</i></b> tab for the stack. This section will have the public IP that you need to SSH into the TGW Migrator EC2
  </li>
  <li>
    SSH into the TGW Migrator EC2 then change into the tool's directory:<br>
    <code>cd tgw-migrator/</code>
  </li>
  <li>
    Finally, start the tool:<br>
    <code>./tgw-migrator.py</code>
  </li>
 </ol>
 <br>
  <a name="transitvpc"></a>
  
 ## Instructions to Migrate Transit VPC to Transit Gateway<br>
 If you have been using a transit VPC in AWS then your architecture likely looks like the following diagram<br>
 <br>
 <img src="https://github.com/secureoptions/transit-gateway-migrator/raw/master/Illustrations/Figure1.PNG" align="center" width="700" height="350"/>
 <br>
 When you deploy the TGW Migrator tool, you will move your VPCs off of transit VPC unto Transit Gateway in two steps:
 <ol>
  <li> Start the tool and choose <b>A) Attach VPCs to TGW</b>. At some point during the attachment process, you will be asked to provide the Customer Gateway (CGW) public IP of one of the hub routers in your transit VPC. Note that if you are running two or more hub routers for redundancy, you can simply choose one of their IPs at random (so '1.1.1.1' could be used in the Figure 1 for example). The tool will use this to trace down all the connected VPNs and their respective spoke VPCs that need to be migrated to the Transit Gateway.
  <li> Once the tool has finished attaching the VPCs, run it again and choose <b>B) Enable routing between attached VPCs</b>. This step will actually move your traffic off of the transit VPC since it inserts static routes into each of the VPCs' main route table which point to the Transit Gateway as the next hop (static routes take preference over BGP propagated routes from the transit VPC)</li>
  <li> If you find that the migration was not successful you can roll it back by starting the tool once more and choosing <b>C) Disable routing between VPCs and detach VPCs from TGW</b></li>
  </ol>
 <br>
 Below is an illustration of these steps:
 <img src="https://github.com/secureoptions/transit-gateway-migrator/raw/master/Illustrations/Figure2.PNG" align="center" width="700" height="350"/>
 <br>
  <a name="taggedvpc"></a>
  
## Instructions to Attach Standalone VPCs to Transit Gateway
The TGW Migrator can also be used as a tool to easily and quickly attach any VPC to a Transit Gateway, not just spoke VPCs that are part of a transit VPC solution. To attach a standalone VPC simply add a tag to the VPC, with the <i>Key</i> being <b>attach-tgw</b> and the <i>Value</i> being <b>true</b> (note: this is case sensitive so make sure to make them lowercase)
<br>
<br>
Your tag should then look like this<br>
 <img src="https://github.com/secureoptions/transit-gateway-migrator/raw/master/Illustrations/Figure3.PNG" align="center" width="700" height="400"/>
<br>
Once you have tagged the VPCs you want to attach:
<ol>
  <li>Start the tool and choose <b>A) Attach VPCs to TGW</b></li>
  <li>Once the tool has finished attaching the VPCs, run it again and choose <b>B) Enable routing between attached VPCs</b>. This step will actually start routing your traffic between VPCs over the TGW</li>
  <li> If you find that the migration was not successful you can roll it back by starting the tool once more and choosing <b>C) Disable routing between VPCs and detach VPCs from TGW</b></li>
  </ol>
<br>
<a name="contributors"></a>

## Author
<a href="https://www.linkedin.com/in/bhavin-desai"/>Bhavin Desai</a> - Solutions Architect and Evangelist
<a href="https://www.linkedin.com/in/fowlerbenjamin/">Ben Fowler</a> - Solutions Builder and Software Developer
<br>
Please feel free to subscribe to this YouTube channel <a href="https://www.youtube.com/channel/UCQ5TipUHtc2LKLVEvCkzCvg">here</a> for frequently demos on different network use-cases, solutions and tools in AWS.
