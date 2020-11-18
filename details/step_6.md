## Step 6. Hardening Nodes
    
[Return to Main Menu](../README.md#index)
  
#### 6.1 Add Chef Hardening cookbooks to the Chef Server
  
Extract the Hardening Cookbooks on the Chef Workstation in the `C:\chef-repo\cookbooks` directory:  
- cis-windows-ms-2016-master.tgz
- cis-rhel-master.tgz
  
To upload the cookbooks to the Chef Server, run:  
```bash
berks install
berks upload
```
  
  
##### 6.2 Create a Windows Hardening Policy
  
On the Chef Workstation in the `C:\chef-repo\policyfiles` directory, create a new policy called `base-win2016.rb` with the following content:  
```ruby
# base-windows.rb - Describe how you want Chef to build your system.
#
# For more information on the Policyfile feature, visit
# https://docs.chef.io/policyfile.html

# A name that describes what the system you're building with Chef does.
name 'base-win2016'

include_policy 'base', path: './base.lock.json'

# Where to find external cookbooks:
default_source :chef_server, "https://YOUR_AUTOMATE_HOSTNAME/organizations/YOUR_CHEF_ORG"

# Specify a custom source for a single cookbook:
# cookbook 'example_cookbook', path: '../cookbooks/example_cookbook'
cookbook 'cis-windows-ms-2016', '~> 0.3.0'  ## Stage 3 - Correct

# run_list: chef-client will run these recipes in the order specified.
run_list 'cis-windows-ms-2016'  ## Stage 3

```
  
  
Now install the policy file.  
```bash
chef install base-win2016.rb
```
  
  
Add the new policy to the Chef Server.  
```bash
chef push development base-win2016.rb
```
  
  
Now you can assign the Windows Node to the policy `base-win2016` and it will have the hardening cookbook called `cis-windows-ms-2016` converged against it.
  
  
  
##### 6.3 Create a Linux Hardening Policy
  
On the Chef Workstation in the `C:\chef-repo\policyfiles` directory, create a new policy called `base-linux.rb` with the following content:  
```ruby
# base-windows.rb - Describe how you want Chef to build your system.
#
# For more information on the Policyfile feature, visit
# https://docs.chef.io/policyfile.html

# A name that describes what the system you're building with Chef does.
name 'base-linux'

include_policy 'base', path: './base.lock.json'

# Where to find external cookbooks:
default_source :chef_server, "https://YOUR_AUTOMATE_HOSTNAME/organizations/YOUR_CHEF_ORG"

# Specify a custom source for a single cookbook:
# cookbook 'example_cookbook', path: '../cookbooks/example_cookbook'
cookbook 'cis-rhel', '~> 0.3.1'  ## Stage 3 - Correct

# run_list: chef-client will run these recipes in the order specified.
run_list 'cis-rhel'  ## Stage 3

```
  
  
Now install the policy file.  
```bash
chef install base-linux.rb
```
  
  
Add the new policy to the Chef Server.  
```bash
chef push development base-linux.rb
```
  
  
Now you can assign the Linux Node to the policy `base-linux` and it will have the hardening cookbook called `cis-rhel` converged against it.
  
  
  
[Return to Main Menu](../README.md#index)
  
