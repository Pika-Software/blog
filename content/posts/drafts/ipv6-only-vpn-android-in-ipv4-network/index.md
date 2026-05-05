+++
date = '2025-11-04T13:40:10+02:00'
draft = true
title = 'IPv6 Wireguard, IPv4 network, Android and AAAA domains'
+++

# The Beginnings
Recently I have been playing with Wireguard as point to point VPN to access my servers infastructure,
and just because I wanted. So I picked a short format of addresses, like `fd99::ea`.
Everything was working perfectly, I fell in love with Wireguard, and access to servers has never been easier.

Then I have noticed that at home I have too many different services, and I can't remember correct port for a service.
Solution? Local DNS! ([CoreDNS](https://coredns.io/) with [DNSControl](https://dnscontrol.org/))

The idea is simple, I have AAAA records for my IPv6 addresses in Wireguard network, and CNAME records 
which point to my home server AAAA record. Then any reverse proxy server can route to different services depending on a domain.

While I was organizing my home services on a Linux and MacOS machine, everything was going great, and on a phone too!
Until...

# The Ugly Reality
After connecting to an IPv4 only network, suddenly browser couldn't resolve my local domains. Damn it... It seems like my weird choice for IPv6 addresses backfired...

I have googled if someone have already found a fix for the problem, but no luck. I tried adding IPv4 addresses just for my home server and phone, but A records were messing with other peers. And I am too lazy to rewrite all addresses to IPv4. Fuck that, my current addresses pretty, I won't betray them :P

So what to do? Something needs to be done.

# The Solution
[In this StackExchange question](https://android.stackexchange.com/a/257790) Daniel Gnoutcheff has answered that Android won't resolve AAAA records unless routing table has route to address `2000::`. Solution includes advertisement of a route `2000::/128`, or `2000::/64` if your device is a bitch. But solution includes usage of `radvd` service for route advertisements,
what about Wireguard?

That's actually very easy, `AllowedIPs` parameter acts as routing table, so all you need to do is just add `2000::/128`,
i.e `AllowedIPs = fd99::ea, 2000::` (Wireguard will asume single address subnet if you don't specify one)

After adding the route, Android started resolving my AAAA addresses, so I can keep my IPv6 addresses.

![Good Ending](./good-ending.gif)
