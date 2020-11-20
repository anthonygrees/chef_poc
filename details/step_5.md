## Step 5. Bootstrap Nodes
    
[Return to Main Menu](../README.md#index)
  
#### 5.1 Windows Manual Bootstrap
  
  
Install the Chef Client on the Windows VM.  Here is the link - https://downloads.chef.io/products/infra-client?os=windows
  
  
Copy the Org Validator `.pem` file that was created when you configured the Chef Server.  Copy the pem file to:  
```c:\chef\```
  
  
Create a `client.rb` file in the `c:\chef\` directory with the following:  
```ruby
chef_server_url 'https://YOUR_CHEF_AUTOMATE/organizations/YOUR_ORG'
validation_key 'C:/chef/YOUR_ORG_validator.pem'
node_name 'Win2016-Ant-24223'
policy_group 'development'
policy_name 'base'
ssl_verify_mode :verify_none
chef_license 'accept'
```
  
For waivers, create a file called `c:\waiver.yml` and add the following:
```yaml
cis-access-cred-manager-2.2.1:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2025"
cis-act-as-os-2.2.3:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2025"
cis-add-workstations-2.2.4:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
cis-network-access-2.2.2:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
disable-windows-store:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
microsoft-online-accounts:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
windows-account-100:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
windows-audit-203:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
windows-audit-206:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
windows-base-201:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
windows-ie-101:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
windows-ie-102:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_18.2.1_L1_Ensure_LAPS_AdmPwd_GPO_Extension__CSE_is_installed_MS_only:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_18.2.2_L1_Ensure_Do_not_allow_password_expiration_time_longer_than_required_by_policy_is_set_to_Enabled_MS_only:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_18.2.3_L1_Ensure_Enable_Local_Admin_Password_Management_is_set_to_Enabled_MS_only:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_18.2.4_L1_Ensure_Password_Settings_Password_Complexity_is_set_to_Enabled_Large_letters__small_letters__numbers__special_characters_MS_only:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_18.2.5_L1_Ensure_Password_Settings_Password_Length_is_set_to_Enabled_15_or_more_MS_only:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_18.2.6_L1_Ensure_Password_Settings_Password_Age_Days_is_set_to_Enabled_30_or_fewer_MS_only:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_2.2.21_L1_Ensure_Deny_access_to_this_computer_from_the_network_is_set_to_Guests_Local_account_and_member_of_Administrators_group_MS_only:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_2.2.26_L1_Ensure_Deny_log_on_through_Remote_Desktop_Services_is_set_to_Guests_Local_account_MS_only:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_2.3.1.1_L1_Ensure_Accounts_Administrator_account_status_is_set_to_Disabled_MS_only:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_2.3.1.5_L1_Configure_Accounts_Rename_administrator_account:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_2.3.17.1_L1_Ensure_User_Account_Control_Admin_Approval_Mode_for_the_Built-in_Administrator_account_is_set_to_Enabled:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_2.3.7.5_L1_Configure_Interactive_logon_Message_title_for_users_attempting_to_log_on:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_9.3.5_L1_Ensure_Windows_Firewall_Public_Settings_Apply_local_firewall_rules_is_set_to_No:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
cis-network-access-2.2.2:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
```
  
This waiver file will be picked up by the `audit_agr` cookbook and applied.
  
Now run the following command from a Windows PowerShell terminal running as `Administrator`:  
```bash
chef-client
```
  
  
You should now see your node in Chef Automate under the `Infrastructure` tab:
![windows](/images/windows.png "Windows")
  
  
#### 5.2 Linux Manual Bootstrap
  
  
Install the Chef Client on the Linux VM.  Here are the download links:  
- Amazon Linux : https://downloads.chef.io/products/infra-client?os=amazon
- Debian : https://downloads.chef.io/products/infra-client?os=debian
- Ubuntu : https://downloads.chef.io/products/infra-client?os=ubuntu
- RHEL / CentOS : https://downloads.chef.io/products/infra-client?os=el
  
Copy the Org Validator `.pem` file that was created when you configured the Chef Server.  Copy the pem file to:  
```/etc/chef```
  
  
Create some directories that are needed:  
```bash
/bin/mkdir -p /etc/chef
/bin/mkdir -p /var/lib/chef
/bin/mkdir -p /var/log/chef
```
  
  
Create a `client.rb` file in the `/etc/chef/` directory with the following:  
```ruby
log_location     STDOUT
chef_server_url 'https://YOUR_CHEF_AUTOMATE/organizations/YOUR_ORG'
validation_key '/etc/chef/YOUR_ORG_validator.pem'
node_name 'CentOS-Reesy-nNGX'
ssl_verify_mode :verify_none
policy_group 'development'
policy_name 'base'
chef_license 'accept'
```
  
For waivers, create a file called `/home/centos/waiver.yml` and add the following:
```yaml
os-05:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
os-05b:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
os-06:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
os-10:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
package-08:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-05:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-06:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-07:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-08:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-09:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-10:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-18:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-21:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-22:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-23:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-24:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-26:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-27:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-28:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
sysctl-30:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_1.1.10_Add_nodev_Option_to_home:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_1.1.1_Create_Separate_Partition_for_tmp:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_1.1.2_Set_nodev_option_for_tmp_Partition:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_1.1.3_Set_nosuid_option_for_tmp_Partition:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_1.1.4_Set_noexec_option_for_tmp_Partition:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_1.1.5_Create_Separate_Partition_for_var:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_1.1.6_Bind_Mount_the_vartmp_directory_to_tmp:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_1.1.7_Create_Separate_Partition_for_varlog:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_1.1.8_Create_Separate_Partition_for_varlogaudit:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_1.1.9_Create_Separate_Partition_for_home:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_1.5.3_Set_Boot_Loader_Password:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_3.1_Set_Daemon_umask:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_3.6_Configure_Network_Time_Protocol_NTP:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_3.8_Disable_NFS_and_RPC:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_4.7_Enable_firewalld:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_5.1.4_Create_and_Set_Permissions_on_rsyslog_Log_Files:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_5.1.5_Configure_rsyslog_to_Send_Logs_to_a_Remote_Log_Host:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_6.2.11_Use_Only_Approved_Cipher_in_Counter_Mode:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_6.3.4_Limit_Password_Reuse:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_7.1.1_Set_Password_Expiration_Days:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_7.1.2_Set_Password_Change_Minimum_Number_of_Days:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_7.1.3_Set_Password_Expiring_Warning_Days:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_7.4_Set_Default_umask_for_Users:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
xccdf_org.cisecurity.benchmarks_rule_7.5_Lock_Inactive_User_Accounts:
  expiration_date: 2025-02-28
  run: false
  justification: "Security have signed off not doing this check until the end of February 2021"
```
  
This waiver file will be picked up by the `audit_agr` cookbook and applied.
  
  
Now run the following command:  
```bash
sudo chef-client
```
  
  
You should now see your node in Chef Automate under the `Infrastructure` tab:
![linux](/images/linux.png "Linux")
  
  
#### 5.3. Knife bootstrap off the Chef Workstation (Needs Port 22)
  
This command runs on the Chef Workstation 
  
```bash
knife bootstrap $node_dns \
                --node-name $node_name \
                --connection-user $user \
                --ssh-identity-file $creds_file \
                --policy-name $policy_name \
                --policy-group $policy_group \
                --connection-port $node_port \
                --ssh-verify-host-key=never \
                --chef-license=accept \
                --sudo \
                -y
```
  
  
#### 5.4 Knife bootstrap commands
  
AWS Centos knife bootstrap.  
```bash
knife bootstrap -i ~\.ssh\id_rsa centos@ec2-XX-XXX-XXX-XXX.us-west-2.compute.amazonaws.com -N Your_Node_Name --sudo -run-list 'recipe[Your_Cookbook_Name]'
```
  
  
AWS RHEL knife bootstrap.  
```bash
knife bootstrap -i Path_to_Identity_file  ec2-user@ec2-XX-XXX-XXX-XXX.us-west-2.compute.amazonaws.com -N Your_Node_Name --sudo -run-list 'recipe[Your_Cookbook_Name]'
```
  
  
AWS Windows knife bootstrap.  
```bash
##To bootstrap the node, replace the node IP, Password and Node Name and run the following command

knife bootstrap windows winrm ec2-xx-xxx-xxx-xxx.us-west-2.compute.amazonaws.com --winrm-user Administrator --winrm-password 'PASSWORD' --node-name Your_Node_Name
```
  
#### 5.4 Automated Bulk Bootstrap
  
Bootstrapping the chef-client on many nodes in bulk can present a challenge. Using the traditional bootstrapping tools included with the ChefDK (knife bootstrap) to install and register the chef-client on a bulk number (hundreds or thousands) of nodes will result in exhausting resources on the bastion host before your operations are complete, including CPU, memory, and TCP connections.  
  
Read this blog post first:  
https://blog.chef.io/bootstrapping-nodes-in-bulk/
  
  
Linux bulk bootstrapping using a bash script - https://github.com/saccy/chef_library/tree/master/knife/bootstrap
  
Windows bulk bootstrapping using Microsoft Endpoint Configuration Manager  - https://github.com/jmassardo/Install-Chef-using-ConfigMgr
  
Automated bootstrapping with User Data - https://docs.chef.io/install_bootstrap/#bootstrapping-with-user-data
  
    
[Return to Main Menu](../README.md#index)
  
  
## Next Step
  
[Next Steps >> Step 6](/details/step_6.md)
  
