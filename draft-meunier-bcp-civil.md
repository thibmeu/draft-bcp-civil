---
title: "Principles for Web Traffic Management by Intermediaries"
abbrev: "Web Traffic Management Principles"
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
 - web intermediaries
 - traffic management
 - security
venue:
#  group: WG -- no idea which group this falls into
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
  RFC9110:

informative:
  RFC9111:
  RFC9309:
  RFC9421:
  RFC9577:
  CBCP: I-D.draft-illyes-webbotauth-cbcp
  draft-ietf-httpapi-ratelimit-headers: I-D.draft-ietf-httpapi-ratelimit-headers
  draft-meunier-webbotauth-httpsig-protocol: I-D.draft-meunier-webbotauth-httpsig-protocol

...

--- abstract

This document describes principles for web intermediaries that balance security
needs with open web accessibility.


--- middle

# Introduction

Web intermediaries such as content delivery networks, security services, and
edge platforms increasingly implement traffic management systems to protect
against abuse. As these systems become more sophisticated, there is growing
concern about their impact on legitimate users and the open nature of the web.

This document establishes principles for responsible traffic management that
preserve web accessibility while maintaining necessary protections.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

The following terms are used throughout this document:

Active signal:
: Client property asserted by the client or its operator, verifiable by the
  intermediary, and not assertable by an unrelated party

Passive signal:
: Inferred/heuristic client property

Graduated response:
: Configurable action spectrum

Intermediary:
: As defined in {{Section 3.7 of RFC9110}}, an HTTP intermediary is a proxy,
  gateway, or tunnel that acts on behalf of either the client or server.

# Principles

The following principles should be followed by web intermediaries to balance\
security requirements with web openness:

1. Intermediaries must classify traffic by specific behaviors, not by client
   identity
2. Intermediaries must offer response options other than block or allow
3. Intermediaries must favor open protocols for traffic management decisions
4. Intermediaries must prefer active signals over passive signals
5. Intermediaries must document where their services diverge from published
   standards.

## Behavior-based classification

Intermediaries must classify traffic by specific behaviors, not by client identity.

Classifications based on inferred client types (such as "bot" or "automated")
are unreliable as the distinction between human and automated web browsing
continues to blur. Modern web applications routinely use automation, and
browsers themselves perform automated requests for prefetching, security
scanning, and other legitimate purposes.

Behavior-based classification focuses on observable actions that may indicate
abuse, such as excessive request rates, unusual access patterns, or failure to
respect machine-readable signals such as caching directives {{RFC9111}} and the
Robots Exclusion Protocol {{RFC9309}}. This approach enables more precise enforcement while avoiding false
positives based on client characteristics.

Policies applied to every client must be general, such as not causing adverse
impact on the origin. More specific expectations apply according to the purpose
a client declares, such as {{CBCP}} for crawlers.

## Graduated enforcement

Intermediaries must default to allowing authenticated traffic, and offer
response options other than block or allow.

Binary blocking decisions create unnecessary friction for legitimate users while
failing to address the spectrum of potentially unwanted behavior. Graduated
responses allow intermediaries to apply appropriate restrictions based on
confidence levels and threat severity.

Effective responses include rate limiting (HTTP 429 status code), request delays
(HTTP 503 with Retry-After), content modification, and verification challenges.
Rate limiting implementations SHOULD follow emerging standards such as
{{draft-ietf-httpapi-ratelimit-headers}} for consistent client communication.
These responses can be applied progressively based on behavior patterns rather
than defaulting to complete blocking for ambiguous cases.

## Protocol neutrality

Intermediaries must favor open protocols for traffic management decisions and
communicate responses using standard HTTP semantics.

Traffic management decisions should be based on standardized mechanisms that any
client can implement, rather than proprietary systems or curated lists. This
allows legitimate automated clients to identify themselves appropriately and
follow established conventions for access control.

When blocking or restricting access, intermediaries MUST use appropriate HTTP
status codes as defined in {{RFC9110}}: 403 Forbidden for policy violations, 429
Too Many Requests for rate limiting, and 503 Service Unavailable for temporary
restrictions. Response headers SHOULD provide actionable information about
restrictions and recovery paths.

Open protocols enable website owners to express their preferences consistently
across different intermediaries, rather than managing separate configurations
for each vendor's proprietary system.

## Active signals over passive fingerprints

Intermediaries must prefer active signals over passive signals.

Passive fingerprinting techniques identify users without their participation, and
create false positives for users sharing IP addresses, using privacy tools, or
connecting from networks with unusual characteristics. These techniques also
degrade over time as clients change behavior or implement countermeasures.

Active signals provide stronger assurance of client legitimacy, because the
client or its operator positively asserts a property that the intermediary can
verify. Examples include HTTP Message Signatures {{RFC9421}} profiles such as
{{draft-meunier-webbotauth-httpsig-protocol}}, Privacy Pass tokens
{{RFC9577}}, published IP ranges as required by {{CBCP}}, and forward-confirmed
reverse DNS.

Note that what differs is not necessarily the data itself. An observed IP
address is a passive signal, while the same address published by its operator is
an active signal.

## Implementation transparency

Intermediaries must document where their services diverge from published
standards.

Intermediaries that develop or promote standards operate multiple services that
may implement those standards differently. An intermediary that promotes a
standard should disclose where its services do not follow it.

While documenting every deviation may not be practical, intermediaries should
prioritize disclosure in areas where they have established published guidance.
Consistent disclosure demonstrates accountability and encourages broader
industry adoption.


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
