

## Security > NHN Bastion > Console User Guide
NHN Bastion allows you to control access to instances on NHN Cloud. This document describes how to create an NHN Bastion in the NHN Cloud console, connect it to the instances you need access to, and manage users and their policies, resources, and history.
### Get Started

1. To use NHN Bastion, the first step is to enable the NHN Bastion service.
2. NHN Bastion requires permission to access your resources through API connections to provide services.

### Create an NHN Bastion
![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/1bd26cd2-66f8-461c-9034-e4e8b4f9fabe)

1. Go to **Security > NHN Bastion**.
2. Set each item and click **Create Instance** at the bottom.
    * **Name**(required): A name to distinguish between web terminals if you use multiple web terminals.
    * **Type**(required): Instance performance of the web terminal.
        * Performance refers to the number of SSH sessions a web terminal can create simultaneously
    * **VPC**(required): The VPC to use for the web terminal.
    * **Subnet**(required): The subnet to use for the web terminal.
    * **Floating IP**(optional): Whether to use a floating IP for the web terminal.

> [Note]
> * The first web terminal created can't change or delete its specs until the service is deactivated.
> * You can only connect a floating IP to a VPC that has an Internet gateway connected.
> * The web terminal must allow SSH communication with the access target in order to access the access target (e.g., Security Groups, etc.).

### Connecte Instances

In <strong>Manage Policies</strong>, users only see a list of instances they are allowed to access, and they can access the instances they are allowed to access.

#### Access Instances

1. On the instance you need access to, click **Connect**.
2. Select the web terminal you want to use to access your instance.
3. If there are multiple IPs of instances to access, select the IPs that are accessible.
4. Select an authentication method.
    * SSH key authentication
        * Enter the operating system account for the instance.
        * Upload the key you set when you created the instance.
    * Password authentication
        * Enter the operating system account for the instance.
        * Enter the password for the instance.
    * Authenticate with a temporary SSH key
        * Enter the operating system account for the instance.

> [Caution]
> * You need to select a web terminal that can SSH communicate with your instance.
> * Password authentication requires the operating system to enable password access.
> * Temporary SSH key authentication is only accessible through the web terminal from which you copied the script.

### Manage Users

You can view a list of users authorized to use the NHN Bastion service and create and manage user groups.

#### User List
![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/38558289-7b24-4030-899a-9a202b75a2e4)

A list of users authorized to use the NHN Bastion service. You can check the user's permissions and when they last accessed the instance.

#### Group
![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/99d6d052-bca4-4150-a55e-99d375de7dc1)

You can create and manage user groups, and the groups you create can be enrolled as access subjects on the <strong>Manage Policies</strong> tab.

* **+ Create Group**: You can create a group by adding or deleting users who are authorized to the NHN Bastion service. 
* **Copy**: Copy the created user group.
* **Modify**: Modify the created user group.
* **Delete**: Delete the selected user group.

### Manage Policies
![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/aa73734d-0e34-4411-91a8-3b2831a2f4b9)

You can set access control policies and command control policies for instances enrolled in the connection target.
Each policy has a priority, and policies are applied in order of highest priority.

[Example]
If the policy is applied as shown in the [table] below,
* user A can access Instance A, only the `shutdown` command is unavailable, other commands are available
* user A can access Instance B, only the `cd` command is available, no other commands
* user B can access Instance A, only the `reboot` command is unavailable, other commands are available
* user B cannot access Instance B

| Priority | User | Access Target | Command Policy |
| ---- | --- | ---- | ------ |
| #1 | user A | Instance A | [Deny] shutdown |
| #2 | user A | Instance A, Instance B | [Allow] cd |
| #3 | user B | Instance A | [Deny] reboot |

#### Create a policy
![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/3d0c97b7-43fd-459c-95f3-a9873cc1f861)

* **Name**: You can type a name for the policy.
* **Description**: You can enter a description of the policy.
* **User and Group**: You can enroll users and user groups as access subjects.
* **Resource and Resource Group**
    * **All Resources**: You can register all resources added to the connection target as access targets.
    * **Select Resource**: You can enroll instances, autoscaling groups, and resource groups as access targets.
* **Command Control Policy**
    * **Allow**: Only registered commands can be used (whitelist approach).
    * **Deny**: Using registered commands is blocked (blacklisting method).


> [Caution]
The following commands are blocked regardless of whether you have a command policy enrollment.
> * Bypass blocking commands: SSH, TELNET, SFTP, RCP, SCP, FTP, RSAP, RLOGIN, etc.



#### Change the policy order
![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/2342d3d9-a7f9-43d4-a474-0c514d5dda0b)

You can change the priority of a policy.
   1. Change the order of the selected policies to the desired priority, and click **Modify**.
   2. In **After Reorder**, see a preview of the priorities you modified. The preview shows the policies in first and last order based on the policy you modified. 
   3. Click **Save** to change the priority of the policy.

#### Manage Policies

* **Modify**: You can edit the contents of the selected policy.
* **Copy**: You can copy the selected policy.
* **Delete**: You can delete the selected policy.
* **View Details**: You can view the details of the selected policy.

### Manage Resources

#### Manage Instances
![image](https://github.com/user-attachments/assets/6f1b2e4d-4214-409b-83a1-1e92e37c2d65)

On **Manage Resources > Manage Instances** tab, you can add instances registered within a project as connection targets.
It provides access control policy and command control policy capabilities for registered instances.

* **+ Add**: You can select an instance that exists within your project and add it as a connection target.
* **Delete**: You can delete the selected instance from the connection target.
* **Delete All**: You can delete all registered instances from the connection target.
* **Auto-Registration**: All instances that exist within a project are automatically added to the connection target. Instance creation/deletion is also automatically reflected and added as a connection target.

> [Caution]
Changes to create or delete instances can take up to 5 minutes to be reflected.

* Connection Settings
    * You can change the session timeout for the selected instance.
        * Default setting: Session timeout applied in preferences
    * You can change the connection port for the selected instance.
        * Default setting: Connection port applied in preferences

#### Web terminal management
![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/96c8c655-c9f2-45a7-a56c-acccc16cce59)

In **Manage Resources > Manage Web Terminals**, you can create/manage web terminal instances that provide the terminals and bathtubs needed to access the instance.

* **+ Create**
    * **Name** (required): A name to distinguish between web terminals if you use multiple web terminals.
    * **Type** (required): Instance performance of the web terminal.
        * **Performance**: The difference in performance is the number of SSH sessions the web terminal can create simultaneously.
    * **VPC**(required): The VPC to use for the web terminal.
    * **Subnet**(required): The subnet to use for the web terminal.
    * **Floating IP** (optional): Whether to use a floating IP for the web terminal.
* **Delete**: You can delete the selected web terminal.

> [Caution]
You cannot delete the first web terminal created when the service is activated; you must deactivate the service to delete it.

* **Floating IP**

    ![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/5daf1846-662a-4e0f-bfc7-96120c4fcedf)
    * You can set whether to use a floating IP for the web terminal.
    * Customers using the Network Firewall service to DNAT a public IP can set the **Redirection** feature to **enable** to enter the public IP.
    * Customers using on-premises DNS can **enable** the **Redirect** feature to enter their domain address.
* **IP Access Control**
  
    ![image](https://github.com/user-attachments/assets/0559ba4a-960c-42cc-918f-37c83fd95ac0)
    * You can enter a CIDR that requires access to the web terminal.


> [Note]
IP access control is provided on a whitelist basis.

* **Script**
  
     ![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/d62cab1a-a01b-42f3-aca3-0afc4577c9e9)
    * Provides a script that needs to be run on the target instance to utilize the approach with ephemeral SSH keys.


> [Caution]
> * The temporary SSH key approach only applies to the selected web terminal, and is not available when accessed through other paths.


#### Resource Groups

The **Manage Resources > Resource Groups** tab lets you create and manage groups of instances that are registered to a connection target.

![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/9d4a9853-d841-4f02-9aa3-c1f88e96d708)
* **+ Create Group**: You can create a group by adding or deleting instances that are registered with the connection target.
* **Copy**: You can select a group of resources to copy.
* **Modify**: You can edit the resource group.
* **Delete**: You can select a group of resources to delete them.

### Manage History

You can use the NHN Bastion service to manage the history of accesses to your instance. 
History is kept for up to 6 months, and if you need to keep it longer than 6 months, you can back it up to Object Storage in <strong>Preferences > Manage Logs<strong>.

#### Live Sessions
![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/6add576a-bb0b-4082-9630-92efc11dc0e1)

You can see which sessions are connected to your instance in real time, and you can block connections.

* **Block**: Block a connected user session.
* **View**: View details of connected user sessions.

#### User Access History
![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/0ed78c6d-a19b-4bcf-bacf-f845749af783)

You can see the history of how users have accessed your instance, and you can use search criteria and access times to get the history you want.

* **Details**
    * **Basic Information**: View session information for the selected history.
    * **Log**: View the history of commands used in the selected history session.

#### Command Usage History
![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/68817f99-ec1e-473f-a7bf-2019b45be283)

You can see what commands users have used, and you can use search criteria and access times to look up the history of any command you want.

* **Register Command**: Register your key commands so you can look up their usage history at your fingertips.
* **Details**
    * **Basic Information**: View session information for the selected history.
    * **Log**: View the history of commands used in the selected history session.

### Set up Preferences
![image](https://github.com/user-attachments/assets/7b1afdc1-42db-4ab9-9c5a-f473a985d16e)

#### Session Timeout

You can set the session timeout time served by the web terminal when accessing an instance.


> [Note]
This is a session timeout setting provided by the web terminal, so it works independently of the session timeout set by the operating system.


#### Maximum connection sessions

You can set the maximum number of sessions a user can connect to at the same time.

#### Manage Logs

You can back up logs provided by the NHN Bastion service to your own Object Storage.

* **Access Key**: S3 API credentials access key
* **Secret** key: S3 API credentials secret key
* **Bucket Name**: The name of the bucket you created
* **Endpoint**
  * Korea (Pangyo) region: https://kr1-api-object-storage.nhncloudservice.com
  * Korea (Pyeongchon) region: https://kr2-api-object-storage.nhncloudservice.com
  * Korea (Gwangju) region: https://kr3-api-object-storage.nhncloudservice.com
* **Region**
  * Korea (Pangyo) Region: KR1
  * Korea (Pyeongchon) Region: KR2
  * Korea (Gwangju) Region: KR3
  
> [Caution]
Object Storage backups in other regions are not supported if the VPC where the web terminal was initially created does not have an Internet gateway.

#### Manage Connection Ports

You can specify the SSH port used to access the instance.


> [Note]
The port used to connect from the web terminal to the instance being accessed, which must be set to the SSH port set by your operating system.


#### Delete NHN Bastion

Delete all resources created by the NHN Bastion service.


> [Caution]
Data created while using the NHN Bastion service and all resources within the service will be deleted, and information cannot be recovered once deleted.

### Web Terminal
Provides a browser-based web terminal with file upload/download capabilities.

#### Transfer Files
You can launch the file navigator by clicking the right arrow button. The file navigator allows you to upload or download files from the desired path.

>[Caution]
File transfer is based on the permissions of the operating system account that was initially accessed, and account changes via the su command are not reflected.

