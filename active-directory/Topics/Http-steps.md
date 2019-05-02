<a name="http"></a>

[back to Nested Groups](./Groups.md/#nested)

 ## HTTP steps for Nested Groups with Microsoft Graph:
### 1) AW creates a group with a client's name (VIHA)
    Endpoint: POST
        https://graph.microsoft.com/v1.0/groups

    Body:
        {
            "description": "group with desc",
            "displayName": "VIHA",
            "mailEnabled": false,
            "mailNickname": "default",
            "securityEnabled": true
        }


### 2) AW adds Admin as the owner
    Endpoint: POST
        https://graph.microsoft.com/v1.0/groups/{VIHAId}/owners/$ref

    Body:
        {
	        "@odata.id": "https://graph.microsoft.com/v1.0/users/{AdminId}"
        }

### 3) AW adds VIHA as a member of AW group

    Endpoint: POST
       https://graph.microsoft.com/v1.0/groups/{AWGroupId}/members/$ref

    Body:
        {
	        "@odata.id": "https://graph.microsoft.com/v1.0/directoryObjects/{VIHAId}"
        }

### 4) Admin creates a project (Surgical)

    Endpoint: POST
        https://graph.microsoft.com/v1.0/groups

    Body:
        {
            "description": "project desc here",
            "displayName": "Surgical",
            "mailEnabled": false,
            "mailNickname": "default",
            "securityEnabled": true
        }

### 5) Admin adds himself as an owner of the project (Surgical)

    Endpoint: POST
        https://graph.microsoft.com/v1.0/groups/{SurgicalId}/owners/$ref

    Body:
        {
            "@odata.id":"https://graph.microsoft.com/v1.0/users/{adminId}"
        }

### 6) Admin adds project (Surgical) to client (VIHA)

    Endpoint: POST
        https://graph.microsoft.com/v1.0/groups/{VIHAId}/members/$ref

    Body:
        {
            "@odata.id": "https://graph.microsoft.com/v1.0/directoryObjects/{SurgicalId}"
        }
---

[back to Nested Groups](./Groups.md/#nested)
