# Authentication related
## Exchange administrator Token

### **A brief description**

   AppServer obtains or refreshes the administrator token by calling (auth/user_token) to obtain super permissions. When calling all the following APIs, the administrator token must be obtained and set to the request header (key is token). Unless otherwise specified, the request method is always POST

### **Request URL**

- `http://x.x.x.x:10002/auth/user_token`


### **Request method**

  - `POST`

### **Request Example**

   ```json
{
     "secret": "tuoyun",
     "platform": 1,
     "userID": "openIM123456",
     "operationID": "asdfasdfsadf"
}
   ```

### **Request parameters**

| parameter name | type | required | description |
| :---------: | ------ | :--: | :------------------------ -------------------------------------- |
| secret | string | yes | OpenIM secret key |
| platform | int | yes | administrator fills in 8 |
| userID | string | yes | administrator userID, the userID here must be one of the appManagerUids in the configuration file config/config.yaml |
| operationID | string | yes | |

### **Back Example**

    ```json
{
     "errCode": 0,
     "errMsg": "",
     "data": {
         "userID": "openIM123456",
         "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVSUQiOiJvcGVuSU0xMjM0NTYiLCJQbGF0Zm9ybSI6IklPUyIsImV4cCI6MTY0NjI5Mzk5NCwibmJmIjoxNjQ1Njg5MTk0LCJp YXQiOjE2NDU2ODkxOTR9.cHEgLEHyzC1bmHoPjz7chYqIanfodeLT6LmQhPDeGPA",
         "expiredTime": 1646293994
     }
}
    ```

### **Return parameters**

| parameter name | type | description |
| :---------- | :----- | --------------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| userID | string | user ID |
| token | string | user token |
| expiredTime | int | token expiration timestamp (seconds) |

# group related

## create group

### **A brief description**

- APP administrators create groups

### **Request URL**

- `http://x.x.x.x:10000/group/create_group`

### **Request data mode**

- POST

### **Request Example**

```
{
     "ownerUserID": "19828950910",
     "groupType": 0,
     "groupName": "test group name",
     "notification": "",
     "introduction": "",
     "faceURL": "",
     "ex": "",
     "operationID": "sdadfsdfssdfasdafvcdxsdafdsfsdfaa",
     "memberList": [
         {
             "userID": "18666662412",
             "roleLevel": 1
         }
     ]
}
```

### **Request parameters**

| Parameter name | Mandatory | Description |
| :---------- | :--- | -------------- |
| memberList | yes | specify initial group members |
| ownerUserID | Yes | Group owner UserID |
| operationID | is | operation ID |

### **Back Example**

```
{
     "errCode": 0,
     "errMsg": "",
     "data": {
         "creatorUserID": "openIM123456",
         "groupID": "f69e9aae6edb2c86b3380b7b0125e579",
         "groupName": "test group name",
         "memberCount": 2,
         "ownerUserID": "openIM123456"
     }
}
```

### **Return parameter**

| parameter name | type | description |
| :------------ | :------- | -------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| creatorUserID | json object | creator userID, |
| groupID | string | group ID |
| groupName | string | group name |
| memberCount | int | Number of group members |
| ownerUserID | string | Group owner UserID |

## Invite into the group

### **A brief description**

- APP administrators invite users to directly join the group

### **Request URL**

- `http://x.x.x.x:10000/group/invite_user_to_group`

### **Request method**

- POST

### **Request Example**

```
{
     "groupID": "f69e9aae6edb2c86b3380b7b0125e579",
     "operationID": "341323253454352",
     "invitedUserIDList": [
         "18349115126"
     ],
     "reason": "hello"
}
```

### **Request parameters**

| parameter name | type | required | description |
| :---------------- | -------- | :--- | ----------------- ----- |
| groupID | string | yes | group ID |
| invitedUserIDList | json array | yes | list of userIDs invited into the group |
| reason | string | No | The reason for joining the group |
| operationID | string | yes | operation ID |

### **Back Example**

```
{
     "errCode": 0,
     "errMsg": "",
     "data": [
         {
             "userID": "18349115126",
             "result": 0
         }
     ]
}
```

### **Return parameters**

| parameter name | type | description |
| :------- | :----- | -------------------------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| userID | string | UserID invited into the group |
| result | int | operation result 0 means success, other means failure |

## kick the user out of the group

### **A brief description**

- The APP administrator directly kicks the user out of the group

### **Request URL**

- `http://x.x.x.x:10000/group/kick_group`

### **Request method**

- POST

### ** Request Example **

```
{
     "groupID": "f69e9aae6edb2c86b3380b7b0125e579",
     "operationID": "dasdavdsadsfasdf",
      "reason": "kkk",
"kickedUserIDList": [
         "18666662412"
     ]
   
}
```

### **Request parameters**

| parameter name | type | required | description |
| :--------------- | -------- | :--- | -------------- |
| groupID | string | yes | group ID |
| kickedUserIDList | json array | yes | kicked UserID list |
| reason | string | No | The reason for being kicked |
| operationID | string | yes | operation ID |

### **Back Example**

```
{
     "errCode": 0,
     "errMsg": "",
     "data": [
         {
             "userID": "18666662412",
             "result": 0
             }
     ]
}
```

### **Return parameters**

| parameter name | type | description |
| :------- | :----- | -------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| userID | string | kicked UserID |
| result | int | 0 for success, non-zero for failure |

# Friends related

## import friends

### **A brief description**

- The APP administrator makes a user (fromUserID) and other users (friendUserIDList) as friends

### **Request URL**

- `http://x.x.x.x:10000/friend/import_friend`

### **Request method**

- POST

### ** Request Example **

```
{
     "friendUserIDList": [
         "21979710c3fe454d",
         "3434344"
     ],
     "operationID": "1111111222",
     "fromUserID": "f732156059eeb5d0"
}
```

### **Request parameters**

| parameter name | type | required | description |
| :--------------- | ------ | :--- | -------------------- ------ |
| friendUserIDList | string | yes | UserID list to establish friend relationship |
| operationID | string | yes | operation ID |
| fromUserID | string | is | a UserID |

### **Back Example**

```
{
     "errCode": 0,
     "errMsg": "",
     "data": [
         {
             "userID": "12121",
             "result": -1
         },
         {
             "userID": "23465",
             "result": 0
         }
     ]
}
```

### **Return parameters**

| parameter name | type | description |
| :------- | :----- | ---------------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| userID | string | userID to establish friendship |
| result | int | 0 means success, -1 means failure |

# Blacklist related

## add to blacklist

### **A brief description**

- The APP administrator adds toUserID to the blacklist of fromUserID, that is, fromUserID blacklists toUserID

### **Request URL**

- `http://x.x.x.x:10000/friend/add_black`

### **Request method**

- POST

### request example

```
{
     "toUserID": "21979710c3fe454d",
     "operationID": "1111111222",
     "fromUserID": "f732156059eeb5d0"
}
```

### **Request parameters**

| parameter name | type | required | description |
| :---------- | :----- | ---- | ----------------------- ----------------------------- |
| toUserID | string | Yes | The blocked UserID, add this user to the blacklist of fromUserID |
| operationID | string | yes | operation ID |
| fromUserID | string | Yes | To blacklist someone else's UserID, add toUserID to this user's blacklist |

### **Back Example**

```
{
     "errCode": 0,
     "errMsg": ""
}
```

### **Return parameters**

| parameter name | type | description |
| :------- | :----- | -------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |

# user related

## Update user information

### **A brief description**

APP administrator updates user information

### **Request URL**

- `http://x.x.x.x:10000/user/update_user_info`

### **Request method**

- POST

### request example

```
{
     "userID": "21979710c3fe454d",
     "operationID": "1111111222",
     "nickname": "sssssssssskkkkk",
     "faceURL": "",
     "gender": 1,
     "phoneNumber": "",
     "birth": 167762763,
     "email": "",
     "ex": ""
}
```

### **Request parameters**

| parameter name | type | required | description |
| :---------- | :----- | :--- | ----------------------- ------------------------ |
| operationID | string | yes | operation ID, keep unique, it is recommended to use the current time in microseconds + random number |
| nickname | string | No | User nickname or group nickname |
| faceURL | string | No | User avatar or group avatar url, understood according to the context |
| gender | int | No | User gender 1 means male, 2 means female |
| phoneNumber | string | No | The user's mobile phone number, including the region, (such as Hong Kong: +852-xxxxxxxx), |
| birth | int | no | user birthday, Unix timestamp (seconds) |
| email | string | no | email address |
| ex | string | No | User extension information |
| userID | string | Yes | User ID, must be unique in IM |

### **Back Example**

```
{
     "errCode": 0,
     "errMsg": ""
}
```

### **Return parameters**

| parameter name | type | description |
| :------- | :----- | -------------- |
| errCode | int | 0 for success, non-zero for failure |
| errCode | string | error message |

## delete users

### **A brief description**

  - The administrator invokes the delete IM user interface.

### **Request URL**


  - `http://x.x.x.x:10000/manager/delete_user`


### **Request method**


  - `POST`

### **Request Example**

   ```json
{
     "operationID": "1111111222",
     "deleteUserIDList": [
         "123",
         "343",
         "456"
     ]
}
   ```

### **Request parameters**

| parameter name | type | required | description |
| :--------------: | :------: | :--: | :-------------- ------------------------- |
| operationID | string | yes | operation ID, keep unique, it is recommended to use the current time in microseconds + random number |
| deleteUserIDList | json array | yes | userID array list to be deleted |


### **Back Example**

   ```json
{
     "errCode": 0,
     "errMsg": "",
     "data": [
         "122",
         "466"
     ]
}
   ```

### **Return parameter**

| parameter name | type | description |
| :------- | :------- | -------------------- |
| errMsg | string | error message |
| errCode | int | 0 for success, non-zero for failure |
| data | json array | failed to delete user UserID |

## Get user details

### **A brief description**

  - The administrator calls the Get User Details interface to get the details of multiple user registrations.

### **Request URL**


  - `http://x.x.x.x:10000/user/get_users_info`


### **Request method**


  - `POST`

### **Request Example**

   ```json
{
     "operationID": "1111111222",
     "userIDList": [
         "123",
         "3434"
     ]
}
   ```

### **Request parameters**

| parameter name | type | required | description