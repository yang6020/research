
# Schema extensions
Custom fields to directory objects like users, groups, organizations etc.

## Prerequisites
- Permission required to access schemaExtension
    - Directory.AccessAsUser.All
- Access token for Microsoft Graph or AD Graph
    - Schema extensions for AD Graph [here](https://www.idampundit.com/2017/05/extending-azure-ad-schema-via-graph-api-n00b-guide/)

see [roles and permissions](../roles-permissions/) for more information.

## Schema extensions are...
- AD specific -> attaching an attribute to a User won't be carried over to other ADs.
- Aren't visible in the Portal.
- Are selected and edited using APIs or Powershell.
- Are tagged to the object
    - Ex. Tagging a group won't tag users inside but the group itself

## Steps with examples - add attribute "md" with field "code" for a USER object
### 1) Declare a schemaExtension (custom attribute)
```
POST https://graph.microsoft.com/v1.0/schemaExtensions/
```

| Headers       |                  |
| ------------- | :--------------: |
| Authorization |   Bearer $token  |
| Content-Type  | application/json |

**Body**
```
{
    "id":"md",
    "description": "MD Code",
    "targetTypes": [
        "User"
    ],
    "properties": [
        {
            "name": "code",
            "type": "String"
        }
    ]
}
```

**Response**
```
{
    "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#schemaExtensions/$entity",
    "id": "ext{8randomcharactershere}_md",
    "description": "MD Code",
    "targetTypes": [
        "User"
    ],
    "status": "InDevelopment",
    "owner": $applicationId,
    "properties": [
        {
            "name": "code",
            "type": "String"
        }
    ]
}
```

### 2) Assign the custom attribute
```
PATCH https://graph.microsoft.com/v1.0/users/$userId
```
Headers - same as above

**Body**

example, we want to assign code as "123123"
```
{
    "ext{8randomcharactershere}_md":{
	    "code": "123123"
    }
}
```
Response

204 No Content

-----

## Getting a user with custom attributes
- Getting the user normally will only return default values. For custom attributes, they need to be "selected"

Headers - Authorization only

```
GET https://graph.microsoft.com/v1.0/users/$userId?$select= businessPhones,displayName,
givenName,mail,mobilePhone,surname,userPrincipalName,id,ext{8randomcharactershere}_md
```

Response
```
{
    "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users(businessPhones,displayName,givenName,mail,mobilePhone,surname,userPrincipalName,id,ext{8randomcharactershere}_md)/$entity",
    "businessPhones": [
        "+1 4444444444"
    ],
    "displayName": "Justin Yang",
    "givenName": "Justin",
    "mail": "jyang@analysisworks.com",
    "mobilePhone": "+1 3333333333",
    "surname": "Yang",
    "userPrincipalName": $userName,
    "id": $userId,
    "ext{8randomcharactershere}_md": {
        "@odata.type": "#microsoft.graph.ComplexExtensionValue",
        "code": "123123"
    }
}
```



