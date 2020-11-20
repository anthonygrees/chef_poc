## Step 2. Install Chef Workstation
  
[Return to Main Menu](../README.md#index)
  
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
  
  

  
[Return to Main Menu](../README.md#index)
  
  
## Next Step
  
[Next Steps >> Step 3](/details/step_3.md)
  
