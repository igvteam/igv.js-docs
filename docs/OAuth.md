
This section describes support for OAuth access tokens.  Special thanks to @vrockai for his 
significant contributions here. 

Files provided behind an OAuth secured endpoint need a bearer access token to be accessed. The  access token can 
be provided as a string, a function, a promise, or a function that returns a promise.

## Setting the token in the track configuration

1. As a function returning the token string itself or a promise:
```json
{
  "name": "Phase 3 WGS variants",
  "format": "vcf",
  "url": "https://s3.amazonaws.com/1000genomes/release/20130502/ALL.wgs.phase3_shapeit2_mvncall_integrated_v5b.20130502.sites.vcf.gz",
  "indexURL":  "https://s3.amazonaws.com/1000genomes/release/20130502/ALL.wgs.phase3_shapeit2_mvncall_integrated_v5b.20130502.sites.vcf.gz.tbi",
  "type": "variant",
  "oauthToken": myOauthTokenFn
}
```
2. As a string:
```json
{
  "name": "Phase 3 WGS variants",
  "format": "vcf",
  "url": "https://s3.amazonaws.com/1000genomes/release/20130502/ALL.wgs.phase3_shapeit2_mvncall_integrated_v5b.20130502.sites.vcf.gz",
  "indexURL":  "https://s3.amazonaws.com/1000genomes/release/20130502/ALL.wgs.phase3_shapeit2_mvncall_integrated_v5b.20130502.sites.vcf.gz.tbi",
  "type": "variant",
  "oauthToken": "F0jh9korTyzd9kaZqZ0SzjKZuS3ut0i4P4234352m2JYHiLIcqzFAumpyxshU9mMQ13gJHtxD2fy"
}
```

The function approach (1.) is preferred because it's a safe way how to provide up-to-date token to the igv.js browser 
all the time. The value approach (2.) will work only for a life of the token and the browser would need to be refreshed 
after the token is expired. The function approach works out of the box with promises. The `myOauthTokenFn` could look 
like this:

```js
function myOauthTokenFn() {
  return $http.get(myOauthEndpointURL).then(parseAccessToken);      
}
```

## Setting the token globally

To set a token for all requests to a particular host or hosts

```javascript

igv.setOauthToken(accessToken, "*foobar*");

```

The above example will include the supplied bearer access token with every request to hosts matching the 
string "*foobar*",  for example  "foobar.data.org".

As illustrated in the track configuration examples accessToken can be a string, function, or promise.

## Setting a Google access token

For Google resources  (Google Cloud Storage and  Google Drive) the oAuth access token can 
alternatively be set globally as shown below.  The token will be added as an Authorization header to all requests to 
google servers.  The test for google server is host name matches  *googleapis* or protocol is gs://.

``` js
igv.setGoogleOauthToken(accessToken)
```