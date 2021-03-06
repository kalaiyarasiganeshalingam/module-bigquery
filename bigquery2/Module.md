Connects to Bigquery from Ballerina.

# Module Overview

The Bigquery connector allows you to access and run queries on the Bigquery through the Bigquery REST API.

**Bigquery Operations**

The `wso2/bigquery2` module contains operations that retrieve projects, tables, and datasets. And also it allows you to run the queries.

## Compatibility

|                             |       Version               |
|:---------------------------:|:---------------------------:|
| Ballerina Language          | 0.990.3                     |
| Bigquery API Version        | V2                          |

## Sample

Import the `wso2/bigquery2` module into the Ballerina project.

```ballerina
import wso2/bigquery2;
```
Instantiate the connector by giving the authentication details in the HTTP client config. The HTTP client config has built-in support for BasicAuth and OAuth 2.0. Bigquery uses OAuth 2.0 to authenticate and authorize requests. The Bigquery connector can be minimally instantiated in the HTTP client config using the access token or the client ID, client secret, and refresh token.

###Obtaining Tokens to Run the Sample

1. Visit Google API Console, click Create Project, and follow the wizard to create a new project.
2. Go to Credentials -> OAuth consent screen, enter a product name to be shown to users, and click Save.
3. On the Credentials tab, click Create credentials and select OAuth client ID.
4. Select an application type, enter a name for the application, and specify a redirect URI (enter https://developers.google.com/oauthplayground if you want to use OAuth 2.0 playground to receive the authorization code and obtain the access token and refresh token).
5. Click Create. Your client ID and client secret appear.
6. In a separate browser window or tab, visit OAuth 2.0 playground, select the required Bigquery scopes, and then click Authorize APIs.
7. When you receive your authorization code, click Exchange authorization code for tokens to obtain the refresh token and access token.

You can now enter the credentials in the HTTP client config:
```ballerina

bigquery2:BigqueryConfiguration bigqueryConfig = {
    clientConfig: {
        auth: {
            scheme: http:OAUTH2,
            accessToken:testAccessToken,
            clientId:testClientId,
            clientSecret:testClientSecret,
            refreshToken:testRefreshToken
        }
    }
};

bigquery2:Client bigqueryClient = new(bigqueryConfig);
```
The `listProjects` function lists all projects to which current user have been granted any project role.
```ballerina
// List projects.
var response = bigqueryClient->listProjects();
```
The response from `listProjects` is a `ProjectList` object if the request was successful or a `error` on failure.
```ballerina
    if (bigqueryRes is ProjectList) {
        io:print(bigqueryRes);
    } else {
        // Error will be printed
        test:assertFail(msg = <string>bigqueryRes.detail().message);
    }
```
