## Security > NHN Bastion > Console User Guide
NHN Bastion allows you to control access to instances on NHN Cloud. This document describes how to create an NHN Bastion in the NHN Cloud console, connect it to the instances you need access to, and manage users and their policies, resources, and history.
### Get Started

1. To use NHN Bastion, the first step is to enable the NHN Bastion service.
2. NHN Bastion requires permission to access your resources through API connections to provide services.

### Create an NHN Bastion
![image](https://github.com/user-attachments/assets/bdcf129e-996a-44b1-8c17-09f4cddf9008)

1. Go to **Security > NHN Bastion**.
2. Set each item and click **Create Instance** at the bottom.
    * **Name**(required): A name to distinguish between web terminals if you use multiple web terminals.
    * **Availability** zone (required): Availability zone to use in the web terminal.
    * **Type**(required): Instance performance of the web terminal.
        * Performance refers to the number of SSH sessions a web terminal can create simultaneously
    * **VPC**(required): The VPC to use for the web terminal.
    * **Subnet**(required): The subnet to use for the web terminal.
    * **Floating IP**(required): Whether to use a floating IP for the web terminal.
    * **Encryption**(required): Whether to store logs encrypted or not
        * Symmetric key ID: The symmetric key ID managed by the Secure Key Manager service.
        * To set up encryption, you must generate a symmetric key in the Secure Key Manager service beforehand.

> [Note]
> * The first web terminal created can't change or delete its specs until the service is deactivated.
> * You can only connect a floating IP to a VPC that has an Internet gateway connected.
> * The web terminal must allow SSH communication with the access target in order to access the access target (e.g., Security Groups, etc.).
> * Log encryption for: user ID, email, commands

> [Caution]
> * If the Secure Key Manager service rotates the symmetric key you set up for log encryption, you need to be careful not to immediately delete the old version of the key.
> * If you delete the symmetric key that you set for log encryption in the Secure Key Manager service, the encrypted logs cannot be decrypted. You must manage the symmetric key carefully to avoid accidentally deleting it.

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
![image](https://github.com/user-attachments/assets/0a3683bb-5661-4867-83a6-6a8ae795116f)

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

#### Batch Register
![image](https://github.com/user-attachments/assets/0f6e47fe-f870-4a8a-a08b-1749a39decef)

You can register policies in bulk using the provided templates, and you can download and back up created policies.
* **Batch Register**: Provides policy uploads in bulk using templates
* **Batch Download of Policies**: Provides a download of a list of currently applied policies

### Manage Resources

#### Manage Instances
![image](https://github.com/user-attachments/assets/5313b554-47f7-4b54-acbe-0f6b29424a39)

On **Manage Resources > Manage Instances** tab, you can add instances registered within a project as connection targets.
It provides access control policy and command control policy capabilities for registered instances.

* **+ Add**: You can select an instance that exists within your project and add it as a connection target.
* **Delete**: You can delete the selected instance from the connection target.
* **Delete All**: You can delete all registered instances from the connection target.

* **Connection Settings**
    * You can change the session timeout for the selected instance.
        * Default setting: Session timeout applied in preferences
    * You can change the connection port for the selected instance.
        * Default setting: Connection port applied in preferences
     
* **External Resources**
   * **Registration**: You can manually register servers in legacy environments, instances from other projects, and more.
![image](https://github.com/user-attachments/assets/1ab9806a-061c-478b-b9e0-38a5dabe4f50)

   * **Batch Register**: You can register external resources in bulk using templates.
![image](https://github.com/user-attachments/assets/6806a22f-8a6a-4d38-b42e-1aa857004670)

> [Caution]
For registered external resources, SSH communication is attempted from the web terminal based on IP. You can use the service only in an environment where communication from the web terminal to the destination server is possible.
     
* **Auto-Registration**: All instances that exist within a project are automatically added to the connection target. Instance creation/deletion is also automatically reflected and added as a connection target.

> [Caution]
Changes to create or delete instances can take up to 5 minutes to be reflected.

* **Download service utilization**: You can download a list of currently registered service targets.

#### Web terminal management
![image](https://github.com/user-attachments/assets/237364f8-f54b-4e32-9f6c-5a5dfef6574e)

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
    * **If** a customer using the Network Firewall service uses the DNAT feature to assign a public IP to the web terminal, you can enable the **redirection** **feature**to enter the public IP of the web terminal.
    * Customers using on-premises DNS can **enable**the **redirect** feature to enter the domain address of the web terminal.

* **IP access control** 

     ![image](https://github.com/user-attachments/assets/0559ba4a-960c-42cc-918f-37c83fd95ac0)
    * You can enter a CIDR that requires access to the web terminal.

> [Note]
> IP access control is provided on a whitelist basis.

* **Update Static Routing**

   * Apply the static routing policy applied to the subnet to the web terminal.

* **Script**
  
    ![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/d62cab1a-a01b-42f3-aca3-0afc4577c9e9)
    * Provides a script that needs to be run on the target instance to utilize the approach with ephemeral SSH keys.

> [Caution]
> * The temporary SSH key approach only applies to the selected web terminal, and is not available when accessed through other paths.
> * The instance IP to access must be added to **IP Access Control** on the web terminal.
> * Port 443 outbound policy must be added to the web terminal IP in the Security Groups of the instance to access.


#### Resource Groups

The **Manage Resources > Resource Groups** tab lets you create and manage groups of instances that are registered to a connection target.

![image](https://github.com/jongwoo-kim-nhn/NHNBastion/assets/174567179/9d4a9853-d841-4f02-9aa3-c1f88e96d708)
* **+ Create Group**: You can create a group by adding or deleting instances that are registered with the connection target.
* **Copy**: You can select a group of resources to copy.
* **Modify**: You can edit the resource group.
* **Delete**: You can select a group of resources to delete them.

### Manage History

You can use the NHN Bastion service to manage the history of accesses to your instance. You can set the log retrieval period up to one week.
History is kept for up to 6 months, and if you need to keep it longer than 6 months, you can back it up to Object Storage in <strong>Preferences > Manage Logs<strong>.

#### Live Sessions
![image](https://github.com/user-attachments/assets/ca70cf06-2d74-4ca7-ab57-51e02ec6b083)

You can see which sessions are connected to your instance in real time, and you can block connections.

* **View Details**: View details about the connected user session.
* **Block Sessions**: Block a connected user session.

#### User Access History
![image](https://github.com/user-attachments/assets/2defa233-35b5-4dde-82bf-7e0238bc055b)

You can see the history of how users have accessed your instance, and you can use search criteria and access times to get the history you want.

* **View details**
    * **Basic Information**: View session information for the selected history.
    * **Log**: View the history of commands used in the selected history session.

#### Command usage history
![image](https://github.com/user-attachments/assets/1c6446b9-e452-49fa-837d-5d5122191678)

You can see what commands users have used, and you can use search criteria and access times to look up the history of any command you want.

* **+ Command register**: Register your favorite commands so you can conveniently look up their usage history.
* **Select commands**: You can select any of your registered commands to add to your search criteria.
* **View details**
    * **Basic Information**: View session information for the selected history.
    * **Log**: View the history of commands used in the selected history session.
 
#### File transfer history
![image](https://github.com/user-attachments/assets/27313eff-6b28-4908-943a-c6c3d621df57)

You can see the history of users uploading/downloading files to your instance, and you can use search criteria and access times to get the history you want.

* **View details**
    * **Basic Information**: View session information for the selected history.
    * **Log**: View the history of commands used in the selected history session.

### Set Up an Environment
![image](https://github.com/user-attachments/assets/b88fbdf3-a16c-426a-8a5f-a71ced18cc98)

#### Session timeouts

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
  * Korea (Pangyo) region
  * Korea (Pyeongchon) region
  * Korea (Gwangju) region: https://kr3-api-object-storage.nhncloudservice.com
* **Region**
  * Korea (Pangyo) Region: KR1
  * Korea (Pyeongchon) Region: KR2
  * Korea (Gwangju) Region: KR3

> [Note]
> When log encryption is enabled, logs are decrypted and backed up to Object Storage.
> Logs are backed up to Object Storage from the time you set up the backup. Previously saved logs are not backed up.

> [Caution]
> Object Storage backups in other regions are not supported if the VPC where the web terminal was initially created does not have an Internet gateway.

#### Manage Connection Ports

You can specify the SSH port used to access the instance.

> [Note]
> The port used to connect from the web terminal to the instance being accessed, which must be set to the SSH port set by your operating system.

#### Encryption

You can see whether encryption is enabled and the applied symmetric key ID.
If you performed key rotation in Secure Key Manager, you can update an older version of the key to the latest version via the Rotate key button.

> [Caution]
> If you do not update the key in the NHN Bastion service after performing key rotation in Secure Key Manager, an error might occur in the log inquiry.

#### Delete NHN Bastion

Delete all resources created by the NHN Bastion service.

> [Caution]
> Data created while using the NHN Bastion service and all resources within the service will be deleted, and information cannot be recovered once deleted.


### Web Terminal
Provides a browser-based web terminal with file upload/download capabilities.

#### Transfer Files
You can launch the file navigator by clicking the right arrow button. The file navigator allows you to upload or download files with the desired path.
You can adjust the font size by clicking the right arrow button.

> [Note]
> File transfer is based on the permissions of the operating system account that was initially accessed, and account changes via the su command are not reflected.
> If you upload files by dragging and dropping, they are saved in your home directory, regardless of the current directory path.
