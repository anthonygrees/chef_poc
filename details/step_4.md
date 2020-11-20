## Step 4. Create a `base` policy and upload the Cookbooks you need to the Chef Server
    
[Return to Main Menu](../README.md#index)
  
Before you bootstrap your nodes, you need a base policy to apply and the cookbooks.
  
#### 4.1. Add Cookbooks to the Chef Server
  
On the Chef Workstation developer machine, open PowerShell and go to the cookbooks directory.  
```bash 
cd C:\chef-repo\cookbooks
```
  
Now clone all the required Cookbooks.  
```bash
    git clone https://github.com/anthonygrees/audit_agr --quiet
    git clone https://github.com/anthonygrees/chef-client --quiet
    git clone https://github.com/anthonygrees/cis-win2012r2-l1-hardening-cookbook --quiet
```
  
Upload the Audit Cookbook. This cookbook is used to automatically run the correct InSpec profiles.  
  
NOTE: If you are using a different Chef Automate user you will need to modify the `attributes/defaul.rb`.  Speak to Anthony for help before uploading !   
```bash
cd audit_agr
berks install -q
berks upload -q
```
  
Upload the Chef Client Cookbook.  
```bash
cd chef-client
berks install -q
berks upload -q
```
  
  
Upload the Windows 2012 Hardening Cookbook.  
```bash
cd cis-win2012r2-l1-hardening-cookbook
berks install -q
berks upload -q
```
  
#### 4.2. Add Policyfile to the Chef Server
  
On the Chef Workstation developer machine, open PowerShell and go to the cookbooks directory.  
```bash 
cd C:\chef-repo\policyfiles
```
  
Create a new policy called `base.rb` using VS Code.  
```bash
code base.rb
```
  
Add the following code to your `base.rb` and save it.  Don't forget to update `YOUR_AUTOMATE_HOSTNAME` and `YOUR_CHEF_ORG`.  
```ruby
# base.rb - Describe how you want Chef to build your system.
#
# For more information on the Policyfile feature, visit
# https://github.com/opscode/chef-dk/blob/master/POLICYFILE_README.md
# A name that describes what the system you're building with Chef does.

name 'base'

# Where to find external cookbooks:
default_source :chef_server, "https://YOUR_AUTOMATE_HOSTNAME/organizations/YOUR_CHEF_ORG"

# Specify a custom source for a cookbook:
cookbook 'chef-client', '~> 12.2.0' ## Stage 1 - Base
cookbook 'audit_agr', '~> 2.2.4' ## Stage 2 - Detect

# run_list: chef-client will run these recipes in the order specified.
run_list 'chef-client', 'audit_agr'  ## Stage 2
```
  
Upload the policy to the Chef Server and apply it to the `development` policy group.
```bash
chef install base.rb
chef push development base.rb
```
  
Check the policy on the Chef Server with the following command:  
```bash
chef show-policy base
```
  
  
[Return to Main Menu](../README.md#index)
  
  
## Next Step
  
[Next Steps >> Step 5](/details/step_5.md)
  
