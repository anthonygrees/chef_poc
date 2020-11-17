```bash
 _____ _           __  ______     _____   _____          _                   _   _                 
/  __ \ |         / _| | ___ \   /  __ \ |_   _|        | |                 | | (_)                
| /  \/ |__   ___| |_  | |_/ /__ | /  \/   | | _ __  ___| |_ _ __ _   _  ___| |_ _  ___  _ __  ___ 
| |   | '_ \ / _ \  _| |  __/ _ \| |       | || '_ \/ __| __| '__| | | |/ __| __| |/ _ \| '_ \/ __|
| \__/\ | | |  __/ |   | | | (_) | \__/\  _| || | | \__ \ |_| |  | |_| | (__| |_| | (_) | | | \__ \
 \____/_| |_|\___|_|   \_|  \___/ \____/  \___/_| |_|___/\__|_|   \__,_|\___|\__|_|\___/|_| |_|___/
 ```
  
Chef Software delivers value to our customers by taking an “Enterprise as Code” approach to simplify, standardize and secure day to day operations across both on-premise and multi-cloud environments.  
  
Here’s an overview of the Chef development workflow:  
![Dev Workflow](/images/dev_workflow.png "Dev Workflow")
  
The following describes the basic local development workflow for creating / modifying both Chef cookbooks and / or InSpec profiles.  
Step 1: The Developer clones the Chef Cookbook or InSpec Profile from the source code management (SCM) repository.  
Step 2: The Developer then isolates the code changes by creating a branch.  
Step 3: The Developer now modifies the code and then tests and verifies the code changes by using Test Kitchen.  
Step 4: The Developer then commits the code changes to the local branch.  
Step 5: The Developer then pushes the branch to the source code management (SCM) repository.  
Step 6: The Developer then initiates a pull request (PR) in the source code management (SCM) repository.  
Step 7: The CI/CD pipeline (i.e. Jenkins, Harness, Bamboo or Azure DevOps etc )  picks up the pull request which triggers automated testing and approvals.  
  
  
## Index
| Item | Topic  | 
| :---: |:-------------| 
|     | **Chef PoC Instructions** |
| 0.  | [Step 0. Prerequisites](README.md#step-0-prerequisites) |
| 1.  | [Step 1. Install Chef Automate and Chef Server](README.md#step-1-install-chef-automate-and-chef-server) |
| 2.  | [Step 2. Install Chef Workstation](README.md#step-2-install-chef-workstation) |
| 3.  | [Step 3. Chef Automate Profiles](README.md#step-3-chef-automate-profiles) |
| 4.  | [Step 4. Create a base policy and upload the Cookbooks you need to the Chef Server](README.md#step-4-create-a-base-policy-and-upload-the-cookbooks-you-need-to-the-chef-server) |
| 5.  | [Step 5. Bootstrap Nodes](README.md#step-5-bootstrap-nodes) |
|     | |
  
  
## Step 0. Prerequisites
  
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
  
  
## Step 1. Install Chef Automate and Chef Server
For a PoC we can install Chef Automate and Chef Server using an `all in one` deployment pattern.  
  
### 1.1. System Requirements
For further details please refer to the Chef Docs page - https://docs.chef.io/automate/system_requirements/
  
#### 1.1.1 Hardware
Chef Automate requires a minimum of:  
- 16 GB of RAM
- 80 GB of disk space (available to /hab)
- 4 vCPUs
  
#### 1.1.2 Operating system
Chef Automate requires:  
- a Linux kernel of version 3.2 or greater
- systemd as the init system
- useradd
- curl or wget
- The shell that starts Automate should have a max open files setting of at least 65535
- Commercial support for Chef Automate is available for platforms that satisfy these criteria.
  
#### 1.1.3 Supported Browsers
Chef Automate supports the current browser versions for Chrome, Edge, and Firefox. Chef Automate does not support other browsers and may not be compatible with older browser versions.
  
### 1.2. Installation Guide
For further details please refer to the Chef Docs page - https://docs.chef.io/automate/install/
  
#### 1.2.1 Download the Chef Automate Command-Line Tool
Download and unzip the Chef Automate command-line tool:
```bash
curl https://packages.chef.io/files/current/latest/chef-automate-cli/chef-automate_linux_amd64.zip | gunzip - > chef-automate && chmod +x chef-automate
```
  
#### 1.2.2 Create Default Configuration
Create a `config.toml` file with default values for your Chef Automate installation:
```bash
sudo ./chef-automate init-config
```
  
#### 1.2.3 Configure Chef Automate
You can customize your FQDN, login name, and other values, by changing the values in the config.toml in your editor.  
  
#### 1.2.4 Deploy Chef Automate and Chef Server
The following command will deploy Chef Automate and Chef Server
```bash
sudo ./chef-automate deploy config.toml --product automate --product chef-server --accept-terms-and-mlsa
```
  
if you require Habitat On Premise Builder, then use this command
```bash
sudo ./chef-automate deploy config.toml --product automate --product chef-server --product builder --accept-terms-and-mlsa
```
  
Deployment takes a few minutes. The first step is accepting the terms of service in the command line, after which the installer performs a series of pre-flight checks; any unsuccessful checks have information for resolving issues or skipping the check. Run the deploy command again, after resolving any pre-flight issues.
  
At the end of the deployment process you will see:
```bash
Deploy complete
```
  
#### 1.2.5 Chef Automate Credentials
The deployment process writes login credentials to the `automate-credentials.toml` in your current working directory.
  
  
#### 1.2.6 Set the Chef Infra Server User and Org
  
First create a `user` on the Chef Server.  The `.pem` file will be used on your developer Chef Workstation machine.
```bash
sudo chef-server-ctl user-create Your_User_Name chef user Your_User_Name@email.com 'Your_Password' --filename $HOME/Your_User_Name.pem
```
  
Next, create an organisation for the Chef Infra Server.  The `.pem` file will be used as the 1 time validation key to bootstrap your Windows and Linux nodes.
```bash
sudo chef-server-ctl org-create Your_Org_Name 'automate' --association_user Your_User_Name  --filename $HOME/Your_Org_Name-validator.pem
```
  
You will need to copy both `.pem` files from the Chef Automate server as they will be needed on the developer Chef Workstation.
  
  
  
## Step 2. Install Chef Workstation
Chef Workstation gives you everything you need to get started with Chef - ad hoc remote execution, remote scanning, configuration tasks, cookbook creation tools as well as robust dependency and testing software - all in one easy-to-install package.  
  
Chef Workstation includes:
- Chef Infra Client
- Chef InSpec
- chef and knife command line tools
- Testing tools such as Test Kitchen, ChefSpec, and Cookstyle
- Everything else needed to author cookbooks and upload them to the Chef Infra Server
  
#### 2.1. System Requirements
Minimum system requirements:
- RAM: 2GB
- Disk: 4GB
- Running minimum settings may limit your ability to take advantage of Chef Workstation tools such as Test Kitchen which creates and manages virtualized test environments.
  
Recommended system requirements:
- RAM: 4GB
- Disk 8GB
  
#### 2.2. Installation
The Chef Workstation installer must run as a privileged user.  
  
Chef Workstation installs to /opt/chef-workstation/ on macOS / Linux and C:\opscode\chef-workstation\ on Windows. These file locations should help avoid interference between these components and other applications that may be running on the target machine.  
  
##### 2.2.1 macOS
###### 2.2.1.1. Dependency: Xcode is recommended for running Chef Workstation on macOS. While Chef Workstation works without Xcode, it is required for native Ruby Gem installation. Run xcode-select --install from the terminal to install Xcode.  
  
###### 2.2.1.2. Visit the Chef Workstation downloads page and select the appropriate package for your macOS version. Click on the Download button.  https://downloads.chef.io/products/workstation#mac_os_x
  
###### 2.2.1.3. Follow the steps to accept the license and install Chef Workstation.  
  
Alternately, install Chef Workstation using Homebrew:  
```bash
brew cask install chef-workstation
```
    
##### 2.2.2 Windows
###### 2.2.2.1. Visit the Chef Workstation downloads page and select the appropriate package for your Windows version. Click on the Download button.  https://downloads.chef.io/products/workstation#windows
  
###### 2.2.2.2. Follow the steps to accept the license and install Chef Workstation. You will have the option to change your install location; by default the installer uses the `C:\opscode\chef-workstation\` directory.  
  
###### 2.2.2.3. Optional: Set the default shell. On Microsoft Windows it is strongly recommended to use Windows PowerShell instead of cmd.exe.
  
#### 2.3. Configure Chef Workstation
Here are the steps to set up a Windows Chef Workstation for development.  
  
#### 2.3.1. Install Chocolatey first as it is really handy.  
```bash
PowerShell.exe -Command "[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))"
```

#### 2.3.2. Install Chef Workstation if you have not already done so.  
```bash
PowerShell.exe -Command "iex (irm 'https://omnitruck.chef.io/install.ps1'); Install-Project -project chef-workstation -channel stable"
```
  
#### 2.3.3. Install the following tools.
```bash
choco install googlechrome -y --no-progress --ignore-checksums
choco install vscode -y --no-progress
choco install cmder -y --no-progress
choco install git -y --no-progress
choco install openssh -y --pre --no-progress
```
  
#### 2.3.4. Configure Git
Create a file called `.gitconfig` in the directory `C:\Users\chef\` with the following:
```yaml
[user]
  email = student@chef.com
  name = chef
```
Note: Feel free to use your own values.
  
#### 2.3.5. Configure Knife to speak to your Chef Server
  
a) Create a directoy for Chef. Run the PowerShell command:    
```bash
chef generate repo c:\chef-repo --chef-license accept
``` 
  
b) The `.pem` files you created when you configured the Chef Server need to be copied in the directory:  
`C:\chef-repo\.chef`   
  
There will be 2 .pem files:
- chef_user.pem
- chef_organization.pem
  
#### 2.3.6. Create a `config.rb` so Knife can communicate with Chef Server.  
  
In the `C:\chef-repo\.chef` directory, create a file called `config.rb`.  To do this you can run the command `code config.rb`.
  
```ruby
current_dir = File.dirname(__FILE__)
log_level :info
log_location STDOUT
node_name "chef_user"
client_key "#{current_dir}/chef_user.pem"
chef_server_url "https://automate_hostname/organizations/chef_organization"
cookbook_path ["#{current_dir}/../cookbooks"]
```
  
#### 2.3.7. Test Knife config with `knife user list`
```bash
PS C:\chef-repo> knife user list
anthony
```
Note: The user you see will be the one you created on the Chef Server above.
  
  
## Step 3. Chef Automate Profiles
Log into Chef automate and select all the profiles you require.  These profiles need to match the ones specified in the Audit Cookbook otherwise you will receive an error saying the profile cannot be found.  
  
The Audit Cookbook example I provide uses the following:
```ruby
default['audit']['fetcher'] = 'chef-server'
default['audit']['reporter'] = 'chef-server-automate'
default['audit']['profiles'] =
  case node['platform']
  when 'centos'
    default['audit']['waiver_file'] = '/home/centos/waiver.yml'
    default['audit']['profiles'] = [
      {
        name: 'DevSec Linux Security Baseline',
        compliance: 'workstation-1/linux-baseline',
      },
      {
        name: 'CIS CentOS Linux 7 Benchmark Level 1',
        compliance: 'workstation-1/cis-centos7-level1',
      },
    ]
  when 'ubuntu'
    default['audit']['profiles'] = [
      {
        name: 'DevSec Linux Security Baseline',
        compliance: 'workstation-1/linux-baseline',
      },
      {
        name: 'CIS Ubuntu Linux 16.04 LTS Benchmark Level 1 - Server',
        compliance: 'workstation-1/cis-ubuntu16.04lts-level1-server',
      },
    ]
  when 'windows'
    case  node['platform_version']
      when /^10/ # 2016
        default['audit']['waiver_file'] = 'c:\\waiver.yml'
        default['audit']['profiles'] = [
          {
           name: 'DevSec Windows Security Baseline',
           compliance: 'workstation-1/windows-baseline',
         },
          {
           name: 'CIS Microsoft Windows Server 2016 RTM (Release 1607) Benchmark Level 1 - Member Server',
           compliance: 'workstation-1/cis-windows2016rtm-release1607-level1-memberserver',
         },
        ]
      when /^6.3/ # 2012R2
            default['audit']['waiver_file'] = 'c:\\waiver.yml'
            default['audit']['profiles'] = [
              {
              name: 'DevSec Windows Security Baseline',
              compliance: 'workstation-1/windows-baseline',
            },
              {
              name: 'CIS Microsoft Windows Server 2012 R2 Benchmark Level 1 - Member Server',
              compliance: 'workstation-1/cis-windows2012r2-level1-memberserver',
            },
            ]
  end
  when 'redhat'
    default['audit']['profiles'] = [
      {
        name: 'DevSec Linux Security Baseline',
        compliance: 'workstation-1/linux-baseline',
      },
      {
        name: 'CIS Red Hat Enterprise Linux 7 Benchmark Level 1 - Server',
        compliance: 'workstation-1/cis-rhel7-level1-server',
      },
    ]
  end
```
  
  
Therefore you will need the following profiles:  
- linux-baseline
- cis-centos7-level1
- cis-ubuntu16.04lts-level1-server
- windows-baseline
- cis-windows2016rtm-release1607-level1-memberserver
- cis-windows2012r2-level1-memberserver
- cis-rhel7-level1-server
  
You will also note that the `audit_agr` cookbook expects the profiles to be under user `workstation-1`.  If you use another user then you will need to modify the `audit_agr` cookbook to match your user.  
  
Next, log into Chef Automate and go to the `Compliance` tab at the top.  Then click on the `profiles` tab on the left and then the `399 Available`.  
  
![Automate1](/images/automate1.png "Chef Automate")
  
  
Once you have selected the profiles by clicking the `Get` button on the right hand side, they will appear in the `Profiles` tab and you will see the `Identifier` next to the profile.  This Identifier needs to match what is in the `audit_agr` cookbook.
  
![Automate2](/images/automate2.png "Chef Automate")
  
  
## Step 4. Create a `base` policy and upload the Cookbooks you need to the Chef Server
  
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
  
## Step 5. Bootstrap Nodes
  
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
  
This waiver file will be picjed up by the `audit_agr` cookbook and applied.
  
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
  
Instructions coming soon.
  
  
  
## Step 6. Hardening Nodes
  
#### 6.1 Add Chef Hardening cookbooks to the Chef Server
  
Extract the Hardening Cookbooks on the Chef Workstation in the `C:\chef-repo\cookbooks` directory:  
- cis-windows-ms-2016-master.tgz
- cis-rhel-master.tgz
  
To upload the cookbooks to the Chef Server, run:  
```bash
berks install
berks upload
```
  
  
##### 6.1 Create a Windows Hardening Policy
  
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
default_source :supermarket

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
  
