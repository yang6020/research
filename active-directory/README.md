<a name="ad"> </a>

# **Active Directory Basics (AD)**<sub><img src="./assets/azure.png" height="50"></sub>


## Quick Overview
- Azure Active Directory (Azure AD) is a cloud-based service for user and resource management.

- Users and Groups are the main objects that will be read or written. Users are individual accounts while groups can contain many users.

- This is done through either the Azure Portal, Microsoft Graph, or Graph API.

- Microsoft Graph and Graph API both require an access token with proper privileges to read / read and write users and groups.

- These privileges, officially called permissions, provide a way to limit the amount of access that is granted to an access token.

----

### Learn more about AD with the links below.
- ### <div>[Users](./Topics/Users.md/#users)</div>
    - Types, sources
- ### [Groups](./Topics/Groups.md/#groups)
    - Types, owners, members, and nested groups
- ### [Permissions](./Topics/Permissions.md/#permissions)
    - Read this before proceeding to Microsoft Graph or Graph API
- ### [Access Tokens](./Topics/Access-Tokens.md/#access_token)
    - Authentication
- ### [Microsoft Graph](./Topics/Microsoft-Graph.md/#microsoftgraph)
    - Used for most queries in the Client Portal Admin Module
- ### [Graph API](./Topics/Graph-API.md/#graph)
    -  Older version of Microsoft Graph
    -   Will be deprecated soon but has some functionality Microsoft Graph hasn't implemented yet
    - Used to invite users to the Client Portal

