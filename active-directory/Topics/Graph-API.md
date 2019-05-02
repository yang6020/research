<a name="graph"></a>

[back to Active Directory](../README.md/#ad)

# **Graph API Queries**

### Before you jump into this section, check out [Permissions](./Permissions.md/#permissions) first!

Graph API is an API platform that enables developers to connect to many types of resources (ex. Users, Groups) and allows you to manipulate them all though a single endpoint - https://graph.windows.net//

----

## Prerequisites
-  access token of user who has authorization (ex. admin, member)

### Notes
- Your access token contains user details therefore you will have the corresponding permissions

- Graph API will be deprecated soon but some functionality hasn't been implemented on Microsoft Graph yet.


## API
http://graph.windows.net/{tenantId}/{resource}?{apiVersion}

(Required)
- resource (ex. users, groups, me -> _if you are logged in_)

The only query we make the Graph API in the Client Portal is to add the user to the Client Portal app.
This became required when we changed Client Portal to 'user assigned' to secure the app internally.



resource = {enterpriseAppId}

Headers

| Key        | Value
| ------------- |:-------------:
| Authorization     | Bearer {access_token}
| Content-Type  | application/json|


<a name="enterprise"></a>
### Invite User to Enterprise App

1) POST  https://graph.windows.net/{tenantId}/users/{userId}/appRoleAssignments?api-version=1.6
2) Request Body (JSON):
    ```
    {
        "id":"{roleId}",
        "resourceId":"{enterpriseAppId}",
        "principalId":{userId}
    }
    ```
How to get:

- enterpriseAppId
    ```
    ⟶ Active Directory
        ⟶ Enterprise applications
            ⟶ Click on your app
                ⟶ Properties
                    ⟶ Object ID is enterpriseAppId
    ```
- roleId
    - appRoles are [here](./Permissions.md/#appRoles)
    - when an appRole is created, it will have an id, that id is roleId


[read more about app roles in general.](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)

---

[back to Active Directory](../README.md/#ad)

