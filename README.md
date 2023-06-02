# ⚠️ The OFFICIAL docker image is avalable here: https://github.com/Stremio/server-docker. 

**This project is no longer maintained.** 

---

# Portable Streaming Server for Stremio
This docker image allows you to run Stremio's streaming server as a standalone server (i.e. separate from the desktop application). You could drop a streaming server container on a separate linux-based server and remotely connect your Stremio interface to it. Or, you could run this together with Stremio 5 (a.k.a [stremio-web](https://github.com/Stremio/stremio-web), which is currently in **alpha**).

If you're interested in building Stremio components yourself, check out the instructions at the [stremio-shell](https://github.com/Stremio/stremio-shell/) repository.

Every 24 hours a bot checks for a new version of Stremio's streaming server. When a new version is found, a new tag will be published to this repository, which in turn triggers a rebuild on docker hub. Please open an issue if you believe a version is missing.

## Usage
Execute the following command to pull and run the docker image:

`$ docker run -p 11470:11470 -e FIX_UNSUPPORTED_MEDIA=1 -v ${PWD}:/root/.stremio-server --name=stremio-streaming-server sleeyax/stremio-streaming-server`

This will run the **latest version** of the streaming server, map the configuration directory `.stremio-server/` to your current working directory `${PWD}` and expose it on port `11470`. A patch to fix unsupported media errors is also applied here. See below for more information about [patches](#patches).

### Patches
By default, all streaming server dependencies are downloaded and stored unmodified.
You may want to apply one or more patches to improve usability for your use case though.
Patches can be applied at container creation via environment variables: `$ docker run [ARGS] -e PATCHNAME=1 sleeyax/stremio-streaming-server`.
Set to `1` to enable and `0` to disable (all patches are disabled by default).

Available patches:

`FORCE_HTTPS`

Enable this if you are using a reverse HTTPS proxy (e.g localtunnel) to access the streaming server. See [#10#issuecomment-1174508779](https://github.com/sleeyax/stremio-streaming-server/issues/10#issuecomment-1174508779) for more information about why enabling this is recommended.

`FIX_UNSUPPORTED_MEDIA`

Attempts to fix 'unsupported media' errors. Only enable this if you are experiencing this exact issue as this patch is not well tested ([source](https://github.com/n0bodysec/docker-images/blob/main/stremio-web/patches.sh)).

### Advanced
Normally the latest version should be fine but if for some reason you'd like to run a different version of the streaming server, follow these steps:
1. Clone this repository `$ git clone https://github.com/sleeyax/stremio-streaming-server && cd stremio-streaming-server`.
2. Change the `VERSION` build argument. Examples: `ARG VERSION=v4.4.148`, `ARG VERSION=master`. You can also override this argument with the `--build-arg` flag while running the command below.
3. rebuild the container `$ docker build -t sleeyax/stremio-streaming-server .` or `docker buildx build --platform linux/arm64,linux/arm/v7 -t sleeyax/stremio-streaming-server .` for multi-arch builds.

---

**This project is not officialy maintained by Stremio.**
