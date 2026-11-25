---
title: GPXDash Support
description: Getting started, prerequisites, live sharing, privacy, and support for GPXDash.
---

# GPXDash Support

GPXDash is a server-based GPX library and browser dashboard for viewing, organizing, and sharing GPS tracks.

**GPXDash and GPSDash are separate products:**

- **GPXDash** provides the server, browser dashboard, and server-side library.
- **GPSDash** is the iPhone and iPad application. Connecting it to GPXDash requires a compatible app version and a configured server.

This repository provides public documentation and support information. It does not provide a hosted GPXDash account or automatically install a server.

## Contents

- [Prerequisites](#prerequisites)
- [Getting started](#getting-started)
- [Working with tracks](#working-with-tracks)
- [Connecting GPSDash](#connecting-gpsdash)
- [Live sharing](#live-sharing)
- [Embedding a shared map](#embedding-a-shared-map)
- [Privacy and security](#privacy-and-security)
- [Troubleshooting](#troubleshooting)
- [Getting help](#getting-help)

## Prerequisites

### To use the browser dashboard

You need:

- Access to an existing GPXDash server.
- The server's dashboard address, provided by its administrator.
- A GPXDash account and its sign-in credentials.
- A current web browser with JavaScript and cookies enabled.
- A network connection to the server.

An Internet connection may also be needed for external map tiles.

**There is no universal GPXDash server address.** Use the address supplied by the person operating your installation.

For remote access, the administrator must configure secure HTTPS access. Do not bypass browser certificate warnings or send credentials over an unsecured connection.

### To import existing tracks

You need GPX files that you are authorized to upload.

Keep a separate copy of important original files. A server library is not a substitute for an independent backup.

### To connect the GPSDash app

You need:

- GPSDash installed on your iPhone or iPad.
- An app version that supports your server's connection and upload protocol.
- An operational GPXDash installation with device access configured by its administrator.
- Access to the dashboard's device setup controls.
- A network connection from the device to the configured device service.

Device access may use a different address from the browser dashboard. Use the connection information generated or supplied for your installation.

**Compatibility matters
