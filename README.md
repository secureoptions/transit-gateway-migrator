# TGW Migrator

**Description** <br>
The TGW Migrator is a tool which provides a relatively seamless migration path from transit VPC solutions to AWS Transit Gateway. It can also be used to quickly attach and enable routing between VPCs through a common AWS Transit Gateway even if they are not part of a transit VPC.

## Important Limitations <br>
- This tool can only migrate VPCs to a Transit Gateway which are all in the same AWS region.
- It does not work across AWS accounts (but this will hopefully change over next couple of weeks)
- The tool is capable of migrating all "spoke VPCs" in a transit VPC to a Transit Gateway. However, it cannot automate the migration of on-prem VPNs to the Transit Gateway. This portion must be done manually.

## General Deployment
<ol>
  <li> <a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=CloudShroud&templateURL=https://s3.amazonaws.com/secure-options/tgw-migrator-cf.json"><img src="https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png"/></a>
  </li>
 </ol>
