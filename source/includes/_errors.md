# Errors

In general, the Splitwise API returns the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request. Something about the request was invalid, and you will probably need to change your request before trying again.
401 | Unauthorized. You are not logged in — your OAuth authentication may not be configured correctly.
403 | Forbidden. The current user is not allowed to perform this action.
404 | Not Found. The endpoint that you were trying to call does not exist.
500 | Internal Server Error. Our server had an unexpected error while trying to process your request. This may be a temporary problem, or there may be a problem with your request that is causing the server to crash.
503 | Service Unavailable. We're temporarily offline for maintenance. Please try again later.

In addition, even when a call is successful and returns a `200 OK` HTTP response, the body of the response may include a key called `error` or `errors`. This is usually used to communicate validation errors. For example, if you submit an expense without any cost, we may return an `errors` key as part of the JSON response.

The format of these errors is somewhat inconsistent, unfortunately. We're working on standardizing it, but it's a work in progress.