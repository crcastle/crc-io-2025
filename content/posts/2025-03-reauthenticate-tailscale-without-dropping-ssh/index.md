---
draft: false
slug: reauthenticate-tailscale-without-dropping-ssh
# description: 
date: 2025-03-11
title: Reauthenticate Tailscale Without Dropping Current SSH Session
tags:
  - technical
  - learning
---
I have some of the machines on my tailnet configured to require reauthentication every 90 days. I like machines that I don't have physical control over to not be permanently authenticated to the tailnet.

The problem is that when their 90 day key expires, I can't access them. Fortunately, Tailscale added a "temporarily extend key" to their web console. This gives you 30 minutes to SSH to the machine and reauthenticate for another 90 days.

But if you reauthenticate when you're SSH'd into a machine, you risk losing SSH access if you don't do the process correctly or something breaks. Here's how I handle that. First, [generate a new auth key](https://login.tailscale.com/admin/settings/keys). I normally just use the default settings. Then use that key in the `tailscale up` command below:

```shell
$ ssh machine
$ screen; # sudo apt install screen if necessary
$ sudo bash
$ tailscale up --force-reauth --accept-risk=lose-ssh --authkey tskey-auth-XXXXXX
```

Normally when I do this, I don't lose the SSH connection. But if I do, the re-auth should happen anyway, and I should be able to SSH back to the machine. I can then verify this by running `screen -r` to reconnect to the screen session and see what happened.

After you've done this, check the Tailscale web console to verify that the machine's new expiry is 90 days from now (or whatever duration you set when you generated the auth key).