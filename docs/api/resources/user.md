---
layout: default
title: Zumba API User Resource - Documentation
description: User resource documentation
author: cjsaylor
---

<ul class="breadcrumb">
	<li><a href="{{site_url}}/docs">Documentation</a></li>
	<li><a href="{{site_url}}/docs/api">API</a></li>
	<li><a href="{{site_url}}/docs/api/resources">Resources</a></li>
	<li class="active">User</li>
</ul>

**TOC**

<ul>
	<li><a href="#getUser">GET /user</a></li>
	<li><a href="#getUserClasses">GET /user/classes</a></li>
	<li><a href="#getClassCheckins">GET /class/checkins]</a></li>
</ul>

<hr>

<span id="getUser"></span>

### `GET /user` <span class="label label-info">Proposed</span><span class="label label-danger">Incomplete</span>

> Retrieve user info.

**Response**

```
{
  "id": String,
  "username": String,
  "email_address": String,
  "first_name": String,
  "last_name": String,
  "pid": String,
  "branch": String,
  "isZIN": Boolean,
  "isZES": Boolean,
  "isZJ": Boolean,
  "isGym": Boolean
}
```

If the scope of the OAuth token includes nutrition, then the response will include additionally:

```
{
  nutrition_status: Integer
}
```

<hr>

<span id="getUserClasses"></span>

### `GET /user/classes` <span class="label label-info">Proposed</span><span class="label label-danger">Incomplete</span>

> Retrieve a list of an instructor's weekly classes.

**Parameters**

{:.table}
*Name* | *Description*
--- | ---
type | Class type (`?type=nutrition`, or multiple: `?type[]=nutrition&type[]=zumba_toning`)

Note: `day_of_week` starts `0` = Sunday.

**Response**

```
[
  {
    "id": String,
    "start_time": "00:00:00+00:00",
    "end_time": "00:00:00+00:00",
    "day_of_week": Integer,
    "type": String,
    "location": {
      "name": String,
      "street": String,
      "street_2": String,
      "city": String,
      "state": String,
      "country": String,
      "postal_code": String
    },
    "_uris": {
      "class": "https://apiv3.zumba.com/class/:id"
    }
  }
]
```

<hr>

<span id="getClassCheckins"></span>
### `GET /user/checkins` <span class="label label-info">Proposed</span><span class="label label-danger">Incomplete</span>

> Retrieve a user's checkin history.

**Parameters**

{:.table}
*Name* | *Description*
--- | ---
`start_date` | <span class="label label-warning">Required</span> Date to begin looking for records.
`offset` | Number of records to offset on the request.
`max` | Number of records to return in the response.

**Response**

```
[
  {
    "id": String,
    "class_id": String,
    "source": String,
    "type": String,
    "datetime": "0000-00-00T00:00:00+00:00",
    "location": {
      "city": String,
      "state": String,
      "country": String
    },
    "_uris": {
      "class": "https://apiv3.zumba.com/class/:id"
    }
  }
]
```

**Response Headers**

See [pagination documentation]({{site_url}}/docs/api/pagination.html) for the pagination handling mechanism.