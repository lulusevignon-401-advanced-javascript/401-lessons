# Bearer Authorization

Using a "Bearer Token" to re-authenticate with a server following a successful login, or obtaining/generating a permanent key


#### Describe and Define

- Bearer Authentication
- JSON Web Tokens (jwt)
- Web Security
- When to use Basic or Bearer Authentication

#### Execute

- Create a Bearer Token Auth System
- Secure tokens

## Notes

### Bearer Tokens

- Bearer Tokens are sent to the user/client after the initial `signin` process has completed.
- Clients must make every subsequent request to the server with that token, in the header
  - `Authorization: Bearer encoded.jsonwebtoken.here`
- The server opens the token, does the re-authentication, and then grants or denies access
- In express servers, this can be done in middleware, in conjunction with a user model

  ```javascript
  app.get('/somethingsecret', bearerToken, (req,res) => {
    res.status(200).send('secret sauce');
  });

  function bearerToken( req, res, next ) {
    let token = req.headers.authorization.split(' ').pop();
    try {
      if ( tokenIsValid(token) ) { next(); }
    }
    catch(e) { next("Invalid Token") }
  }

  function tokenIsValid(token) {
    let parsedToken = jwt.verify(token, SECRETKEY);
    return Users.find(parsedToken.id);
  }
  ```