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

### SPOILERS AHEAD

Busqueda is an Easy rated box on HackTheBox and was the first box I ever finished with minimal guidance.  It makes use of outdated software versions with an exploit openly available on the web as is pretty common with other Easy rated boxes.  Password reuse allows for a good amount of lateral movement and misconfigured sudo permissions is at the heart of allowing for extensive information gathering on this box.

## Enumeration

We will first run an nmap scan.  Notice that port 80 is open indicating an HTTP web server is open.

<img class="img-fluid" src="/img/busqueda/nmap.png">

First add searcher.htb to the /etc/hosts file.

<img class="img-fluid" src="/img/busqueda/hosts.png">

Then browse to the default webpage.  We may also type in the IP address into the search engine since /etc/hosts will perform name resolution.

<img class="img-fluid" src="/img/busqueda/webpage.png">

Scrolling to the bottom of the page reveals a technology being used: Searchor 2.4.0.  A Google search for this version reveals a proof-of-concept (POC) exploit for remote code execution.

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

Running the -h options reveals what command we can run with system-checkup.py.

<img class="img-fluid" src="/img/busqueda/system-checkup_help.png">

Here is sample output from each of the commands:

<img class="img-fluid" src="/img/busqueda/system-checkup_output.png">
<img class="img-fluid" src="/img/busqueda/docker-inspect_output.png">

Looking closer, the output of the full-checkup command reveals a virtual host: gitea.searcher.htb.

<img class="img-fluid" src="/img/busqueda/gitea_vhost.png">

Add this as an entry to the /etc/hosts file and navigate to the default webpage.  On here there is a login page.  Don't forget to try default credentials.  However, in this case it doesn't work.

<img class="img-fluid" src="/img/busqueda/gitea_webpage.png">

Another one of the commands, docker-inspect, actually ends up revealing a MySQL database password.

<img class="img-fluid" src="/img/busqueda/docker-inspect_credentials.png">

Trying the combination of administrator:yuiu1hoiu4i5ho1uh happens to give access to the gitea administration interface.

<img class="img-fluid" src="/img/busqueda/gitea_admin_login.png">

## Privilege Escalation

In this interface, we can inspect the source code of system-checkup.py.  Unfortunately we can't modify the code, even as the administrator, which would result in a simple Python reverse shell payload being appended to the end of the source code and being called whenever we ran system-checkup.py back in the shell.

We find something else that is interesting however.  The full-checkup command is being called using a relative path.  Because of this, we can create our own full-checkup.sh script and have the system-checkup.py script run our own script as the root user.  That doesn't sound good...

<img class="img-fluid" src="/img/busqueda/source_code_inspection.png">

Create the full-checkup.sh script in a directory that the svc user owns with the following bash reverse shell:

<img class="img-fluid" src="/img/busqueda/reverse_shell_payload.png">

Set up a nc listener and run the full-checkup command using sudo.

<img class="img-fluid" src="/img/busqueda/full-checkup.png">

And congratulations, you are now the root user with access to the root flag.

<img class="img-fluid" src="/img/busqueda/root_flag.png">
