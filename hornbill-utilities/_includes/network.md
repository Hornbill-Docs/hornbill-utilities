## Network

The utility connects to the Hornbill instance in the cloud over HTTPS/SSL, so as long as you have standard internet access then you should be able to use it without the need to make any proxy or firewall configuration changes.

### HTTP Proxies
If you use a proxy for all of your internet traffic, the HTTP_PROXY and HTTPS_PROXY Environment variables need to be set. These environment variables hold the hostname or IP address of your proxy server. It is a standard environment variable and like any such variable, the specific steps you use to set it depends on your operating system.

For Windows machines, it can be set from the command line using the following:
```bat
set HTTP_PROXY=HOST:PORT
set HTTPS_PROXY=HOST:PORT
```

Where `HOST` is the IP address or hostname of your Proxy Server and `PORT` is the specific port number. If you require a username and password to go through the proxy, the format for the setting is as follows:
```bat
set HTTP_PROXY=username:password@HOST:PORT
set HTTPS_PROXY=username:password@HOST:PORT
```

### URLs to White List
Occasionally, on top of setting the HTTP_PROXY and HTTPS_PROXY variables, the following URLs may need to be white-listed to allow access out from your network to the required endpoints in the Hornbill network:

- `https://files.hornbill.com/instances/INSTANCENAME/zoneinfo` - Allows access to lookup your Instance API Endpoint
- `https://files.hornbill.co/instances/INSTANCENAME/zoneinfo` - Backup URL for when files.hornbill.com is unavailable
- `https://mdh-p01-api.hornbill.com/INSTANCENAME/xmlmc/` - This is your Instance API Endpoint, where `mdh-p01-api` is specific to your region and instance. See the [API Concepts documentation](/esp-api-api/concepts#api-endpoint) for more information.
- `https://api.github.com/repos/hornbill/asset-rel-import/tags`- Allows the utility to self-update. Optional
- `https://\*.hornbill.com/\*` - allows access to the required Hornbill instance information and API endpoints
- `https://api.github.com/repos/hornbill/\*` - allows the utility to self-update
