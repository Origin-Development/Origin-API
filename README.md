# Origin Development API endpoints

### BASE DETAILS

I need the API to work off of endpoint sections
The way that works is like users have their own section. Each section is given a code for error, eg, for users it 150.
the error codes work like this.
```yaml
150233
^
This is the major API version

150233
 ^ This is the API section

150233
  ^ This is subsections of that section like /users/token

150233
   ^^^ starts at 230 always then counts up per one per error
```
Each designated section should have a default environment variable for default permissions. The API should first check from a table called `api_sections` where the table looks like this.<br>
| id | section_id | section_name | section_default_permission |
|----|------------|--------------|----------------------------|
| 1  | 150        | users        | 942734                     |
| 2  | 160        | staff        | 284829                     |
| 3  | 170        | customers    | 372848                     |

<br>If the section does not exist then it should read from the config file, up to yall how the config file should look.
The authenication should take place using a key system. The user will placed in the headers under authorization.
If the bearer token can not be authorized then it should always return an not authorized error code for that endpoint section, eg, not enough perms for users should be 150235

## User Endpoints

## Error Codes

#### User Not Found - 150231

**Code**: 150231<br>
**Message**: The requested user was not found.<br>
**Details**: User with ID `{id}` was not found.<br>

#### Invalid User ID Format - 150232

**Code**: 150232<br>
**Message**: The provided user ID has an invalid format.<br>
**Details**: The user ID should be in a valid format.<br>

#### Username Already Taken - 150233

**Code**: 150233<br>
**Message**: The requested username is already taken.<br>
**Details**: The username `{username}` is not available.<br>

#### Missing Required Fields - 150234

**Code**: 150234<br>
**Message**: Some required fields are missing in the request.<br>
**Details**: The fields `{key} | {id}` are required for this operation.<br>
**Comments**: The details is an example so if a user fetch endpoint is missing the field id, then it would be id, etc

#### Invalid Permission Level - 150235

**Code**: 150235<br>
**Message**: The provided permission level is invalid.<br>
**Details**: The permission level should be within a valid range.<br>
**Comments**: If you understand the bitwise code I sent correctly you can Identify which permission they are missing

#### Failed to Create User - 150236

**Code**: 150236<br>
**Message**: Failed to create a new user.<br>
**Details**: An error occurred while trying to create the user.<br>

#### Failed to Update User - 150237

**Code**: 150237<br>
**Message**: Failed to update user information.<br>
**Details**: An error occurred while trying to update the user.<br>

#### Failed to Delete User - 150238

**Code**: 150238<br>
**Message**: Failed to delete the user.<br>
**Details**: An error occurred while trying to delete the user.<br>

#### Invalid Date Format - 150239

**Code**: 150239<br>
**Message**: The provided date has an invalid format.<br>
**Details**: The date should be in a valid format.<br>

#### Invalid Discord ID Format - 150240

**Code**: 150240<br>
**Message**: The provided Discord ID has an invalid format.<br>
**Details**: The Discord ID should be in a valid format.<br>

#### Failed to Authenticate User - 150241

**Code**: 150241<br>
**Message**: Failed to authenticate the user.<br>
**Details**: An error occurred during user authentication.<br>

#### Unauthorized Access - 150242

**Code**: 150242<br>
**Message**: You do not have permission to access this resource.<br>
**Details**: You do not have the necessary permissions to access this endpoint.<br>

#### User Creation Limit Exceeded - 150243

**Code**: 150243<br>
**Message**: The limit for creating new users has been exceeded for this time frame.<br>
**Details**: The maximum number of users that can be created has been reached for this time frame.<br>

### Create User

**Method**: POST<br>
**Endpoint**: /users/create<br>
**Description**: Create a new user.<br>
**Successful Request Body**: JSON containing user information.<br>
```json
{
    "id": 1,
    "user_id": "8374936482648364832",
    "user_uuid": "f5a35dda-fd87-4004-b13b-c24112d06bbb",
    "discord_id": "1059225948618240080",
    "username": "origincookie",
    "permissions": "957385724732",
    "created_date": 1685223356,
    "updated_date": 1693154156
}
```

**Unsuccessful Request Body**: JSON Containing an 150236 error message

```json
{
    "error": {
        "code": 150236,
        "message": "Failed to create a new user.",
        "details": "An error occurred while trying to create the user."
    }
}
```

**Body to Send**: JSON Containing username, discord_id, permissions. Everything else will be generated server-sided

```json
{
    "discord_id": "1059225948618240080",
    "username": "origincookie",
    "permissions": "957385724732",
}
```

### Get All Users

**Method**: GET<br>
**Endpoint**: /users/all<br>
**Description**: Retrieve a list of all users.<br>
**Request Body**: JSON Containing a list of users.<br>

```json
{
    "users": [
        {
            "id": 1,
            "user_id": "8374936482648364832",
            "user_uuid": "f5a35dda-fd87-4004-b13b-c24112d06bbb",
            "discord_id": "1059225948618240080",
            "username": "origincookie",
            "permissions": "957385724732",
            "created_date": 1685223356,
            "updated_date": 1693154156
        },
        {
            "id": 2,
            "user_id": "3284264826",
            "user_uuid": "bb19a04c-dab8-45ac-abb8-ab5605a62a8f",
            "discord_id": "1030288893259550741",
            "username": "chis",
            "permissions": "957385724732",
            "created_date": 1687901756,
            "updated_date": 1693154156
        }
    ]
}
```

### Get User by ID

**Method**: GET<br>
**Endpoint**: /users/fetch<br>
**Query Parameters**: id => user_id<br>
**Description**: Retrieve user details by their ID.<br>
**Successful Request Body**: JSON Containing the user who got its ID<br>

```json
{
    "id": 1,
    "user_id": "8374936482648364832",
    "user_uuid": "f5a35dda-fd87-4004-b13b-c24112d06bbb",
    "discord_id": "1059225948618240080",
    "username": "origincookie",
    "permissions": "957385724732",
    "created_date": 1685223356,
    "updated_date": 1693154156
}
```

**Unsuccessful Request Body**: JSON Containing an error message

```json
{
    "error": {
        "code": 150231,
        "message": "The requested user was not found.",
        "details": "User with ID {id} was not found."
    }
}
```

### Update User by ID

**Method**: PUT<br>
**Endpoint**: /users/update<br>
**Description**: Update user details by their ID.<br>
**Query Parameters**: id => user_id<br>
**Successful Request Body**: JSON containing user with their updated information.<br>

```json
{
    "id": 1,
    "user_id": "8374936482648364832",
    "user_uuid": "f5a35dda-fd87-4004-b13b-c24112d06bbb",
    "discord_id": "1059225948618240080",
    "username": "origincookie_updated",
    "permissions": "957385724732",
    "created_date": 1685223356,
    "updated_date": 1693154756
}
```

**Unsuccessful Return Request Body**: JSON containing error 150237

```json
{
    "error": {
        "code": 150237,
        "message": "Failed to update user information.",
        "details": "An error occurred while trying to update the user."
    }
}
```

**Body to send**: JSON of updated information, lets say I want to update the ID then i would just put the id in the body. If not specified in body then the information should stay the same

```json
{
    "id": 3,
    "username": "origincookie_updated"
}
```

### Delete User by ID

**Method**: DELETE<br>
**Endpoint**: /users/delete<br>
**Description**: Delete a user by their ID.<br>
**Query Parameters**: id => user_id<br>
**Successful Return Request Body**: JSON Containing success message<br>

```json
{
    "success": true,
    "message": "User deleted successfully",
    "details": "The user {userid} was deleted successfully"
}
```

**Unsuccessful Return Request Body**: JSON containing error 150238

```json
{
    "error": {
        "code": 150238,
        "message": "Failed to delete the user.",
        "details": "An error occurred while trying to delete the user."
    }
}
```

### Get User by UUID

**Method**: GET<br>
**Endpoint**: /users/fetch<br>
**Query Parameters**: uuid => user_uuid<br>
**Description**: Retrieve user details by their UUID.<br>
**Successful Request Body**: JSON Containing user information

```json
{
    "id": 1,
    "user_id": "8374936482648364832",
    "user_uuid": "f5a35dda-fd87-4004-b13b-c24112d06bbb",
    "discord_id": "1059225948618240080",
    "username": "origincookie",
    "permissions": "957385724732",
    "created_date": 1685223356,
    "updated_date": 1693154156
}
```

### Get User by Discord ID

**Method**: GET<br>
**Endpoint**: /users/fetch<br>
**Query Parameters**: discordid => user_discord_id<br>
**Description**: Retrieve user details by their Discord ID.<br>
**Successful Request Body**: JSON containing user information<br>

```json
{
    "id": 1,
    "user_id": "8374936482648364832",
    "user_uuid": "f5a35dda-fd87-4004-b13b-c24112d06bbb",
    "discord_id": "1059225948618240080",
    "username": "origincookie",
    "permissions": "957385724732",
    "created_date": 1685223356,
    "updated_date": 1693154156
}
```

### Get Users with Permissions

**Method**: GET<br>
**Endpoint**: `/users/permissions/{permission}`<br>
**Description**: Retrieve a list of users with specific permissions.<br>
**Request Body**: JSON Containing list of user information who have that specific permission<br>

```json
{
    "users": [
        {
            "id": 5,
            "user_id": "9384275825",
            "user_uuid": "a7f6b409-7d04-4c31-92e1-f4f6d123eac5",
            "discord_id": "2910485739874388",
            "username": "poweruser",
            "permissions": "957385724732",
            "created_date": 1678925111,
            "updated_date": 1693154156
        },
        {
            "id": 12,
            "user_id": "5839204985",
            "user_uuid": "c8b2c1d9-05b7-4ac0-9c4f-8f6e126cb5c7",
            "discord_id": "8723456728319875",
            "username": "accessgranted",
            "permissions": "957385724732",
            "created_date": 1679432190,
            "updated_date": 1693154156
        }
    ]
}
```

### Get Users Created on Date

**Method**: GET<br>
**Endpoint**: `/users/created/{date}`<br>
**Description**: Retrieve users created on a specific date.<br>
**Request Body**: JSON containing list of users who were created on the date specified<br>

```json
{
    "users": [
        {
            "id": 8,
            "user_id": "3982572485",
            "user_uuid": "b987b395-789e-4b65-bc9f-1b6c243d8a0c",
            "discord_id": "4528190845678910",
            "username": "newuser1",
            "permissions": "957385724732",
            "created_date": 1692155555,
            "updated_date": 1693154156
        }
    ]
}
```
### Get Users Updated on Date

**Method**: GET<br>
**Endpoint**: `/users/updated/{date}`<br>
**Description**: Retrieve users updated on a specific date.<br>
**Request Body**: JSON containing list of users who were updated on the specified date<br>

```json
{
    "users": [
        {
            "id": 3,
            "user_id": "9827458632",
            "user_uuid": "ac0e698d-98ad-412d-b703-eb1d8764a8a3",
            "discord_id": "6248190481023467",
            "username": "updateduser",
            "permissions": "957385724732",
            "created_date": 1679422200,
            "updated_date": 1692155555
        }
    ]
}
```

### Get Users Created Between Dates

**Method**: GET<br>
**Endpoint**: `/users/created/{start_date}/{end_date}`<br>
**Description**: Retrieve users created between a date range.<br>
**Request Body**: JSON containing a list of users created between the specified date range

```json
{
    "users": [
        {
            "id": 15,
            "user_id": "9827465821",
            "user_uuid": "f98e1d9c-1927-4bca-a202-cf0e561ac025",
            "discord_id": "7210836491827264",
            "username": "daterangeuser1",
            "permissions": "957385724732",
            "created_date": 1692592956,
            "updated_date": 1693154156
        },
        {
            "id": 17,
            "user_id": "7832598462",
            "user_uuid": "d6c29524-3df1-45e1-b1b0-9e73e9c8b87b",
            "discord_id": "8951728374612910",
            "username": "daterangeuser2",
            "permissions": "957385724732",
            "created_date": 1692891983,
            "updated_date": 1693154156
        }
    ]
}
```

### Get Users Updated Between Dates

**Method**: GET<br>
**Endpoint**: `/users/updated/{start_date}/{end_date}`<br>
**Description**: Retrieve users updated between a date range.<br>
**Request Body**: JSON containing a list of users who were updated between the specified date range<br>

```json
{
    "users": [
        {
            "id": 4,
            "user_id": "7629841058",
            "user_uuid": "f48b9c8e-62b4-4d4c-b614-8e1e7a9cf6bf",
            "discord_id": "3297465821019485",
            "username": "daterangeupdate1",
            "permissions": "957385724732",
            "created_date": 1679527834,
            "updated_date": 1692682999
        },
        {
            "id": 16,
            "user_id": "9038402754",
            "user_uuid": "b345a63c-1871-4db7-8eeb-5f6dc71a1e2a",
            "discord_id": "7654890123569283",
            "username": "daterangeupdate2",
            "permissions": "957385724732",
            "created_date": 1692641827,
            "updated_date": 1692967745
        }
    ]
}
```
