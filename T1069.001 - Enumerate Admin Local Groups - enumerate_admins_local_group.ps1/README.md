## T1069.001 Traces

Executions of powershell script [enumerate_admins_local_group.ps1](https://raw.githubusercontent.com/mthcht/Purpleteam/main/Simulation/Windows/Accounts/enumerate_admins_local_group.ps1) from windows [simulation scripts repo](https://github.com/mthcht/Purpleteam/tree/main/Simulation/Windows).

This script is used to simulated [T1069.001](https://attack.mitre.org/techniques/T1069/001/) MITRE technique and trigger EventID 4799.

In these directories you should see all the EventID details triggered by the execution of the script.

### Triggered Events
| EventLog Path                            | EventIDs       |
|------------------------------------------|----------------|
| Security                                 | 4799,4688,4656 |
| Microsoft-Windows-PowerShell/Operational | 4104           |
| Windows PowerShell                       | 400,403,600    |

### EventID 4799:
Enumerating a security local group should generate this EventID.

We enumerated the users of all the groups with high privileges on the machine, below we can see the 4799 EventID triggered for the user enumeration of Administrators group 
![image](https://user-images.githubusercontent.com/75267080/208239540-11109c7c-057b-455c-979b-6234d3c9f8f0.png)

For each group enumerated we will have an Event like this one.

### Detection:
Look for EventID 4799 and filter false positives based on user Subject Account Name and Process Name.

Normal Behavior that should be exclude from your detection rule:
![image](https://user-images.githubusercontent.com/75267080/208239855-d8b94b7e-cd77-4702-918c-0221ead8e042.png)

System Subject Account Names (`*$`) and System32 executables

- Detection proposals
  - User enumeration of a group with high privileges 
  - User enumeration of multiple groups with high privileges in a short period of time (higher severity)

The other EventID should be used for event correlation :
- powershell event log events allows us to retrive the script executed and his content
- security event logs allows us to see how the script was executed.
These are specifically triggered by this script but other methods of group enumeration can be logged elsewhere, the important EventID that should be triggered everytime is 4799.
