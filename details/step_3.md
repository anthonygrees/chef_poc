## Step 3. Chef Automate Profiles
  
[Return to Main Menu](../README.md#index)
  
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
  
  
  
[Return to Main Menu](../README.md#index)
  
  
## Next Step
  
[Next Steps >> Step 4](/details/step_4.md)
  
