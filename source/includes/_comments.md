# Comments

## get_comments?expense_id=:id

```json
{
  "comments": [
    {
      "id": 79800950,
      "content": "Something about this expense",
      "comment_type": "User",
      "relation_type": "ExpenseComment",
      "relation_id": 855870953,
      "created_at": "2020-05-14T04:12:25Z",
      "deleted_at": null,
      "user": { /* <user object> */ }
    }
  ]
}
```

`GET https://secure.splitwise.com/api/v3.0/get_comments?expense_id=:id`

## create_comment

```json
{
  "comment": { /* <comment object> */ },
  "errors": {}
}
```

`POST https://secure.splitwise.com/api/v3.0/create_comment`

### Query parameters

Parameter | Type | Description
--------- | ---- | -----------
expense_id | Integer | The expense the comment is for
content    | String  | The comment contents

## delete_comment

```json
{
  "comment": { /* <comment object> */ },
  "errors": {}
}
```

`POST https://secure.splitwise.com/api/v3.0/delete_comment/:id`

### Query parameters

Parameter | Type | Description
--------- | ---- | -----------
id | Integer | The comment id
