# Users

<aside class="notice">For the active user, all fields will be returned. For other users (e.g. friends), only some fields will be returned.</aside>

## get_current_user

> Example Response:

```json
{
  "user": {
    "id": 1,
    "first_name": "Ada",
    "last_name": "Lovelace",
    "picture": {
      "small": "image_url",
      "medium": "image_url",
      "large": "image_url"
    },
    "email": "ada@example.com",
    "registration_status": "confirmed",           //'dummy', 'invited', or 'confirmed'
    "default_currency": "USD",
    "locale": "en",
    "notifications_read": "2017-06-02T20:21:57Z", // the last time notifications were marked as read
    "notifications_count": 12,                    // the number of unread notifications
    "notifications": {                            // notification preferences
      "added_as_friend": true,
      // ...
    }
  }
}
```

`GET https://www.splitwise.com/api/v3.0/get_current_user`

Retrieve info about the user who is currently logged in.

## get_user/:id

> Example Response:

```json
{
  "user": {
    "id": 1,
    "first_name": "Ada",
    "last_name": "Lovelace",
    "picture": {
      "small": "image_url",
      "medium": "image_url",
      "large": "image_url"
    },
    "email": "ada@example.com",
    "registration_status": "confirmed"           //'dummy', 'invited', or 'confirmed'
    }
  }
}
```

`GET https://www.splitwise.com/api/v3.0/get_user/:id`

Retrieve info about another user that the current user is acquainted with (e.g. they are friends, or they both belong to the same group).


## update_user/:id

`POST https://www.splitwise.com/api/v3.0/update_user/:id`

Update a specific user. A user can edit anything about their own account, and may edit the `first_name`, `last_name`, and `email` for any acquaintances who have not logged in yet.

### Query Parameters

Parameter | Type | Description
--------- | ---- | -----------
first_name            | String | User's first name
last_name             | String | User's last name
email                 | String | User's email address
password              | String | User's password
locale                | String | User's locale (ISO 639-1)
date_format           | String | Preferred Date Format (e.g. MM/DD/YYYY or 
default_currency      | String | User's default currency (ISO 4217)
default_group_id      | String | Default Group ID (set -1 for none)
notification_settings | Object <br>`{ notification_type: bool, ... }` | Set notification types on or off: added_as_friend, added_to_group, expense_added, expense_updated, bills, payments, monthly_summary, announcements,}






