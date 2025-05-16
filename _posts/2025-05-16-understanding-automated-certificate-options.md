---
permalink: /2025/05/16/understanding-automated-certificate-options/
title: Understanding automated certificate options
date: 2025-05-16
publish: true
tags:
- security
---

In April 2025, [CA/B Forum ballot SC-081v3](https://groups.google.com/a/groups.cabforum.org/g/servercert-wg/c/bvWh5RN6tYI)
was [passed](https://groups.google.com/a/groups.cabforum.org/g/servercert-wg/c/9768xgUUfhQ)
to reduce the maximum certificate validity to 47 days by the 15th of March 2029.
The first step will be to reduce the maximum validity period to 200 days by the 15th of March 2026.

There has been [a great deal of debate](https://github.com/cabforum/servercert/pull/553) on whether this will be useful or, in some environments, even feasible.

Regardless of our personal opinions on the matter, I think its worth clarifying the options we have today for certificate issuance and renewal. I believe that if you're
aware of all the options, some scenarios where 47-day certificates seemed daunting will become far more achievable.

Naturally we can still obtain certificates through manual processes which I've always found tedious and error prone.
I'm sure this option will continue to exist for some time yet, but clearly automation is the path to consistently reliable certificate renewal on a near monthly cadence.

The most important point I'd like to make is that while Let's Encrypt popularised the Automatic Certificate Management Environment (ACME) protocol, it is now an 
IETF RFC, [RFC 8555](https://datatracker.ietf.org/doc/html/rfc8555), supported by at least five major Certificate Authorities (CAs) at the time of writing.

Let's Encrypt have also open-sourced their [boulder](https://github.com/letsencrypt/boulder) implementation of the ACME CA backend, licensed for other CAs to either
adopt or learn from, so we should expect more CAs to support ACME too in the coming years. This also means that internal PKI infrastructure should also be empowered
to support ACME protocols entirely behind the enterprise firewall.

Similarly, [client support](https://acmeclients.com/) for requesting certificates via ACME-capable CAs is already pervasive and still growing.

The second point I'd like to make is that most people have probably only seen the HTTP-01 challenge for performing Domain Control Validation via the ACME protocol,
but there are at least two other challenge mechanisms, and they each have their own benefits and shortcomings.

The `HTTP-01` challenge, which is arguably the simplest, requires the CA to be able to resolve the domain name for which the certificate will be issued,
and make HTTP requests to the resolved IP(s) with a challenge and receive corresponding responses.

The benefit of HTTP-01 is that the certificate requester only needs to control the webserver for the domain. Some of the shortcomings are:
- the server must speak HTTP on TCP port 80 on the public Internet and be accessible from at least two discrete network vantage points from typically undisclosed source IPs, so this is not suitable for services that don't speak HTTP, don't use TCP port 80, and/or cannot be expose to the public Internet.
- for load-balanced and/or multi-location services, the ACME challenge requests can be routed anywhere and adds some complexity implementing the challenge responses.

Another challenge option is `TLS-ALPN-01`, described in [RFC 8737](https://datatracker.ietf.org/doc/html/rfc8737),
where the CA resolves domain the same as HTTP-01 but then connects to TCP port 443 and sends a TLS Client Hello with a specific ALPN value and
expects a corresponding TLS Server Hello response.

Like HTTP-01, TLS-ALPN-01 still only requires control of the server for the domain, but now does not require plain-text HTTP to be open to the public Internet, and can
also support servers that don't speak HTTP(S) at all. Some of the shortcomings are:
- similar to HTTP-01, the server must have TCP port 443 open to the public Internet, and will have complexity if the service is load-balanced or in multiple locations.
- additionally some TLS-protected protocols don't expect the TLS Client Hello to be sent first and will not work with TLS-ALPN-01. Some examples include smtp, ftp, and mysql.
- TLS-ALPN-01 is quite new and is not yet supported in all TLS server implementations.
- it can also add complexity if TLS has been offloaded to an upstream system such as a load balancer or application gateway.

The last challenge option I'll cover is `DNS-01`, where the CA resolves a `TXT` DNS record for the domain name prefixed with `_acme-challenge.`
and expects the record to contain a specific value.

DNS-01 does not require the actual service that will use the certificate to be Internet exposed at all, even the `A`/`AAAA` records for the service can be absent from public DNS.
DNS-01 does not require the service to speak any particular protocol on any particular port, so it can be used for LDAP or a relational database server for example.

The shortcomings for DNS-01 however include:
- the certificate requester must be authorized to very quickly modify the corresponding DNS TXT record. This could technically be done manually but typically involves API automation of the DNS service. There are good patterns for authorizing an operator to only modify the specific record needed for ACME instead of an entire DNS zone but in practice I've found many environments still have antiquated DNS management practices that inhibit this.
- the DNS TXT record must be resolvable via public authoritative Internet-facing DNS. For internal services, split-horizon DNS may be needed to support DNS-01 challenges. If you're concerned about leaking the name of your internal service via public DNS, using a public CA already means your service name will appear in Certificate Transparency logs which are much more discoverable than short-lived public DNS TXT records.

My personal preference is to use DNS-01 first, then fallback to TLS-ALPN-01, then HTTP-01 as a last resort. I've been using ACME for certificate management for long enough that I don't
even consider manually procured certificates anymore. I believe that for most use cases, if you start to take an ACME-first mindset to certificate management, you too will soon wonder
why you previously bothered with manual certificates.
