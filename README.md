# TGW Migrator

**Description** <br>
The TGW Migrator is a tool which provides a seamless migration path from transit VPC solutions to AWS Transit Gateway. It can also be used to quickly attach and enable routing between VPCs through a common AWS Transit Gateway even if they are not part of a transit VPC.

## Important Limitations <br>
- This tool can only migrate VPCs to a Transit Gateway which are all in the same AWS region.
- It does not work across AWS accounts (but this will hopefully change over next couple of weeks)
- The tool is capable of migrating all "spoke VPCs" in a transit VPC to a Transit Gateway. However, it cannot automate the migration of on-prem VPNs to the Transit Gateway. This portion must be done manually.

## General Deployment
<ol>
  <li> <a href="https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=TGW-Migrator&templateURL=https://s3.amazonaws.com/secure-options/tgw-migrator-cf.json">Click Here</a> to launch the Cloudformation Stack
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
