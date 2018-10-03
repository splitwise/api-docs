---
title: Splitwise API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - ruby--oauth2
  - ruby--oauth1
  - javascript--oauth2
  - javascript--sdk

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

```ruby--oauth2
#!/usr/bin/env ruby
require 'oauth2' # gem 'oauth2'
require 'pp'

CONSUMER_KEY = <fill in your key>
CONSUMER_SECRET = <fill in your secret>
TOKEN_URL = 'https://secure.splitwise.com/oauth/token'
AUTHORIZE_URL = 'https://secure.splitwise.com/oauth/authorize'
MY_CALLBACK_URL = 'http://localhost:8080/callback' # Make sure to set the redirect URL that you registered with Splitwise when creating your app so that it matches this URL
BASE_SITE = 'https://secure.splitwise.com/'

client = OAuth2::Client.new(CONSUMER_KEY, CONSUMER_SECRET, site: BASE_SITE)
authorize_url = client.auth_code.authorize_url(redirect_uri: MY_CALLBACK_URL)
# => "https://www.splitwise.com/oauth/authorize?response_type=code&client_id=#{CONSUMER_KEY}&redirect_uri=#{MY_CALLBACK_URL}

require 'webrick'
require 'cgi'

server = WEBrick::HTTPServer.new(
  Port: 8080,
  StartCallback: proc { puts "Opening 'localhost:8080'"; `open 'http://localhost:8080/'` })

server.mount_proc "/" do |req, res|
  res.body = "<a href=\"#{authorize_url}\"> Get Code </a>"
end

server.mount_proc "/callback" do |req, res|
  authorization_code = CGI.parse(req.query_string)['code']
  access_token = client.auth_code.get_token(
    authorization_code,
    redirect_uri: MY_CALLBACK_URL
  )

  # This is your actual bearer token! Your bearer token will be printed out to the console.
  # You can then use that bearer token to make additional API requests to Splitwise. For example:
  # curl -XGET "http://secure.splitwise.com/api/v3.0/get_current_user" -H "Authorization: Bearer YOUR_TOKEN"
  puts "***"
  puts "Here is your OAuth Bearer token!"
  pp access_token.to_hash
  puts "***"

  response = access_token.get('/api/v3.0/get_current_user')
  res.body = response.body
end

# Triggered by ^C on os x; run `$ stty -a |grep intr` to find appropriate key combination
trap('INT') { server.stop }
server.start
```
```ruby--oauth1
#!/usr/bin/env ruby
require 'oauth' # gem oauth

CONSUMER_KEY = <fill in your key>
CONSUMER_SECRET = <fill in your secret>
REQUEST_TOKEN_URL ='https://secure.splitwise.com/oauth/request_token'
ACCESS_TOKEN_URL = 'https://secure.splitwise.com/oauth/access_token'
AUTHORIZE_URL = 'https://secure.splitwise.com/oauth/authorize'
MY_CALLBACK_URL = 'http://localhost:8080/callback'

consumer = OAuth::Consumer.new(CONSUMER_KEY, CONSUMER_SECRET, site: 'https://www.splitwise.com')
request_token = consumer.get_request_token(oauth_callback: MY_CALLBACK_URL)
# => "https://www.splitwise.com/oauth/authorize?oauth_token="#{request_token}"

require 'webrick'
require 'cgi'
server = WEBrick::HTTPServer.new(
  Port: 8080,
  StartCallback: proc { puts "Opening 'localhost:8080'"; `open 'http://localhost:8080/'` })

server.mount_proc "/" do |req, res|
  res.body = "<a href=\"#{request_token.authorize_url}\"> Get Code </a>"
end

server.mount_proc "/callback" do |req, res|
  oauth_verifier = CGI.parse(req.query_string)['oauth_verifier'].first
  access_token = request_token.get_access_token(oauth_verifier: oauth_verifier)
  access_token.params.each do |k, v|
    puts "  #{k}: #{v}" unless k.is_a?(Symbol)
  end
  response = access_token.request(:get, '/api/v3.0/get_current_user')
  res.body = response.body
end

# Triggered by ^C on os x; run `$ stty -a |grep intr` to find appropriate key combination
trap('INT') { server.stop }
server.start
```

```javascript--oauth2
#!/usr/bin/env node
'use strict';

const OAuth = require('oauth');
const {exec} = require('child_process');
const qs = require('querystring');
const http = require('http');

const CONSUMER_KEY = <fill in your key>;
const CONSUMER_SECRET = <fill in your secret>;
const TOKEN_URL = '/oauth/token';
const AUTHORIZE_URL = '/oauth/authorize';
const MY_CALLBACK_URL = 'http://localhost:8080/callback';
const BASE_SITE = 'https://www.splitwise.com';

var authURL;
const client = new OAuth.OAuth2(
  CONSUMER_KEY,
  CONSUMER_SECRET,
  BASE_SITE,
  AUTHORIZE_URL,
  TOKEN_URL,
  null);

const server = http.createServer(function(req, res) {
  console.log(req.url);
  var p = req.url.split('/');
  console.log(p);

  var pLen = p.length;

  authURL = client.getAuthorizeUrl({
    redirect_uri: MY_CALLBACK_URL,
    response_type: 'code'
  });

  /**
   * Creating an anchor with authURL as href and sending as response
   */
  var body = '<a href="' + authURL + '"> Get Code </a>';
  if (pLen === 2 && p[1] === '') {
    res.writeHead(200, {
      'Content-Length': body.length,
      'Content-Type': 'text/html'
    });
    res.end(body);
  } else if (pLen === 2 && p[1].indexOf('callback') === 0) {
    /** To obtain and parse code='...' from code?code='...' */
    var qsObj = qs.parse(p[1].split('?')[1]);
    console.log(qsObj.code);
    /** Obtaining access_token */
    client.getOAuthAccessToken(
      qsObj.code,
      {
        'redirect_uri': MY_CALLBACK_URL,
        'grant_type': 'authorization_code'
      },
      function(e, access_token, refresh_token, results) {
        if (e) {
          console.log(e);
          res.end(JSON.stringify(e));
        } else if (results.error) {
          console.log(results);
          res.end(JSON.stringify(results));
        }
        else {
          console.log('Obtained access_token: ', access_token);
          client.get('https://secure.splitwise.com/api/v3.0/get_current_user', access_token, function(e, data, response) {
            if (e) console.error(e);
            res.end(data);
          });
        }
      });

  } else {
    // Unhandled url
  }
});
server.listen({port: 8080}, serverReady);

function serverReady() {
  console.log(`Server on port ${server.address().port} is now up`);
  exec(`open http://localhost:8080/`, (err, stdout, stderr) => {
    if (err) {
      // node couldn't execute the command
      return;
    }

    // the *entire* stdout and stderr (buffered)
    console.log(`stdout: ${stdout}`);
    console.log(`stderr: ${stderr}`);
  });
}

module.exports = client;

```

```javascript--sdk
#!/usr/bin/env node
'use strict';

const Splitwise = require('splitwise');
const express = require('express');
const {exec} = require('child_process');

const app = express();
const port = 8080;

const sw = Splitwise({
  consumerKey: '<fill in your key>',
  consumerSecret: '<fill in your secret>'
});

app.get('/', (req, res) => {
  sw.getAccessToken().then(token => {
    console.log(`Obtained access_token: ${token}`);
  });
  sw.getCurrentUser().then(data => res.send(data)).catch(console.log);
});


app.listen(port, () => {
  console.log(`Server on port ${port} is now up`);
  exec(`open http://localhost:${port}/`, (err, stdout, stderr) => {
    if (err) {
      return;
    }
    console.log(`stdout: ${stdout}`);
    console.log(`stderr: ${stderr}`);
  });
});
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
