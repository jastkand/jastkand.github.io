---
layout: post
title:  "How to stub an Upload request to Google API using webmock"
date:   2017-11-17
categories: ruby rspec webmock
---
The `google-api-client` gem doesn't return any response for an upload request when the
`X-Goog-Upload-Status` is not set to `'final'`.

When you want to stub an API request you usually write code like this:

```ruby
stub_request(:post, 'https://www.googleapis.com/upload/gmail/v1/users/me/messages/send')
  .with(headers: { 'X-Goog-Upload-Header-Content-Type': 'message/rfc822' })
  .to_return(body: upload_response.to_json, headers: { content_type: 'application/json' })
```

But it does not work for Google Upload API request. As a response it will return `nil`. In order to fix this you should add the
`'X-Goog-Upload-Status': 'final'` as a response header. So the code will look this way:

```ruby
stub_request(:post, 'https://www.googleapis.com/upload/gmail/v1/users/me/messages/send')
  .with(headers: { 'X-Goog-Upload-Header-Content-Type': 'message/rfc822' })
  .to_return(body: upload_response.to_json, headers: { content_type: 'application/json', 'X-Goog-Upload-Status': 'final' })
```

Hope this will save you some time!
