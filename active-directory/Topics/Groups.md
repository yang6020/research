<a name="groups"></a>

[back to Active Directory](../README.md/#ad)

# **Groups**
For the Client Portal, all projects are groups. The group type used is Security. Its purpose is for controlling user access to resources

### About Groups

With [Microsoft Graph](./Microsoft-Graph.md/#microsoftgroups) / [Graph API](./Graph-API.md/#graph), you can perform CRUD operations on groups depending on your [permissions](./Permissions.md/#permissions).

### Users in Groups
1) Group Owners
   - Group Owners are able to manage users inside the groups they own or in this case, Projects.
    - This includes being able to add and remove members.
    -  By default, the one who creates the project is an owner.
2) Group Members
    - Group Members will be able to see the group but won't have sufficient privileges to add or remove users. (not to be confused with userType [Member](./Users.md/#users))

### See Group Permissions [here](./Permissions.md/#groupPermissions)

<a name="nested"></a>

# **Nested Groups**

Diagram:

     [1]  Analysis Works Group
                /    \
     [2]      NHL    VIHA
                     /    \
     [3]         Beds   Surgical
                            \
     [4]                other projects

| Level   |      Owner      |  Members |
|----------|:-------------:|------:|
| [1] | AW | AW |
| [2] | VIHA admin   |   Project admins like VIHA Surgical admin|
| [3] | VIHA admin, VIHA Surgical admin |    VIHA Surgical admin, VIHA employees |
| [4] | VIHA admin, VIHA Surgical admin etc... |    VIHA employees |

### Project Flow

 - adding owner together with generating the group may also cause some permission errors

---
Legend:
- Client Group - VIHA
- Client Admin - Admin

Steps by level in the diagram

        1) AW creates a group with a client's name (VIHA)

        2) AW adds Admin as the owner

        3) AW adds VIHA as a member of AW group

        4) Admin creates a project (Surgical)

        5) Admin adds project (Surgical) to client (VIHA)

[HTTP Guide](./Http-steps.md/#http) for Nested Groups By Step for [Microsoft Graph](./Microsoft-Graph.md/#microsoftgraph)

---

[back to Active Directory](../README.md/#ad)

