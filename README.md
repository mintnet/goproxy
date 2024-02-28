# goproxy

This package provides enhanced support for HTTP requests in Go, enabling seamless integration of proxy servers into your applications. It allows for HTTP GET requests and other operations through proxies, supporting both IPv4 and IPv6 addresses, HTTPS proxies, and proxies that require authentication.

## Features

- **Easy Integration**: Integrates with the `net/http` package to allow for easy implementation into existing Go applications.
- **IPv4 and IPv6**: Compatible with proxy servers accessible via both IPv4 and IPv6 addresses.
- **HTTPS Proxy Support**: Supports routing requests through HTTPS proxies, enhancing security and privacy.
- **Authenticated Proxy Support**: Supports routing requests through proxies that require authentication in the user:pass@ip:port format.

## Installation

To include this package in your project, use the following command:

```bash
go get -u github.com/mintnet/goproxy
```

## Usage

### Basic HTTP GET Request Through a Proxy

```go
package main

import (
    "fmt"
    "crypto/tls"
    "net/http"

    "github.com/mintnet/goproxy"
)

func main() {
    // Define the proxy URL
    proxyURL, err := goproxy.Parse("http://username:password@ip:port")
    if err != nil {
        panic(err)
    }

    // Configure the HTTP client to use the proxy
    httpClient := &http.Client{
        Transport: &http.Transport{
            Proxy: http.ProxyURL(proxyURL),
            TLSClientConfig: &tls.Config{
                InsecureSkipVerify: true, // Consider enabling this for testing with self-signed certificates
            },
        },
    }

    // Make an HTTP GET request through the proxy
    resp, err := httpClient.Get("http://example.com")
    if err != nil {
        panic(err)
    }
    defer resp.Body.Close()

    // Process the response
    fmt.Println("Response status:", resp.Status)
}
```

This example demonstrates how to configure an HTTP client to route requests through an authenticated proxy server. Adjust the `proxyURL` to match your proxy server's credentials, address, and port.

## Contributing

Contributions to this package are welcome! Please feel free to submit issues, feature requests, or pull requests.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.