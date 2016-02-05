# Multitenant JWT Auth sample
This sample shows how to implement an API that:

* Uses JWTs for authentication
* Uses claims in those JWTs for authorization
* Supports multiple tenants
* Supports blacklisting JWTs

## Installation
Clone this repository. Then run:
```
npm i
```

## Running the sample
The sample has two components:
* A server that hosts the API
* A CLI that can be used to perform requests to the API.

### Running the server
```
node server.js
```

### Using the CLI
```
./cli --help

  Usage: cli [options]

  Options:

    -h, --help         output usage information
    -V, --version      output the version number
    --tenant <tenant>  The tenant id. Either "tenant_1" or "tenant_2"
    --token <token>    The JWT for the tenant. Either 1 or 2
```

Using each tenant token combo yields a different result:
* Token 1 for **tenant_1** will send a response the users. The JWT has the correct scopes and is not blacklisted.
  ```
  > ./cli --tenant tenant_1 --token 1
  Success [{"name":"Jane Doe"},{"name":"John Doe"}]
  ```

* Token 2 for **tenant_1** will send a response with an error because the token is revoked.
  ```
  > ./cli --tenant tenant_1 --token 2
  {"name":"UnauthorizedError","code":"revoked_token"}
  ```
  
* Token 1 for **tenant_2** will send a response with an error because the token does not have the required scope.
  ```
  >./cli --tenant tenant_2 --token 1
  {"name":"UnauthorizedError","code":"insufficient_scopes"}
  ```
  
* Token 2 for **tenant_2** will send a response with an error because the token is revoked. It does not have the required scope, but that check is done before.
  ```
  > ./cli --tenant tenant_2 --token 2
  {"name":"UnauthorizedError","code":"revoked_token"}
  ```

## Contributing
Just send a PR, you know the drill.

## Issue Reporting

If you have found a bug or if you have a feature request, please report them at this repository issues section. Please do not report security vulnerabilities on the public GitHub issue tracker. The [Responsible Disclosure Program](https://auth0.com/whitehat) details the procedure for disclosing security issues.

## Author

[Auth0](auth0.com)

## License

This project is licensed under the MIT license. See the [LICENSE](LICENSE) file for more info.
