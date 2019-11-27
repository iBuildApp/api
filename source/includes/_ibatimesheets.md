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

## Get User Dashboard Data

> Request body (application/json):

```json
{
	"user_id":1,
	"last_update":1565768191
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "1 task(s) found.",
   "data": {
       "current_tasks": [
           {
               "id": 1,
               "name": "testTask",
               "status": "in-progress"
           }
       ],
       "current_time_entry": {
           "id": 3,
           "user_id": 1,
           "task_id": 1,
           "status": "in-progress",
           "start_time": "2019-08-13 12:04:03"
       }
   },
   "timestamp": 1565954217
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

> Response body (application/json):

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
               "project_id": 2,
               "name": "Test task 1",
               "status": "in-progress",
           }
       },
       "projects": {
           "2": {
               "id": 2,
               "name": "Test project 1",
               "status": "in-progress",
           }
       }
   },
   "timestamp": 1565954217
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

## Start Time Tracking

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
   "message": "Starteed."
}
```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description |
|-----------|---------|-------------|
| task_id   | Integer | Task id     |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Stop Time Tracking

> Request body (application/json):

```json
Token in header request
```

> Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "Stopped."
}
```

### HTTP Request

`POST /api/{APP_CODE}/ts/stop`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

Token in header request

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Add New Timesheet Entry

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /api/{APP_CODE}/ts/add-time-entry`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type                   | Description                |
|------------|------------------------|----------------------------|
| user_id    | Integer | GUID (*Required*)| User ID                    |
| start_time | Datetime (*Required*)       | Period start date and time |
| end_time   | Datetime (*Required*)       | Period end date and time   |
| comment    | String                 | comment                    |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## <ins>**USER**</ins>

User API for iBuildApp Timesheets, handling getting user information, creating/deleting/updating a user, and searching for user(s)

## Get User Info

> Request body (application/json):

```json
{
	"user_id": 1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "data": {
       "id": 1,
       "email": "test@test.com",
       "first_name": "John",
       "last_name": "Doe"
   }
}
```

### HTTP Request

`POST /api/{APP_CODE}/user/create`

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
	"first_name": "John",
	"last_name": "Smith"
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

`POST /api/{APP_CODE}/user/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type           | Description |
|------------|----------------|-------------|
| email      | String (*Required*) |             |
| first_name | String         |             |
| last_name  | String         |             |
| position   | String         |             |
| is_active  | Boolean           |             |

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

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully updated."
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
| sex        | String  |                                                       |
| birth_date | Date    |                                                       |
| position   | String  |                                                       |
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

> Response body (application/json):

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
   ]
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

## Delete User

> Request body (application/json):

```json
{
	"user_id": 2
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

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "data": {
       "id": 1,
       "name": "test_client"
   }
}
```

### HTTP Request

`POST /api/{APP_CODE}/client`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type                   | Description |
|-----------|------------------------|-------------|
| client_id | Integer | GUIDRequired | Client ID   |

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
	"address": "test city, test street, test house",
	"phone": 12345678912,
	"email": "mail@mail.com"
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
| contact_info | String         |             |
| address      | String         |             |
| phone        | String         |             |
| email        | String         |             |
| is_active    | Boolean           |             |

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

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully updated."
}
```

### HTTP Request

`POST /api/{APP_CODE}/client/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter    | Type            | Description |
|--------------|-----------------|-------------|
| client_id    | Integer (*Required*) | Client ID   |
| name         | String (*Required*)  |             |
| contact_info | String          |             |
| address      | String          |             |
| phone        | String          |             |
| email        | String          |             |
| is_active    | Boolean            |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Clients

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
	"message": "1 client(s) found.",
	"data": [
		{
			"id": 1,
			"name": "test_client"
		}
	]
}
```

### HTTP Request

`POST /api/{APP_CODE}/client/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description |
|-----------|---------|-------------|
| client_id | Integer | Client ID   |
| name      | String  |             |
| is_active | Boolean    |             |
| limit     | Integer     |             |
| offset    | Integer     |             |

### Response data: JSON string with the following elements

| Parameter | Type                                         | Description                                         |
|-----------|----------------------------------------------|-----------------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                       | Error description or successful info message        |
| data      | Array of group objects packed to JSON string | Client objects array with all fields or empty array |

## Delete Client

> Request body (application/json):

```json
{
    "client_id": 1
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

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /timesheets/task`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type           | Description |
|-----------|----------------|-------------|
| token     | String (*Required*) | Auth token  |
| task_id   | Integer        | Task ID     |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |

## Create New Task

> Request body (application/json):

```json
{
	"project_id":1
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

`POST /api/{app_path}/task/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type            | Description |
|---------------|-----------------|-------------|
| project_id    | Integer (*Required*) | Project ID  |
| name          | String (*Required*)  |             |
| description   | String (*Required*)    |             |
| estimate      | Float (*Required*)   |             |
| estimate_unit | String (*Required*)  |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                      | ID of created task                           |

## Update Task

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /timesheets/task/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type            | Description |
|---------------|-----------------|-------------|
| token         | String (*Required*)  | Auth token  |
| task_id       | Integer (*Required*) |             |
| name          | String (*Required*)  |             |
| description   | String (*Required*)    |             |
| estimate      | Float (*Required*)   |             |
| estimate_unit | String (*Required*)  |             |
| status        | String (*Required*)  |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Set Task Status

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /timesheets/task/change_status`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type            | Description |
|------------|-----------------|-------------|
| token      | String (*Required*)  | Auth token  |
| task_id    | Integer (*Required*) |             |
| new_status | String (*Required*)  |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Tasks

> Request body (application/json):

```json
{
	"user_id":1
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
           "project": 1,
           "name": "testTask",
           "status": "new"
       }
   ]
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
| limit      | Integer     |                                                              |
| offset     | Integer     |                                                              |

### Response data: JSON string with the following elements

| Parameter | Type                                   | Description                                  |
|-----------|----------------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                 | Error description or successful info message |
| data      | Array of objects packed to JSON string | Task objects with all fields or empty        |

## Delete Task

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /timesheets/task/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| token     | String (*Required*)  | Auth token  |
| taks_id   | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Add User to Task

> Request body (application/json):

```json
{
  "user_id":2,
  "task_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "User successfully added to task."
}
```

### HTTP Request

`POST /api/{app_path}/task/add_user`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| user_id   | Integer (*Required*) | User ID     |
| task_id   | Integer (*Required*) | Task ID     |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                      | ID of created task                           |

## Delete User from Task

> Request body (application/json):

```json
{
  "user_id":2,
  "task_id":1
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "User successfully deleted from task."
}
```

### HTTP Request

`POST /api/{app_path}/task/delete_user`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| user_id   | Integer (*Required*) | User ID     |
| task_id   | Integer (*Required*) | Task ID     |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                      | ID of created task                           |

## Restore Task

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /timesheets/task/restore`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| token     | String (*Required*)  | Auth token  |
| taks_id   | Integer (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## <ins>**PROJECT**</ins>

Project API for iBuildApp Timesheets, handling getting project information, creating/deleting/updating a project, and searching for projects.

## Get Project Info

> Request body (application/json):

```json
{
	"project_id":1
}
```

> Response body (application/json):

```json
{
    "status": "SUCCESS",
    "data": {
        "id": 1,
        "name": "test_project",
        "status": "new",
        "client_id": 1
    }
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
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |

## Create New Project

> Request body (application/json):

```json
{
	"name": "Test",
	"description": "test project",
	"client_id": 1
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

`POST /api/{APP_CODE}/project/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter          | Type                                             | Description |
|--------------------|--------------------------------------------------|-------------|
| client_id          | Integer (*Required*)                                  |             |
| name               | String (*Required*)                                   |             |
| description        | String                                             |             |
| category           | String                                           |             |
| status             | String                                           |             |
| planned_start_date | Date                                             |             |
| actual_start_date  | Date                                             |             |
| planned_end_date   | Date                                             |             |
| actual_end_date    | Date                                             |             |
| estimated_duration | Float                                            |             |
| actual_duration    | Float                                            |             |
| duration_unit      | String from the list:minuteshoursdaysweeksmonths |             |

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

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully updated."
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
| status             | Integer         |             |
| planned_start_date | Timestamp       |             |
| actual_start_date  | Timestamp       |             |
| planned_end_date   | Timestamp       |             |
| actual_end_date    | Timestamp       |             |
| estimated_duration | Float           |             |
| actual_duration    | Float           |             |
| duration_unit      | String          |             |

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
	"limit": 1,
	"offset": 0
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "1 project(s) found.",
   "data": [
      {
          "id": 1,
          "name": "test_project",
          "status": "new",
          "client_id": 1
      }
   ]
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
| status    | String  | if needed search with status   |
| limit     | Integer     |                                |
| offset    | Integer     |                                |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |

## Delete Project

> Request body (application/json):

```json
{
	"project_id": 1
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

## Get Time Entry Info

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /timesheets/time_entry`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type           | Description |
|---------------|----------------|-------------|
| token         | String (*Required*) | Auth token  |
| time_entry_id | Integer        | ID          |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | User object with all fields or empty object  |

## Create New Time Entry

> Request body (application/json):

```json

```

> Response body (application/json):

```json

```

### HTTP Request

`POST /timesheets/time_entry/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter      | Type              | Description |
|----------------|-------------------|-------------|
| token          | String (*Required*)    | Auth token  |
| task_id        | Integer (*Required*)   |             |
| schedule_id    | Integer (*Required*)   |             |
| start_datetime | Timestamp (*Required*) |             |
| end_datetime   | Timestamp         |             |
| status         | Integer (*Required*)   |             |
| payment_status | Integer (*Required*)   |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Time Entries

> Request body (application/json):

```json
{
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "2 time entries found.",
   "data": [
       {
           "id": 2,
           "user_id": 1,
           "task_id": 1,
           "start_time": "2019-08-22 07:03:46",
           "end_time": "2019-08-22 07:33:46"
       },
       {
           "id": 3,
           "user_id": 1,
           "task_id": 1,
           "status": "in-progress",
           "start_time": "2019-08-22 07:53:46",
           "end_time": "2019-08-28 07:42:24"
       }
   ]
}
```

### HTTP Request

`POST /api/{app_path}/ts/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter      | Type                                | Description         |
|----------------|-------------------------------------|---------------------|
| task_id        | Integer (*Required*)                     |                     |
| user_id        | Integer                             | Optional for admin. |
| schedule_id    | Integer                             |                     |
| start_datetime | Timestamp                           |                     |
| end_datetime   | Timestamp                           |                     |
| status         | String from list: in-progress, done |                     |
| payment_status | String from list: pending, payed    |                     |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Update Time Entry

> Request body (application/json):

```json
{
	"time_entry_id": 1,
	"status": "done"
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

`POST /api/{app_path}/ts/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter      | Type                                | Description |
|----------------|-------------------------------------|-------------|
| time_entry_id  | Integer (*Required*)                     |             |
| start_datetime | Timestamp                           |             |
| end_datetime   | Timestamp                           |             |
| status         | String from list: in-progress, done |             |
| payment_status | String from list: pending, payed    |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Delete Time Entry

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
   "message": "Successfully deleted."
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
           "status": "done",
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

## <ins>**Changelog**</ins>
