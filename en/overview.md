## Security > NHN Bastion > Overview


NHN Bastion is a server access control service for secure access to infrastructure assets used for instances on NHN Cloud.


### Main Features


* Allows you to control SSH access to your instance, and register commands to allow and block.

* Provides a history of instance access and command usage, and you can back up the history to Object Storage.

* Provides a variety of instance authentication methods, including SSH keys, passwords, and temporary SSH keys.

* Provides a browser-based SSH experience to easily connect to your instances.

* Reflects instances that are created or deleted automatically as service targets.
* Provides real-time monitoring and session blocking.


### Feature Description


#### Connect Instances


* Displays only the instances you can currently access based on the access control policies granted to you.

* Provides three access methods - SSH key, password, and temporary SSH key - so you can use the authentication method that fits your environment.


#### Manage Users


* Provides user information authorized to use the NHN Bastion service and shows the date of last access.

* Creates and manages user groups, and group users by role, department, and more.


#### Manage Policies


* Creates and manages access control and command and control policies for users.


#### Manage Resources


* Registers instances that are subject to access control.

* Autoscaling groups can be registered as access control targets and will be automatically reflected when scale in/out occurs.

* When you enable the auto-registration feature, all instances within the project are registered, and changes to the instances are automatically reflected.

* Manages servers that provide terminals.
* Allows you to check scripts for using temporary SSH key access methods.


#### Manage History


* Monitors connected sessions in real time, and you can block sessions that are connecting.

* Audits user access history and command usage history.

#### Set up Preferences


* Provides configuration features such as session timeout, maximum connected sessions, log backup, and connection port management.

