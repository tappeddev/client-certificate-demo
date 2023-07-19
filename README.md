Client Certificate Demo
=======================

The server's private key is in `server.key`, which we use for our server certificate (`server.pem`). This is a self-signed certificate, issued by ourselves (Demo CA).

We have two PKCS#12 client certificates
 - `alice.pfx` which was issued by us (the issuer is Demo CA)
 - `bob.pfx`, which is a self-signed certificate (the issuer is Bob himself)

To try the demo, import both client certificates into your browser. In case of Firefox, go to Settings -> Advanced -> View certificates -> Import. Leave the passphrase empty.

Start the server with `npm install && npm start`, open https://localhost:9999 in your browser, click on the link (you have to add an exception to make the browser accept our "Demo CA" certificate). It should show you the subject's common name (Alice), and the issuer's common name (Demo CA).

Notice that the browser only offers Alice's certificate: Bob's certificate is not issued by Demo CA, so it cannot be used.

You can circumvent this by using cURL to call the authenticate endpoint. Note the `--insecure` option: we need this to make cURL accept our Demo CA server certificate.

```
$ curl --insecure --cert alice.pfx --cert-type p12 https://localhost:9999/authenticate
Hello Alice, your certificate was issued by Demo CA!
$ curl --insecure --cert bob.pfx --cert-type p12 https://localhost:9999/authenticate
Sorry Bob, certificates from Bob are not welcome here.
```

Add the key to the heroku server: `heroku certs:add server_cert.pem server_key.pem`

------------

First of all, we need to setup the api

1. Call `npm install`
2. Call `npm start`

After we also have the api, we could go to postman and setup the request

1. Add a new request `get` request 
2. The url need to be `https://localhost:9999/authenticate`
3. Go to the settings on the top right edge
4. Click _Certificates_
5. Click _Add Certificates_
6. Add the following information:
   1. Host = localhost / 9999
   2. PFX file = select either `alice.p12` (working) or `bob.p12` (not working)
   3. Passphrase = 1234
