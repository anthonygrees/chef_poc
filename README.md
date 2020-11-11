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
  
### System Requirements
For further details please refer to the Chef Docs page - https://docs.chef.io/automate/system_requirements/
  
#### Hardware
Chef Automate requires a minimum of:  
- 16 GB of RAM
- 80 GB of disk space (available to /hab)
- 4 vCPUs
  
#### Operating system
Chef Automate requires:  
- a Linux kernel of version 3.2 or greater
- systemd as the init system
- useradd
- curl or wget
- The shell that starts Automate should have a max open files setting of at least 65535
- Commercial support for Chef Automate is available for platforms that satisfy these criteria.
  
#### Supported Browsers
Chef Automate supports the current browser versions for Chrome, Edge, and Firefox. Chef Automate does not support other browsers and may not be compatible with older browser versions.
