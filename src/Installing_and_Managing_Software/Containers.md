---
title: Containers
---

# Containers

## What are containers?

Containers are isolated environments for software to run.  Bazzite utilizes containers in a software context for applications that either run better in a container environment or are not available in the Bazaar app store.

## Distrobox

[Distrobox](<link docs>) allows users to run other minimal versions Linux operating systems within their own containerized environment with access to their own repositories.  Real world examples include applications that only have Linux support for Ubuntu and ship a `.deb` file on their website.  An Ubuntu container can be created with DistroShelf and then the `.deb` file can be installed within the container and exported so it appears in your applications.

[Quadlet](<link docs>) is specifically for running services like media servers, game servers, etc.
