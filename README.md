# IdentityServer-fun

quickstarts zero to hero (hopefully)

add templates `dotnet new -i IdentityServer4.Templates`

## Protecting an API using Client Credentials
https://identityserver4.readthedocs.io/en/latest/quickstarts/1_client_credentials.html

```
dotnet new is4empty -n IdentityServer
// added a sln
```

in config added an API ` new ApiResource("api1", "My API")` and a Client (non interactive user, use just the embedded clientid/secret for authentication)

run and go to disco doc https://localhost:5001/.well-known/openid-configuration

Added an API on :5002. added IdentityController with `[Authorize]`. `install-package Microsoft.AspNetCore.Authentication.JwtBearer`. 

Register services and add middleware via Startup.cs. This will

+ validate the incoming token to make sure it is coming from a trusted issuer
+ validate that the token is valid to be used with this api (aka audience)

at this point a reques to http://localhost:5002/api/identity will 401 due to the Authorize attribute

Add a console app "Client" and `install-package IdentityModel`. IdentityModel includes a client library to use with the discovery endpoint. This way you only need to know the base-address of IdentityServer - the actual endpoint addresses can be read from the metadata:

After a bit of faff with https (where i had veered from the docs) this works as expected, namely:

+ 3 apps are running on different endpoints: IDP, Client, Api
+ Client gets disco doc and Requests ClientCredentials, sending client secrets and required scope
+ IDP recieves request at TokenEndpoint, checks ok and returns an access token
+ client attaches access token to the api request and hits API
+ API behind the scenes contacts IDP and validates access token, and allows client access to resource.


There is a lot of magic in the last stage that I can't see  - but I think that is what is happening.

## Interactive Applications with ASP.NET Core
https://identityserver4.readthedocs.io/en/latest/quickstarts/2_interactive_aspnetcore.html


add the mvc ui templates `dotnet new is4ui`. New MVCClient, `install-package Microsoft.AspNetCore.Authentication.OpenIdConnect` and Startup amends.
... ok
