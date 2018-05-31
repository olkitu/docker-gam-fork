# Docker Image for Dito GAM

[![Docker Hub](https://img.shields.io/docker/pulls/broadinstitute/gam.svg)](https://hub.docker.com/r/broadinstitute/gam/)
[![Docker Hub](https://img.shields.io/docker/build/broadinstitute/gam.svg)](https://hub.docker.com/r/broadinstitute/gam/)
[![Docker Repository on Quay](https://quay.io/repository/broadinstitute/gam/status "Docker Repository on Quay")](https://quay.io/repository/broadinstitute/gam)

[https://github.com/jay0lee/GAM](https://github.com/jay0lee/GAM)

## Setting up tokens for GAM

Full instructions for GAM setup can be located at: [https://github.com/jay0lee/GAM/wiki#1-enabling-the-apis](https://github.com/jay0lee/GAM/wiki#1-enabling-the-apis)

Specifically, you will need to enable the APIs required by GAM as well as get OAuth2 tokens by which GAM can access your domain through the APIs.  The following link has instructions on the specifics of enabling the APIs and generating the token files needed by GAM:

[https://github.com/jay0lee/GAM/wiki/CreatingClientSecretsFile](https://github.com/jay0lee/GAM/wiki/CreatingClientSecretsFile)

Both of the files generated by this process (`client_secrets.json` and `oauth2service.json`) will be used by this container via volume mounts to import the credentials to the disposable container per run.

## Quick Start

Make sure the `client_secrets.json` and `oauth2service.json` files have been created prior to running the container, and also touch the `oauth2.txt` file that will be written to by the container.  You can then run one-off commands like:

```sh
sudo docker run -it --rm \
  -v /path/to/client_secrets.json:/gam/client_secrets.json:ro \
  -v /path/to/oauth2.txt:/gam/oauth2.txt \
  -v /path/to/oauth2service.json:/gam/oauth2service.json:ro \
  broadinstitute/gam:latest \
  /usr/bin/gam.sh info domain
```

The GitHub repository ([https://github.com/broadinstitute/docker-gam](https://github.com/broadinstitute/docker-gam)) for this container also contains a `gam` script with an accompanying `config.sh` script that can be used to more easily run this container as you would run GAM normally without a container.

```sh
./gam info domain
```

### Mounted Volumes

Storing these JSON files inside the container is a *VERY BAD* idea, since anyone with the container would then be able to act as an admin on your Google Apps domain.  This is why we manually volume mount the files in per run of the container so that the files can be securely managed outside of the container context to prevent misuse.

### Base Image

Built using the DockerHub base [Alpine](https://hub.docker.com/r/library/alpine/) 3.7 image
