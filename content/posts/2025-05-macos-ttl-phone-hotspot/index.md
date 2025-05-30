---
draft: false
title: Bypass Phone Hotspot Speed Restrictions by Changing MacOS IP TTLs
slug: macos-ttl-phone-hotspot
# description: "-"
date: 2025-05-12
tags:
  - technical
  - advice
  - learning
  - work
---
Many mobile phone service providers reduce internet connection speed when you're using your phone as a hotspot. They do this (naively) by looking at the TTL value in the IP packets. The TTL value of a packet (on MacOS) defaults to 64 and is decremented at each network hop. If the mobile phone operator receives an IP packet with a TTL of 64, they assume it's from you using your phone directly and allow traffic to move at full speed.

If they receive a packet with a TTL of less than 64, they can (again, naively) assume that the packet originated on a laptop or other device using a mobile hotspot. The packet starts on the laptop with a TTL of 64. It hops to the phone, where the TTL is decremented to 63. The mobile phone operator's infrastructure receives the packet with a TTL of 63.

So they have a network management rule that says, if the TTL of a received packet is less than 64, slow down the connection speed.

Well, you can override the MacOS default TTL so that it starts as 65. Now when the packet hits the mobile phone operator's infrastructure, it sees a TTL of 64 thinking it came from direct phone use (not hotspot use).

Change the TTL like this:

```shell
sudo sysctl net.inet.ip.ttl=65 #set default TTL for IPv4 packets
sudo sysctl net.inet6.ip6.hlim=65 #set default TTL for IPv6 packets
```

Now all packets originating from your laptop will have a TTL of 65.

But these settings will default back to 64 when you restart the computer. Set them permanently with a Global Daemon. Place the following files (one for IPv4 and one for IPv6) in `/Library/LaunchDaemons`:

### `com.custom.ttl.ipv4.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>com.custom.ttl.ipv4</string>
	<key>ProgramArguments</key>
	<array>
		<string>/usr/sbin/sysctl</string>
		<string>net.inet.ip.ttl=65</string>
	</array>
	<key>RunAtLoad</key>
	<true/>
</dict>
</plist>
```

### `com.custom.ttl.ipv6.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>com.custom.ttl.ipv6</string>
	<key>ProgramArguments</key>
	<array>
		<string>/usr/sbin/sysctl</string>
		<string>net.inet6.ip6.hlim=65</string>
	</array>
	<key>RunAtLoad</key>
	<true/>
</dict>
</plist>
```

Now tell MacOS to run these automatically on startup. From the command line, run:

```shell
sudo launchctl load /Library/LaunchDaemons/com.custom.ttl.ipv4.plist
sudo launchctl load /Library/LaunchDaemons/com.custom.ttl.ipv6.plist
```

And enjoy faster mobile hotspot connection speeds!

