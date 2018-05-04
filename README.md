# HTTPS for `localhost`

A set of scripts to quickly generate a HTTPS certificate for your local development environment.

## How-to

1. Clone this repository and `cd` into it:

```
git clone https://github.com/dakshshah96/local-cert-generator.git
cd local-cert-generator
```
2. Run the script to create a root certificate:

```
sh createRootCA.sh
```

3. Trust this certificate after importing it to your System keychain:

    - For Mac: use Keychain Access

    ![Trust root certificate](https://cdn-images-1.medium.com/max/1600/1*NWwMb0yV9ClHDj87Kug9Ng.png)

    - For Linux: use `trust`, `update-ca-certificates` or any command used by your distribution

    Note: you may need to restart your browser to make it correctly load the trusted rootCA

4. Run the script to create a domain certificate for `localhost`: 

```
sh createSelfSigned.sh
```

5. Move `server.key` and `server.crt` to an accessible location on your server and include them when starting it. In an Express app running on Node.js, you'd do something like this:

```js
var path = require('path')
var fs = require('fs')
var express = require('express')
var https = require('https')

var certOptions = {
  key: fs.readFileSync(path.resolve('build/cert/server.key')),
  cert: fs.readFileSync(path.resolve('build/cert/server.crt'))
}

var app = express()

var server = https.createServer(certOptions, app).listen(443)
```
