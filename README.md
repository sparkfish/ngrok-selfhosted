## ngrok Relay Setup

This repository provides a script and outlines a procedure for building ngrok clients, and initial server setup.

## Overview

ngrok is a tool that tunnels requests from `localhost` over a public endpoint.

It can be used to expose services over the internet from behind a NAT or firewall.

ngrok was open source through version 1.7 with source [available on GitHub](https://github.com/inconshreveable/ngrok). The current version (2.x) is closed-source.

When ngrok 1.7 is built, the result is a pair of binaries (one for the client, and one for the server) that are associated with an OpenSSL certificate. Therefore certificates must be generated prior to the build process if HTTPS is required.

## Requirements

- [Docker Desktop>=17.0](https://www.docker.com/products/docker-desktop)
- [openssl](https://www.openssl.org/)
- bash

## Script

When invoked, the included `setup` script does the following

1. It generates the required certificates using OpenSSL on the host
2. It builds a Docker golang environment (defined in `gateway/Dockerfile`)
3. The Docker environment prepares ngrok client and server binaries against the SSL certificates generated in the host
4. The client and server binaries are copied the back to the host from the container
4. An ngrok relay server is started within the container
5. An image running a test service is built  (defined in `test_service/Dockerfile`)
6. The test service is started
7. A client configuration file is written to `ngrok-config`

During the run, if existing Docker containers corresponding to the relay or test service images are found to be running, they will be killed.

## Usage

Before running the script, configure your `/etc/hosts` file (or similar) to include the following records (or equivalent):

```
	127.0.0.1       ngrok.local
	127.0.0.1       test.ngrok.local
```

Then ensure the script is executable (`chmod +x setup` or similar), and run it with `./setup`.

## Configuration

To use a different `NGROK_DOMAIN` (`ngrok.local` above), do `NGROK_DOMAIN=my.custom.domain ./setup`.

To build client binaries for different architectures or platforms, do `./setup <arch> <os>`, e.g. `./setup amd64 darwin` (the default is `amd64 linux`).
