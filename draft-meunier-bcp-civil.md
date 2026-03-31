---
title: "Best practices for CDN traffic management"
abbrev: "Best practices for CDN traffic management"
category: info

docname: draft-meunier-bcp-civil-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
# area: AREA
# workgroup: WG Working Group
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  github: "thibmeu/draft-bcp-civil"
  latest: "https://thibmeu.github.io/draft-bcp-civil/draft-meunier-bcp-civil.html"

author:
 -
    fullname: Thibault Meunier
    organization: Cloudflare
    email: ot-ietf@thibault.uk

normative:

informative:

...

--- abstract

This document describes best practices for content delivery networks.


--- middle

# Introduction

Problem: CDNs sit between clients and origins
Goal: Responsible traffic management without breaking the open web


# Conventions and Definitions

{::boilerplate bcp14-tagged}

The following terms are used throughout this document:

Active signal:
: Cryptographically verifiable client property

Passive signal:
: Inferred/heuristic client property

Graduated response:
: Configurable action spectrum

# Recommended best practices

The following best practices should be followed and are already
applied by a vast majority of large-scale reverse proxy on the Internet:

- list the subsection for easy consumption

## Protocol neutrality

CDN traffic management systems MUST support open, standardized protocols for client identification and access control. Implementations SHOULD avoid default configurations that rely on proprietary mechanisms or hardcoded allowlists.

## Graduated enforcement

Operators SHOULD support configurable response tiers beyond
block/allow (e.g., rate-limit, challenge, degrade).

Example: Suspected automation → reduce rate limit; confirmed
abuse → block.

## Active signals over passive fingerprints

Implementations SHOULD prefer cryptographically verifiable client credentials (e.g., HTTP Message Signatures, Privacy Pass tokens) over passive fingerprinting techniques (e.g., TLS characteristics, IP heuristics).

## Behaviour-based classification

Operators MUST classify traffic by defined behaviors (e.g.,
"excessive request rate") rather than client type (e.g., "bot").

Example: "Request rate >100/min from single IP" not "bot detected".

## Implementation consistency

Cloudflare products should natively support and simplify adherence to the open web and bot standards that Cloudflare publishes and promotes externally. This is true of "CDN" products, but also of workers or zero trust.

Example: Cloudflare offers origins to serve content-signal allowing creators to specify preferences. We should make it easy to consume in Workers. Similarly, as Cloudflare CDN identify platforms, we should ensure that Workers and ZT identify themselves as well.


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
