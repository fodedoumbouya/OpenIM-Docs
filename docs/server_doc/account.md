## Register a new user

### **A brief description**

 -Apserver calls this new user in the Openim system when the user registers a new account.

### ** Request URL **


 - `http://x.x.x.x:10002/auth/user_register`

### ** Request **


 - `POST`

### ** Request example **

  ```json
 {
    "secret": "tuoyun",
    "platform": 1,
    "userID": "openIM1111",
    "nickname": "2222",
    "faceURL": "https://oss.com.cn/head", 
    "gender": 1,
    "phoneNumber": "18666667777",
    "birth":1640692941,
    "email": "xxxx@qq.com",
    "ex": "xxx",
    "operationID": "123111111"
}
  ```
### ** Request parameter **

| Parameter name | Type | Must -choose | Description     |
| :---------: | :----: | ---- | :----------------- |
|   secret    | string |Yes | Openim secrets |
|  platform   |  int   | Yes | Platform type of user registration |
|   userID    | string | Yes | User ID |
|  nickname   | string | Yes | User Nickname |
|   faceURL   | string | No | User avatar URL |
|   gender    |  int   | No | User Gender |
| phoneNumber | string | No | User mobile phone number |
|    birth    |  int   | No | User Birthday |
|    email    | string | No | Email address |
|     ex      | string | No | Extension field |
| operationID | string |Yes | Operation ID |
### ** Return to Example **

  ```json
{
    "errCode": 0,
    "errMsg": "",
    "data": {
        "userID": "openIM1111",
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVSUQiOiJvcGVuSU0xMTExIiwiUGxhdGZvcm0iOiJJT1MiLCJleHAiOjE2NDYyODAxMDQsIm5iZiI6MTY0NTY3NTMwNCwiaWF0IjoxNjQ1Njc1MzA0fQ.xhqmRBC3XpMwMQL2i3sRh6JArRZg1PFjFjRl9N1Kc9o",
        "expiredTime": 1646280104
    }
}
  ```
### ** Return to parameters **

| Parameter name | Type | Description |
|: ----------- |: ---------------------------------------------------------------------------------
| Errcode | int | 0 Success, non -0 failure |
| Errmsg | String | Error Information |
| USERID | String | User ID |
| Token | String | User Token |
| Expiredtime | int | Token Expired Time Stamp (second) |

## Get user token

### **A brief description**

AppServer calls this interface to get token. IMSDK needs to be introduced to the token when login

### ** Request URL **

-  ` http://x.x.x.x:10002/auth/user_token`


### ** Request **

 -  `POST`

### ** Request example **

  ```json
 {
    "secret": "tuoyun",
    "platform": 1,
    "userID": "openIM1111",
    "operationID": "asdfasdfsadf"
}
  ```

### ** Request parameter **

| Parameter name | Type | Must -choose | Description |
| :---------: | ------ | :--: | :----------------- |
|   secret    | string | Yes | Openim secrets |
|  platform   | int    | Yes | Platform type of user login |
|   userID    | string |  Yes | User ID |
| operationID | string | Yes | Operation ID |

### ** Return to Example **

   ```json
{
    "errCode": 0,
    "errMsg": "",
    "data": {
        "userID": "openIM1111",
        "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJVSUQiOiJvcGVuSU0xMTExIiwiUGxhdGZvcm0iOiJJT1MiLCJleHAiOjE2NDYyOTM2NTksIm5iZiI6MTY0NTY4ODg1OSwiaWF0IjoxNjQ1Njg4ODU5fQ.C5v6RS6yAPh0-4ZeQHmKon1rwC2GmZfc09xYoi67SOM",
        "expiredTime": 1646293659
    }
}
   ```

### ** Return to parameters **

| Parameter name | Type | Description |
| :---------- | :----- | --------------------- |
| errCode     | int    | 0Success, non -0 failure |
| errMsg      | string | Error message              |
| userID      | string | User ID                |
| token       | string | User token             |
| expiredTime | int    | Token Expired Time Stamp (second) |

