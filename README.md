---
title: GPXDash Support
description: Server prerequisites, getting started, live sharing, privacy, and support for GPXDash.
---

# GPXDash Support

GPXDash is a self-hosted server and browser dashboard for storing, viewing, organizing, and sharing GPS tracks.

**GPXDash and GPSDash are separate products:**

- **GPXDash** provides the server, browser dashboard, and server-side track library.
- **GPSDash** is the iPhone and iPad application. Connecting it to GPXDash requires a compatible app version and a configured server.

You operate GPXDash on your own infrastructure or use an installation managed by someone you trust. This repository provides support information; it does not provide a hosted account or automatically install a server.

## Contents

- [What GPXDash provides](#what-gpxdash-provides)
- [Server prerequisites](#server-prerequisites)
- [Getting started](#getting-started)
- [Working with tracks](#working-with-tracks)
- [Connecting GPSDash](#connecting-gpsdash)
- [Live sharing](#live-sharing)
- [Embedding a shared map](#embedding-a-shared-map)
- [Privacy and security](#privacy-and-security)
- [Troubleshooting](#troubleshooting)
- [Getting help](#getting-help)
- [Availability and compatibility](#availability-and-compatibility)

## What GPXDash provides

GPXDash brings your server-side track library into a web browser.

It provides:

- A map-based dashboard for inspecting tracks.
- GPX import and export.
- Track search, renaming, organization, and deletion.
- Device connection and recording uploads when configured with a compatible GPSDash app.
- Controlled sharing of live information and selected historical tracks.
- Website map embedding and read-only access to authorized shared data.

Your installation's available features depend on its version and configuration.

## Server prerequisites

GPXDash is self-hosted software. Before setting up a server, you need the following.

### 1. A machine to host it

An always-on server, virtual machine, or compatible NAS capable of running Docker containers.

- A Linux host is recommended for the current deployment configuration.
- The container dependencies support x86-64 and ARM64 architectures.
- Persistent disk storage is required for the PostgreSQL database.
- Storage requirements grow with the number of recordings and location points you retain.

**Hardware minimums have not yet been established.** The resource limits in the supplied Compose configuration are not minimum hardware requirements.

### 2. Docker and Docker Compose

Install Docker Engine and the Docker Compose plugin on the host.

GPXDash runs separate services for:

- The private browser dashboard.
- The PostgreSQL database.
- Device connections and uploads, when enabled.
- Public sharing and embedded maps, when enabled.

You do not need to install Python or PostgreSQL separately on the host when using the container deployment.

### 3. The GPXDash installation files

You need access to the GPXDash server source and its Docker build and Compose files.

The current configuration builds the application image from source. This support repository alone is not the server installer; do not assume that a publicly downloadable, ready-to-run application image is available.

### 4. A domain and HTTPS access

For remote device connections and public sharing, you need:

- A domain name with DNS records you can manage.
- HTTPS addresses with valid TLS certificates.
- A configured access gateway or reverse proxy that satisfies GPXDash's trusted-proxy and HTTPS requirements.
- Connectivity between that gateway and the GPXDash host.

The private dashboard, device service, and public-sharing service have different access requirements. Configure their routing separately rather than exposing the private dashboard to enable another service.

The existing deployment uses **Pangolin**. Device access must support noninteractive requests from the app; an interactive browser sign-in page is not sufficient for background uploads.

**Do not expose PostgreSQL or unprotected application ports directly to the Internet.**

### 5. Persistent storage and installation secrets

Before starting the server, prepare:

- The persistent database directory.
- The persistent support-log directory required by the supplied Compose configuration.
- A database password.
- A separate database credential for the public-sharing service, if enabled.
- A unique authentication-encryption key for the installation.
- The secret files expected by the deployment configuration, including empty files for unused optional settings where required.

Keep secrets outside source control and restrict their file permissions.

**Preserve the authentication-encryption key.** Replacing or losing it can make stored authenticator secrets unreadable.

### 6. A backup and recovery plan

Arrange backups of:

- The PostgreSQL database.
- Required installation secrets, particularly the authentication-encryption key.
- The configuration needed to recreate the installation.

Store backups separately from the running database and establish how you will restore them. A backup file that has never been restored is not proof that recovery works.

### 7. An administrator

Someone must be able to:

- Configure Docker, persistent storage, DNS, HTTPS, and gateway routing.
- Create the first GPXDash administrator account.
- Maintain updates and backups.
- Troubleshoot connectivity and account recovery.

Complete first-account setup from a trusted environment before enabling public access.

### Optional: outbound email

An SMTP server or email-delivery service is needed for password recovery by email.

Without SMTP configuration, email-based password recovery is unavailable.

### Optional: a compatible GPSDash app

The GPSDash iPhone/iPad app is needed only if you want to connect a device and upload its recordings. It is not required to operate the browser dashboard or import existing GPX files.

Device uploads require a compatible app version and the separately configured HTTPS device service.

> This is a prerequisites checklist, not a complete installation procedure. Use the deployment instructions supplied with the server version you intend to install.

## Getting started

### If you are installing the server

1. Prepare the hosting machine, Docker, and Docker Compose.
2. Obtain the GPXDash server installation files.
3. Prepare persistent storage and installation secrets.
4. Follow the version-specific deployment instructions.
5. Create the first administrator account from a trusted environment.
6. Configure and verify secure access for the services you intend to use.
7. Establish backups before storing important recordings.
8. Enable device connections or public sharing only when their separate access configuration is ready.

Do not expose an unconfigured installation directly to the Internet.

### If you are using an existing server

You need:

- The dashboard address supplied by the server administrator.
- Your GPXDash username and password.
- A current browser with JavaScript and cookies enabled.
- A network connection to the server.

An Internet connection may also be needed to load external map tiles.

To begin:

1. Open the dashboard address.
2. Complete any required access-gateway sign-in.
3. Sign in to GPXDash with your **username and password**.
4. Import a GPX file or select a track already in the library.
5. Inspect the track on the map.

There is no universal GPXDash server address. Gateway access does not replace your GPXDash account, and public account registration should not be assumed to be available.

## Working with tracks

The browser library supports importing, viewing, searching, renaming, organizing, exporting, and deleting tracks.

To import existing tracks, you need GPX files that you are authorized to upload.

Tracks stored in GPXDash belong to the server-side library. Do not assume that changes in this library automatically change files or recordings held separately in the GPSDash app.

Before deleting important data:

- Confirm what is selected.
- Read the confirmation carefully.
- Export or back up anything you need to retain.

Keep a separate copy of important original files. A server library is not a substitute for an independent backup.

## Connecting GPSDash

Use the device setup workflow provided by your GPXDash installation and the compatible GPSDash app.

Depending on the supported versions and installation configuration, setup may offer QR pairing, a temporary code, or an advanced manual credential method.

You need:

- A compatible GPSDash app installed on your iPhone or iPad.
- An operational GPXDash device service.
- Access to the dashboard's device setup controls.
- A network connection from your device to the configured service.

Important distinctions:

- Browser sign-in and device authorization are separate.
- The browser dashboard and device service may use different addresses.
- Pairing authorizes a device; it does not by itself mean that existing recordings have been uploaded.
- Connecting a device does not imply automatic two-way synchronization.

Use the connection information generated or supplied for your installation. Do not substitute the dashboard address unless your administrator confirms it is also the correct device address.

Treat pairing invitations and device credentials as confidential. Do not include them in support reports or screenshots.

## Live sharing

Live sharing allows an owner to provide selected access without disclosing their dashboard credentials.

A link's settings determine what recipients can access. Depending on its configuration, this may include live information and selected historical tracks.

**Live access and historical-track access are distinct.** Do not assume that sharing a current location automatically shares the entire library, or that restricting historical tracks disables live access.

Before distributing a link:

1. Review the information it makes available.
2. Review any historical-track selection.
3. Check its expiration and active status.
4. Consider whether recipients may forward it to someone else.

Owners can manage links through **Live sharing**, including editing settings, pausing access, and revoking links.

### Opening a shared map

Recipients need:

- A valid sharing link supplied by its owner.
- A current browser with JavaScript enabled.
- A network connection to the sharing service.

A sharing link does not require the owner's dashboard credentials. Access depends on the link remaining active and on any restrictions applied to it.

Display updates depend on connectivity and refresh timing. Pausing or revoking access does not erase information that someone has already viewed, downloaded, or captured.

## Embedding a shared map

GPXDash supports four integration approaches:

| Method | Purpose |
|---|---|
| **HTML iframe** | Display a server-hosted shared map inside a website. |
| **JavaScript widget** | Insert an interactive shared map with JavaScript configuration. |
| **Web Component** | Add a shared map using a custom HTML element. |
| **Read-only public API** | Retrieve authorized shared data for your own application or map. |

These integrations use the sharing link's permissions. They do not grant access to the owner's private dashboard or unrestricted library.

### Integration prerequisites

You need:

- An active sharing link with the intended permissions.
- A GPXDash public-sharing service reachable by your visitors.
- Permission to edit the destination website.
- Website security settings that permit the selected integration and its required resources.

For JavaScript, Web Component, or API integrations, you also need the ability to add the relevant scripts or application code.

Use **Embed & API** for the selected link in **Live sharing** to obtain the integration example appropriate to your installation.

**Treat embedded sharing credentials as visible to website visitors.** Never place an owner password, device credential, or gateway secret in website source code. Do not commit real sharing tokens to a public repository.

## Privacy and security

GPS tracks can reveal homes, workplaces, routines, and current whereabouts.

- Upload only data you are authorized to store and share.
- Share only the information your audience needs.
- Treat a sharing link as an access capability: someone who receives it may be able to use or forward it.
- Review visible details before publishing a link or embedding a map.
- Keep passwords, authenticator codes, backup codes, device credentials, and pairing invitations private.
- Use HTTPS for remote access.
- Keep independent backups of important tracks.

The person or organization operating your GPXDash installation controls its hosting, administration, backups, and operational policies. This support repository does not host your tracks or administer your account.

External map providers may receive map-tile requests. Your server operator can explain which services are used by your installation.

## Troubleshooting

### I do not know my server address

Contact the person who operates your GPXDash installation. This repository cannot determine the address of a private server.

### I cannot sign in

- Confirm that you are using the correct dashboard address.
- Use your username rather than your recovery email.
- Complete any required gateway access step.
- If you enabled an authenticator, use the requested verification method.

Password recovery by email is available only when your administrator has configured it. If recovery is unavailable, contact the server administrator privately.

Never post passwords, authenticator codes, or backup codes in an issue.

### My device cannot connect

- Confirm that the app and server versions are compatible.
- Use the device setup information for your installation rather than assuming the dashboard address is correct.
- Check that the device can reach the configured service.
- If a temporary invitation has expired, create a new one through the authorized setup workflow.

Ask the administrator to investigate service or gateway configuration problems. Do not publish device credentials while troubleshooting.

### A track has not appeared on the server

- Confirm that the device is connected to the intended server.
- Check the app's transfer status and available upload controls.
- Confirm that the track is eligible for transfer under the app's current settings.
- Check connectivity and any displayed error.

Pairing alone does not prove that a track was transferred. Keep the original recording until you have confirmed the server copy.

### A shared map does not open

The link may be paused, expired, revoked, incomplete, or temporarily unreachable.

Ask its owner to check its status. Do not paste the full sharing link into a public issue.

### A live position is not updating

A visible position is not proof of current device contact or a recent GPS fix.

Check the displayed status and timestamps. Updates depend on the recording device, its location availability, its connection, and the sharing service.

### An embedded map does not load

- Confirm that the original sharing link works.
- Obtain a fresh example from **Embed & API**.
- Check whether the destination website permits the required frame, scripts, and network requests.
- Check the browser console for errors, removing private URLs and tokens before sharing any output.

## Getting help

Use this repository's [Issues](https://github.com/marcoue/GPXDash-Support/issues) for public questions, reproducible problems, and documentation corrections.

For account access, private-server availability, or installation-specific administration, contact your server administrator first.

### Include in a public report

- A short description of the problem.
- What you expected to happen.
- The steps needed to reproduce it.
- Your GPXDash server version, if available.
- Your GPSDash app version, if relevant.
- Your browser and operating system.
- A sanitized error message or screenshot.

You can use this template:

```text
Summary:

Expected result:

Actual result:

Steps to reproduce:
1.
2.
3.

GPXDash server version:
GPSDash app version, if relevant:
Browser and operating system:

Sanitized error message or screenshot:
```

### Do not include

- Passwords or recovery codes.
- Authenticator secrets or backup codes.
- Pairing QR codes, pairing codes, or device credentials.
- Gateway access credentials.
- Full sharing links or sharing tokens.
- Private server addresses.
- Real coordinates, personal routes, or unredacted GPX files.
- Unreviewed logs or screenshots containing personal information.

**Do not report a security vulnerability in a public issue.** Contact the maintainer privately through an available private contact method, or use GitHub's private vulnerability reporting if it is enabled for this repository.

## Availability and compatibility

Features depend on the server version, enabled services, installation configuration, and—where applicable—the GPSDash app version.

A feature described in server documentation may require a compatible app release or additional administrator setup. Confirm availability for your installation before relying on it.
