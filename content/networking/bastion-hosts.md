---
draft: true
title: Bastion Hosts
categories:
  - Networking
---
1. Pro
   - Reduces attack surface
   - Easier to lock down
2. Con
   - Additional overhead when trying to access instances (e.g tunneling etc)
   - Might result in keys being left on the bastion
   - If we enable tunneling, cant track or audit?

Notes

1. The VPN must be configured to allow only remote SSH or remote desktop connections through it. Otherwise, a compromised VPN client host would have unfettered access to your protected network resources.
2. Tracebility of SSH keys?

Sources:

- <https://news.ycombinator.com/item?id=8637154>
- <http://radar.oreilly.com/2014/01/is-the-jump-box-obsolete.html>
- <https://www.quora.com/What-is-the-difference-between-a-bastion-host-and-a-VPN-host>
