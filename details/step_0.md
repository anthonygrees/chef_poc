## Step 0. Prerequisites
  
[Return to Main Menu](../README.md#index)
  
#### 0.1 Servers
For the PoC you will require the following:
- 1 x Server for Chef Automate / Chef Server
- 1 x Server or Laptop for Chef Workstation. (Windows or Linux)
- 1 x Server for each node you intend to manage with Chef and scan with InSpec.  
  
Note: All the server system requirements are listed below.
  
#### 0.2 Ports
For the PoC the Chef servers will require:  
- Internet access.  (although Chef can be used in an airgapped environment.)
- Port 443 open
- Port 80
  
The nodes will also require:  
- Port 22 SSH and / or 5985 WinRM if you intend to bootstrap via Knife
- Port 3389 for Windows Remote Desktop 
  
  
  
[Return to Main Menu](../README.md#index)
  
  
## Next Step
  
[Next Steps >> Step 1](/details/step_1.md)
  
