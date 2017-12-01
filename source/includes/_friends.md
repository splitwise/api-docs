# Friends

Friends of a user are other users with whom the user splits expenses. To split expenses with one another, users must be friends. Users in a group together are automatically made friends. Many of the calls containing the word “friend” return objects representing friends of the current user. In addition to containing the user data, these objects contain information about the current user's balance with each friend.

## get_friends

> Example Response:

```json
{
  "friends":[
    {
      "id": 1,
      "first_name": "Ada",
      "last_name": "Lovelace",
      "picture": {
        "small": "image_url",
        "medium": "image_url",
        "large": "image_url"
      },
      "balance":[
        {
          "currency_code":"USD",
          "amount":"-1794.5"
        },
        {
          "currency_code":"AED",
          "amount":"7.5"
        }
      ],
      "groups":[
        {
          "group_id":3018312,
          "balance":[
            {
              "currency_code":"USD",
              "amount":"414.5"
            }
          ]
        },
        {
          "group_id":2830896,
          "balance":[
          ]
        },
        {
          "group_id":0,
          "balance":[
            {
              "currency_code":"USD",
              "amount":"-2209.0"
            },
            {
              "currency_code":"AED",
              "amount":"7.5"
            }
          ]
        }
      ],
      "updated_at":"2017-11-30T09:41:09Z"
    } // , ...
  ]
}
```

`GET https://www.splitwise.com/api/v3.0/get_friends`

Returns a list of the current user's friends.

## get_friend/:id

> Example Response:

```json
{
  "friend":
    {
      "id": 1,
      "first_name": "Ada",
      "last_name": "Lovelace",
      "picture": {
        "small": "image_url",
        "medium": "image_url",
        "large": "image_url"
      },
      "registration_status": "confirmed", // or 'dummy' or 'invited'
      "balance":[
        {
          "currency_code":"USD",
          "amount":"-1794.5"
        },
        {
          "currency_code":"AED",
          "amount":"7.5"
        }
      ],
      "groups":[
        {
          "group_id":3018312,
          "balance":[
            {
              "currency_code":"USD",
              "amount":"414.5"
            }
          ]
        },
        {
          "group_id":2830896,
          "balance":[
          ]
        },
        {
          "group_id":0,
          "balance":[
            {
              "currency_code":"USD",
              "amount":"-2209.0"
            },
            {
              "currency_code":"AED",
              "amount":"7.5"
            }
          ]
        }
      ],
      "updated_at":"2017-11-30T09:41:09Z"
    }
  }
}
```

`GET https://secure.splitwise.com/api/v3.0/get_group/:id`

Get detailed info on one group that current_user belongs to

## create_friend

> Example Response: same as [get_friend response](#get_friend-id)

`POST https://secure.splitwise.com/api/v3.0/create_friend`

Makes the current user a friend of a user specified with the url parameters user_email, user_first_name, and, optionally, user_last_name.

### Query Parameters

<aside class="notice">You must submit the user_email for existing users or user_email,  first_name, and (optionally) last_name for new users</aside>

Parameter | Type | Description
--------- | ---- | -----------
user_first_name | String | Add a user's first name
user_last_name | String | Add a user's last name
user_email | String | Add a user's email (or find an existing user by email)

## create_friends

> Example Response: same as [get_friends response](#get_friends)


`POST https://secure.splitwise.com/api/v3.0/create_friends`

Make the current user a friend of the specified users.

### Query Parameters

<aside class="notice">You must submit the user_email for existing users or user_email,  first_name, and (optionally) last_name for new users</aside>

Parameter | Type | Description
--------- | ---- | -----------
friends__0__user_first_name | String | Add a user's first name
friends__0__user_last_name | String | Add a user's last name
friends__0__user_email | String | Add a user's email (or find an existing user by email)
friends__1__user_email | String | Find an existing user by email)

## delete_friend/:id

> Example Response:

```json
{
  "success": true, //or false
  "errors": ["any errors"]
}
```

`POST https://secure.splitwise.com/api/v3.0/delete_friend/:id`

Given a friend ID, break off the friendship between the current user and the specified user.
