# Security Certificates

![Manage certificates](images/manage-certificates.png)

On the **Manage domains** page you'll also find the option to upload and configure SSL certificates for your Cloud environments.

Your certificates need to be `.pfx` format and must be set to use a password. Each certificate can then be bound to a hostname you have already added to your site. Make sure you use the hostname you will bind the certificate to as the common name (CN) when generating the certificate.

## Running your site on HTTPS only

Once you've applied a certificate to your site you can make sure that anybody visiting your site will always end up on HTTPS instead of the insecure HTTP.

To accomplish this, add a rewrite rule to the live environment's `web.config` in the `<system.webServer><rewrite><rules>` section. 

For example, the following rule will redirect all requests for the site http://mysite.com URL to the secure https://mysite.com URL and respond with a permanent redirect status. 

    <rule name="HTTP to HTTPS redirect" stopProcessing="true">
      <match url="(.*)" />
      <conditions>
        <add input="{HTTPS}" pattern="off" ignoreCase="true" />
        <add input="{HTTP_HOST}" pattern="localhost" negate="true" />
      </conditions>
      <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" redirectType="Permanent" />
    </rule>        

**Note:** This redirect rule will not apply when the request is already going to the secure HTTPS URL. This redirect rule will also not apply on your local copy of the site running on `localhost`.
