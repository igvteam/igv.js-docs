In general, servers hosting data for use by igv.js must support HTTP Range headers and Cross-Origin Resource Sharing (CORS).  
We use Apache servers as well as Amazon S3 buckets, but most popular servers can be configured to support these requirements.  

## Range requests

For indexed files, the data are retrieved on demand as needed for display. To support this, the data web server must 
support HTTP _Range_ requests in order to allow retrieval of a subset of the data in the file. Many production web 
servers support Range headers by default. Some servers, e.g. Flask, may require configuration settings to add support.  


## Cross-Origin Resource Sharing (CORS)

Data served over HTTP/HTTPS to igv.js, or any other JavaScript application, must include CORS headers unless the data file is served from the same host as the web page containing the JavaScript application.    For a good introduction to CORS see [https://www.html5rocks.com/en/tutorials/cors/](https://www.html5rocks.com/en/tutorials/cors/).  As an example, below are the settings used for our test Apache server.  Note that these might not be appropriate for your data server.  These settings are configured in an _.htaccess_ file. For details on setting response headers for your server, see the server documentation.

    <IfModule mod_headers.c>
        Header set Access-Control-Allow-Origin "*"
        Header set Access-Control-Allow-Methods: "GET,POST,PUT,OPTIONS"
        Header set Access-Control-Allow-Headers: "RANGE, Cache-control, If-None-Match, Content-Type"
        Header set Access-Control-Expose-Headers: "Content-Length, Content-Range, Content-Type"
    </IfModule>

If serving data from an Amazon S3 bucket, CORS headers can be figured as a property of the bucket.  See Amazon documentation for details.  For our public test data, the headers are configured as follows:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>*</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>HEAD</AllowedMethod>
    <MaxAgeSeconds>3000</MaxAgeSeconds>
    <ExposeHeader>Content-Length</ExposeHeader>
    <ExposeHeader>Content-Type</ExposeHeader>
    <ExposeHeader>Content-Range</ExposeHeader>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```
## BAM Mime Type

In some cases it might be necessary to explicitly define a mime type for ".bam" files when using an Apache server. 
If you encounter errors such as "invalid stored block length" or "invalid block type" when accessing a .bam file 
check the response headers for  "Content-encoding: gzip".   If this header is present you will need to explicitly 
define a mime type for the .bam extension.  This can be done in httpd.conf as follows  

AddType application/octet-stream .bam

  
