# How to load balance Jitsi Meet with multiple Videobridge

## Introduction
In this document, we going to link the Jitsi meet installed in [previous article](./README.md) with multiple Videobridges.
Videobridge communicates using XMPP protocol, so we can setup a XMPP Pubsub node that listens to all Videobridge nodes, and pushes the events to Jicofo.

## Prerequisits
In this article, we assume you have two server machines, one (Server A) with default Jitsi Meet installation, and the other (Server B) is empty.

- Debian machine with [Default Jitsi Meet installation](README.md) (Server A)
- Debian machine OS 9 (Skretch) or later  (Server B)

## Step 1. Configuration Change to Server A 
First, ssh onto Server A machine, again here we assume that you have Jitsi meet installed on Server A. You can refer to [this guide for default Jitsi Meet installation](README.md).

### a.) Add the JIDs of the Videobridges as Admins
Edit the Prosody configuration in `/etc/prosody/conf.d/<dnsname>.cfg.lua`, add the following line under `authentication = ...`

```sh
  admins = {
    "jitsi-videobridge.<dnsname>",
    "videobridge2.<dnsname>",
  }
```
So your configuration file should look like below.

```sh
VirtualHost "<dnsname>"
  -- enabled = false -- Remove this line to enable this host
  authentication = "internal_plain" //Note this value can be "anonymous", if you haven't enabled authentication
  admins = {
    "jitsi-videobridge.<dnsname>",
    "videobridge2.<dnsname>",
  }
  ...
```

### b.) Change the components interface from localhost to public interface
