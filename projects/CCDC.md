---
layout: project
type: project
image: img/ccdc.png
title: "National Collegiate Cyber Defense Competition"
date: 2023
published: true
labels:
  - Linux
  - Windows
  - Blue Team
summary: "In a group of 8, defended a simulated business virtual machine network against professional hackers while simultaneously performing IT business tasks."
---

Earlier this year with a group of 8 and as part of Grey Hats at UH Manoa, I participated in the National Collegiate Cyber Defense Competition (CCDC).  This was a 2-day, 16-hour long competition where we were assigned with managing a virtual machine network of servers and workstations on VMWare ESXi.  There were many factors involved in this competition.  For one, we were assigned a purposely vulnerable network with outdated OS versions we could not update without permission, servers running many unnecessary services, and even credentials to the MySQL database on the desktop as a text file on the MySQL server!  In addition, we were periodically given business tasks by a simulated upper management that included tasks such as setting up a printer, containerizing a web server, etc.  Finally, there was a professional red team that were allowed to attack our network and shut down our services.

Most of us coming into this competition had little to no experience in actually defending networks.  For example, we needed to configure a Palo Alto firewall from scratch in the command line and none of us had done that before.  It was a very stressful weekend as we had no idea where the red team was penetrating our defenses from and as such led to us reverting to snapshots quite often.  However, it was probably one of the best learning experiences for cybersecurity I have ever experienced.  We came out of the competition defeated (not last place though) but more knowledgeable of what we were lacking in preparing a proper defense.  I look forward to seeing how much we can improve in the upcoming 2024 competition.
