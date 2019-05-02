<a name="permissions"> </a>

Back to:
- <div ><a href="../README.md/#ad">Active Directory</a></div>
- <div ><a href="./Microsoft-Graph.md#microsoftgraph">Microsoft Graph</a></div>
- <div ><a href="./Graph-API.md/#graph">Graph API</a></div>

# **Permissions**
Also known as scopes, permissions provide a way to limit the amount of access that is granted to an [access token](./Access-Tokens.md/#access_token). Example, a token issued could grant only READ access or it could also grant READ and WRITE access that enables the app/user to only see or make changes to protected resources.

Permissions make authorization quick. The resource only needs to check if the access token contains the appropriate permission to whatever API the app/user is calling.

### Important notes for Microsoft Graph for Client Portal

- Authentication is implemented based on delegated permissions with the access token given from AD.
- Members and Guest userTypes are set to have the same privileges
- AW members should be able to manage all users and groups
    ### Implications for permission management in Groups (Projects)
    ```
    a) All AW members need to have the User Account Administrator role
    b) Only Global Admins/ Privileged Role Administrator can assign roles
    c) Only users can be assigned roles. Not groups
    ```
    ### Conclusion
    ```
    -Manually assign new AW members the User Access Administrator role through the Azure Portal using the AnalysisWorks account.

    -Adding roles to groups is currently a high priority for Microsoft Azure on their website. For now, the work around is  to deploy a Powershell that synchronizes group members with role members of a specific role.
    ```

---

### Types:
1) Delegated permissions
    - Are used by apps that have a signed-in user present

2) Application permissions
    - Are used by apps that run without a signed-in user present; for example, apps that run as background services

### AD-Wide Permissions

Roles:

- There are many roles you can choose to manage your AD.
A comprehensive list can be found [here](https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-assign-admin-roles)


How to assign roles

     ⟶ Sign-in as Global Admin/Privileged Role Administrator (ex. AnalysisWorks)
        ⟶ Active Directory
            ⟶ Roles and administrators
                ⟶ Click on the role
                    ⟶ + Add member

- You can also create custom roles by [powershell](https://docs.microsoft.com/en-us/azure/role-based-access-control/tutorial-custom-role-powershell) or [cli](https://docs.microsoft.com/en-us/azure/role-based-access-control/tutorial-custom-role-cli)


<a name="appRoles"></a>


### App Permissions (app specific)


Aside from AD-Wide Roles, you can have app specific ones as well
- If you don't want the entire organization directory to have access to a specific app, you can enable user assignment. (Client Portal uses this).
- App roles are simply defined when created. To use these roles, you'll need to implement them on the **front end**. (more roles = future consideration for more security in Client Portal)
- While inviting a user to Active Directory, Client Portal also attaches an appRole to register them in the app as well.
    -  An example Graph API call to set the App Role can be seen [here](./Graph-API.md/#enterprise)



How to create app roles


        ⟶ Active Directory
            ⟶ App registrations
                ⟶ ClientPortalWebFrontEnd
                    ⟶ Manifest
                        ⟶ Field called appRoles



- an appRole's id should be a new GUID.
Get a new one [here](https://www.guidgenerator.com/)

    it will look something like this (appRole id's are dummy values).
    <img src="../../../assets/Active-Directory-Images/appRole.png">


<a name="userPermissions"></a>

### <div ><a href="./Users.md#users">User Permissions</a></div>

With [Microsoft Graph](./Microsoft-Graph.md/#microsoftusers) / [Graph API](./Graph-API.md/#graph), you can perform CRUD operations on users depending on your permissions.


By default, both Members and Guests have different privileges but this is easily changed.

Client Portal currently gives both Guests and Members the same privileges.

How to edit this

        ⟶ Active Directory
            ⟶ Users
                ⟶ User settings
                    ⟶ Manage external collaboration settings
                        ⟶ Guest users permissions are limited (Set this to No)

- Members and guests can be separately given permission to invite others to AD in the external collaboration settings

- By default, invited users are given the user type  **Guest**.

 - It's possible to <a href="./Microsoft-Graph.md/#invite">invite a user</a>  as a  Member instead in the request body if the person sending the invitation has sufficient permission.
    -   (see invitedUserType property in [invitation object](https://docs.microsoft.com/en-us/graph/api/resources/invitation?view=graph-rest-1.0) for your request body)

<img src="../../../assets/Active-Directory-Images/memberVguest.png">


<a name="groupPermissions"></a>

### <div ><a href="./Groups.md#groups">Group Permissions</a></div>

Group owners:
- Owners can manage properties of the group like user management.
- Owners can only manage groups they own.
- To assign a group owner see <a href="./Microsoft-Graph.md/#microsoftqueryowner">Microsoft Graph</a>

Group members:
- Can read things inside the group including users and content
- Can't manage users

---


Back to:
- <div ><a href="../README.md/#ad">Active Directory</a></div>
- <div ><a href="./Microsoft-Graph.md#microsoftgraph">Microsoft Graph</a></div>
- <div ><a href="./Graph-API.md/#graph">Graph API</a></div>
