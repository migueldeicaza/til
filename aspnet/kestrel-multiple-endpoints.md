I am running a handful of small ASP.NET servers, and the Kestrel
server is enough for my use case.    I am using Kestrel with proper
HTTPS certificates created using Let's Encrypt.

I wanted to have my Kestrel server listen to request for different
domains, and could not find a good reference to this online, so I
wanted to document what I did.

Inside the `"EndPoints"` section inside the `"Kestrel"` section of
`appsettings.json` it turns out that you are not limited to using the
names "Http" and "Https", those seemed to be just arbitrary labels,
and you can create your own.

To work with multiple domains and multiple certificates, you need to
add a section "Sni", like this:

```
"Kestrel": {
    "EndPoints": {
        "Geo": {
            "Url": "https://*:443",
            "Sni": {
                "service1.example.com": {
                    "Certificate": {
                        "Path": "/etc/letsencrypt/live/service1.example.com/cert.pem",
                        "KeyPath": "/etc/letsencrypt/live/service1.example.com/privkey.pem"
                    }
                },
                "api.non-profit.org": {
                    "Certificate": {
                        "Path": "/etc/letsencrypt/live/api.non-profit.org/cert.pem",
                        "KeyPath": "/etc/letsencrypt/live/api.non-profit.org/privkey.pem"
                    }
                },
                "*": {
                    // At least one subproperty needs to exist per SNI section or it
                    // cannot be discovered via IConfiguration
                    "Protocols": "Http1",
                }
            }
        },
        "Http": {
            "Url": "http://0.0.0.0:80"
        }
    }
}
```