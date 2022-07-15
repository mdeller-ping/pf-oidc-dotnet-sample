
Demonstrates using PingFederate as the OpenID Connect Provider an ASP.Net API.  This sample code uses Microsoft.AspNetCore.Authentication.OpenIDConnect.  Proof Key for Code Exchange (PKCE) is also used to improve security.

## Prerequisites

* ASP.NET Core 6.0
* PingFederate
* Trusted SSL Certificate
* dotnet cli

## Assumptions

Your PingFederate runtime is available at the URL https://pingfederate:9031.  You are running this sample dotnet application locally, and it will be available at https://localhost:5001.  Update accordingly.

A valid SSL certificate is assigned to your PingFederate instance.  The dotnet application will fail to trust PingFederate's well known endpoint without a signed certificate.

## Clone this repository

```Shell
git clone https://github.com/mdeller-ping/pf-oidc-dotnet-sample.git
cd pf-oidc-dotnet-sample/pf-oidc-dotnet-sample
```

## Configure an OAuth Client

We need an OAuth Client in PingFederate.  Default values are great, with a couple of exceptions:

* Client ID: sampleOIDC
* Client Name: sampleOIDC
* Client Authentication: None
* Redirect URIs: https://localhost:5001/signin-oidc
* Allowed Grant Types: Authorization Code
* Require Proof Key for Code Exchange (PKCE): Enabled

## Edit Program.cs

The Issuer (aka Authority) Client ID values are hard coded in Program.cs.  Update these values with your real URL and Client.

```C#
.AddOpenIdConnect(options => {
    options.ClientId = "sampleOIDC";
    options.Authority = "https://pingfederate:9031";
    options.ResponseType = "code";
    options.GetClaimsFromUserInfoEndpoint = true;
    options.UsePkce = true;
});
```

## Run the sample

```Shell
dotnet watch
```

## Test the Application

Navigate to the Privacy link of the presented application.  This will redirect you to PingFederate's authorization URL where you will be authenticated.
