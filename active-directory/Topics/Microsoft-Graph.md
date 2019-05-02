<a name="microsoftgraph"> </a>

[back to Active Directory](../README.md/#ad)

# **Microsoft Graph Queries**

### Before you jump into this section, check out [Permissions](./Permissions.md/#permissions) first!

Microsoft Graph is an API platform that enables developers to connect to many types of resources (ex. Users, Groups) and allows you to manipulate them all though a single endpoint - https://graph.microsoft.com

<img src="../assets/microsoft-graph.png" >

----

## Prerequisites
-  access token of user who has authorization (ex. admin, member)

### Notes
- Your access token contains user details therefore you will have the corresponding permissions

- At this point in time, Microsoft Graph is on version v1.0.


### Important notes for Microsoft Graph for Client Portal
- When creating a project there are 3 steps involved

    - These steps are all done in our API. The resource locations are listed below

    1) Create group in Active Directory (Analysis works account in Azure Portal)
        ```
        ⟶ Active Directory
            ⟶ Groups
        ```
    2) Create file container in Active Directory (it account in Azure Portal)
        ```
        ⟶ Storage accounts
            ⟶ clientportalprojects
                ⟶ Scroll down left nav bar to Blob Service
                    ⟶  Blobs
        ```
    3) Create a Project in Microsoft SQL (in Microsoft SQL Server Management Studio)
        ```
        ⟶ Databases
            ⟶ clientPortal2
                ⟶ Tables
                    ⟶ dbo.Projects
        ```

- You can delete groups but for Client Portal, we simply set their ActiveInd field in Microsoft SQL to false

- You can also delete users but for Client Portal, we simply set their Account Enabled field in MicrosoftGraph to false

- When creating a group(project), you are  **NOT**  automatically added as an owner or a member
    ### Implications to Admin/Projects table
    ```
    a) You can only manage users in groups you own (unless you are an admin)
    b) The query below returns us the groups (projects) which the user is only a member of

    We query a user's projects on the front-end by hitting the endpoint: https://graph.microsoft.com/v1.0/me/memberOf

    c) Does not query the projects a user owns ⟶ projects a user creates won't show
    ```
    ### Conclusion
    ```
    ⟶ Create the group
        ⟶ Add user as an owner to give privileges to manage users
            ⟶ Add user as a member to render on the front-end
---

## Endpoint
http://graph.microsoft.com/v1.0/{resource}/

- resource (ex. users, groups, me)


## Queries The Client Portal Uses

<a name="microsoftusers"></a>

## Users
Headers

| Key        | Value
| ------------- |:-------------:
| Authorization     | Bearer {access_token}
| Content-Type  | application/json|


### Get
- all users
    - GET  https://graph.microsoft.com/v1.0/users/$filter=something&$select=something

- a specific user
    - GET https://graph.microsoft.com/v1.0/users/{id | userPrincipalName}

- groups a user belongs to
    - GET https://graph.microsoft.com/v1.0/users/{userId}/memberOf

- logged-in user
    - GET http://graph.microsoft.com/v1.0/me/

- groups a logged-in user belongs to
    - GET http://graph.microsoft.com/v1.0/me/memberOf

<a name="invite"></a>

### Create
-  a Guest (_more of an invite_) or Member
    1) POST https://graph.microsoft.com/v1.0/invitations
    2) Request Body (JSON):
        ```
        {
        "invitedUserEmailAddress":"{userEmail}",
        "inviteRedirectUrl":"{redirectLink}",
        "sendInvitationMessage":true
        "invitedUserType":"member" or "guest" //optional
        }

        - if invitedUserType is not specified, it defaults to a guest member
        ```
    Successful response: 201 Created

    [read more.](https://docs.microsoft.com/en-us/graph/api/invitation-post?view=graph-rest-1.0)

### Edit
- a User
  1) PATCH https://graph.microsoft.com/v1.0/users/{userId | userPrincipalName}
  2) Request Body (JSON):
        ```
        {
            {UserField}:{NewValue}
        }
        ```
    Successful response: 204 No Content

<a name="microsoftgroups"></a>

## Groups
Headers:

| Key        | Value
| ------------- |:-------------:
| Authorization     | Bearer {access_token}
| Content-Type  | application/json|

### Get

- all groups
    - GET http://graph.microsoft.com/v1.0/groups/
- a specific group
    - GET http://graph.microsoft.com/v1.0/groups/{groupId}
-  members of a group
    - GET http://graph.microsoft.com/v1.0/groups/{groupId}/members

### Create
- a Group (project) in AD
  1) POST https://graph.microsoft.com/v1.0/groups
  2) Request Body (JSON):
        ```
        {
            "description": {Project Description},
            "displayName": {Project Name},
            "mailEnabled": boolean,
            "mailNickname": {mailNickname},
            "securityEnabled": boolean,
            "owners@odata.bind": [
                "https://graph.microsoft.com/v1.0/users/{ownerId}"
            ]
        }

        - owner will be Analysis Work's user id
        ```
    Successful response: 201 Created


    [read more.](https://docs.microsoft.com/en-us/graph/api/group-post-groups?view=graph-rest-1.0)



### Add
- an existing user to a group

  1) POST https://graph.microsoft.com/v1.0/groups/{groupId}/members/$ref
  2) Request Body (JSON):
        ```
        {
            "@odata.id": "https://graph.microsoft.com/v1.0/directoryObjects/{userId}"
        }
        ```

    Successful response: 204 No Content

    [read more.](https://docs.microsoft.com/en-us/graph/api/group-post-members?view=graph-rest-1.0)

<a name="microsoftqueryowner"></a>

- an existing user as a group owner

    1) POST https://graph.microsoft.com/v1.0/groups/{groupId}/owners/$ref
    2) Request Body (JSON):
        ```
        {
            "@odata.id":"https://graph.microsoft.com/v1.0/users/{userId}"
        }
        ```

    Successful response: 204 No Content

    [read more.](https://docs.microsoft.com/en-us/graph/api/group-post-owners?view=graph-rest-1.0)


---

[back to Active Directory](../README.md/#ad)

