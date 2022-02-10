# Restrict app permissions to mailboxes

When building an application which needs to send email there are two types of API permissions **Delegated** and **Application**. **Delegated** is used if the application acts on behalf of a user, where the user signs into the application. **Application** on the other hand allows the application to impersonate any and all users and do not require the consent of the user. This is useful if having a background application. From a security standpoint this is undesirable, a way to solve this is by applying a **ApplicationAccessPolicy** which limits which users the application can impersonate.

1. Fire up powershell with elevated rights
```shell
sudo pwsh
```
2. Install the necessary modules
```powershell
Install-Module -Name PSWSMan
Install-WSMan
Install-Module -Name ExchangeOnlineManagement
```
3. After installation Import and connect to exchange online
```powershell
Import-Module ExchangeOnlineManagement
Connect-ExchangeOnline -UserPrincipalName <UID>
```
\<UID> should be the email of a user/serviceaccount with admin rights, otherwise you can't use the following commands

Before the next step you need the following
* The app´s application (client) ID
* A mail-enabled security group and the email for that group

4. Apply an new ApplicationAccessPolicy
```powershell
New-ApplicationAccessPolicy -AppId e7e4dbfc-046f-4074-9b3b-2ae8f144f59b -PolicyScopeGroupId EvenUsers@contoso.com -AccessRight RestrictAccess -Description "Restrict this app to members of distribution group EvenUsers."
```
5. If created succesful you can test the policy 
```powershell
Test-ApplicationAccessPolicy -Identity user1@contoso.com -AppId e7e4dbfc-046-4074-9b3b-2ae8f144f59b
```
And it will test if the application has access to user1´s mailbox