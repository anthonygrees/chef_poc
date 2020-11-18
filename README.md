```bash
 _____ _           __  ______     _____   _____          _                   _   _                 
/  __ \ |         / _| | ___ \   /  __ \ |_   _|        | |                 | | (_)                
| /  \/ |__   ___| |_  | |_/ /__ | /  \/   | | _ __  ___| |_ _ __ _   _  ___| |_ _  ___  _ __  ___ 
| |   | '_ \ / _ \  _| |  __/ _ \| |       | || '_ \/ __| __| '__| | | |/ __| __| |/ _ \| '_ \/ __|
| \__/\ | | |  __/ |   | | | (_) | \__/\  _| || | | \__ \ |_| |  | |_| | (__| |_| | (_) | | | \__ \
 \____/_| |_|\___|_|   \_|  \___/ \____/  \___/_| |_|___/\__|_|   \__,_|\___|\__|_|\___/|_| |_|___/
 ```
  
## About
This repo contains detailed instructions on how to setup and configure a Chef Proof of Concept environment.  
  
  
## The Chef Approach
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
| 0.  | [Step 0. Prerequisites](/details/step_0.md) |
| 1.  | [Step 1. Install Chef Automate and Chef Server](/details/step_1.md) |
| 2.  | [Step 2. Install Chef Workstation](/details/step_2.md) |
| 3.  | [Step 3. Chef Automate Profiles](/details/step_3.md) |
| 4.  | [Step 4. Create a base policy and upload the Cookbooks you need to the Chef Server](/details/step_4.md) |
| 5.  | [Step 5. Bootstrap Nodes](/details/step_5.md) |
| 6.  | [Step 6. Hardening Nodes](/details/step_6.md) |
|     | |
  
  
  
  
## License and Author
  
* Author:: [Anthony Rees](<anthony@chef.io>)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
