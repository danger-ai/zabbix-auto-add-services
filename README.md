# zabbix-auto-add-services

> Zabbix Script - Create a Service for each Host/Trigger when it contains a TAG_NAME.

Creating IT Services in Zabbix could be exhausting in an environment with many Hosts and Triggers.

This Script allows you to manage the creation of IT Services automatically based on Triggers TAG NAME.

The services are created hierarchically in 3 levels:\
The first level correspond to Host Group that has Hosts with Triggers contain a certain TAG NAME . \
The second level correspond to Hosts that has Triggers with a certain TAG NAME. \
Finally, the third level is services that correspond to Triggers.
The Hierarchy is like that:

- HOST_GROUP_NAME |GroupID=#ID|
  - HOST_NAME |HostID=#ID|
    - TRIGGER_NAME

![Hierarchy_sample](/hierarquia.png)

## How It Works

The script scan all HOST_GROUP defined in config file then scan all host that has certain trigger with certain TAG_NAME.\
These triggers that have a TAG_NAME will be added to calculate the SLA. 
So the TAG_NAME needs to be added to a trigger. For example:

![Trigger_Sample](/trigger_tag.png)\
The above trigger was configured to have "SLA" TAG_NAME. Also, what is needed is to configure the variable TAG_NAME as "SLA"\
After execute the script the Services will be like this:

![Hierarchy_sample](/hierarquia.png)


## Requirements
 - python2 or python3
 - zabbix-api

## Installation
```
pip install zabbix-api
git clone https://github.com/danger-ai/zabbix-auto-add-services.git ./somedirectory
```
## Config

The config file has two semicolon-delimited columns (";"). This is required.\
The first column is the name of group host.\
The second is the algorithm used to calc SLA, possible values are:\
 0 - do not calculate;\
 1 - problem, if at least one child has a problem;\
 2 - problem, if all children have problems.

Example of the config file:
```
GROUP_NAME_A;1
GROUP_NAME_B;1
GROUP_NAME_C;2
GROUP_NAME_D;2
```

## Usage example

The config file has to be in the same directory of script with the name "config".

Modify the follow variables in auto-add-services.py to fit your needs.
```python
# Parameters
SERVER = "http://127.0.0.1" # Your Zabbix IP
USERNAME = "your_username"
PASSWORD = "your_pass"
TAG_NAME = "SLA"
```

After this you can execute the script:

```
./somedirectory/auto-add-services.py
```

## Release History
* 0.2
    * Child Services for each host are only added if there is more than one Trigger associated. PEP Conformity fixes.
* 0.1
    * Initial code
