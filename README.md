# AdobeAnalyticsClient
Adobe Analytics API client (support to both 1.4 and 2.0) for Google App Script (.gs)

## Requirements
1. Created Google App Script Project, and edit right enabled. The library makes API calls to external Adobe endpoint, to be sure to Allow in your Google account
2. JWT authentication enabled Adobe IO integration with sufficent right to access relevant Adobe Analytics resources
3. Access to Adobe IO console to retrieve all configuration options

Links to Adobe API documents:

1.4: https://github.com/AdobeDocs/analytics-1.4-apis

2.0: https://github.com/AdobeDocs/analytics-2.0-apis


## Installation
1. Go to your Google spreadsheet > Tools > Script editor
2. On opened app script project, navigate to Resources > Libraries, then in Add a library field, copy and paste in project ID `MFKxOa7HSAfXZBCca0icJ7FS-o4wWKLZ2`
3. Click Add then Save, make sure no error message is shown

## Example Code

### Initialization

API client instance is created by calling:

```
var client = AdobeAnalyticsClient.init();
```


### Configuration

API client needs configurations from your Adobe IO integration for JWT authentication.
Read more here: https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/AuthenticationGuide.md

```
function getAdobeAnalyticsClient () {
  return AdobeAnalyticsClient.init()
  .setClientId('') // set client ID
  .setClientSecret('') // set client secret
  .setPrviatekey('') // string format private key, replace line breaks with /n, generated when Adobe IO integration was created with JWT
  .setIss('') // set iss
  .setSub('') // set sub
  .setAud('') // set aud
  .setTokenLifeSec(); // set bearer token lifetime in seconds
}

function callAPI () {
  var client = getAdobeAnalyticsClient();
  if(client.bearerTokenReady()){ // verify bearer token is received when token is exchanged
    Logger.log("Ready");
    // Call invoke function
  }
   
}

```


### Calling Adobe Analytics API

API client support both 1.4 and 2.0 Adobe API, however, the invoker needs different parameters to execute those API calls as 2.0 requires more verbs while 1.4 uses POST

#### 1.4 Example: List all data sources
```
var result = client.invoke ('1.4', 'DataSources.Get', {'reportSuiteID' : 'my_report_suite_id'});
Logger.log(result); // Log recieved API response
```

#### 2.0 Example: Get user me
```
var result = client.invoke('2.0','/users/me',{},'company-id',{'method':'GET'});
Logger.log(result);
```

## TODOs
1. API 2.0 support is not fully tested, use with cautious
2. To add JWT token auto-renewal

## License
MIT