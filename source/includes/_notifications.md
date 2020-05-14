# Notifications

## get_notifications

```json
{
  "notifications": [
    {
      "id": 32514315,
      "type": 0,
      "created_at": "2020-05-13T20:58:17Z",
      "created_by": 2,
      "source": {
        "type": "Expense",
        "id": 865077,
        "url": null
      },
      "image_url": "https://s3.amazonaws.com/splitwise/uploads/notifications/v2/0-venmo.png",
      "image_shape": "square",
      "content": "<strong>You</strong> paid <strong>Jon H.</strong>.<br><font color=\"#5bc5a7\">You paid $23.45</font>"
    } //, ...
  ]
}
```

`GET https://secure.splitwise.com/api/v3.0/get_notifications`

Return a list of recent activity on the users account with the most recent items first. `content` will be suitable for display in HTML and uses only the `<strong>`, `<strike>`, `<small>`, `<br>` and `<font color="#FFEE44">` tags.

The `type` value indicates what the notification is about. Notification `type`s may be added in the future without warning. Below is an incomplete list of notification types.

Type | Meaning
---- | -------
0    | Expense added
1    | Expense updated
2    | Expense deleted
3    | Comment added
4    | Added to group
5    | Removed from group
6    | Group deleted
7    | Group settings changed
8    | Added as friend
9    | Removed as friend
10   | News (a URL should be included)
11   | Debt simplification
12   | Group undeleted
13   | Expense undeleted
14   | Group currency conversion
15   | Friend currency conversion

### Query parameters

<aside class="notice">All parameters are optional. The server sets arbitrary (but large) limits on the number of notifications returned if you set a very old updated_after value or limit of 0 for a user with a lot of notifications</aside>

Parameter | Type | Description
--------- | ---- | -----------
updated_after | Time   | ISO 8601 Date and time string with timezone offset. Return notifications after this time.
limit  				| Integer | How many notifications to fetch. Defaults to 20. 0 for all.
