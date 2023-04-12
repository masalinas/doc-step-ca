## Description
Documentation of step-ca certificates manager service

## Running service

```
docker run -it --name step-ca -v step:/home/step \
    -p 9000:9000 \
    -e "DOCKER_STEPCA_INIT_NAME=Smallstep" \
    -e "DOCKER_STEPCA_INIT_DNS_NAMES=localhost,$(hostname -f)" \
    -e "DOCKER_STEPCA_INIT_REMOTE_MANAGEMENT=true" \
    smallstep/step-ca
```  

Response

```
Generating root certificate... done!
Generating intermediate certificate... done!

✔ Root certificate: /home/step/certs/root_ca.crt
✔ Root private key: /home/step/secrets/root_ca_key
✔ Root fingerprint: 4cbdddc13d630bd2fa58fe8caa19a2162b00870107ad882887ee03a0543498e6
✔ Intermediate certificate: /home/step/certs/intermediate_ca.crt
✔ Intermediate private key: /home/step/secrets/intermediate_ca_key
badger 2023/04/12 17:59:21 INFO: All 0 tables opened in 0s
✔ Database folder: /home/step/db
✔ Default configuration: /home/step/config/defaults.json
✔ Certificate Authority configuration: /home/step/config/ca.json
✔ Admin provisioner: admin (JWK)
✔ Super admin subject: step

Your PKI is ready to go. To generate certificates for individual services see 'step help ca'.

FEEDBACK 😍 🍻
  The step utility is not instrumented for usage statistics. It does not phone
  home. But your feedback is extremely valuable. Any information you can provide
  regarding how you’re using `step` helps. Please send us a sentence or two,
  good or bad at feedback@smallstep.com or join GitHub Discussions
  https://github.com/smallstep/certificates/discussions and our Discord
  https://u.step.sm/discord.

👉 Your CA administrative username is: step
👉 Your CA administrative password is: PiODFAQQhaQybKXXUMqSMZtD3FVg9n72tKcC9ynW
🤫 This will only be displayed once.
badger 2023/04/12 17:59:21 INFO: All 0 tables opened in 0s
badger 2023/04/12 17:59:21 INFO: Replaying file id: 0 at offset: 0
badger 2023/04/12 17:59:21 INFO: Replay took: 37.875µs
2023/04/12 17:59:21 Starting Smallstep CA/0.23.2 (linux/arm64)
2023/04/12 17:59:21 Documentation: https://u.step.sm/docs/ca
2023/04/12 17:59:21 Community Discord: https://u.step.sm/discord
2023/04/12 17:59:21 Config file: /home/step/config/ca.json
2023/04/12 17:59:21 The primary server URL is https://localhost:9000
2023/04/12 17:59:21 Root certificates are available at https://localhost:9000/roots.pem
2023/04/12 17:59:21 Additional configured hostnames: MacBook-Pro-de-Miguel-2.local
2023/04/12 17:59:21 X.509 Root Fingerprint: 4cbdddc13d630bd2fa58fe8caa19a2162b00870107ad882887ee03a0543498e6
2023/04/12 17:59:21 Serving HTTPS on :9000 ...
```

## Install step CLI

In linux Debian OS

```
wget https://dl.step.sm/gh-release/cli/docs-cli-install/v0.23.4/step-cli_0.23.4_amd64.deb
sudo dpkg -i step-cli_0.23.4_amd64.deb
```

In Mac OS

```
brew install step
```

## Configure step CLI

```
step ca bootstrap --ca-url localhost:9000 --fingerprint 4cbdddc13d630bd2fa58fe8caa19a2162b00870107ad882887ee03a0543498e6

The root certificate has been saved in /Users/miguel/.step/certs/root_ca.crt.
The authority configuration has been saved in /Users/miguel/.step/config/defaults.json.
```

## register certificates to be trusted from our CA

```
step certificate install $(step path)/certs/root_ca.crt
```

## Using the service

Generating a private key and obtaining a signed certificate

```
step ca certificate svc.example.com svc.crt svc.key --ca-url localhost:9000

Please enter the password to decrypt the provisioner key:
✔ CA: https://localhost:9000
✔ Certificate: svc.crt
✔ Private Key: svc.key
```

We must introduce the CA administrative password to continue

## Some links

* [step-ca dockerhub](https://hub.docker.com/r/smallstep/step-ca/)
