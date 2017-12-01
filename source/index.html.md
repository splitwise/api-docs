---
title: Splitwise API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - ruby

toc_footers:
  - <a href='https://secure.splitwise.com/apps'>Sign up for an API key</a>
  - <a href='https://github.com/lord/slate'>Documentation powered by Slate</a>

includes:
  - users
  - groups
  - friends
  - expenses
  - comments
  - notifications
  - other
  - errors
  - terms_of_use

search: true
---

# Introduction

Hey there! We're glad you're interested in the Splitwise API. This documentation will help you to fetch information on users, expenses, groups, and much more.

If something in the API is confusing you, you can open an [issue](https://github.com/splitwise/api-docs/issues) about it on GitHub. We're a small team, so we may not have an instant fix, but we'll get back to you as soon as we're able. (If you spot an issue in our API documentation itself, feel free to open a [pull request](https://github.com/splitwise/api-docs/pulls) to update this website!)


<aside class="warning"> We're still in the process of creating this new API documentation site, so not all API calls are documented here yet. You can find our old API docs <a href="http://dev.splitwise.com">here</a> along with several discussions in the comments. </aside>

# Authentication

```ruby
require 'oauth2'

client = OAuth2::Client.new(
  'consumer_key',
  'consumer_secret',
  site: 'https://www.splitwise.com/'
)
client.auth_code.authorize_url(
  redirect_uri: 'http://localhost:8080/oauth2/callback'
)
# => "https://www.splitwise.com/oauth/authorization?response_type=code&client_id=consumer_key&redirect_uri=http://localhost:8080/oauth2/callback"

# after opening the above URL and clicking "Authorize", the user is redirected to:
# http://localhost:8080/oauth2/callback?code=authorization_code

access_token = client.auth_code.get_token(
  'authorization_code',
  redirect_uri: 'http://localhost:8080/oauth2/callback'
)
response = access_token.get('/api/v3.0/get_current_user')
puts response.body
```

Splitwise uses OAuth for authentication. To connect via OAuth, you'll need to [register your app](https://secure.splitwise.com/apps) on Splitwise. When you register, you'll be given a consumer key and a consumer secret, which can be used by your application to make requests to the Splitwise server.

<aside class="notice">
  OAuth can be a very confusing protocol to implement correctly, and we <strong>strongly</strong> recommend that you use an existing OAuth library to connect to Splitwise. You can find a list of OAuth client libraries at the <a href="https://oauth.net/code/#client-libraries">OAuth community site</a>.
</aside>

For more information on using OAuth, check out the following resources:

- The OAuth community [getting started](http://oauth.net/documentation/getting-started/) guide
- The term.ie [OAuth test server](http://term.ie/oauth/example/) (great for debugging authorization issues)
- This old [Splitwise blog post](https://blog.splitwise.com/2013/07/15/setting-up-oauth-for-the-splitwise-api/) about OAuth

# An important note about nested parameters

Due to a quirk in Splitwise's servers, nested parameters (e.g. `users[1][first_name]`) cannot currently be used when submitting a request. Instead, to indicate nested parameters, use double underscores (e.g. `users__1__first_name`). We hope to support proper nested parameters in future API versions.
