# PAM Auth Request Service.

Used with NGINX [ngx_http_auth_request_module](http://nginx.org/en/docs/http/ngx_http_auth_request_module.html)

## Sample nginx config:

```
  ## just add auth_request /pam_auth; to any location
  location = /pam_auth {
    internal;
    proxy_pass              http://localhost:3043;
    proxy_pass_request_body off;
    proxy_set_header        Content-Length "";
    proxy_set_header        X-Original-URI $request_uri;
    proxy_cookie_domain     localhost <nginx_configured_hostname>;
    proxy_cookie_domain     <nginx_configured_hostname> localhost;
  }


  location ^~ /<prefix> {
    auth_request /pam_auth;
    ...
  }
```

## Run PAM Auth Service

```bash
 $ ./authpam
```


## .authpam whitelist configuration

If .authpam file present, PAM auth service will use it during autentication.
It can be used to configure group wihide, or explicit list of users in the group
```json
{
  "groups": {
    "group_name1": ["alex", "bob", "alice"], ## allow any user listed in this group
    "group_name2": [] ## allow any user with in this goup
  }
}
```
