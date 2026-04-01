---
###
# Internet-Draft Markdown Template
#
# Rename this file from draft-todo-yourname-protocol.md to get started.
# Draft name format is "draft-<yourname>-<workgroup>-<name>.md".
#
# For initial setup, you only need to edit the first block of fields.
# Only "title" needs to be changed; delete "abbrev" if your title is short.
# Any other content can be edited, but be careful not to introduce errors.
# Some fields will be set automatically during setup if they are unchanged.
#
# Don't include "-00" or "-latest" in the filename.
# Labels in the form draft-<yourname>-<workgroup>-<name>-latest are used by
# the tools to refer to the current version; see "docname" for example.
#
# This template uses kramdown-rfc: https://github.com/cabo/kramdown-rfc
# You can replace the entire file if you prefer a different format.
# Change the file extension to match the format (.xml for XML, etc...)
#
###
title: "Principles for Web Traffic Management by Intermediaries"
abbrev: "Web Traffic Management Principles"
category: info

docname: draft-meunier-civil-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: AREA
workgroup: WG Working Group
keyword:
 - web intermediaries
 - traffic management
 - security
venue:
  group: WG
  type: Working Group
  mail: WG@example.com
  arch: https://example.com/WG
  github: thibmeu/draft-bcp-civil
  latest: https://example.com/LATEST

author:
 -
    fullname: Thibault Meunier
    organization: Cloudflare
    email: ot-ietf@thibault.uk

normative:
  RFC7230:
  RFC7234:
  RFC9309:

informative:
  RFC3040:
  draft-illyes-aipref-cbcp: I-D.draft-illyes-aipref-cbcp

...

--- abstract

This document describes principles for web intermediaries that balance security needs with open web accessibility.


--- middle

# Introduction

Web intermediaries such as content delivery networks, security services, and edge platforms increasingly implement traffic management systems to protect against abuse. As these systems become more sophisticated, there is growing concern about their impact on legitimate users and the open nature of the web. This document establishes principles for responsible traffic management that preserve web accessibility while maintaining necessary protections.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

The following terms are used throughout this document:

Active signal:
: Cryptographically verifiable client property

Passive signal:
: Inferred/heuristic client property

Graduated response:
: Configurable action spectrum

# Principles

The following principles should be followed by web intermediaries to balance security requirements with web openness:

1. Intermediaries must classify traffic by specific behaviors, not client types
2. Intermediaries must offer response options other than block or allow
3. Intermediaries must favor open protocols for traffic management decisions
4. Intermediaries must prefer cryptographically verifiable signals over passive fingerprinting
5. Intermediaries should strive to follow published standards across their products

## Behavior-based classification

Intermediaries must classify traffic by specific behaviors, not client types.

Classifications based on inferred client types (such as "bot" or "automated") are unreliable as the distinction between human and automated web browsing continues to blur. Modern web applications routinely use automation, and browsers themselves perform automated requests for prefetching, security scanning, and other legitimate purposes.

Behavior-based classification focuses on observable actions that may indicate abuse, such as excessive request rates, unusual access patterns, or violation of website terms. This approach enables more precise enforcement while avoiding false positives based on client characteristics.

## Graduated enforcement

Intermediaries must default to allowing authenticated traffic, and offer response options other than block or allow.

Binary blocking decisions create unnecessary friction for legitimate users while failing to address the spectrum of potentially unwanted behavior. Graduated responses allow intermediaries to apply appropriate restrictions based on confidence levels and threat severity.

Effective responses include rate limiting, request delays, content modification, and verification challenges. These responses can be applied progressively based on behavior patterns rather than defaulting to complete blocking for ambiguous cases.

## Protocol neutrality

Intermediaries must favor open protocols for traffic management decisions.

Traffic management decisions should be based on standardized mechanisms that any client can implement, rather than proprietary systems or curated lists. This allows legitimate automated clients to identify themselves appropriately and follow established conventions for access control.

Open protocols enable website owners to express their preferences consistently across different intermediaries, rather than managing separate configurations for each vendor's proprietary system.

## Active signals over passive fingerprints

Intermediaries must prefer client initiated signals over passive fingerprinting.

Passive fingerprinting techniques create false positives for users sharing IP addresses, using privacy tools, or connecting from networks with unusual characteristics. These techniques also degrade over time as clients change behavior or implement countermeasures.

Cryptographically verifiable credentials provide stronger assurance of client legitimacy while preserving user privacy. Examples include HTTP Message Signatures, Privacy Pass tokens, and other attestation mechanisms that prove client properties without revealing identifying information.

## Implementation consistency

Intermediaries should strive to follow published standards across their products.

Organizations that develop or promote web standards should implement those standards consistently across their product portfolio where feasible. This includes ensuring that different product lines respect the same protocols for content access, bot identification, and traffic management.

While complete consistency across all standards may not be practical, intermediaries should prioritize consistency in areas where they have established public positions or published guidance. When intermediaries follow their own published standards uniformly, it demonstrates the viability of these approaches and encourages broader industry adoption.


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
