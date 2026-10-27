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
  RFC6585:
  RFC9110:

informative:
  RFC7754:
  RFC8890:
  RFC9111:
  RFC9309:
  RFC9421:
  RFC9577:
  CBCP: I-D.draft-illyes-webbotauth-cbcp
  draft-ietf-httpapi-ratelimit-headers: I-D.draft-ietf-httpapi-ratelimit-headers
  draft-ietf-webbotauth-httpsig-protocol: I-D.draft-ietf-webbotauth-httpsig-protocol

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
Human users can be represented by software, including automated clients
({{Section 2 of RFC8890}}). Representing a user implies neither unconditional
access nor an override of site policy.

Behavior-based classification focuses on observable actions that may indicate
abuse, such as excessive request rates, unusual access patterns, or failure to
respect machine-readable signals such as caching directives {{RFC9111}} and,
for crawlers, the Robots Exclusion Protocol {{RFC9309}}. This approach enables
more precise enforcement while avoiding false positives based on client
characteristics.

Policies applied to every client must be general, such as not causing adverse
impact on the site. More specific expectations apply according to the purpose
a client declares, such as {{CBCP}} for crawlers.

## Graduated enforcement

Intermediaries must default to allowing authenticated traffic, subject to site
access policy and behavior-based limits, and offer response options other than
block or allow.

Binary blocking decisions create unnecessary friction for legitimate users while
failing to address the spectrum of potentially unwanted behavior. Graduated
responses allow intermediaries to apply appropriate restrictions based on
confidence levels and threat severity.
Intermediaries should narrowly target restrictions to the relevant behavior,
minimizing effects on unrelated users sharing infrastructure, applying the scope
and granularity considerations in {{Section 4.1.1 of RFC7754}} and
{{Section 4.1.2 of RFC7754}}.

Effective responses include rate limiting (HTTP 429 status code), requests to
retry after temporary overload (HTTP 503 with Retry-After), content modification
per site policy (for instance, a summary or lower-quality variant), and verification challenges.
Rate limiting implementations SHOULD follow emerging standards such as
{{draft-ietf-httpapi-ratelimit-headers}} for consistent client communication.
These responses can be applied progressively based on behavior patterns rather
than defaulting to complete blocking for ambiguous cases.

Challenges and content modification can prevent legitimate clients from
processing a response. Where site policy otherwise permits access,
intermediaries should offer a documented recovery or alternative access path
that does not depend on human interaction or a particular browser's capabilities.
Such a path does not guarantee access and remains subject to the availability
protections in {{security-considerations}}.

## Protocol neutrality

Intermediaries must favor open protocols for traffic management decisions and
communicate responses using standard HTTP semantics.

Traffic management decisions should be based on standardized mechanisms that any
client can implement, rather than proprietary systems or curated lists. This
allows legitimate automated clients to identify themselves appropriately and
follow established conventions for access control.

When responding to blocked or restricted requests, intermediaries MUST use
appropriate HTTP status codes: 403 Forbidden for policy violations and 503
Service Unavailable for temporary overload or maintenance, as defined in
{{RFC9110}}, and 429 Too Many Requests for rate limiting, as defined in
{{RFC6585}}. Response headers SHOULD provide actionable information about
restrictions and recovery paths. For 429 and 503 responses, intermediaries SHOULD
include Retry-After ({{Section 10.2.3 of RFC9110}}) when they can estimate when
the client should retry.

Open protocols enable website owners to express their preferences consistently
across different intermediaries, rather than managing separate configurations
for each vendor's proprietary system.

## Active signals over passive fingerprints

Intermediaries must prefer active signals over passive signals.

Passive fingerprinting techniques identify users without their participation, and
create false positives for users sharing IP addresses, using privacy tools, or
connecting from networks with unusual characteristics. These techniques also
degrade over time as clients change behavior or implement countermeasures.

Active signals provide assurance about the asserted property, because the
client or its operator positively asserts a property that the intermediary can
verify. Examples include HTTP Message Signatures {{RFC9421}} profiles such as
{{draft-ietf-webbotauth-httpsig-protocol}}, Privacy Pass tokens
{{RFC9577}}, published IP ranges as recommended by {{CBCP}}, and forward-confirmed
reverse DNS.

Note that what differs is not necessarily the data itself. An observed IP
address is a passive signal, while the same address published by its operator is
an active signal.

Authentication and privacy-preserving tokens can improve traffic management,
but do not by themselves resolve how legitimate clients without recognized
credentials or supported challenge capabilities are treated. Absence of an
active signal does not itself establish abuse.

## Implementation transparency

Intermediaries must document where their services diverge from published
standards.

Intermediaries that develop or promote standards operate multiple services that
may implement those standards differently. An intermediary that promotes a
standard should disclose where its services do not follow it.

While documenting every deviation may not be practical, intermediaries should
prioritize disclosure in areas where they have established published guidance.

Where safe and feasible, intermediaries should distinguish explicit site-policy
denial from suspected abuse or temporary overload, and provide operators enough
information to investigate unintended blocking. This need not disclose detection
rules or sensitive signals.


# Security Considerations {#security-considerations}

Authentication does not by itself establish authorization or harmless behavior.
Authenticated clients remain subject to site access policy and behavior-based
limits. The preference for active signals does not require clients to identify
themselves to access public resources.

Client assertions can be spoofed. Intermediaries should verify active signals
using the relevant mechanism, including the authenticity and integrity of
published identity data, before using them in access decisions.

Protecting availability can require immediate blocking or dropping connections.
Graduated enforcement does not require trying every response first
({{Section 7.2 of RFC6585}}). Intermediaries should also limit the resources spent
on signal verification and challenges.

Active identifiers and passive fingerprints can enable tracking. Intermediaries
should minimize the collection and retention of identifying data and prefer
mechanisms that disclose only the property needed for a decision.


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
