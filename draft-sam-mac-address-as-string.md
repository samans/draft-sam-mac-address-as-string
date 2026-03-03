---
coding: utf-8

title: "MAC Address as String"
abbrev: "MAC Strings"
category: info
docname: draft-sam-mac-address-as-string-latest
ipr: trust200902
submissiontype: IETF
area: OPS
workgroup: NETMOD Working Group
keyword: Internet-Draft

stand_alone: yes
smart_quotes: no
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: S. Mansfield
    name: Scott Mansfield
    organization: Ericsson
    email: scott.mansfield@ericsson.com

normative:

informative:

   IEEE-802-1Qcw:
      title: "IEEE Standard for Local and Metropolitan Area Networks--Bridges and Bridged Networks Amendment 36: YANG Data Models for Scheduled Traffic, Frame Preemption, and Per‐Stream Filtering and Policing"
      date: 2023-11-17
      target: https://doi.org/10.1109/IEEESTD.2023.10317806


--- abstract
IETF and IEEE 802.1 have different patterns for mac addresses in their respective YANG types modules.  Therefore equivalent mac addresses may or may not match if a mac-address that uses the IETF datatype is compared to a mac-address that uses the IEEE datatype (and vise-versa).
--- middle

# Introduction

MAC Address Formats in the IETF and IEEE YANG modules are different.

# Problem Statement

The IETF YANG module {{?RFC9911}} and IEEE YANG {{IEEE-802-1Qcw}} module use a datatype of String to store MAC Addresses.  The issue is that the IETF and IEEE use different patterns and have different canonical forms, which leads to a situation where equivalent MAC Addresses will not match.

This internet-draft is meant to document the issue, raise awareness, and identify potential solutions.

For example, the following MAC Address is in IETF Canonical Format

~~~~
   90-10-00-01-02-AA
~~~~

For example, the following MAC Address is in IEEE Canonical Format

~~~~
   90:10:00:01:02:aa
~~~~

The MAC Address are equivalent, but will not match if used in an XPATH, or as a key, or any string comparison.

There are several potential trouble spots in published IETF YANG modules.

# Detail

## IETF Format

The IETF Format (from ietf-yang-types@2013-07-15.yang) used in the mac-address typedef is found below.

~~~~
typedef mac-address {
 type string {
   pattern '[0-9a-fA-F]{2}(:[0-9a-fA-F]{2}){5}';
 }
 description
   "The mac-address type represents a 48-bit IEEE 802 Media
    Access Control (MAC) address.  The canonical representation
    uses lowercase characters.  Note that there are IEEE 802 MAC
    addresses with a different length that this type cannot
    represent.  The phys-address type may be used to represent
    physical addresses of varying length.

    In the value set and its semantics, this type is equivalent
    to the MacAddress textual convention of the SMIv2.";
 reference
   "IEEE 802: IEEE Standard for Local and Metropolitan Area
              Networks: Overview and Architecture
    RFC 2579: Textual Conventions for SMIv2";
}
~~~~

## IEEE Format

The IEEE Format used in the mac-address typedef is found below.

~~~~
typedef mac-address {
 type string {
   pattern "[0-9a-fA-F]{2}(-[0-9a-fA-F]{2}){5}";
 }
 description
   "The mac-address type represents a MAC address in the canonical
    format and hexadecimal format specified by IEEE Std 802. The
    hexadecimal representation uses uppercase characters.";
 reference
   "3.1, 8.1 of IEEE Std 802";
}
~~~~

# Options
- Create a new mac-address type and deprecate the mac-address types in IETF and IEEE types files.
- Modify the mac-address patterns in both IETF and IEEE to be inclusive of everything, and fix the descriptions to warn the implementers about the issue with equivalence.
- Add something to the YANG language that will provide a way to indicate an "equivalence" pattern.
- Do what SMNP did, abandon strings and store the MAC Address as 6 octets and provide the ability to use a display hint style facility.
- Do Nothing

- Some other brilliant solution?

# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
