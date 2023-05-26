# Openim field description

| Parameter Title | Type | Maximum String length limit | Description | Value range |
|: -----------: |: --------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | |
| Secret | String | 32 | Openim secrets, the secret field of config.yaml configuration file config.yaml, pay attention to safe saving | string
| Platform | int | | User login or registered platform type | iOS 1, Android 2, Windows 3, OSX 4, Web 5, Small Program 6, Linux 7 |
| USERID | String | 64 | User ID, you must ensure the unique | string in the IM |
| nickName | String | 255 | User nickname or group nickname | string
| FACEURL | String | 255 | User avatar or group avatar URL, understand according to the context | |
| Gender | int
| PHONENUMBER | String | 32 | User's mobile phone number, including region, (such as Hong Kong:+852-xxxxxxxx), | | | |
| BIRTH | UINT32 | | User Birthday, UNIX Timeline (second) | |
| Email | String | 64 | Email address | | | |
| EX | String | 1024 | Extended fields, users can expand themselves, it is recommended to be encapsulated into JSON string | |
| Operationid | String | | Operation ID, keep the unique, it is recommended to use the current time microsecond+random number | |
| Expiredtime | int | | Expired time, unit (second) | |
| ROLELEVEL | int | | Member types in the group | 1 Ordinary member, 2 group owners, 3 administrators |
| groupype | int | | group type | Uniform filling 0 |
| OwnerUserid | String | 64 | Group owner userid | |
| GroupName | String | 255 | Group name | | |
| Notification | String | 255 | Group Announcement | |
| INTRODUCTION | String | 255 | Group Introduction | |
| Memberlist | JSON array | | Member list | |
| Reason | String | 64 | Cause, such as kicking people and other reasons | |
| Token | String | | When calling the API, set it to the request header | |
| Useridlist | JSON array | | Userid list of users | |