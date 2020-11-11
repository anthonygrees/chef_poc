```bash
 _____ _           __  ______     _____   _____          _                   _   _                 
/  __ \ |         / _| | ___ \   /  __ \ |_   _|        | |                 | | (_)                
| /  \/ |__   ___| |_  | |_/ /__ | /  \/   | | _ __  ___| |_ _ __ _   _  ___| |_ _  ___  _ __  ___ 
| |   | '_ \ / _ \  _| |  __/ _ \| |       | || '_ \/ __| __| '__| | | |/ __| __| |/ _ \| '_ \/ __|
| \__/\ | | |  __/ |   | | | (_) | \__/\  _| || | | \__ \ |_| |  | |_| | (__| |_| | (_) | | | \__ \
 \____/_| |_|\___|_|   \_|  \___/ \____/  \___/_| |_|___/\__|_|   \__,_|\___|\__|_|\___/|_| |_|___/
 ```
  
## Install Chef Automate and Chef Server
For a PoC we can install Chef Automate and Chef Server using an `all in one` deployment pattern.  
  
### 1. System Requirements
For further details please refer to the Chef Docs page - https://docs.chef.io/automate/system_requirements/
  
#### 1.1 Hardware
Chef Automate requires a minimum of:  
- 16 GB of RAM
- 80 GB of disk space (available to /hab)
- 4 vCPUs
  
#### 1.2 Operating system
Chef Automate requires:  
- a Linux kernel of version 3.2 or greater
- systemd as the init system
- useradd
- curl or wget
- The shell that starts Automate should have a max open files setting of at least 65535
- Commercial support for Chef Automate is available for platforms that satisfy these criteria.
  
#### 1.3 Supported Browsers
Chef Automate supports the current browser versions for Chrome, Edge, and Firefox. Chef Automate does not support other browsers and may not be compatible with older browser versions.
  
### 2. Installation Guide
For further details please refer to the Chef Docs page - https://docs.chef.io/automate/install/
  
#### 2.1 Download the Chef Automate Command-Line Tool
Download and unzip the Chef Automate command-line tool:
```bash
curl https://packages.chef.io/files/current/latest/chef-automate-cli/chef-automate_linux_amd64.zip | gunzip - > chef-automate && chmod +x chef-automate
```
  
#### 2.2 Create Default Configuration
Create a `config.toml` file with default values for your Chef Automate installation:
```bash
sudo ./chef-automate init-config
```
  
#### 2.3 Configure Chef Automate
You can customize your FQDN, login name, and other values, by changing the values in the config.toml in your editor.  
  
#### 2.4 Deploy Chef Automate and Chef Server
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
  
#### 2.5 Chef Automate Credentials
The deployment process writes login credentials to the `automate-credentials.toml` in your current working directory.
  
  
