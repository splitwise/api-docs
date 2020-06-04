# Expenses

## get_expense/:id

Return full details on an expense involving the current user. There are some additional values included in the expense object than shown here but they should be ignored.

```json
{
  "expense": {
    "id": 368887,
    "group_id": 18417, //or null
    "description": "Grocery run",
    "repeats": false,
    "repeat_interval": "never", //or "weekly", "fortnightly", "monthly", "yearly"
    "email_reminder": false,
    "email_reminder_in_advance": -1, // or 0, 1, 3, 5, 7, 14
    "next_repeat": null,
    "details": "Additional notes about the expense",
    "comments_count": 0,
    "payment": false,
    "transaction_confirmed": false,
    "cost": "25.0",
    "currency_code": "USD",
    "repayments": [
      {
        "from": 6788709,
        "to": 270896089,
        "amount": "25.0"
      }
    ],
    "date": "2012-07-27T06:17:09Z",
    "created_at": "2012-07-27T06:17:09Z",
    "created_by": { /* <user object> */ },
    "updated_at": "2012-12-23T05:47:02Z",
    "updated_by": { /* <user object> */ },
    "deleted_at": "2012-12-23T05:47:02Z",
    "deleted_by": { /* <user object> */ },
    "category": {
      "id": 18,
      "name": "General"
    },
    "receipt": {
      "large": "https://splitwise.s3.amazonaws.com/uploads/expense/receipt/3678899/large_95f8ecd1-536b-44ce-ad9b-0a9498bb7cf0.png",
      "original": "https://splitwise.s3.amazonaws.com/uploads/expense/receipt/3678899/95f8ecd1-536b-44ce-ad9b-0a9498bb7cf0.png"
    },
    "users": [
      {
        "user": { /* <user object> */ },
        "user_id": 270896089,
        "paid_share": "25.0",
        "owed_share": "0.0",
        "net_balance": "25.0"
      },
      {
        "user": { /* <user object> */ },
        "user_id": 6788709,
        "paid_share": "0.0",
        "owed_share": "25.0",
        "net_balance": "-25.0"
      }
    ],
    "comments": [ /* <comment object>, <comment object>,... */ ]
  }
}
```

`GET https://secure.splitwise.com/api/v3.0/get_expense/:id`

## get_expenses

Return expenses involving the current user, in reverse chronological order

```json
{
  "expenses": [ /* <expense object>, <expense object>, ... */ ],
}
```

`GET https://secure.splitwise.com/api/v3.0/get_expenses`

### Query parameters

<aside class="notice">All parameters are optional. Calling this endpoint with no parameters will return the 20 most recent expenses for the user. The server sets arbitrary (but large) limits on the number of expenses returned if you set a limit of 0 for a user with a lot of expenses</aside>

Parameter | Type | Description
--------- | ---- | -----------
group_id        | Integer | Return expenses for specific group
friend_id       | Integer | Return expenses for a specific friend that are not in any group
dated_after     | Time    | ISO 8601 Date time. Return expenses later that this date
dated_before    | Time    | ISO 8601 Date time. Return expenses earlier than this date
updated_after   | Time    | ISO 8601 Date time. Return expenses updated after this date
updated_before  | Time    | ISO 8601 Date time. Return expenses updated before this date
limit           | Integer | How many expenses to fetch. Defaults to 20; set to 0 to fetch all
offset          | Integer | Return expenses starting at limit * offset

## create_expense

```json
{
  "expense": { /* <expense object> */ },
  "errors": { }
}
```

`POST https://secure.splitwise.com/api/v3.0/create_expense`

### Query parameters

#### Required

Parameter | Type | Description
--------- | ---- | -----------
cost        | String  | A string representation of a decimal value, limited to 2 decimal places
description | String  | A short description of the expense
payment     | Boolean | `true` if this is a payment, `false` otherwise

#### Split configuration

<aside class="notice">You may either provide a group_id and split_equally as true or provide a list of user shares. If you set a group_id and split_equally to true the expense will be split equally with all members of the group. If setting detailed shares, either provide the user_id (if you are already friends with the user) or first_name, last_name and email (if you are not already friends) to identify the user, then set the owed_share and paid_share. If you are not friends with the user, adding them to an expense with first_name, last_name and email will add them to your friends list and invite them to Splitwise</aside>

Parameter | Type | Description
--------- | ---- | -----------
group_id              | Integer | The group to put this expense in.
split_equally         | Boolean | Set this to `true` if using it
users\__0\__user_id    | Integer | The user id of a friend for this share
users\__0\__paid_share | String  | Decimal amount as a string with 2 decimal places. The amount this user paid for the expense
users\__0\__owed_share | String  | Decimal amount as a string with 2 decimal places. The amount this user owes on the expense
users\__1\__first_name | String  |
users\__1\__last_name  | String  |
users\__1\__email      | String  | Valid email address for this user
users\__1\__paid_share | String  | Decimal amount as a string with 2 decimal places. The amount this user paid for the expense
users\__1\__owed_share | String  | Decimal amount as a string with 2 decimal places. The amount this user owes on the expense
users\__*\__key_value  | String  | Add additional user shares with indexes 2,3,4,5,...

#### Optional parameters

Parameter | Type | Description
--------- | ---- | -----------
group_id        | Integer | The group to put the expense in
details         | String  | More detailed notes
date            | Time    | ISO 8601 date time
repeat_interval | String  | One of: never, weekly, fortnightly, monthly, yearly
currency_code   | String  | ISO 4217 currency code. Must be in the list from `get_currencies`
category_id     | Integer | A category id from `get_categories`

## update_expense/:id

```json
{
  "expense": { /* <expense object> */ },
  "errors": { }
}
```

`POST https://secure.splitwise.com/api/v3.0/update_expense/:id`

### Query parameters

These are the same as for `create_expense` except you only need to include parameters that are changing from the previous values.

## delete_expense/:id

```json
{
  "success": true, //or false
}
```

`POST https://secure.splitwise.com/api/v3.0/delete_expense/:id`

## undelete_expense/:id

```json
{
  "success": true, //or false
}
```

`POST https://secure.splitwise.com/api/v3.0/undelete_expense/:id`
