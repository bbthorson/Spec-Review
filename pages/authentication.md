##Authentication

Publishers must support authenticating advertiser and agency users. Publishers must use OAuth 2.0 for user authentication. Publishers must support the implicit and authorization code grant flows.

Each request must include an AccessToken header that is set to the user’s access token. If the token is not valid, the request must fail with HTTP status code 401 Unauthorized.
