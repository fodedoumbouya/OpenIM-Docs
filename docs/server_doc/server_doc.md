# **Server API**

The server-side API is a series of HTTP(s) interfaces provided by the OpenIM backend, which are used for connecting business systems and IM. For example, import friends, create groups, send messages, etc. as an APP administrator.

## **Register New User**

### **A brief description**

 - User registration calls AppServer, and AppServer calls IMServer (auth/user_register) to create a new user. Administrators need to call the registration interface to obtain their own administrator token, and then use the administrator token to call other server APIs

### **Request URL**


 - `http://x.x.x.x:10002/auth/user_register`


### **Request method**


 - `POST`

### **Request Example**

  ```json
 {
  "secret": "tuoyun", 
  "platform": 1, 
  "uid": "d5645454517", 
  "name": "Zhang San",
  "icon": "https:oss.com.cn/head", 
  "gender": 1, 
  "mobile": "17812457845", 
  "birth": "1998.12.15", 
  "email": "xxxx@qq.com", 
  "ex": "xxx"
 }
  ```

### **Request parameters**

| parameter name | required | type | description |
| :------: | :--: | :----: | :----------------------------------------------------------- |
| secret | is | string | The secret key used by AppServer to request IMToken, the maximum length is 32 characters, the secret key must be consistent between AppServer and IMServer, secret leakage is risky, it is best to save it on the user server side |
| platform | is | int | platform type iOS 1, Android 2, Windows 3, OSX 4, WEB 5, applet 6, linux 7 |
| uid | is | string | user ID, the maximum length is 64 characters, and must be unique within an APP. If it is an administrator registration, it needs to be consistent with the appmanageruid in the IM server configuration config.yaml. You can modify the appmanageruid in the configuration file by yourself |
| name | is | string | user nickname, the maximum length is 64 characters, it can be set as an empty string |
| icon | No | string | User avatar, the maximum length is 1024 bytes, which can be set as an empty string |
| gender | No | int | User gender, 0 means unknown, 1 means male, 2 female means female, other parameter errors will be reported |
| mobile | No | string | user mobile, the maximum length is 32 characters, the country code (such as the United States: +1-xxxxxxxxxx) or area code (such as Hong Kong: +852-xxxxxxxxx) is required for non-Chinese mainland mobile phone numbers, which can be set to empty string |
| birth | no | string | user's birthday, the maximum length is 16 characters, can be set as an empty string |
| email | No | string | User email, the maximum length is 64 characters, it can be set as an empty string |
| ex | No | string | The user card extension field, the maximum length is 1024 characters, the user can expand it by himself, it is recommended to encapsulate it into a JSON string, or it can be set to an empty string |

### **Back Example**

  ```json
{
  "errCode": 0, 
  "errMsg": "", 
  "data": {
      "uid": "", 
      "token": "", 
      "expiredTime": 0
  }
}
  ```

  ### **Return parameters**

| parameter name | type | description |
| :---------- | :----- | --------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| uid | string | Uid of registered user |
| token | string | Generated user token |
| expiredTime | int | expired timestamp |

## **Exchange for IMToken**

### **A brief description**

  AppServer creates or refreshes the token by calling IMServer (auth/user_token). AppClientSDK needs to fill in the token when connecting to IMServer, and IMServer will verify the validity of the token.

### **Request URL**

-  ` http://x.x.x.x:10002/auth/user_token`


### **Request method**

 -  `POST`

  ### **Request Example**

  ```json
 {
  "secret": "tuoyun", 
  "platform": 1, 
  "uid": "d5645454517"
 }
  ```

### **Request parameters**

| parameter name | required | type | description |
| :------: | :--: | :----: | :----------------------------------------------------------- |
| secret | is | string | The secret key used by AppServer to request IMToken, the maximum length is 32 characters, the secret key must be consistent between AppServer and IMServer, secret leakage is risky, it is best to save it on the user server side |
| platform | is | int | platform type iOS 1, Android 2, Windows 3, OSX 4, WEB 5, applet 6, linux 7 |
| uid | is | string | user ID, the maximum length is 64 characters, and it must be unique within an APP. If you are an administrator, you need to fill in the same appmanageruid as the IM server configuration config.yaml, and you can modify the appmanageruid in the configuration file by yourself |

### **Back Example**

   ```json
 {
 "errCode": 0, 
 "errMsg": "", 
 "data": {
     "uid": "", 
     "token": "", 
     "expiredTime": 0
 }
}
   ```

   ### **Return parameters**

| parameter name | type | description |
| :---------- | :----- | --------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| uid | string | Uid of registered user |
| token | string | Generated user token |
| expiredTime | int | expired timestamp |

## create group

### **A brief description**

- The APP administrator creates a group and needs to designate the group owner

### **Request URL**

- `http://x.x.x.x:10002/group/create_group`

### **Request data method**

- POST

### **Request Example**

```
 {
    "memberList":[{"uid":"21979710c3fe454d","setRole":1},{"uid":"89b8924ea455a642","setRole":2}],
    "groupName":"groupName_on10",
    "introduction": "Introduction",
    "notification":"公告",
    "faceUrl":"url",
    "operationID":"1"
}
```

### **Request parameters**

| parameter name | required | type | description |
| :----------- | :--- | :----- | ------------------------------------------------------------ |
| memberList | Yes | Array | Specify the initial group member setRole 0 is an ordinary member 1 is a group owner 2 is an administrator |
| groupName | is | string | the name of the group chat |
| introduction | no | string | group introduction |
| notification | No | string | Group announcement |
| faceUrl | No | string | Group Avatar |
| token | is | string | obtained from the header, this token must be generated by calling auth/user_token as an APP administrator |
| operationID | is | string | operation id, use a random string |

### **Back Example**

```
 {
    "errCode": 0,
    "errMsg": "",
    "data": {
        "groupID": "05dc84b52829e82242a710ecf999c72c"
    }
}
```

### **Return parameter**

| parameter name | type | description |
| :------ | :------- | -------------------------------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| data | json object | return group creation result groupID is the successfully created group id |



## Invite into the group

### **A brief description**

- APP administrators invite users to directly join the group

### **Request URL**

- `http://x.x.x.x:10002/group/invite_user_to_group`

### **Request method**

- POST

### **Request Example**

```
{
	"groupID": "f21f9f84c14f3a978352ff339f1a800a",
	"uidList": [
		 "f732156059eeb5d0"
	],
	"reason": "reason",
	"operationID": "1111111111111 "
}
```

### **Request parameters**

| parameter name | required | type | description |
| :---------- | :--- | :------- | ------------------------------------------------------------ |
| groupID | is | string | group id |
| uidList | is | json array | uid list of users invited to the group |
| reason | is | string | reason for joining the group |
| token | is | string | obtained from the header, this token must be generated by calling auth/user_token as an APP administrator |
| operationID | is | string | operation id, use a random string |

### **Back Example**

```
{
    "errCode": 0,
    "errMsg": "ok",
    "data": [
        {
            "uid": "f732156059eeb5d0",
            "result": 0
        }
    ]
}
```

### **Return parameter**

| parameter name | type | description |
| :------ | :------- | --------------------------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| data | json object | uid of the invited user, result 0 means success |



## kick the user out of the group

### **A brief description**

- The APP administrator directly kicks the user out of the group

### **Request URL**

- `http://x.x.x.x:10002/group/kick_group`

### **Request method**

- POST

### **Request Example**

```
{
	"groupID": "56d274fad86685be3ea4ee70498eca61",
	"uidListInfo": [{
		"userId": "21979710c3fe454d"
	}],
	"operationID": "1111111111111 "
}
```

### **Request parameters**

| parameter name | required | type | description |
| :---------- | :--- | :------- | ------------------------------------------------------------ |
| groupID | is | string | group id |
| uidListInfo | is | json array | kicked user id list |
| token | is | string | obtained from the header, this token must be generated by calling auth/user_token as an APP administrator |
| operationID | is | string | operation id, use a random string |

### **Back Example**

```
{
    "errCode": 0,
    "errMsg": "",
    "data": [
        {
            "uid": "21979710c3fe454d",
            "result": 0
        }
    ]
}
```

### **Return parameter**

| parameter name | type | description |
| :------ | :------- | ------------------------------------ |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| data | json object | uid: kicked user uid, result: 0 means success |



## import friends

### **A brief description**

- The APP administrator makes users A and B friends

### **Request URL**

- `http://x.x.x.x:10002/friend/import_friend`

### **Request method**

- POST

### **Request Example**

```
{
	"uid":"21979710c3fe454d",
	"operationID": "1111111222",
	"ownerUid" : "f732156059eeb5d0"
}
```

### **Request parameters**

| parameter name | required | type | description |
| :---------- | :--- | :----- | ------------------------------------------------------------ |
| uid | is | string | A user uid |
| operationID | is | string | operation id, use a random string |
| ownerUid | is | string | B user uid |
| token | is | string | obtained from the header, this token must be generated by calling auth/user_token as an APP administrator |

### **Back Example**

```
{
    "errCode": 0,
    "errMsg": ""
}
```

### **Return parameter description**

| parameter name | type | description |
| :------ | :----- | -------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |



## add to blacklist

### **A brief description**

- APP administrator package uid is added to the blacklist of ownerUid

### **Request URL**

- `http://x.x.x.x:10002/friend/add_blacklist`

### **Request method**

- POST

### request example

```
{	"uid":"21979710c3fe454d",	"operationID": "1111111222",	"ownerUid" : "f732156059eeb5d0"}
```

### **Request parameters**

| parameter name | required | type | description |
| :---------- | :--- | :----- | ------------------------------------------------------------ |
| uid | is | string | blocked user id |
| operationID | is | string | operation id, use a random string |
| token | is | string | obtained from the header, this token must be generated by calling auth/user_token as an APP administrator |
| ownerUid | | | user id |

### **Back Example**

```
{
    "errCode": 0,
    "errMsg": ""
}
```

### **Return parameter**

| parameter name | type | description |
| :------ | :----- | -------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |



## Update user information

### **A brief description**

APP administrator updates user information

### **Request URL**

- `http://x.x.x.x:10002/user/update_user_info`

### **Request method**

- POST

### request example

```
{
	"uid":"21979710c3fe454d",
	"operationID": "1111111222",
	"name":"sssssssssskkkkk"
}
```

### **Request parameters**

| parameter name | required | type | description |
| :---------- | :--- | :----- | ------------------------------------------------------------ |
| token | is | string | obtained from the header, this token must be generated by calling auth/user_token as an APP administrator |
| operationID | is | string | operation id, use a random string |
| name | No | string | User nickname |
| icon | No | string | Avatar URL |
| gender | No | int | Gender, 0 unknown, 1 male, 2 female |
| mobile | No | string | User mobile number |
| birth | no | string | user's birthday |
| email | no | string | user email |
| ex | No | string | User extension information |
| uid | is | string | updated user id |

### **Back Example**

```
{
    "errCode": 0,
    "errMsg": ""
}
```

### **Return parameter**

| parameter name | type | description |
| :------ | :----- | -------------- |
| errCode | int | 0 for success, non-zero for failure |
| errCode | string | error message |

## **delete users**

### **A brief description**

 - The administrator invokes the delete IM user interface.

### **Request URL**


 - `http://x.x.x.x:10002/manager/delete_user`


### **Request method**


 - `POST`

### **Request Example**

  ```json
 {
    "operationID": "1111111222",
    "deleteUidList": [
        "123", 
        "343", 
        "456"
    ]
}
  ```

### **Request parameters**

| parameter name | required | type | description |
| :-----------: | :--: | :------: | :----------------------------------------------------------- |
| operationID | is | string | operation id, use a random string |
| token | is | string | Note: placed in the header of the POST request, this token must be generated by calling auth/user_token as an APP administrator |
| deleteUidList | is | json array | uid array list that needs to be deleted |


### **Back Example**

  ```json
{
    "errCode": 0, 
    "errMsg": "", 
    "failedUidList": [
        "122", 
        "466"
    ]
}
  ```

### **Return parameter**

| parameter name | type | description |
| :------------ | :------- | ----------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| failedUidList | json array | delete failed user Uid |

## **Get user details**

### **A brief description**

 - The administrator calls the Get User Details interface to get the details of multiple user registrations.

### **Request URL**


 - `http://x.x.x.x:10002/user/get_user_info`


### **Request method**


 - `POST`

### **Request Example**

  ```json
 {
    "operationID": "1111111222",
    "uidList":["123","3434"]
}
  ```

### **Request parameters**

| parameter name | required | type | description |
| :---------: | :--: | :------: | :----------------------------------------------------------- |
| operationID | is | string | operation id, use a random string |
| token | is | string | Note: placed in the header of the POST request, this token must be generated by calling auth/user_token as an APP administrator |
| uidList | is | json array | user Uid array that needs to get detailed information |


### **Back Example**

  ```json
{
    "errCode": 0,
    "errMsg": ""，
    "data": [
        {
            "uid": "06577eb8ed751416",
            "name": "Oxcupb....cool",
            "icon": "",
            "gender": 0,
            "mobile": "",
            "birth": "",
            "email": "",
            "ex": ""
        },
        {
            "uid": "1308",
            "name": "Allen",
            "icon": "https://yt-bullet-s.oss-ap-southeast-1.aliyuncs.com/1-10.png",
            "gender": 0,
            "mobile": "",
            "birth": "",
            "email": "",
            "ex": ""
        },
        {
            "uid": "1514",
            "name": "angel 1514",
            "icon": "https://indie-project-earthangle.oss-cn-beijing.aliyuncs.com/earthAngel/20210827043129233-6806.jpg",
            "gender": 0,
            "mobile": "",
            "birth": "",
            "email": "",
            "ex": ""
        }
    ]
}
  ```

### **Return parameter**

| parameter name | type | description |
| :------ | :----------- | ---------------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| data | array of json objects | list of obtained user details |



## **Get all users (Uid) registered by IM**

### **A brief description**

 - The administrator calls the interface to obtain the UIDs of all users registered in the IM.

### **Request URL**


 - `http://x.x.x.x:10002/manager/get_all_users_uid`


### **Request method**


 - `POST`

### **Request Example**

  ```json
 {
    "operationID": "1111111222"
}
  ```

### **Request parameters**

| parameter name | required | type | description |
| :---------: | :--: | :----: | :----------------------------------------------------------- |
| operationID | is | string | operation id, use a random string |
| token | is | string | Note: placed in the header of the POST request, this token must be generated by calling auth/user_token as an APP administrator |


### **Back Example**

  ```json
{
    "errCode": 0, 
    "errMsg": "", 
    "uidList": [
        "122", 
        "466"
    ]
}
  ```

### **Return parameter**

| parameter name | type | description |
| :------ | :------- | ------------------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| uidList | json array | Uid array of all registered users |

## **Send a single chat group chat message**

### **A brief description**

 - The administrator sends a single chat group chat message through the background interface.

### **Request URL**


 - `http://x.x.x.x:10002/manager/send_msg`


### **Request method**


 - `POST`

### **Request Example**

  ```json
{
    "operationID": "1111111222", 
    "sendID": "1111111222", 
    "recvID": "1111111222", 
    "senderNickName": "Zhang San",
    "senderFaceURL": "http://www.head.com", 
    "forceList": [
        "122", 
        "223"
    ], 
    "content": {
        "text": "nihao"
    }, 
    "contentType": 101, 
    "sessionType": 1
}
  ```

### **Request parameters**

| parameter name | required | type | description |
| :------------: | :--: | :------: | :----------------------------------------------------------- |
| operationID | is | string | operation id, use a random string |
| token | is | string | Note: placed in the header of the POST request, this token must be generated by calling auth/user_token as an APP administrator |
| sendID | is | string | sender ID |
| recvID | yes | string | receiver ID, user ID for single chat, group ID for group chat |
| senderNickName | no | string | sender nickname |
| senderFaceURL | no | string | sender avatar |
| forceList | No | string[] | When the chat type is group chat, use @ to specify the list of forced users |
| content | is | string | the specific content of the message, the internal is a json object, and the detailed fields of other messages, please refer to [message type](https://doc.rentsoft.cn/server_doc/server_doc.html#%E6%B6% 88%E6%81%AF%E7%B1%BB%E5%9E%8B%E6%A0%BC%E5%BC%8F%E6%8F%8F%E8%BF%B0) format description document |
| contentType | is | int | message type, 101 means text, 102 means picture.. For details, refer to [Message Type](https://doc.rentsoft.cn/server_doc/server_doc.html#%E6%B6%88%E6 %81%AF%E7%B1%BB%E5%9E%8B%E6%A0%BC%E5%BC%8F%E6%8F%8F%E8%BF%B0) format description document |
| sessionType | Yes | int | Whether the message sent is a single chat or a group chat, 1 for a single chat, 2 for a group chat |


### **Back Example**

  ```json
{
    "errCode": 0, 
    "errMsg": "", 
    "sendTime": 156454545565, 
    "msgID": "454dfddfjjfg"
}
  ```

### **Return parameter**

| parameter name | type | description |
| :------- | :----: | -------------------------------------- |
| errCode | int | 0 for success, non-zero for failure |
| errMsg | string | error message |
| sendTime | int | The specific time when the message is sent, specifically the timestamp in nanoseconds |
| msgID | string | Unique ID of the message |

## Message type format description

### **A brief description**

 - Description of the message types supported by the contentType in the administrator message sending field and the specific field description of the message content.

### **ContentType Message Type Description**

| ContentType value | type description |
| :-----------: | :---------------- |
| 101 | Text Messages |
| 102 | Picture message |
| 103 | Audio Messages |
| 104 | Video Message |
| 105 | File Messages |
| 106 | @type messages in group chats |
| 107 | Merge and Forward Type Messages |
| 108 | Business card message |
| 109 | Geolocation Type Messages |
| 110 | Custom message |
| 111 | Withdrawal type message |
| 112 | Read receipt type message |
| 114 | Reference Type Messages |



### **Content specific content**

- The content inside is a specific json object, and different message types are different json objects

#### **Text Message**

  ```json
{
    ...,
    "content": {
        "text": "nihao"
    },
    ....
}
  ```

| parameter name | required | type | description |
| :----: | :--: | :----: | :----------------- |
| text | is | string | the specific content of the text message |

  #### **Custom Message**

  ```json
{
    ...,
    "content": {
        "data": "", 
        "description": "", 
        "extension": ""
    },
    ....
}
  ```

| parameter name | required | type | description |
| :---------: | :--: | :---------: | :------------------------------------------------- |
| data | is | json string | the user-defined message is the converted string of the json object |
| description | No | json string | The extended description information is the converted string of the json object, which can not be used |
| extension | No | json string | Extension field, temporarily unused |


