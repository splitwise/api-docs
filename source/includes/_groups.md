# Groups

A Group represents a collection of users who share expenses together. For example, some users use a Group to aggregate expenses related to an apartment. Others use it to represent a trip. Expenses assigned to a group are split among the users of that group. Importantly, two users in a Group can also have expenses with one another outside of the Group.

## get_groups

> Example Response:

```json
{
  "groups":[
    // Non-group expenses are listed in a group with id 0
    {
      "id":0,
      "name":"Non-group expenses",
      "updated_at": "2017-08-30T20:31:51Z", //<current time in UTC>
      "members":[
        {
            "id": 1,
            "first_name": "Ada",
            "last_name": "Lovelace",
            "picture": {
              "small": "image_url",
              "medium": "image_url",
              "large": "image_url"
            },
            "email": "ada@example.com",
            "registration_status": "confirmed",          //'dummy', 'invited', or 'confirmed'
            "balance":[
               {
                  "currency_code":"AED",
                  "amount":"0.0"
               },
               {
                  "currency_code":"ALL",
                  "amount":"0.0"
               },
               {
                  "currency_code":"EUR",
                  "amount":"-5.0"
               },
               {
                  "currency_code":"USD",
                  "amount":"3730.5"
               } //, ...
            ]
         } // , ...
      ],
      "simplify_by_default":false,
      "original_debts":[
         {
            "from": 12345,          // user_id
            "to": 54321,            // user_id
            "amount":"414.5",       // amount as a decimal string
            "currency_code":"USD"   // three-letter currency code
         } // , ...
      ]
    },
    {
      "id":3018312,
      "name":"a test group",
      "updated_at":"2017-08-30T20:31:51Z",
      "members":[ /* <User object> , <User object>, ... */ ],
      "simplify_by_default":false,
      "original_debts":[
         {
            "from": 12345,          // user_id
            "to": 54321,            // user_id
            "amount":"414.5",       // amount as a decimal string
            "currency_code":"USD"   // three-letter currency code
         } // , ...
      ],
      "simplified_debts":[
         {
            "from": 12345,          // user_id
            "to": 54321,            // user_id
            "amount":"414.5",       // amount as a decimal string
            "currency_code":"USD"   // three-letter currency code
         } // , ...
      ],
      "whiteboard":"a message!",
      "group_type":"apartment",
      "invite_link":"https://www.splitwise.com/join/abcdef1232456"
    } // , ...
}
```

`GET https://www.splitwise.com/api/v3.0/get_groups`

Returns list of all groups that the current_user belongs to

## get_group/:id

> Example Response:

```json
{
  "group":
    {
      "id":3018312,
      "name":"a test group",
      "updated_at":"2017-08-30T20:31:51Z",
      "members":[ /* <User object> , <User object>, ... */ ],
      "simplify_by_default":false,
      "original_debts":[
         {
            "from": 12345,          // user_id
            "to": 54321,            // user_id
            "amount":"414.5",       // amount as a decimal string
            "currency_code":"USD"   // three-letter currency code
         } // , ...
      ],
      "simplified_debts":[
         {
            "from": 12345,          // user_id
            "to": 54321,            // user_id
            "amount":"414.5",       // amount as a decimal string
            "currency_code":"USD"   // three-letter currency code
         } // , ...
      ],
      "whiteboard":"a message!",
      "group_type":"apartment",
      "invite_link":"https://www.splitwise.com/join/abcdef1232456"
    }
}
```

`GET https://www.splitwise.com/api/v3.0/get_group/:id`

Returns information about the specified group (as long as the current user has access)

## create_group

> Example Response:

```json
{
  "group":
    {
      "id":3018312,
      "name":"a test group",
      "updated_at":"2017-08-30T20:31:51Z",
      "members":[ /* <User object> , <User object>, ... */ ],
      "simplify_by_default":false,
      "original_debts":[],
      "simplified_debts":[],
      "whiteboard":"a message!",
      "group_type":"apartment",
      "invite_link":"https://www.splitwise.com/join/abcdef1232456",
      // or if create failed
      "errors": ["something went wrong", "with your group"]
    }
}
```

`POST https://secure.splitwise.com/api/v3.0/create_group`

Create a new group. Adds the current user to the group by default.

### Query Parameters

<aside class="notice">name and at least one user is required</aside>

<aside class="notice">User params are in the format users__index__param where index is an array index (0-based) and param is either first_name, last_name, email, or user_id</aside>


Parameter | Type | Description
--------- | ---- | -----------
name | String | Group name
whiteboard             | String  | Text to display on the group whiteboard
group_type             | String  | What the group is being used for (apartment, trip, couple, etc.)
simplify_by_default    | Boolean | Turn on simplify debts?
users\__0\__first_name | String  | Add a user's first name
users\__0\__last_name  | String  | Add a user's last name
users\__0\__email      | String  | Add a user's email
users\__1\__user_id    | Integer | Add an existing user by id

## delete_group/:id

> Example Response:

```json
{
  "success": true, // or false
  "errors": ["any errors"]
}
```

`POST https://secure.splitwise.com/api/v3.0/delete_group/:id`

Delete an existing group. Destroys all associated records (expenses, etc.)


## undelete_group/:id

> Example Response:

```json
{
  "success": true, //or false
  "errors": ["any errors"]
}
```

`POST https://secure.splitwise.com/api/v3.0/undelete_group/:id`

## add_user_to_group

> Example Response:

```json
{
  "success": true, //or false
  "errors": ["any errors"]
}
```

`POST https://secure.splitwise.com/api/v3.0/add_user_to_group`

Add a user to a group

### Query Parameters

<aside class="notice">You must submit the group_id and user_id or the group_id, first_name, last_name, and email</aside>

Parameter | Type | Description
--------- | ---- | -----------
group_id | Integer | Existing group to add the user to
first_name | String | Add a user's first name
last_name | String | Add a user's last name
email | String | Add a user's email
user_id | Integer | Add an existing user by id

## remove_user_from_group

> Example Response:

```json
{
  "success": true, //or false
  "errors": ["any errors"]
}
```

`POST https://secure.splitwise.com/api/v3.0/remove_user_from_group`

Remove a user from a group if their balance is 0

### Query Parameters

Parameter | Type | Description
--------- | ---- | -----------
group_id | Integer | Group to remove the user from
user_id | Integer | Id of user to remove
