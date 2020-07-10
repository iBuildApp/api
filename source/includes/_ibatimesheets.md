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
### HTTP Request

`POST /api/{APP_CODE}/login`

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

### HTTP Request

`POST /api/{APP_CODE}/register`

### Request data: JSON string with the following elements

| Parameter  | Type           | Description   |
|------------|----------------|---------------|
| email      | String (*Required*) | Login (email) |
| password   | String (*Required*) | Password |
| first_name | String         | First name    |
| last_name  | String         | Last name     |
| sex        | String         | male, female  |
| position   | String         |               |
| birth_date | String         |  YYYY-MM-DD   |

### Response data: JSON string with the following elements

| Parameter | Type                          | Description                                                             |
|-----------|-------------------------------|-------------------------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR  | Operation result successful / execution error                               |
| message   | String                        | Error description or successful info message                            |
| token     | String                        | Auth token of user session. Will be used to sign every next request.    |
| status    | String from list: ACTIVE, PENDING | User status active / pending activation (Unable to login and use the system) |

## Request for change password
Sending email to user with link for reset password

> Request body (application/json):

```json
{
	"email": "test@test.com"
}
```

> Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Link to reset password successfully sent to your email."
}
```

### HTTP Request

`POST /api/{APP_CODE}/restore`

### Request data: JSON string with the following elements

| Parameter  | Type           | Description   |
|------------|----------------|---------------|
| email      | String (*Required*) | Login (email) |


### Response data: JSON string with the following elements

| Parameter | Type                          | Description                                                             |
|-----------|-------------------------------|-------------------------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR  | Operation result successful / execution error                               |
| message   | String                        | Error description or successful info message                            |

## Reset password
If email and token valid, generated new password end sended to user email.
### HTTP Request

`GET /api/{APP_CODE}/reset`

### Request data: GET parameters 

| Parameter  | Type           | Description   |
|------------|----------------|---------------|
| email      | String (*Required*) |  |
| token      | String (*Required*) |  |

## Get App settings

> Request body (application/json):

```json
{

}
```

> Response body (application/json):

```json
{
    "status": "SUCCESS",
    "data": {
        "path": "for_test",
        "settings": []
    }
}
```

### HTTP Request

`POST /api/{APP_CODE}/app`

### Request data: JSON string with the following elements

| Parameter  | Type           | Description   |
|------------|----------------|---------------|
|       | | |


### Response data: JSON string with the following elements

| Parameter | Type                          | Description                                                             |
|-----------|-------------------------------|-------------------------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR  | Operation result successful / execution error                               |
| data      | Array of App parameters                        |                            |

## Update App settings

> Request body (application/json):

```json
{
	"settings":{
        "option_1":true,
        "option_2":4
    }
}
```

> Response body (application/json):

```json
{
    "status": "SUCCESS",
    "message": "Successfully updated."
}
```
OR (if nothing changed)
```json
{
    "status": "SUCCESS",
    "message": "Not updated - no changes."
}
```

### HTTP Request

`POST /api/{APP_CODE}/app`

### Request data: JSON string with the following elements

| Parameter  | Type           | Description   |
|------------|----------------|---------------|
|  settings     |  Array of parameters with values|  param1: value, param2: value     |


### Response data: JSON string with the following elements

| Parameter | Type                          | Description                                                             |
|-----------|-------------------------------|-------------------------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR  | Operation result successful / execution error                               |
| data      | Array of App parameters                        |                            |

## Get timeline (for dashboard)
Returns a list of recent events by application (10 are saved)

> Response body (application/json):

```json
{
    "status": "SUCCESS",
    "data": [
        {
            "title": "9",
            "author": "Root Adminson",
            "event_type": "approved",
            "datetime": "1583324692"
        },
        {
            "title": "2019-08-27...2019-08-27 hours approved ",
            "author": "Root Adminson",
            "picture": "url_to_user_avatar",
            "event_type": "approved",
            "datetime": "1586330574"
        }
    ]
}
```

### HTTP Request

`POST /api/{APP_CODE}/timeline`

### Response data: JSON string with the following elements

| Parameter | Type                          | Description                                                             |
|-----------|-------------------------------|-------------------------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR  | Operation result successful / execution error                               |
| data      | Array of Event objects                        |                            |

## Get dashboard 
Returns a dashboard with generalized application information

> Request body (application/json):

```json
{
  "period":"this_month"
}
```

> Response body (application/json):

```json
{
    "status": "SUCCESS",
    "data": {
        "hours_clocked": {
            "current": 0,  
            "previous": 6.9, 
            "percent": -100  
        },
        "tasks": {
            "current": 0,
            "previous": 1,
            "percent": -100
        },
        "clients": {
            "current": 0,
            "previous": 1,
            "percent": -100
        },
        "average_session": {
            "current": 0,
            "previous": 1.4,
            "percent": -100
        },
        "revenue_projection": {
            "current": "$2K",
            "percent": -11.1
        }
    }
}

```

### HTTP Request

`POST /api/{APP_CODE}/dashboard`

### Request data: JSON string with the following elements

| Parameter  | Type           | Description   |
|------------|----------------|---------------|
|  period    |  String (*Required*)       |  from list: this_day, this_week, this_month, this_year, yesterday, last_week, last_month, last_year     |

### Response data: JSON string with the following elements

| Parameter | Type                          | Description                                                             |
|-----------|-------------------------------|-------------------------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR  | Operation result successful / execution error                               |
| data      | Array of data objects   |  Each object (exclude revenue_projection)  has current value, previous and difference in %                           |

## Get Tracking Trend (for dashboard)
Returns a dashboard with generalized application information

> Request body (application/json):

```json
{
  "period":"this_month"
}
```

> Response body (application/json):

```json
{
    "status": "SUCCESS",
    "data": {
        "1583283600": {  
            "pure": 4,  
            "dirty": 4  
        },
        "1583370000": {
            "pure": 0,
            "dirty": 1
        },
        "1583456400": {
            "pure": 1,
            "dirty": 1
        },
        "1583816400": {
            "pure": 1,
            "dirty": 1
        },
        "1583902800": {
            "pure": 0.8,
            "dirty": 1
        }
    }
}
```

```
Format data:
"1583283600": {  // timestamp
    "pure": 4,  // float. pure time i.e. minus breaks
    "dirty": 4  // float. dirty time i.e. without deduction of breaks
}
```

### HTTP Request

`POST /api/{APP_CODE}/tracking_trend`

### Request data: JSON string with the following elements

| Parameter  | Type           | Description   |
|------------|----------------|---------------|
|  period    |  String (*Required*)       |  from list: this_day, this_week, this_month, this_year, yesterday, last_week, last_month, last_year     |

### Response data: JSON string with the following elements

| Parameter | Type                          | Description                                                             |
|-----------|-------------------------------|-------------------------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR  | Operation result successful / execution error                               |
| data      | Array of data objects   |                            |

## Get Money Trend (for dashboard)
Returns data for the dashboard finance trend

> Request body (application/json):

```json
{
  "period":"this_year",
  "group":"daily"
}
```

> Response body (application/json):

```json
{
    "status": "SUCCESS",
    "data": {
        "revenue": {
            "data": [
                {
                    "amount": "20",
                    "year": "2020",
                    "month": "2", 
                    "day": "9"  
                },
                {
                    "amount": "10",
                    "year": "2020",
                    "month": "2",
                    "day": "10"
                },
                {
                    "amount": "30",
                    "year": "2020",
                    "month": "2",
                    "day": "11"
                },
                {
                    "amount": "10",
                    "year": "2020",
                    "month": "3",
                    "day": "10"
                }
            ],
            "average": 17.5   
        },
        "expediture": {
            "data": [
                {
                    "sum": "60",
                    "year": "2020",
                    "month": "2",
                    "day": "18"
                },
                {
                    "sum": "40",
                    "year": "2020",
                    "month": "2",
                    "day": "19"
                },
                {
                    "sum": "50",
                    "year": "2020",
                    "month": "3",
                    "day": "4"
                }
            ],
            "average": 50  
        }
    }
}
```

```
Format data:
"data": [
    {
        "amount": "20", // value
        "year": "2020",
        "month": "2", //may be absent - depending on grouping
        "day": "9"  //may be absent - depending on grouping
    }
],
"average": 17.5    //average for the period
```

### HTTP Request

`POST /api/{APP_CODE}/money_trend`

### Request data: JSON string with the following elements

| Parameter  | Type           | Description   |
|------------|----------------|---------------|
|  period    |  String (*Required*)       |  from list: this_day, this_week, this_month, this_year, yesterday, last_week, last_month, last_year     |
|  group     |  String  (*Required*)      |  Grouping. From list: daily, weekly, monthly, yearly     |

### Response data: JSON string with the following elements

| Parameter | Type                          | Description                                                             |
|-----------|-------------------------------|-------------------------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR  | Operation result successful / execution error                               |
| data      | Array of data objects   |                            |

## Import users from CSV

> Request body (multipart/form-data):

```
  "file":"file.csv",
  "field_mapping": {
        "first_name": 0,
        "last_name": 1,
        "email": 2
  }
```

> Response body (application/json):

```json
{
    "status": "SUCCESS",
    "message": "Import of users success.",
    "data": {
        "success": [
            {
                "email": "1@email.com",
                "first_name": "John",
                "last_name": "Smith"
            },
            {
                "email": "2@email.com",
                "first_name": "Will",
                "last_name": "Doe"
            }
],
        "fail": []
    }
}
```

### HTTP Request

`POST /api/{APP_CODE}/import_users_csv`

### Request data: multipart/form-data

| Parameter  | Type           | Description   |
|------------|----------------|---------------|
|  file      |  file (*Required*)       |      |
|  field_mapping      |  Array       |  Order of fields in file. Default: first_name => 0, last_name => 1, email => 2,    |

### Response data: JSON string with the following elements

| Parameter | Type                          | Description                                                             |
|-----------|-------------------------------|-------------------------------------------------------------------------|
| status    | String from list: SUCCESS, ERROR  | Operation result successful / execution error                               |
| message   | String                        | Error description or successful info message                               |
| data      | Array of data objects         |  success-imported users, fail-NOT imported                          |

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

> 200 Response body (application/json):

```json
{
    "status": "SUCCESS",
    "message": "Successfully found.",
    "data": {
        "id": 1,
        "name": "Group 1",
        "status": "active",
        "pay_rate": 10
    }
}
```

> 404 Not found. Response body (application/json):

```json
{
   "status": "ERROR",
    "message": "Group not found.",
    "error_code": 5
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
	"name":"nameTest"
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
| description | String     |             |
| notes     | String          |             |
| pay_rate  | Float           |             |
| status    | Boolean         |             |

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
    "name": "New_name",
    "description": "New_description"
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
    "message": "Group not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{app_path}/group/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter   | Type            | Description     |
|-------------|-----------------|-----------------|
| group_id    | Integer (*Required*) | Group ID        |
| name        | String (*Required ?*)  | Must be new name        |
| description | String (*Required ?*)          | OR new description |
| notes       | String          |                 |
| status      | String of list: active, inactive          |                 |

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
            "name": "Group 1",
            "status": "active",
            "pay_rate": 10
        }
    ],
    "count": 1
}
```

### HTTP Request

`POST /api/{app_path}/group/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements
Must be at least one of parameters: group_id, name, keyword

| Parameter | Type    | Description |
|-----------|---------|-------------|
| group_id  | Integer (*Required ?*)| Group ID    |
| name      | String (*Required ?*)  |             |
| keyword   | String (*Required ?*)  |  Find in name           |
| limit     | Integer |             |
| offset    | Integer |             |

### Response data: JSON string with the following elements

| Parameter | Type                                         | Description                                        |
|-----------|----------------------------------------------|----------------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                       | Error description or successful info message       |
| data      | Array of group objects packed to JSON string | Group objects array with all fields or empty array |
| count     | Integer                      |                                              |

## Add User to Group

> Request body (application/json):

```json
{
	"group_id":1,
	"user_id":2
}
```

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully added."
}
```

> 404 Not found. Response body (application/json):

```json
{
   "status": "ERROR",
    "message": "Group not found.",
    "error_code": 5
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

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Delete User from Group

> Request body (application/json):

```json
{
	"group_id":1,
	"user_id":2
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
    "message": "Group not found.",
    "error_code": 5
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

## Get Users in Group

> Request body (application/json):

```json
{
	"group_id":1
}
```

> 200 Response body (application/json):

```json
{
    "status": "SUCCESS",
    "message": "1 user(s) found.",
    "data": [
        {
            "id": 111,
            "email": "222@222.ru",
            "first_name": "222",
            "last_name": "2222",
            "hourly_rate": 0,
            "roles": ["ROLE_USER"],
            "created": "1591787712",
            "exported": "946684800",
            "updated": "1593163617",
            "activated": true,
            "settings": {
                "datetime_format": "d/m/Y h:ia"
            },
            "paid_hours": 0,
            "unapproved_hours": 4.01,
            "teams": [1],
            "birth_date": "1990-05-01"
        }
    ],
    "count": 1
}
```

> 404 Not found. Response body (application/json):

```json
{
   "status": "ERROR",
    "message": "Group not found.",
    "error_code": 5
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
| data      | Array of User Object         |                                              |
| count     | Integer                      |                                              |

## Delete Group

> Request body (application/json):

```json
{
	"group_id":1
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
   "message": "Group not found.",
   "error_code": 5
}
```

### HTTP Request

`POST /api/{app_path}/group/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type                   | Description |
|-----------|------------------------|-------------|
| group_id  | Integer (*Required*)   | Group ID    |

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
    "message": "Successfully found.",
    "data": {
        "id": 1,
        "due_date": "1587340800",
        "payed_date": "1587340800",
        "amount": 100,
        "status": "paid"
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
| data      | Object packed to JSON string | Invoice object with all fields or empty object  |

## Create New Invoice

> Request body (application/json):

```json
{
	"client_id":1,
	"task_ids":[1,2]
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
   "message": "Client not found.",
   "error_code": 5
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
| due_date  | Timestamp                     | Default = current Timestamp                   |
| status    | String of: new, paid          | Initial status. Default = 'new'       |
| amount    | Float                         |                                                 |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                      | ID of created invoice |

## Update Invoice

> Request body (application/json):

```json
{
	"invoice_id":1,
	"amount":30.5
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
   "message": "Invoice not found.",
   "error_code": 5
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
| status     | String of: new, paid         |             |
| amount     | Float           |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Set Invoice Status (Paid)
*Set payed this invoice and all his timeEntries. ADMIN_ACCESS_ONLY*

> Request body (application/json):

```json
{
	"invoice_id":2,
	"payed_date":1587081600
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
| payed_date | Timestamp (*Required*) |             |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Invoices

> Request body (application/json):

```json
{
	"client_id":[1]
}
```

> Response body (application/json):

```json
{
    "status": "SUCCESS",
    "message": "1 invoice(s) found.",
    "data": [
        {
            "id": 1,
            "due_date": "1587340800",
            "payed_date": "1587340800",
            "amount": 100,
            "status": "paid"
        }
    ],
    "count": 1
}
```

### HTTP Request

`POST /api/{app_path}/invoice/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| client_id | Array of Integer (*Required*) |             |
| due_date  | Timestamp       |  <= due_date           |
| payed_date| Timestamp       |  <= payed_date           |
| limit     | Integer             |             |
| offset    | Integer             |             |

### Response data: JSON string with the following elements

| Parameter | Type                                   | Description                                  |
|-----------|----------------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                 | Error description or successful info message |
| data      | Array of objects packed to JSON string | Invoice object with all fields or empty object  |
| count     | Integer                | Count of objects  |

## Search Invoices with Grouping

> Request body (application/json):

```json
{
    "client_id":[1],
    "group_by": "status",
    "order_by": "asc"
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
                "value": "paid",
                "count": 1,
                "items": [
                    {
                        "id": 1,
                        "due_date": "1587340800",
                        "payed_date": "1587340800",
                        "amount": 100,
                        "status": "paid"
                    }
                ]
            }
        ]
    },
    "count": 1
}
```

### HTTP Request

`POST /api/{app_path}/invoice/search_group`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| client_id | Array of Integer (*Required*) |             |
| due_date  | Timestamp       |  <= due_date           |
| payed_date| Timestamp       |  <= payed_date           |
| limit     | Integer             |             |
| offset    | Integer             |             |
| group_by  | String                              |   field for grouping          |
| order_by  | String from list: asc, desc     |  sorting order for grouped field          |  

### Response data: JSON string with the following elements

| Parameter | Type                                   | Description                                  |
|-----------|----------------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                                 | Error description or successful info message |
| data      | Array of groups of objects packed to JSON string | Invoice object with all fields or empty object  |
| count     | Integer                | Count of groups  |

## Delete Invoice

> Request body (application/json):

```json
{
	"invoice_id":1
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
   "message": "Invoice not found.",
   "error_code": 5
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

## Add comment to invoice

> Request body (application/json):

```json
{
  "invoice_id": 1,
  "content": "content text"
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "id": 1
}
```

> 404 Not found. Response body (application/json):

```json
{
   "status": "ERROR",
   "message": "Invoice not found.",
   "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/invoice/add_comment`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| invoice_id| Integer (*Required*) |                   |
| content   | String (*Required*)  |                   |



### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| id        | Integer                      | ID of created comment  |

## Get comments of invoice

> Request body (application/json):

```json
{
    "invoice_id": 1,
    "with": ["users"]
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "data": {
    "comments": [
      {
        "id": 0,
        "commentator": 1,
        "content": "Comment text",
        "date": 1566402943
      }
    ],
    "users": {
      "1": {
        "id": 1,
        "first_name": "John",
        "last_name": "Smith",
        "picture": "url_to_user_avatar"
      }
    }
  },
  "count": 1
}
```

> 404 Not found. Response body (application/json):

```json
{
   "status": "ERROR",
   "message": "Invoice not found.",
   "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/invoice/comments`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| invoice_id| Integer (*Required*) |                   |
| with      | Array of String  |   "users"             |



### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| data      | Array of Connemt object                      |  |
| count      | Integer                      | Count of finded comments |

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

## Add comment to task

> Request body (application/json):

```json
{
  "task_id": 1,
  "content": "content text"
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "id": 1
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

`POST /api/{APP_CODE}/task/add_comment`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| task_id| Integer (*Required*) |                   |
| content   | String (*Required*)  |                   |



### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| id        | Integer                      | ID of created comment  |

## Get comments of task

> Request body (application/json):

```json
{
    "task_id": 1,
    "with": ["users"]
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "data": {
    "comments": [
      {
        "id": 0,
        "commentator": 1,
        "content": "Comment text",
        "date": 1566402943
      }
    ],
    "users": {
      "1": {
        "id": 1,
        "first_name": "John",
        "last_name": "Smith",
        "picture": "url_to_user_avatar"
      }
    }
  },
  "count": 1
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

`POST /api/{APP_CODE}/task/comments`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| task_id   | Integer (*Required*) |                   |
| with      | Array of String  |   "users"             |



### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| data      | Array of Comment object                      |  |
| count      | Integer                      | Count of finded comments |

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

## **SCHEDULE**

## Get Schedule Info

> Request body (application/json):

```json
{
  "shedule_id": 1
}
```

> Response body (application/json):

```json
{
  "status": "SUCCESS",
  "data": {
    "id": 1,
    "name": "Schedule_name",
    "days_of_week": {
      "monday": true,
      "tuesday": true,
      "wednesday": true,
      "thursday": true,
      "friday": true,
      "saturday": true,
      "sunday": true
    },
    "start_time": "10:00",
    "end_time": "19:59",
    "is_holiday": true,
    "is_overnight": true
  }
}
```

### HTTP Request

`POST /api/{APP_CODE}/schedule`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter  | Type            | Description |
|------------|-----------------|-------------|
| schedule_id | Integer (*Required*) | Schedule ID  |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| data      | Object packed to JSON string | Schedule object with all fields or empty object  |

## Create New Schedule

> Request body (application/json):

```json
{
  "name": "Name of schedule",
  "days_of_week": {
    "monday": true,
    "tuesday": true,
    "wednesday": true,
    "thursday": true,
    "friday": true,
    "saturday": true,
    "sunday": true
  },
  "start_time": "10:00",
  "end_time": "19:59",
  "is_holiday": true,
  "is_overnight": true
}
```

> Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "Successfully created.",
  "id": 0
}
```

### HTTP Request

`POST /api/{APP_CODE}/schedule/create`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter          | Type                                             | Description |
|--------------------|--------------------------------------------------|-------------|
| name               | String (*Required*)                              |             |
| days_of_week       | Array of days (Boolean) (*Required*)             |  "monday": Boolean,"tuesday": Boolean,"wednesday": Boolean,"thursday": Boolean,"friday": Boolean,"saturday": Boolean,"sunday": Boolean           |
| start_time         | String (*Required*)                              |   (HH:mm)   |
| end_time           | String (*Required*)                              |   (HH:mm)   |
| is_holiday         | Boolean  (*Required*)                            |             |
| is_overnight       | Boolean  (*Required*)                            |             |


### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| id        | Integer                          | New Schedule id                               |

## Update Schedule

> Request body (application/json):

```json
{
  "schedule_id": 1,
  "name": "Name of schedule",
  "days_of_week": {
    "monday": true,
    "tuesday": true,
    "wednesday": true,
    "thursday": true,
    "friday": true,
    "saturday": true,
    "sunday": true
  },
  "start_time": "10:00",
  "end_time": "19:59",
  "is_holiday": true,
  "is_overnight": true
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
    "message": "Schedule not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/schedule/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter          | Type                                             | Description |
|--------------------|--------------------------------------------------|-------------|
| schedule_id        | Integer (*Required*)                              |             |
| name               | String                                            |             |
| days_of_week       | Array of days (Boolean)             |  "monday": Boolean,"tuesday": Boolean,"wednesday": Boolean,"thursday": Boolean,"friday": Boolean,"saturday": Boolean,"sunday": Boolean           |
| start_time         | String                                           |   (HH:mm)   |
| end_time           | String                                          |   (HH:mm)   |
| is_holiday         | Boolean                                        |             |
| is_overnight       | Boolean                                    |             |


### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |

## Search Schedules

> Request body (application/json):

```json
{
  "name": "test"
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "message": "1 schedule(s) found.",
  "data": [
    {
      "id": 0,
      "name": "test name",
      "days_of_week": {
        "monday": true,
        "tuesday": true,
        "wednesday": true,
        "thursday": true,
        "friday": true,
        "saturday": true,
        "sunday": true
      },
      "start_time": "10:00",
      "end_time": "19:59",
      "is_holiday": true,
      "is_overnight": true
    }
  ],
  "count": 1
}
```


### HTTP Request

`POST /api/{APP_CODE}/schedule/search`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements
Must be at least one of parameters: name, keyword

| Parameter          | Type                                             | Description |
|--------------------|--------------------------------------------------|-------------|
| name               | String  (*Required ?*)                      |             |
| keyword            | String  (*Required ?*)                         | search in name      |
| limit              | Integer                    |                    |
| offset             | Integer                     |                   |


### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Array of objects packed to JSON string | Schedule objects with all fields or empty        |
| count     | Integer                               | Count of finded objects        |

## Delete Schedule

> Request body (application/json):

```json
{
  "schedule_id": 1
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
    "message": "Schedule not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/schedule/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter          | Type                                             | Description |
|--------------------|--------------------------------------------------|-------------|
| schedule_id        | Integer (*Required*)                              |             |


### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |


## <ins>**RATE**</ins>

Rate API for iBuildApp Timesheets, handling getting rate information, creating/deleting/updating a rate, and searching for rates.

## Get Rate Info

> Request body (application/json):

```json
{
	"rate_id": 1
}
```

> 200 Response body (application/json):

```json
{
    "status": "SUCCESS",
    "data": {
        "id": 1,
        "name": "test_rate",
        "schedule_id": 1,
        "user_id": 1,
        "amount": 100,
        "duration_unit": "days"
    }
}
```

> 404 Not found. Response body (application/json):

```json
{
   "status": "ERROR",
    "message": "Rate not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/rate`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type            | Description |
|-----------|-----------------|-------------|
| rate_id   | Integer (*Required*) | ID Rate         |

### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| message   | String                       | Error description or successful info message |
| data      | Object packed to JSON string | Rate object with all fields or empty object  |

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

> 200 Response body (application/json):

```json
{
   "status": "SUCCESS",
   "message": "Successfully created.",
   "id": 1
}
```

> 404 Not found. Response body (application/json):

```json
{
   "status": "ERROR",
    "message": "User not found.",
    "error_code": 5
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
| amount        | Float                                             |             |
| duration_unit | String from the list: minutes, hours, days, weeks, months, years (*Required*) |             |

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

> 404 Not found. Response body (application/json):

```json
{
   "status": "ERROR",
    "message": "User not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/rate/update`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter     | Type                                             | Description |
|---------------|--------------------------------------------------|-------------|
| rate_id       | Integer (*Required*)                             |             |
| user_id       | Integer                                          |             |
| schedule_id   | Integer                                          |             |
| name          | String                                           |             |
| amount        | Float                                            |             |
| duration_unit | String from the list: minutes, hours, days, weeks, months, years |             |

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
          "schedule_id": 1,
          "user_id": 1,
          "name": "test_rate",
          "amount": 100,
          "duration_unit": "days"
      }
   ],
  "count": 1
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
| count      | Integer                      | Count of finded objects |

## Delete Rate

> Request body (application/json):

```json
{
	"rate_id": 1
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
    "message": "Rate not found.",
    "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/rate/delete`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description |
|-----------|---------|-------------|
| rate_id   | Integer (*Required*)|             |

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

## Export Time Entries

> Request body (application/json):

```json
{
  "time_entry_ids": [0, 10],
  "with_headers": true
}
```

> 200 Response body (text/csv):


### HTTP Request

`POST /api/{app_path}/ts/export`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter      | Type                                | Description         |
|----------------|-------------------------------------|---------------------|
| time_entry_ids | Array[Integer]                      |  TimeEntry IDs      |
| task_id        | Integer                             |                     |
| user_id        | Integer                             |                     |
| status         | String from list: in-progress, rejected, approved         |
| with_headers   | Boolean                             |  Include headers to csv ? default = false |



### Response data: file

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| file      | text/csv                     |              |

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

## Add comment to Time Entry

> Request body (application/json):

```json
{
  "time_entry_id": 1,
  "content": "content text"
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "id": 1
}
```

> 404 Not found. Response body (application/json):

```json
{
   "status": "ERROR",
   "message": "Time Entry not found.",
   "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/ts/add_comment`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| time_entry_id| Integer (*Required*) |                   |
| content   | String (*Required*)  |                   |



### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| id        | Integer                      | ID of created comment  |

## Get comments of Time Entry

> Request body (application/json):

```json
{
    "time_entry_id": 1,
    "with": ["users"]
}
```

> 200 Response body (application/json):

```json
{
  "status": "SUCCESS",
  "data": {
    "comments": [
      {
        "id": 0,
        "commentator": 1,
        "content": "Comment text",
        "date": 1566402943
      }
    ],
    "users": {
      "1": {
        "id": 1,
        "first_name": "John",
        "last_name": "Smith",
        "picture": "url_to_user_avatar"
      }
    }
  },
  "count": 1
}
```

> 404 Not found. Response body (application/json):

```json
{
   "status": "ERROR",
   "message": "Time Entry not found.",
   "error_code": 5
}
```

### HTTP Request

`POST /api/{APP_CODE}/ts/comments`

HEADER: “Authorization” = Bearer {token}  (Auth token. String. Required)

### Request data: JSON string with the following elements

| Parameter | Type    | Description                    |
|-----------|---------|--------------------------------|
| time_entry_id   | Integer (*Required*) |                   |
| with      | Array of String  |   "users"             |



### Response data: JSON string with the following elements

| Parameter | Type                         | Description                                  |
|-----------|------------------------------|----------------------------------------------|
| status    | String from list: SUCCESS, ERROR | Operation result successful / execution error    |
| data      | Array of Comment object                      |  |
| count      | Integer                      | Count of finded comments |

## <ins>**Changelog**</ins>
