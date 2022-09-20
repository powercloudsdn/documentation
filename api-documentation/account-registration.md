---
title: Account Registration
description: 
---

## POST /user
This API is used to create an account for a new customer. Do not confuse this API with the contacts API (which is used to create sub-accounts)

### Request Information

#### URI Parameters
None

#### Body Parameters

| Name | Type | Required | Additional Information |
|======|======|==========|========================|
| name | string | ✔️ | Max 100 characters |
| password | string | ✔️ | See password requirements |
| password_confirmation | string | ✔️ | Should be equal to password field |
| email | string | ✔️ | Will be checked for working DNS - must be verifiable |
| country | string | ✔️ | ISO2 country code |


```json
{
    "name": "John Appleseed",
    "password": "QsE4rY4",
    "password_confirmation": "QsE4rY4",
    "email": "john@example.com",
    "country": "US"
}
```

### Example Response