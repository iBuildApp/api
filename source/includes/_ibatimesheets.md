# iBuildApp Timesheets
## <ins>**INTRO**</ins>

Basic API for registering/authorizing a user, getting dashboard data and user history, starting/stopping time tracking, and adding new time sheet entries.

## User Authorization

> Request body (application/json):

```json
{
	"email": "test@test.com",
	"password": "12345678"
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully authorized.",
   "token": "$2y$10$UP8aZvonwuqnaSBrdpiB4eq5NI3VHZtzcFhC73o0Slf/6uSBU3.je"
}
```

### Request data: JSON string with the following elements

| Parameter | Type            | Description    |
|-----------|-----------------|----------------|
| email     | String (*Required*) | Login (e-mail) |
| password  | String (*Required*) | User password  |

### Response data: JSON string with the following elements

| Parameter | Type                          | Description                                                             |
|-----------|-------------------------------|-------------------------------------------------------------------------|
| status    | String                        | SUCCESS|ERROR                                                           |
| message   | String                        | Error description or successful info message                            |
| token     | String                        | Auth token of user session. Will be used to sign every next request.    |
| status    | String from list: ACTIVE, PENDING | User status active / pending activation (Unable to login and use the system) |

## User Registration

> Request body (application/json):

```json
{
	"email": "test@test.com",
	"password": "12345678",
	"first_name": "John",
	"last_name": "Doe"
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully registered.",
   "token": "$2y$10$qhqCrGv2f6vxxnyy4PCWqe9OAmjjqNRDkfB4F8J6qV2mXq/wpQXni"
}
```

### Request data: JSON string with the following elements

| Parameter  | Type           | Description   |
|------------|----------------|---------------|
| email      | String (*Required*) | Login (email) |
| password   | String (*Required*) | Password      |
| first_name | String         | First name    |
| last_name  | String         | Last name     |

### Response data: JSON string with the following elements

| Parameter | Type                          | Description                                                             |
|-----------|-------------------------------|-------------------------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR  | Operation result successful / execution error                               |
| message   | String                        | Error description or successful info message                            |
| token     | String                        | Auth token of user session. Will be used to sign every next request.    |
| status    | String from list: ACTIVE, PENDING | User status active / pending activation (Unable to login and use the system) |

## <ins>**USER**</ins>

User API for iBuildApp Timesheets, handling getting user information, creating/deleting/updating a user, and searching for user(s)

## Get User Info

> Request body (application/json):

```json
{
	"user_id": 1
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "data": {
       "id": 1,
       "email": "test@test.com",
       "first_name": "John",
       "last_name": "Doe",
       "sex": "male",
        "position": "Manager",
        "hourly_rate": 5.5,
        "roles": [
          "ROLE_USER"
        ],
        "created": 1583905200,
        "updated": 1583905200,
        "exported": 1583905200,
        "activated": true,
        "settings": [
          {}
        ],
        "picture": "url_to_user_avatar",
        "paid_hours": 0,
        "unapproved_hours": 0,
        "teams": [
          0
        ],
        "notes": "description",
        "birth_date": "1988-11-22"
   }
}
```

> 401 Unauthorized. Response body (application/json):
```json
{
     "status": "ERROR",
     "message": "Auth token not specified or incorrect.",
     "error_code": 9
}
```

> 404 User not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "User not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/user`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description                                           |
|-----------|---------|-------------------------------------------------------|
| user_id   | Integer | User ID. If not specified, current user’s id is used. |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields                  |

## Create New User

> Request body (application/json):

```json
{
    "email": "test2@test.com",
    "password": "string",
     "first_name": "John",
     "last_name": "Doe",
     "sex": "male",
     "position": "Manager",
     "hourly_rate": 10.5,
     "settings": [
        {}
      ],
     "notes": "string",
     "birth_date": "1988-11-22"
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully created.",
   "id": 2
}
```

> 401 Response body (application/json):

```json
{
   "status": "ERROR",
    "message": "Auth token not specified or incorrect.",
    "error_code": 9
}
```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type           | Description |
|------------|----------------|-------------|
| email      | String (*Required*) |             |
| password   | String         |             |
| first_name | String         |             |
| last_name  | String         |             |
| sex        | String         | male,female |
| hourly_rate | Float         |             |
| settings   | Array          |             |
| notes      | String         |             |
| birth_date | String         |  YYYY-MM-DD |
| position   | String         |             |
| is_active  | Boolean        |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                          | ID of the new user                           |

## Update User

> Request body (application/json):

```json
{
	"user_id": 2,
	"first_name": "Jack"
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully updated."
}
```

> 401 Unauthorized. Response body (application/json):

```json
{
   "status": "ERROR",
   "message": "Auth token not specified or incorrect.",
   "error_code": 9
}
```

> 404 User not found. Response body (application/json):

```json
{
   "status": "ERROR",
    "message": "User not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/user/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type    | Description                                           |
|------------|---------|-------------------------------------------------------|
| user_id    | Integer | User ID. If not specified, current user’s id is used. |
| email      | String  |                                                       |
| password   | String  | 8< password <15                                       |
| first_name | String  |                                                       |
| last_name  | String  |                                                       |
| sex        | String  |   male, female                                        |
| birth_date | String  |   YYYY-MM-DD                                          |
| position   | String  |                                                       |
| settings   | Array   |                                                       |
| is_active  | Boolean |                                                       |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Users

> Request body (application/json):

```json
{
	"email": "test"
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "2 user(s) found.",
   "data": [
       {
           "id": 1,
           "email": "test@test.com",
           "first_name": "John",
           "last_name": "Doe"
       },
       {
           "id": 2,
           "email": "test2@test.com",
           "first_name": "Jack",
           "last_name": "Smith"
       }
   ],
  "count": 2
}
```

> 401 Unauthorized. Response body (application/json):

```json
{
   "status": "ERROR",
   "message": "Auth token not specified or incorrect.",
   "error_code": 9
}
```

### HTTP Request

`POST /api/{APP_CODE}/user/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type    | Description |
|------------|---------|-------------|
| user_id    | Integer | User ID     |
| email      | String  |             |
| first_name | String  |             |
| last_name  | String  |             |
| position   | String  |             |
| is_active  | Boolean |             |
| limit      | Integer     |             |
| offset     | Integer     |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |
| count     | Integer | Count of Objects |

## Search Users with Grouping

> Request body (application/json):

```json
{
    "group_by": "is_active",
    "order": "asc"
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "1 group(s) found.",
  "data": {
    "groups": [
      {
        "value": true,
        "count": 1,
        "items": [
          {
            "id": 0,
            "email": "email@domain.com",
            "first_name": "John",
            "last_name": "Smith",
            "sex": "male",
            "position": "Manager",
            "hourly_rate": 5.1,
            "roles": [
              "ROLE_USER"
            ],
            "created": 1583905200,
            "updated": 1583905200,
            "exported": 1583905200,
            "activated": true,
            "settings": [
              {}
            ],
            "picture": "url_to_user_avatar",
            "paid_hours": 0,
            "unapproved_hours": 0,
            "teams": [
              0
            ],
            "notes": "notes",
            "birth_date": "1988-11-22"
          }
        ]
      }
    ]
  },
  "count": 1
}
```

> 401 Unauthorized. Response body (application/json):

```json
{
   "status": "ERROR",
   "message": "Auth token not specified or incorrect.",
   "error_code": 9
}
```

### HTTP Request

`POST /api/{APP_CODE}/user/search_group`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type    | Description |
|------------|---------|-------------|
| user_id    | Integer | User ID     |
| email      | String  |             |
| first_name | String  |             |
| last_name  | String  |             |
| position   | String  |             |
| is_active  | Boolean |             |
| group_by   | String |   field for grouping          |
| order      | String |  (asc, desc) -  sorting order for grouped field          |
| limit      | Integer     |             |
| offset     | Integer     |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |
| count     | Integer | Count of Objects |

## Delete User

> Request body (application/json):

```json
{
	"user_id": 2
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully deleted."
}
```

> 401 Unauthorized. Response body (application/json):

```json
{
   "status": "ERROR",
   "message": "Auth token not specified or incorrect.",
   "error_code": 9
}
```

> 404 User not found. Response body (application/json):

```json
{
   "status": "ERROR",
    "message": "User not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/user/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description                                           |
|-----------|---------|-------------------------------------------------------|
| user_id   | Integer | User ID. If not specified, current user’s id is used. |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Get User Dashboard Data

> Request body (application/json):

```json
{
	"user_id":1,
	"last_update":1565768191
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "1 task(s) found.",
   "data": {
       "current_tasks": [
           {
               "id": 1,
               "name": "testTask",
               "status": "in-progress",
               "project": 1,   
               "duration": 10,
               "users": [
                 0
               ]
           }
       ],
       "current_time_entry": {
           "id": 3,
           "user_id": 1,
           "task_id": 1,
           "schedule_id": 1,
           "status": "in-progress",
           "start_time": 1565687043,
            "payment_status": "pending",
            "breaks": 2,
            "comments": "",
            "duration": 10
       }
   },
   "timestamp": 1565954217
}
```

> 401 Unauthorized. Response body (application/json):

```json
{
   "status": "ERROR",
   "message": "Auth token not specified or incorrect.",
   "error_code": 9
}
```

> 404 User not found. Response body (application/json):

```json
{
   "status": "ERROR",
    "message": "User not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/user/dashboard`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter   | Type      | Description                                            |
|-------------|-----------|--------------------------------------------------------|
| user_id     | Integer   | User ID. If not specified, current user’s id is used.  |
| last_update | Datetime  | Timestamp of last update data in the time_entry entity |

### Response data: JSON string with the following elements

| Parameter | Type                                                          | Description                                                                                        |
|-----------|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR, NOT-MODIFIED                | Operation result successful / execution error / data hasn’t been modified on the server from last request |
| message   | String                                                        | Error description or successful info message                                                       |
| data      | JSON String with array of time_entry records for current user | Data for showing in the table part                                                                 |
| timestamp | Integer                                                       | Current server Unix timestamp                                                                      |             |

## Get User History

> Request body (application/json):

```json
{
	"user_id":1,
	"last_update":1565768191,
	"with": ["users", "tasks", "projects"],
	"filters": {"task_id": {"value": 2, "operand": "<"}, "status":"done"},
	"sorts": {"task_id": "desc"}
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "1 time entries found.",
   "data": {
       "time_entries": [
           {
               "id": 3,
               "user_id": 1,
               "task_id": 1,
               "status": "done",
               "start_time": "2019-08-13 12:04:03"
           }
       ],
       "tasks": {
           "1": {
               "id": 1,
               "project": 2,
               "name": "Test task 1",
               "status": "in-progress"
           }
       },
        "users": {
           "1": {
               "id": 1,
               "first_name": "John",
               "last_name": "Smith",
               "picture": "url_to_user_avatar"
           }
               },
       "projects": {
           "2": {
               "id": 2,
               "name": "Test project 1",
               "status": "in-progress"
           }
       }
   },
  "count": 1,
  "timestamp": 1565954217
}
```

> 401 Unauthorized. Response body (application/json):

```json
{
   "status": "ERROR",
   "message": "Auth token not specified or incorrect.",
   "error_code": 9
}
```

> 404 User not found. Response body (application/json):

```json
{
   "status": "ERROR",
    "message": "User not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter   | Type                                          | Description                                                                          |
|-------------|-----------------------------------------------|--------------------------------------------------------------------------------------|
| user_id     | Integer                                       | User ID. If not specified, current user’s id is used.                                |
| last_update | Integer                                       | Timestamp of the last data update.                                                   |
| with        | Array from possible values: users, tasks, projects | The list of linked entities that should be returned from the server in one response. |
| filters     | Object                                        | The list of filters - fields, values and operands.                                   |
| sorts       | Object                                        | The list of sorts - fields and orders (asc, desc).                                   |

### Response data: JSON string with the following elements

| Parameter | Type                                     | Description                                                                                        |
|-----------|------------------------------------------|----------------------------------------------------------------------------------------------------|
| status    | String from list: SUCCESS: ERROR, NOT-MODIFIED | Operation result successful / execution error / data hasn’t been modified on the server from last request |
| message   | String                                   | Error description or successful info message                                                       |
| data      | Array packed into JSON string            | JSON string with array of time_entry and other records for current user                            |
| timestamp | Int                                      | Current server Unix timestamp                                                                      |

## <ins>**GROUP**</ins>

Group/Team API for iBuildApp Timesheets, handling getting group/team information, creating/deleting/updating a group/team, getting user(s) in a group/team, adding/deleting user(s) from group/team, and searching for group/team(s)


## Get Group Info

> Request body (application/json):

```json
{
	"group_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully found.",
   "data": {
       "id": 1,
       "app_id": 1,
       "name": "nameTest"
   }
}
```

### HTTP Request

`POST /api/{app_path}/group`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| group_id  | Integer (*Required*) | Group ID    |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | Group object with all fields or empty object |

## Create New Group

> Request body (application/json):

```json
{
	"name":"nameTest",
	"app_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully created.",
   "id": 1
}
```

### HTTP Request

`POST /api/{app_path}/group/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| name      | String (*Required*)  |             |
| app_id    | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                      | ID created group                             |

## Update Group

> Request body (application/json):

```json
{
	"group_id": 1,
  "name": ”New_name”,
  "description": ”New_description”
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully updated."
}
```

### HTTP Request

`POST /api/{app_path}/group/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter   | Type            | Description     |
|-------------|-----------------|-----------------|
| group_id    | Integer (*Required*) | Group ID        |
| name        | String (*Required*)  | new name        |
| description | String          | new description |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Groups

> Request body (application/json):

```json
{
	"group_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "1 group(s) found.",
   "data": [
       {
           "id": 1,
           "app_id": 1,
           "name": "nameTest"
       }
   ]
}
```

### HTTP Request

`POST /api/{app_path}/group/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description |
|-----------|---------|-------------|
| group_id  | Integer | Group ID    |
| name      | String  |             |
| limit     | Integer     |             |
| offset    | Integer     |             |

### Response data: JSON string with the following elements

| Parameter | Type                                         | Description                                        |
|-----------|----------------------------------------------|----------------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                       | Error description or successful info message       |
| data      | Array of group objects packed to JSON string | Group objects array with all fields or empty array |

## Add User to Group

> Request body (application/json):

```json
{
	"group_id":1,
	"user_id":2,
  "role":”ROLE_USER”
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully added."
}
```

### HTTP Request

`POST /api/{app_path}/group/add_user`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description               |
|-----------|-----------------|---------------------------|
| group_id  | Integer (*Required*) |                           |
| user_id   | Integer (*Required*) |                           |
| role      | user role       | Default = “ROLE_EXECUTOR” |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                      | ID created group                             |

## Delete User from Group

> Request body (application/json):

```json
{
	"group_id":1,
	"user_id":2
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully deleted."
}
```

### HTTP Request

`POST /api/{app_path}/group/delete_user`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| group_id  | Integer (*Required*) |             |
| user_id   | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                      | ID created group                             |

## Get Users in Group

> Request body (application/json):

```json
{
	"group_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "1 user(s) found.",
   "data": [
       {
           "id": 2,
           "ts_user": 1,
           "role": "ROLE_EXECUTOR"
       }
   ]
}
```

### HTTP Request

`POST /api/{app_path}/group/get_users`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type     | Description |
|-----------|----------|-------------|
| group_id  | Integer  (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Array of User                |                                              |

## Delete Group

> Request body (application/json):

```json
{
	"group_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully deleted."
}
```

### HTTP Request

`POST /api/{app_path}/group/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type                   | Description |
|-----------|------------------------|-------------|
| group_id  | Integer | GUIDRequired | Group ID    |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## <ins>**CLIENT**</ins>

Client/Customer API for iBuildApp Timesheets, handling getting client/customer information, creating/deleting/updating a client/customer, and searching for client/customer(s)

## Get Client Info

> Request body (application/json):

```json
{
	"client_id":1
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "data": {
    "id": 0,
    "name": "Client Name",
    "email": "email@client.com",
    "contact_info": "contact_info...",
    "address": "address...",
    "state": "State",
    "website": "site...",
    "notes": "Descriptions...",
    "status": "active"
  }
}
```

> 404 Not found. Response body (application/json):
```json
{
   "status": "ERROR",
    "message": "Client not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/client`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type                   | Description |
|-----------|------------------------|-------------|
| client_id | Integer (*Required*) | Client ID   |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                   |
|-----------|------------------------------|-----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message  |
| data      | Object packed to JSON string | Client object with all fields or empty object |

## Create New Client

> Request body (application/json):

```json
{
    "name": "John Smith",
    "phone": 12345678912,
    "email": "mail@mail.com",
    "contact_info": "contact_info",
    "address": "test city, test street, test house",
    "city": "New-York",
    "state": "NY",
    "industry": "Finance",
    "website": "website.com",
    "notes": "Descriptions...",
    "status": true
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully created.",
   "id": 1
}
```

### HTTP Request

`POST /api/{APP_CODE}/client/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter    | Type           | Description |
|--------------|----------------|-------------|
| name         | String (*Required*) |             |
| phone        | String         |             |
| email        | String         |             |
| contact_info | String         |             |
| address      | String         |             |
| city         | String         |             |
| state        | String         |             |
| industry     | String         |             |
| website      | String         |             |
| notes        | String         |             |
| is_active    | Boolean        |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                          | New client id                                |

## Update Client

> Request body (application/json):

```json
{
  "client_id": 1,
  "name": "John Doe"
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully updated."
}
```

> 404 Not found. Response body (application/json):
```json
{
   "status": "ERROR",
    "message": "Client not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/client/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter    | Type            | Description |
|--------------|-----------------|-------------|
| client_id    | Integer (*Required*) | Client ID   |
| name         | String   |             |
| phone        | String         |             |
| email        | String         |             |
| contact_info | String         |             |
| address      | String         |             |
| city         | String         |             |
| state        | String         |             |
| industry     | String         |             |
| website      | String         |             |
| notes        | String         |             |
| status     | Boolean        |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Clients

> Request body (application/json):

```json
{
	"name": "John"
}
```

> Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "1 client(s) found.",
  "data": [
    {
        "id": 0,
        "name": "John Smith",
        "phone": 12345678912,
        "email": "mail@mail.com",
        "contact_info": "contact_info",
        "address": "test city, test street, test house",
        "city": "New-York",
        "state": "NY",
        "industry": "Finance",
        "website": "website.com",
        "notes": "Descriptions...",
        "status": "active"
    }
  ],
  "count": 1
}
```

### HTTP Request

`POST /api/{APP_CODE}/client/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description |
|-----------|---------|-------------|
| user_id | Integer | User ID   |
| client_id | Integer | Client ID   |
| email     | String  |             |
| name      | String  |             |
| contact_info      | String  |             |
| address   | String  |             |
| keyword  | String                               |                     |
| limit     | Integer                               |                     |
| offset    | Integer                               |                     |

### Response data: JSON string with the following elements

| Parameter | Type                                         | Description                                         |
|-----------|----------------------------------------------|-----------------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                       | Error description or successful info message        |
| data      | Array of objects packed to JSON string       | Client objects array with all fields or empty array |
| count     | Integer                                      | Count of finded objects |

## Delete Client

> Request body (application/json):

```json
{
    "client_id": 1
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully deleted."
}
```

> 404 Not found. Response body (application/json):
```json
{
   "status": "ERROR",
    "message": "Client not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/client/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| client_id | Integer (*Required*) | Client ID   |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## <ins>**COMMENT**</ins>

Comment API for iBuildApp Timesheets, handling getting comment information, creating/deleting/updating a comments, and searching for comments.


## Get Comment Info

> Request body (application/json):

```json
{
	"comment_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully found.",
   "data": {
       "id": 1,
       "commentator": 1,
       "task": 1,
       "content": "text",
       "date": "2019-08-21T08:21:43+00:00"
   }
}
```

### HTTP Request

`POST /api/{app_path}/comment`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type    | Description |
|------------|---------|-------------|
| comment_id | Integer | Comment ID  |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |

## Create New Comment

> Request body (application/json):

```json
{
  "task_id":1,   // (or invoice_id, or time_entry_id)
  "content": "Example Text String"
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully created.",
   "id": 2
}
```

### HTTP Request

`POST /api/{app_path}/comment/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type            | Description                         |
|---------------|-----------------|-------------------------------------|
| task_id       | Integer (*Required*) | One of these options is required    |
| invoice_id    | Integer (*Required*) | One of these options is required    |
| time_entry_id | Integer (*Required*) | One of these options is required    |
| content       | String (*Required*)  |                                     |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                      | ID of created comment                        |

## Update Comment

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /timesheets/comment/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type            | Description |
|---------------|-----------------|-------------|
| token         | String (*Required*)  | Auth token  |
| comment_id    | Integer (*Required*) |             |
| task_id       | Integer (*Required*) |             |
| invoice_id    | Integer (*Required*) |             |
| time_entry_id | Integer (*Required*) |             |
| content       | String            |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Comments

> Request body (application/json):

```json
{
	"task_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "1 comment(s) found.",
   "data": [
       {
           "id": 1,
           "commentator": 1,
           "task": 1, // (or invoice, or time_entry)
           "content": "text",
           "date": "2019-08-20T08:02:07+00:00"
       }
   ]
}
```

### HTTP Request

`POST /api/{app_path}/comment/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter      | Type    | Description                |
|----------------|---------|----------------------------|
| commentator_id | Integer | User ID                    |
| task_id        | Integer |                            |
| invoice_id     | Integer |                            |
| time_entity_id | Integer |                            |
| content        | String  | if needed search with text |
| limit          | Integer     |                            |
| offset         | Integer     |                            |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |

## Delete Comment

> Request body (application/json):

```json
{
	"comment_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully deleted."
}
```

### HTTP Request

`POST /api/{app_path}/comment/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type    | Description |
|------------|---------|-------------|
| comment_id | Integer | Comment ID  |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## <ins>**INVOICE**</ins>

Invoice API for iBuildApp Timesheets, handling getting invoice information, creating/deleting/updating an invoice, changing invoice status, and searching for invoice(s).

## Get Invoice Info

> Request body (application/json):

```json
{
	"invoice_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "data": {
       "id": 1,
       "due_date": "2019-08-14T12:14:27+00:00",
       "payed_date": "2019-08-14T12:14:34+00:00",
       "amount": 30.5,
       "status": 6
   }
}
```

### HTTP Request

`POST /api/{app_path}/invoice`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type            | Description |
|------------|-----------------|-------------|
| invoice_id | Integer (*Required*) | Invoice ID  |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |

## Create New Invoice

> Request body (application/json):

```json
{
	"client_id":1,
	"task_ids":[1,2]
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully created.",
   "id": 2
}
```

### HTTP Request

`POST /api/{app_path}/invoice/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type                     | Description                                     |
|-----------|--------------------------|-------------------------------------------------|
| client_id | Integer (*Required*)          |                                                 |
| task_ids  | Array of Integer (*Required*) |                                                 |
| due_date  | Timestamp (*Required*) ??     | Default = current date / time                   |
| status    | Integer                  | Initial status. Default = 0 (STATUS_NEW)        |
| amount    | Float                    |                                                 |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Update Invoice

> Request body (application/json):

```json
{
	"invoice_id":1,
	"amount":30.5
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully updated."
}
```

### HTTP Request

`POST /api/{app_path}/invoice/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type            | Description |
|------------|-----------------|-------------|
| invoice_id | Integer (*Required*) |             |
| due_date   | Timestamp       |             |
| status     | Integer         |             |
| amount     | Float           |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Set Invoice Status (Paid)

> Request body (application/json):

```json
{
	"invoice_id":2,
	"payed_date":"2019-08-08 13:30:00"
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully payed."
}
```

### HTTP Request

`POST /api/{app_path}/invoice/set_payed`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type             | Description |
|------------|------------------|-------------|
| invoice_id | Integer (*Required*)  |             |
| payed_date | Datetime (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Invoices

> Request body (application/json):

```json
{
	"client_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "1 task(s) found.",
   "data": [
       {
           "id": 1,
           "due_date": "2019-08-14T12:14:27+00:00",
           "amount": 30.5,
           "status": 6
       }
   ]
}
```

### HTTP Request

`POST /api/{app_path}/invoice/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| client_id | Integer (*Required*) |             |
| limit     | Integer             |             |
| offset    | Integer             |             |

### Response data: JSON string with the following elements

| Parameter | Type                                   | Description                                  |
|-----------|----------------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                 | Error description or successful info message |
| data      | Array of objects packed to JSON string | User object with all fields or empty object  |

## Delete Invoice

> Request body (application/json):

```json
{
	"invoice_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully deleted."
}
```

### HTTP Request

`POST /api/{app_path}/invoice/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type     | Description |
|------------|----------|-------------|
| invoice_id | Integer (*Required*) |             |


### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## <ins>**TASK**</ins>

Task API for iBuildApp Timesheets, handling getting task information, creating/deleting/updating a task, setting task status, adding/deleting user(s) from a task, and searching for task(s).

## Get Task Info

> Request body (application/json):
```json
{
  "task_id": 1,
  "with": ["users","projects"]
}
```

> 200 Response body (application/json):
```json
{
  "status": "SUCCESS",
  "message": "string",
  "data": {
    "tasks": [
      {
        "id": 1,
        "name": "Task name",
        "project": 0,
        "duration": 100,
        "estimate": 100,
        "estimate_unit": "minutes",
        "estimate_hours": 3.2,
        "description": "description",
        "status": "new",
        "users": [0],
        "entries": [
          {
            "id": 0,
            "user_id": 0,
            "task_id": 0,
            "start_time": 1583905200,
            "end_time": 1583905200,
            "approve_date": 1583905200,
            "breaks": 0,
            "comments": 0,
            "duration": 0,
            "status": "in-progress",
            "payment_status": "pending",
            "start_location": [
              37.270248,
              -119.989356
            ],
            "end_location": [
              37.270248,
              -119.989356
            ]
          }
        ]
      }
    ],
    "users": {
      "0": {
        "id": 0,
        "email": "email@domain.com",
        "first_name": "John",
        "last_name": "Smith",
        "hourly_rate": 0,
        "picture": "url_to_user_avatar",
        "birth_date": "1988-11-22"
      }
    },
    "projects": {
      "0": {
        "id": 0,
        "name": "Project name",
        "description": "description...",
        "client_id": 0,
        "duration_unit": "minutes",
        "status": "new",
        "planned_start_date": 1583905200,
        "actual_start_date": 1583905200,
        "planned_end_date": 1583905200,
        "actual_end_date": 1583905200,
        "estimated_duration": 100,
        "actual_duration": 100,
        "tasks": [0]
      }
    }
  }
}
```

### HTTP Request

`POST /timesheets/task`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type            | Description |
|------------|-----------------|-------------|
| task_id | Integer (*Required*) | Task ID  |
| with      | Array[String] from list: users, projects |   

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| data      | Object packed to JSON string |  object with all fields or empty object  |

## Create New Task

> Request body (application/json):

```json
{
    "project_id": 1,
    "name": "Task name",
    "estimate": 30,
    "estimate_unit": "minutes",
    "description": "description..."
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully created.",
   "id": 2
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "Project not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /api/{app_path}/task/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type            | Description |
|---------------|-----------------|-------------|
| project_id    | Integer (*Required*) | Project ID  |
| name          | String (*Required*)  |             |
| description   | String    |             |
| estimate      | Float   |             |
| estimate_unit | String from the list:minutes, hours, days, weeks, months, years  |             |
| status        | String of list: new, in-progress, cancelled, done  |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                      | ID of created task                           |

## Update Task

> Request body (application/json):

```json
{
  "task_id": 1,
  "name": "New name",
  "estimate": 32,
  "estimate_unit": "hours"
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "Successfully updated."
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "Task not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /timesheets/task/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type            | Description |
|---------------|-----------------|-------------|
| task_id       | Integer (*Required*) |             |
| name          | String  |             |
| description   | String    |             |
| estimate      | Float  |             |
| estimate_unit | String from the list:minutes, hours, days, weeks, months, years  |             |
| status        | String of list: new, in-progress, cancelled, done  |  

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Tasks

> Request body (application/json):

```json
{
  "project_id": 0,
  "with": ["users","projects"]
}
```

> Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "1 task(s) found.",
  "data": {
    "tasks": [
      {
        "id": 0,
        "name": "Task name",
        "project": 0,
        "duration": 10,
        "status": "new",
        "users": [0],
        "entries": {
          "0": {
            "start_datetime": 1583905200,
            "end_datetime": 1583905200,
            "duration": 3600
          }
        }
      }
    ],
    "users": {
      "0": {
        "id": 0,
        "email": "email@mail.com",
        "first_name": "John",
        "last_name": "Smith",
        "hourly_rate": 10,
        "picture": "url_to_user_avatar",
        "birth_date": "1988-11-22",
        "teams": [0]
      }
    },
    "teams": {
      "0": {
        "id": 0,
        "name": "Group name",
        "description": "description...",
        "notes": "notes...",
        "status": "new",
        "pay_rate": 10.5
      }
    },
    "projects": {
      "0": {
        "id": 0,
        "name": "Project name",
        "description": "description...",
        "client_id": 0,
        "duration_unit": "minutes",
        "status": "new",
        "planned_start_date": 1583905200,
        "actual_start_date": 1583905200,
        "planned_end_date": 1583905200,
        "actual_end_date": 1583905200,
        "estimated_duration": 100,
        "actual_duration": 100,
        "tasks": [0]
      }
    },
    "count": 1
  }
}
```

### HTTP Request

`POST /api/{app_path}/task/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type    | Description                                                  |
|------------|---------|--------------------------------------------------------------|
| task_id    | Integer | probably don't need to search by task_id, similar to get_task_info |
| project_id | Integer |                                                              |
| user_id    | Integer |                                                              |
| name       | String  | if needed search with name                                   |
| keyword    | String  | search in name                                   |
| limit      | Integer     |                                                              |
| offset     | Integer     |                                                              |
| with      | Array[String] from list: users, projects |   

### Response data: JSON string with the following elements

| Parameter | Type                                   | Description                                  |
|-----------|----------------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                 | Error description or successful info message |
| data      | Array of objects packed to JSON string | Task objects with all fields or empty        |
| count     | Integer                               | Count of finded objects        |

## Search Tasks with Grouping

> Request body (application/json):

```json
{
  "project_id": 0,
  "with": ["users","projects"],
  "group_by": "status"
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "1 group(s) found.",
  "data": {
    "groups": [
      {
        "value": true,
        "count": 1,
        "items": [
          {
            "id": 0,
            "name": "Task name",
            "project": 0,
            "duration": 3600,
            "status": "new",
            "users": [0],
            "entries": {
              "0": {
                "start_datetime": 1583905200,
                "end_datetime": 1583905200,
                "duration": 3600
              }
            }
          }
        ]
      }
    ],
    "users": {
      "0": {
        "id": 0,
        "email": "email@dom.com",
        "first_name": "John",
        "last_name": "Smith",
        "hourly_rate": 10,
        "picture": "url_to_user_avatar",
        "teams": [0],
        "birth_date": "1988-11-22"
      }
    },
    "teams": {
      "0": {
        "id": 0,
        "name": "Group name",
        "description": "description..",
        "notes": "notes..",
        "status": "new",
        "pay_rate": 10.5
      }
    },
    "projects": {
      "0": {
        "id": 0,
        "name": "Project name",
        "description": "description...",
        "client_id": 0,
        "duration_unit": "minutes",
        "status": "new",
        "planned_start_date": 1583905200,
        "actual_start_date": 1583905200,
        "planned_end_date": 1583905200,
        "actual_end_date": 1583905200,
        "estimated_duration": 100,
        "actual_duration": 100,
        "tasks": [0]
      }
    }
  },
  "count": 1
}
```

### HTTP Request

`POST /api/{APP_CODE}/task/search_group`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| client_id | Integer |                                |
| name      | String  | if needed search with name     |
| category  | String  | if needed search with category |
| description  | String  | if needed search with  |
| keyword  | String  | Search in name, description, category  |
| duration_unit  | String from the list:minutes, hours, days, weeks, months, years   | if needed search with duration_unit  |
| status    | String from list: new, in-progress, cancelled, done  | if needed search with status   |
| limit     | Integer     |                                |
| offset    | Integer     |                                |
| with      | Array[String] from list: users, projects |     
| group_by  | String                              |   field for grouping          |
| order_by  | String from list: asc, desc     |  sorting order for grouped field          |  


### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Array of Object packed to JSON string | objects with all fields or empty object  |
| count      | Integer | Count of finded groups  |

## Delete Task

> Request body (application/json):

```json
{
  "task_id": 1
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "Successfully deleted."
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "Task not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /timesheets/task/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| task_id   | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Add User to Task

> Request body (application/json):
```json
{
  "task_id":1,
  "user_id":2
}
```

> 200 Response body (application/json):
```json
{
   "status": "SUCCESS",
   "message": "User successfully added to task."
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "Task not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /api/{app_path}/task/add_user`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| task_id   | Integer (*Required*) | Task ID     |
| user_id   | Integer (*Required*) | User ID     |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Delete User from Task

> Request body (application/json):

```json
{
  "task_id":1,
  "user_id":2
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "User successfully deleted from task."
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "Task not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /api/{app_path}/task/delete_user`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| task_id   | Integer (*Required*) | Task ID     |
| user_id   | Integer (*Required*) | User ID     |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## History of changes task.

> Request body (application/json):

```json
{
  "task_id": 1
}
```

> 200 Response body (application/json):

```json
{
    "status": "SUCCESS",
    "message": "3 history record(s) found.",
    "data": [
        {
            "status": "in-progress",
            "date": "1590652717",
            "user": {
                "first_name": "Member",
                "last_name": "Userov"
            }
        },
        {
            "status": "cancelled",
            "date": "1590652717",
            "user": {
                "first_name": "Member",
                "last_name": "Userov"
            }
        },
        {
            "status": "done",
            "date": "1590652717",
            "user": {
                "first_name": "Member",
                "last_name": "Userov"
            }
        }
    ]
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "Task not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /timesheets/task/history`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type            | Description |
|---------------|-----------------|-------------|
| task_id       | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Array of entity packed to JSON string |  object with all fields or empty object  |

## History of changes task.

> Request body (application/json):
```json
{
  "task_id": 1,
  "with": ["users", "tasks", "projects"]
}
```

> 200 Response body (application/json):
```json
{
    "status": "SUCCESS",
    "message": "2 time entries found.",
    "data": {
        "time_entries": [
            {
                "id": 2,
                "user_id": 111,
                "task_id": 3,
                "status": "rejected",
                "start_time": "1566486923",
                "breaks": 0,
                "comments": 0,
                "duration": 6020,
                "approve_date": "1590652717",
                "end_time": "1566492943",
                "start_location": [37.270241, -119.989356],
                "end_location": [37.537152,-118.857427]
            },
            {
                "id": 3,
                "user_id": 111,
                "task_id": 3,
                "status": "approved",
                "start_time": "1566492323",
                "breaks": 0,
                "comments": 0,
                "duration": 4220,
                "approve_date": "1590652717",
                "end_time": "1566496543",
                "start_location": [37.270242,-119.989356],
                "end_location": [37.537153,-118.857427]
            }
        ],
        "users": {
            "111": {
                "id": 111,
                "first_name": "John",
                "last_name": "Smith"
            }
        },
        "tasks": {
            "3": {
                "id": 3,
                "project": 1,
                "name": "Task #3",
                "status": "in-progress"
            }
        },
        "projects": {
            "1": {
                "id": 1,
                "name": "Project #1",
                "status": "in-progress",
                "client_id": 1,
                "tasks": [9,7,5,3,1]
            }
        }
    },
    "timestamp": 1594127815
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "Task not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /timesheets/task/work_history`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type            | Description |
|---------------|-----------------|-------------|
| task_id       | Integer (*Required*) |             |
| user_id       | Integer  |             | Optional for admin. If unset - for current user |
| with          | Array[String] from list: users, tasks, projects |  


### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Array of entity packed to JSON string |  object with all fields or empty object  |
| timestamp | Timestamp                     |                       |

## <ins>**PROJECT**</ins>

Project API for iBuildApp Timesheets, handling getting project information, creating/deleting/updating a project, and searching for projects.

## Get Project Info

> Request body (application/json):

```json
{
	"project_id":1
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "data": {
        "id": 1,
        "name": "project_name",
        "description": "description",
        "client_id": 21,
        "duration_unit": "minutes",
        "status": "done",
        "planned_start_date": 1565687043,
        "actual_start_date": 1565687043,
        "planned_end_date": 1565687043,
        "actual_end_date": 1565687043,
        "estimated_duration": 1000,
        "actual_duration": 1200,
        "tasks": [0,1,2]
  }
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "Project not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/project`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type            | Description |
|------------|-----------------|-------------|
| project_id | Integer (*Required*) | Project ID  |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| data      | Object packed to JSON string | User object with all fields or empty object  |

## Create New Project

> Request body (application/json):
```json
{
	"client_id": 1,
	"name": "Test",
	"description": "test project"
}
```

> 200 Response body (application/json):
```json
{
   "status": "SUCCESS",
   "message": "Successfully created.",
   "id": 1
}
```

### HTTP Request

`POST /api/{APP_CODE}/project/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter          | Type                                             | Description |
|--------------------|--------------------------------------------------|-------------|
| client_id          | Integer (*Required*)                                  |             |
| name               | String (*Required*)                                   |             |
| description        | String                                             |             |
| category           | String                                           |             |
| status             | String from list: new, in-progress, cancelled, done                                           |             |
| planned_start_date | Date                                             |             |
| actual_start_date  | Date                                             |             |
| planned_end_date   | Date                                             |             |
| actual_end_date    | Date                                             |             |
| estimated_duration | Float                                            |             |
| actual_duration    | Float                                            |             |
| duration_unit      | String from the list:minutes, hours, days, weeks, months, years |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                          | New project id                               |

## Update Project

> Request body (application/json):

```json
{
	"project_id": 1,
	"description": "my project"
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully updated."
}
```

> 404 Not found. Response body (application/json):
```json
{
   "status": "ERROR",
    "message": "Project not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/project/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter          | Type            | Description |
|--------------------|-----------------|-------------|
| client_id          | Integer (*Required*) |             |
| project_id         | Integer         |             |
| name               | String          |             |
| description        | String            |             |
| category           | String          |             |
| status             | String from list: new, in-progress, cancelled, done        |             |
| planned_start_date | Timestamp       |             |
| actual_start_date  | Timestamp       |             |
| planned_end_date   | Timestamp       |             |
| actual_end_date    | Timestamp       |             |
| estimated_duration | Float           |             |
| actual_duration    | Float           |             |
| duration_unit      | String from the list:minutes, hours, days, weeks, months, years          |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Projects

> Request body (application/json):

```json
{
	"name": "test",
	"limit": 10,
	"offset": 0,
    "with": ["tasks" ,"clients"]
}
```

> Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "1 project(s) found.",
  "data": {
    "projects": [
      {
        "id": 0,
        "name": "test project",
        "description": "description",
        "client_id": 11,
        "duration_unit": "minutes",
        "status": "new",
        "planned_start_date": 1565687043,
        "actual_start_date": 1565687043,
        "planned_end_date": 1565687043,
        "actual_end_date": 1565687043,
        "estimated_duration": 20,
        "actual_duration": 10,
        "tasks": [0]
      }
    ],
    "tasks": {
      "0": {
        "id": 0,
        "name": "task name",
        "project": 0,
        "duration": 10,
        "status": "new",
        "users": [0]
      }
    },
    "clients": {
      "11": {
        "id": 11,
        "name": "Client name",
        "status": "active"
      }
    }
  },
  "count": 1
}
```

### HTTP Request

`POST /api/{APP_CODE}/project/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| client_id | Integer |                                |
| name      | String  | if needed search with name     |
| category  | String  | if needed search with category |
| description  | String  | if needed search with  |
| keyword  | String  | Search in name, description, category  |
| duration_unit  | String from the list:minutes, hours, days, weeks, months, years   | if needed search with duration_unit  |
| status    | String from list: new, in-progress, cancelled, done  | if needed search with status   |
| limit     | Integer     |                                |
| offset    | Integer     |                                |
| with      | Array[String] from list: tasks, clients |     

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Array of Object packed to JSON string | objects with all fields or empty object  |
| count      | Integer | Count of finded objects  |

## Search Projects with Grouping

> Request body (application/json):

```json
{
	"name": "test",
	"limit": 10,
	"offset": 0,
    "with": ["tasks" ,"clients"],
    "group_by": "status"
}
```

> Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "1 group(s) found.",
  "data": {
    "groups": [
      {
        "value": "in-progress",
        "count": 1,
        "items": [
          {
            "id": 0,
            "name": "Project test name",
            "description": "description...",
            "client_id": 0,
            "duration_unit": "minutes",
            "status": "new",
            "planned_start_date": 1565687043,
            "actual_start_date": 1565687043,
            "planned_end_date": 1565687043,
            "actual_end_date": 1565687043,
            "estimated_duration": 100,
            "actual_duration": 90,
            "tasks": [0]
          }
        ]
      }
    ],
    "tasks": {
      "0": {
        "id": 0,
        "name": "Task name",
        "project": 0,
        "duration": 0,
        "status": "new",
        "users": [0]
      }
    },
    "clients": {
      "0": {
        "id": 0,
        "name": "Client name",
        "status": "active"
      }
    }
  },
  "count": 1
}
```

### HTTP Request

`POST /api/{APP_CODE}/project/search_group`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| client_id | Integer |                                |
| name      | String  | if needed search with name     |
| category  | String  | if needed search with category |
| description  | String  | if needed search with  |
| keyword  | String  | Search in name, description, category  |
| duration_unit  | String from the list:minutes, hours, days, weeks, months, years   | if needed search with duration_unit  |
| status    | String from list: new, in-progress, cancelled, done  | if needed search with status   |
| limit     | Integer     |                                |
| offset    | Integer     |                                |
| with      | Array[String] from list: tasks, clients |     
| group_by  | String                              |   field for grouping          |
| order_by  | String from list: asc, desc     |  sorting order for grouped field          |  


### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Array of Object packed to JSON string | objects with all fields or empty object  |
| count      | Integer | Count of finded groups  |

## Delete Project

> Request body (application/json):

```json
{
	"project_id": 1
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully deleted."
}
```

> 404 Not found. Response body (application/json):
```json
{
   "status": "ERROR",
    "message": "Project not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/project/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type     | Description |
|------------|----------|-------------|
| project_id | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

<!-- ## **SCHEDULE**

## Get Schedule Info

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements



### Response data: JSON string with the following elements



## Create New Schedule

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements



### Response data: JSON string with the following elements



## Update Schedule

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements



### Response data: JSON string with the following elements



## Search Schedules

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements



### Response data: JSON string with the following elements



## Delete Schedule

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements



### Response data: JSON string with the following elements

 -->

## <ins>**RATE**</ins>

Rate API for iBuildApp Timesheets, handling getting rate information, creating/deleting/updating a rate, and searching for rates.

## Get Rate Info

> Request body (application/json):

```json
{
	"rate_id": 1
}
```

> Response body (application/json):

```json
{
    "status": "SUCCESS",
    "data": {
        "id": 1,
        "name": "test_rate",
        "amount": 100,
        "schedule_id": 1,
        "user_id": 1,
        "duration_unit": "days"
    }
}
```

### HTTP Request

`POST /api/{APP_CODE}/rate`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| rate_id   | Integer (*Required*) | ID          |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |

## Create New Rate

> Request body (application/json):

```json
{
	"user_id": 1,
	"schedule_1": 1,
	"name": "test_rate",
	"duration_unit": "days",
	"amount": 100
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully created.",
   "id": 1
}
```

### HTTP Request

`POST /api/{APP_CODE}/rate/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type                                                     | Description |
|---------------|----------------------------------------------------------|-------------|
| user_id       | Integer (*Required*)                                          |             |
| schedule_id   | Integer (*Required*)                                          |             |
| name          | String (*Required*)                                           |             |
| amount        | Float (*Required*)                                            |             |
| duration_unit | String from the list: minutes, hours, days, weeks, months (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                          | ID of the new Rate entity                    |

## Update Rate

> Request body (application/json):

```json
{
	"rate_id": 1,
	"name": "test_rate1"
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully updated."
}
```

### HTTP Request

`POST /api/{APP_CODE}/rate/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type                                             | Description |
|---------------|--------------------------------------------------|-------------|
| rate_id       | Integer (*Required*)                                 |             |
| user_id       | Integer                                          |             |
| schedule_id   | Integer                                          |             |
| name          | String                                           |             |
| amount        | Float                                            |             |
| duration_unit | String from the list: minutes, hours, days, weeks, months |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Rates

> Request body (application/json):

```json
{
	"name": "test"
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "1 rate(s) found.",
   "data": [
      {
          "id": 1,
          "name": "test_rate",
          "amount": 100,
          "schedule_id": 1,
          "user_id": 1,
          "duration_unit": "days"
      }
   ]
}
```

### HTTP Request

`POST /api/{APP_CODE}/rate/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type                                             | Description                |
|---------------|--------------------------------------------------|----------------------------|
| user_id       | Integer                                          |                            |
| schedule_id   | Integer                                          |                            |
| name          | String                                           | if needed search with name |
| amount        | Float                                            |                            |
| duration_unit | String from the list: minutes, hours, days, weeks, months |                            |
| limit         | Integer                                              |                            |
| offset        | Integer                                              |                            |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |

## Delete Rate

> Request body (application/json):

```json
{
	"rate_id": 1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully deleted."
}
```

### HTTP Request

`POST /api/{APP_CODE}/rate/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description |
|-----------|---------|-------------|
| rate_id   | Integer |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

<!-- ## **TASK HISTORY**

## Get Task History Info

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements



### Response data: JSON string with the following elements



## Create New Task History

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements



### Response data: JSON string with the following elements



## **APPROVAL HISTORY**

## Get Approval History Info

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements



### Response data: JSON string with the following elements



## Create New Approval History

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements



### Response data: JSON string with the following elements
 -->


## <ins>**TIME ENTRY**</ins>

Time Entry API for iBuildApp Timesheets, handling getting time entry information, creating/deleting/updating a time entry, getting time entry status history, setting time entry status, and searching for time entries.

## Start Time Tracking

> Request body (application/json):

```json
{
	"task_id":18,
    "location":[-37.270245, -119.989356],
    "comment":"my comment"
}
```

> 200 Response body (application/json):
```json
{
   "status": "SUCCESS",
   "message": "Starteed."
}
```

> 401 Unauthorized. Response body (application/json):
```json
{
   "status": "ERROR",
   "message": "Auth token not specified or incorrect.",
   "error_code": 9
}
```

### HTTP Request

`POST /api/{APP_CODE}/ts/start`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description |
|-----------|---------|-------------|
| task_id   | Integer | Task id     |
| location  | Array     | Location [latitude,longitude]     |
| comment   | String    |    |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message: "Successfully started.","Task not found.","Found started TimeEntry.”|

## Stop Time Tracking

> Request body (application/json):

```json
{
    "task_id":18,
    "comment":"my comment2",
    "location":[-37.270245, -119.989356]
}

```

> 200 Response body (application/json):
```json
{
  "status": "SUCCESS",
  "message": "Stopped."
}
```

> 401 Unauthorized. Response body (application/json):
```json
{
   "status": "ERROR",
   "message": "Auth token not specified or incorrect.",
   "error_code": 9
}
```

### HTTP Request

`POST /api/{APP_CODE}/ts/stop`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description |
|-----------|---------|-------------|
| task_id   | Integer | Task id     |
| location  | Array     | Location [latitude,longitude]     |
| comment   | String    |    |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Create New Timesheet Entry

> Request body (application/json):

```json
{
    "user_id":1,
    "task_id":18,
    "start_time":1565687043,
    "end_time":1565688043,
    "location":[-37.270245, -119.989356],
    "comment":"my comment"
}
```

> 200 Response body (application/json):
```json
{
  "status": "SUCCESS",
  "message": "Successfully created.",
  "id": 2
}
```

> 401 Unauthorized. Response body (application/json):
```json
{
   "status": "ERROR",
   "message": "Auth token not specified or incorrect.",
   "error_code": 9
}
```

> 404 Task found. Response body (application/json):
```json
{
   "status": "ERROR",
    "message": "Task not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/ts/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type                   | Description                |
|------------|------------------------|----------------------------|
| user_id    | Integer (*Required*)| User ID                    |
| task_id    | Integer | Task ID                    |
| start_time | Timestamp      | Period start date and time. Current Timestamp if empty |
| end_time   | Timestamp       | Period end date and time   |
| comment    | String                 | comment                    |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                       | ID of created TimeEntry |

## Get Time Entry Info

> Request body (application/json):

```json
{
  "time_entry_id": 1,
  "with": [
    "users",
    "projects",
    "comments",
    "breaks"
  ]
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "1 time entries found.",
  "data": {
    "time_entry": {
      "id": 0,
      "user_id": 0,
      "task_id": 0,
      "start_time": 1583905200,
      "end_time": 1583905200,
      "duration": 600,
      "approve_date": 1583905200,
      "breaks": 1,
      "status": "in-progress"
    },
    "users": {
      "0": {
        "id": 0,
        "first_name": "John",
        "last_name": "Smith",
        "hourly_rate": 0,
        "picture": "url_user_avatar",
        "birth_date": "1988-11-22"
      }
    },
    "breaks": [
      {
        "id": 0,
        "start_time": 1583905200,
        "end_time": 1583905800,
        "duration": 600
      }
    ],
    "comments": [
      {
        "id": 0,
        "commentator": 0,
        "content": "content",
        "date": 1566402943
      }
    ],
    "commentators": {
      "0": {
        "id": 0,
        "first_name": "John",
        "last_name": "Smith",
        "picture": "url_user_avatar"
      }
    },
    "tasks": {
      "0": {
        "id": 0,
        "project": 0,
        "name": "Task #0",
        "description": "description",
        "estimate_hours": 10.1,
        "duration": 111,
        "status": "new"
      }
    },
    "projects": {
      "0": {
        "id": 0,
        "name": "Project name",
        "description": "description",
        "client_id": 0,
        "duration_unit": "minutes",
        "status": "new",
        "planned_start_date": 0,
        "actual_start_date": 0,
        "planned_end_date": 0,
        "actual_end_date": 0,
        "estimated_duration": 10,
        "actual_duration": 10,
        "tasks": [
          0
        ]
      }
    },
    "customers": {
      "0": {
        "id": 0,
        "name": "Customer #0",
        "status": "active"
      }
    }
  },
  "count": 1
}
```

> 401 Unauthorized. Response body (application/json):
```json
{
   "status": "ERROR",
   "message": "Auth token not specified or incorrect.",
   "error_code": 9
}
```


### HTTP Request

`POST /api/{APP_CODE}/ts/get`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type           | Description |
|---------------|----------------|-------------|
| time_entry_id | Integer  (*Required*)       | TimeEntry ID          |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | TimeEntry Object with all fields or empty object  |

## Update Time Entry

> Request body (application/json):

```json
{
	"time_entry_id": 1,
	"task_id": 2
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully updated."
}
```

> 401 Unauthorized. Response body (application/json):
```json
{
   "status": "ERROR",
   "message": "Auth token not specified or incorrect.",
   "error_code": 9
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "TimeEntry  not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /api/{app_path}/ts/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter      | Type                                | Description |
|----------------|-------------------------------------|-------------|
| time_entry_id  | Integer (*Required*)                     |             |
| task_id        | Integer                             |             |
| start_datetime | Timestamp                           |             |
| end_datetime   | Timestamp                           |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Time Entries

> Request body (application/json):

```json
{
  "time_entry_ids": [
    0, 10
  ],
  "user_id": 1,
  "task_id": 1,
  "project_id": 1,
  "payment_status": "pending",
  "keywords": "string",
  "with": [
    "users",
    "tasks",
    "projects"
  ]
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "1 time entries found.",
  "data": {
    "time_entries": [
      {
        "id": 0,
        "user_id": 0,
        "task_id": 0,
        "start_time": 1583905200,
        "end_time": 1583905200,
        "approve_date": 1583905200,
        "breaks": 0,
        "comments": 0,
        "duration": 0,
        "status": "in-progress",
        "payment_status": "pending",
        "start_location": [
          37.270248,
          -119.989356
        ],
        "end_location": [
          37.270248,
          -119.989356
        ]
      }
    ],
    "users": {
      "0": {
        "id": 0,
        "email": "string",
        "first_name": "string",
        "last_name": "string",
        "hourly_rate": 0,
        "picture": "string",
        "birth_date": "1988-11-22"
      }
    },
    "tasks": {
      "0": {
        "id": 0,
        "name": "string",
        "project": 0,
        "duration": 0,
        "status": "new",
        "users": [
          0
        ]
      }
    },
    "projects": {
      "0": {
        "id": 0,
        "name": "string",
        "description": "string",
        "client_id": 0,
        "duration_unit": "minutes",
        "status": "new",
        "planned_start_date": 0,
        "actual_start_date": 0,
        "planned_end_date": 0,
        "actual_end_date": 0,
        "estimated_duration": 0,
        "actual_duration": 0,
        "tasks": [
          0
        ]
      }
    }
  },
  "count": 1
}
```

### HTTP Request

`POST /api/{app_path}/ts/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter      | Type                                | Description         |
|----------------|-------------------------------------|---------------------|
| time_entry_ids | Array[Integer]                      |  TimeEntry IDs                 |
| task_id        | Integer                             |                     |
| user_id        | Integer                             | Optional for admin. |
| start_datetime | Timestamp                           |                     |
| end_datetime   | Timestamp                           |                     |
| status         | String from list: in-progress, rejected, approved |                     |
| keywords       | String                               |                     |
| limit          | Integer                               |                     |
| offset         | Integer                               |                     |
| with           | Array[String] from list: "users","tasks","projects" |                     |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | TimeEntry Object with all fields or empty object  |

## Search Time Entries with Grouping

> Request body (application/json):

```json
{
  "project_id": 1,
  "group_by": "status",
  "with": [
    "users",
    "tasks",
    "projects"
  ]
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "1 group(s) found.",
  "data": {
    "groups": [
      {
        "value": true,
        "count": 1,
        "items": [
          {
            "id": 0,
            "user_id": 0,
            "task_id": 0,
            "start_time": 1583905200,
            "end_time": 1583905200,
            "approve_date": 1583905200,
            "breaks": 0,
            "comments": 0,
            "duration": 0,
            "status": "in-progress",
            "payment_status": "pending",
            "start_location": [
              37.270248,
              -119.989356
            ],
            "end_location": [
              37.270248,
              -119.989356
            ]
          }
        ]
      }
    ],
    "users": {
      "0": {
        "id": 0,
        "email": "string",
        "first_name": "string",
        "last_name": "string",
        "hourly_rate": 0,
        "picture": "string",
        "birth_date": "1988-11-22"
      }
    },
    "tasks": {
      "0": {
        "id": 0,
        "name": "string",
        "project": 0,
        "duration": 0,
        "status": "new",
        "users": [
          0
        ]
      }
    },
    "projects": {
      "0": {
        "id": 0,
        "name": "string",
        "description": "string",
        "client_id": 0,
        "duration_unit": "minutes",
        "status": "new",
        "planned_start_date": 0,
        "actual_start_date": 0,
        "planned_end_date": 0,
        "actual_end_date": 0,
        "estimated_duration": 0,
        "actual_duration": 0,
        "tasks": [
          0
        ]
      }
    }
  },
  "count": 1
}
```

### HTTP Request

`POST /api/{app_path}/ts/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter      | Type                                | Description         |
|----------------|-------------------------------------|---------------------|
| time_entry_ids | Array[Integer]                      |  TimeEntry IDs                 |
| task_id        | Integer                             |                     |
| user_id        | Integer                             |             |
| start_datetime | Timestamp                           |                     |
| end_datetime   | Timestamp                           |                     |
| status         | String from list: in-progress, rejected, approved |                     |
| keywords       | String                               |                     |
| with           | Array[String] from list: "users","tasks","projects" |                     |
| group_by        | String                              |   field for grouping          |
| order_by            | String from list: asc, desc     |  sorting order for grouped field          |
| limit           | Integer                           |             |
| offset          | Integer                           |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |
| count     | Integer | Count of Objects |

## Delete Time Entry

> Request body (application/json):

```json
{
	"time_entry_id": 1
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully deleted."
}
```

> 404 User not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "Time entry not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /api/{app_path}/ts/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type     | Description |
|---------------|----------|-------------|
| time_entry_id | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Get Status History of Time Entry

> Request body (application/json):

```json
{
	"time_entry_id": 1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "2 time entries found.",
   "data": [
       {
           "status": "in-progress",
           "event_date": "2019-08-28 12:56:03",
           "user": {
               "first_name": "Root",
               "last_name": "Adminson"
           }
       },
       {
           "status": "approved",
           "event_date": "2019-08-28 13:07:14",
           "user": {
               "first_name": "Root",
               "last_name": "Adminson"
           }
       }
   ]
}

```

### HTTP Request

`POST /api/{app_path}/ts/history`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type     | Description |
|---------------|----------|-------------|
| time_entry_id | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                                  | Description                                             |
|-----------|---------------------------------------|---------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                | Error description or successful info message            |
| data      | Array of object packed to JSON string | Approval History object with all fields or empty object |

## Start Break of Time Entry

> Request body (application/json):

```json
{
	"time_entry_id": 1
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "Successfully started.",
  "id": 0
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "Started TimeEntry not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /api/{app_path}/ts/break_start`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type     | Description |
|---------------|----------|-------------|
| time_entry_id | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                                  | Description                                             |
|-----------|---------------------------------------|---------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                | Error description or successful info message            |
| id      | Integer                                 | ID of Break                           |

## Stop Break of Time Entry

> Request body (application/json):

```json
{
	"time_entry_id": 1
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "Stopped."
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "Started Break not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /api/{app_path}/ts/break_stop`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type     | Description |
|---------------|----------|-------------|
| time_entry_id | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                                  | Description                                             |
|-----------|---------------------------------------|---------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                | Error description or successful info message            |

## Breaks of Time Entry
Return duration of breaks in seconds

> Request body (application/json):

```json
{
	"time_entry_id":26
}
```

> 200 Response body (application/json):

```json
{
    "status": "SUCCESS",
    "message": "2 break(s) found.",
    "data": [
        {
            "id": 1,
            "start_datetime": "1566391583",
            "end_datetime": "1566391703",
            "time_entry": 1,
            "duration": 120
        },
        {
            "id": 2,
            "start_datetime": "1566392183",
            "end_datetime": "1566392303",
            "time_entry": 1,
            "duration": 120
        }
    ],
    "count": 2
}
```

> 404 Not found. Response body (application/json):
```json
{
     "status": "ERROR",
      "message": "TimeEntry not found.",
      "error_code": 5
}
```

### HTTP Request

`POST /api/{app_path}/ts/breaks`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type     | Description |
|---------------|----------|-------------|
| time_entry_id | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                                  | Description                                             |
|-----------|---------------------------------------|---------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                | Error description or successful info message            |
| data      | Array of object packed to JSON string | Approval History object with all fields or empty object |
| count   | Integer                                | Count of breaks            |

## Approve Time Entries
Return duration of breaks in seconds

> Request body (application/json):

```json
{
	"time_entry_ids":[22,23,24]
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "Successfully approved.",
  "data": [1,3],
  "count": 2
}
```


### HTTP Request

`POST /api/{app_path}/ts/approve`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type     | Description |
|---------------|----------|-------------|
| time_entry_ids | Array[Integer] (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                                  | Description                                             |
|-----------|---------------------------------------|---------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                | Error description or successful info message            |
| data      | Array of object packed to JSON string | Array of ID of approved time entry  |
| count   | Integer                                | Count of approved time entry            |

## Calculate a cost of selected time_entries for invoice. 
Return duration in seconds and cost for selected APPROVED time entry

> Request body (application/json):

```json
{
	"time_entry_ids":[22,23,24]
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "Successfully calculated.",
  "data": {
    "duration": 12257,
    "sum": 10.5
  }
}
```


### HTTP Request

`POST /api/{app_path}/ts/calc`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type     | Description |
|---------------|----------|-------------|
| time_entry_ids | Array[Integer] (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                                  | Description                                             |
|-----------|---------------------------------------|---------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                | Error description or successful info message            |
| data: duration | Integer | Duration in seconds  |
| data: sum      | Float | Full cost (with minus breaks)  |

## <ins>**Changelog**</ins>
