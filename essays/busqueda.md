---
layout: essay
type: essay
title: "HTB Busqueda"
# All dates must be YYYY-MM-DD format!
date: 2023-09-01
published: true
labels:
  - Outdated Software
  - Password Reuse
  - Sudo
---

## Enumeration

We will first run an nmap scan.  Notice that port 80 is open indicating an HTTP web server is open.

<img class="img-fluid" src="/img/busqueda/nmap.png">

First add searcher.htb to the /etc/hosts file.

<img class="img-fluid" src="/img/busqueda/hosts.png">

Then browse to the default webpage.  We may also type in the IP address into the search engine since /etc/hosts will perform name resolution.

<img class="img-fluid" src="/img/busqueda/webpage.png">

Scrolling to the bottom of the page reveals a technology being used, Searchor 2.4.0.  A Google search for this version reveals a proof-of-concept (POC) exploit for remote code execution.

<img class="img-fluid" src="/img/busqueda/webpage_searchor.png">

## User Foothold

First set up a nc listener on the attacker machine listening on port 9001.  Then run the GitHub POC exploit with arguments being the hostname of the web server and the attacker machine IP address.

<img class="img-fluid" src="/img/busqueda/run_exploit.png">

The nc listener will respond back with a reverse shell in which we can find the contents of the user flag.

<img class="img-fluid" src="/img/busqueda/nc_listener.png">
<img class="img-fluid" src="/img/busqueda/user_flag.png">

Probing around the filesystem, we can find exposed credentials for the cody user within a file in the .git directory.

<img class="img-fluid" src="/img/busqueda/discovered_credentials.png">

Password reuse is found by attempting to use this password to invoke sudo for the current svc user.  We find that we can execute one specific python script as the root user.

<img class="img-fluid" src="/img/busqueda/sudo_permissions.png">

## Privilege Escalation
