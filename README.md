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

Added an API on :5002. added IdentityController. `install-package Microsoft.AspNetCore.Authentication.JwtBearer`. 

Register services and add middleware via Startup.cs