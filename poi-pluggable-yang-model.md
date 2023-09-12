%%%
title = "A YANG Network Data Model for Pluggable DWDM Circuits Management"  
abbrev = "YANG-POI-PLUGGABLE-NM" 
docName = "draft-bedard-poi-pluggable-nm-yang"
category = "standard"
date = 2023-08-30T00:00:00Z
ipr = "trust200902"
area = "General"
workgroup = "CCAMP"
keyword = ["Internet-Draft"]

[pi]
toc = "yes"

[seriesInfo]
name = "Internet-Draft"
value = "poi-pluggable-nm-yang"
stream = "IETF"
status = "standard"


[[author]]
initials = "P.Bedard"
surname = "Bedard"
fullname = "Phil Bedard"
organization = "Cisco Systems"
  [author.address]
   email = "phbedard@cisco.com"


%%%


.# Abstract

This document defines a an Optical Service Network Model applicable to
provisioning optical circuits across a disaggregated multi-layer network when
the DWDM endpoint is a pluggable optics module in a Packet layer device.

The YANG model defined represents a Network Model used by a network control to
derive the configuration used to a downstream network controller.



[https://github.com/mmarkdown/mmark/](https://github.com/mmarkdown/mmark/).

{mainmatter}
# Status of this Memo 

This is an Internet Standards Track document.

This document is a product of the Internet Engineering Task Force (IETF). It
represents the consensus of the IETF community. It has received public review
and has been approved for publication by the Internet Engineering Steering Group
(IESG). Further information on Internet Standards is available in Section 2 of
RFC 7841.

Information about the current status of this document, any errata, and how to
provide feedback on it may be obtained at
[https://www.rfc-editor.org/info/rfc9291](https://www.rfc-editor.org/info/rfc9291).

# Copyright Notice

Copyright (c) 2023 IETF Trust and the persons identified as the
document authors. All rights reserved.

This document is subject to BCP 78 and the IETF Trust's Legal
Provisions Relating to IETF Documents
(http://trustee.ietf.org/license-info) in effect on the date of
publication of this document. Please review these documents
carefully, as they describe your rights and restrictions with respect
to this document. Code Components extracted from this document must
include Simplified BSD License text as described in Section 4.e of
the Trust Legal Provisions and are provided without warranty as
described in the Simplified BSD License.

# Introduction

The use of pluggable DWDM optics has been utilized in packet devices for many
years, commonly referred to as IPoDWDM. A new generation of pluggable DWDM
optics utilizing coherent transmission are now being widely deployed by
operators for a variety of use cases. The pluggable DCO, external to the
photonic line system, represents a disaggregated DWDM network with the DWDM
circuit endpoints located in a packet device. While some applications may not
utilize a photonic optical network, there will continue to be applications where
traditional optical line systems are utilized to create end to end photonic
paths.  

according to the Packet Optical
   Integration (POI) draft [I-D.draft-ietf-teas-actn-poi-applicability]
   in which ACTN hierarchy is deployed [RFC8453], the PNCs are in charge
   of controlling a single domain (e.g.  Packet or Optical) while the
   MDSC is responsible to coordinate the operations across the different
   domains having the visibility of the whole multi-domain and multi-
   layer network topology.

[@!RFC8453] defines a controller based framework decomposing the control of
networks to specific administrative or technology domains. The Packet Optical
Integration (POI) draft [I-D.draft-ietf-teas-actn-poi-applicability] defines
the roles and responsibilities of network controllers participating in a
multi-layer Packet and Optical network. 

The pluggable optics module residing in a packet device is under the management
of the packet domain. As such, the model defined in this document would
typically be exposed by a packet network controller to a higher layer controller
or orchestrator. This is aligned to the 
 

# Conventions and Terminology  

The key words "**MUST**", "**MUST NOT**", "**REQUIRED**", "**SHALL**", "**SHALL NOT**",
"**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**", "**NOT RECOMMENDED**", "**MAY**", and
"**OPTIONAL**" in this document are to be interpreted as described in BCP 14 [@!RFC2119] [@!RFC8174]
when, and only when, they appear in all capitals, as shown here.

This document uses the term "network model" as defined in Section 2.1 of [@!RFC8969]

The meanings of the symbols in YANG tree diagrams are defined in [@!RFC8340]

The document makes use of the following terms: 

**Pluggable DWDM Transceiver:** Pluggable optics module capable of terminating a
DWDM optical signal. The transceiver may have a number of configuration options
to suit its operation over different networks. Examples are ZR [OIF-400ZR-01-0]
and ZR+ [Open_ZR-Plus_MSA]. 

**MDSC (Multi-Domain Service Controller):** Also known as a Hierarchical
Controller, as defined in [@!RFC8453]

**PNC (Provisioning Network Controller):**  Also known as a domain controller as
defined in [@!RFC8453].

**P-PNC (Packet Provisioning Network Controller):**  Network controller
responsible for packet domain management. 

**O-PNC (Optical Provisioning Network Controller):** Network controller
responsible for optical domain management. 

**OLS (Optical Line System):** Refers to a network carrying network traffic as
an optical signal without terminating higher network layers. Typically consists
of add/drop modules and optical switching nodes such as ROADMs.

**P.N. (Packet Node):** In this document refers to a packet node containing one 
or more pluggable DWDM transceivers. 

**O.N. (Optical Switching Node):** In this document refers to an optical node
receiving a DWMD signal from a Packet Node or switching the optical signal
downsream. 




# Reference Architecture 



{#fig1}
~~~ ascii-art
                           +----------+
                           |   MDSC   |
                           +--+----+--+
                              |    |
     MPI interface    +-------+    +-------+
                      |                    |
                 +----+----+          +----+----+
                 |  P-PNC  |          |  O-PNC  |
                 +--+---+--+          +----+----+
                    ^   ^               ^  ^  ^
                    |   |               |  |  |
            +-------+   +---------------|--|--|------+
            |                           |  |  |      |
            |   +-----------------------+  |  +--+   |
            |   |               +----------+     |   |
            |   |               |                |   |
            |   |               v                |   |
            v   |        +--------------+        |   v
           +-----+      /                \      +-----+
          / P.N.1 \..../..O.N--------O.N..\..../ P.N.2 \
          \       /    \  Optical Domain  /    \       /
           +-----+      \                /      +-----+
                         +--------------+

      P.N. = Packet/Optical Node (IPoWDM router)
      O.N. = Optical Switching DWDM Node (ROADM)
~~~
Figure: [https://datatracker.ietf.org/doc/draft-poidt-ccamp-actn-poi-pluggable]
depicts the ACTN framework as applied to a multi-layer network using packet
nodes equipped with pluggable DWDM transceivers connected to an optical photonic
line system.  

{#fig2}
~~~ ascii-art 
      +------+       +------+  _________  +------+       +------+
      |P.N.1 |       | O.N. | /        /\ | O.N. |       |P.N.2 |
      |    P1| ----- |      ||        |  ||      | ----- |P1    |
    ==|    P2| ----- |      ||Optical |  ||      | ----- |P2    |==
    ==|    P3| ----- |      ||Network |  ||      | ----- |P3    |==
      |    P4| ----- |      ||        |  ||      | ----- |P4    |
      |      |       |ROADM | \________\/ |ROADM |       |      |
      +------+       +------+             +------+       +------+
   Packet+Optical              Optical               Packet+Optical
       Layer                   Layer                      Layer

   P.N. = Packet/Optical Node (IPoDWDM router)
   O.N. = Optical Switching DWDM Node (ROADM)
   ROADM = Lambda/Spectrum switch
   Px = DWDM (coherent pluggable) Router ports
~~~
Figure: Shows a use case when the Packet Node pluggable DWDM ports are
connected to intermediate ROADM ports on the Optical Node  


{#fig3}
~~~ ascii-art 
      +------+            +------+
      |P.N.1 |            |P.N.2 |
      |    P1| ---------- |P1    |
    ==|    P2| ---------- |P2    |==
    ==|    P3| ---------- |P3    |==
      |    P4| ---------- |P4    |
      |      |            |      |
      +------+            +------+
   Packet+Optical     Packet+Optical
       Layer               Layer

   P.N. = Packet/Optical Node (IPoDWDM router)
   Px = DWDM (coherent pluggable) Router ports
~~~
Figure: Depicts a scenario where either the pluggable DWDM transceivers are
connected over dark fiber, or the photonic optical line system is not not
managed within the same framework.  

{#fig4}
~~~ ascii-art
                   +-------------------------+
                   |   MDSC / Orchestrator   |
                   +-------------------------+
                               |    
                               |  POI-Pluggable-NM 
                               |            
                          +----+----+       
                          |  P-PNC  |       
                          +--+---+--+       
                             ^   ^          
                             |   |          
            +----------------+   +----------------+
            |                                     |
            |                                     |
            |                                     |
            v            +--------------+         v
          +-------+     /                \     +-------+
          |     P1|..../..O.N--------O.N..\....|P1     |
          |P.N.2  |    \  Optical Domain  /    |P.N.1  |
          +-------+     \                /     +-------+
                         +--------------+

      P.N. = Packet/Optical Node (IPoWDM router)
      O.N. = Optical Switching DWDM Node (ROADM)
~~~
Figure: Depicts the responsibility of the model defined in this document in
the overall architecture. 





# Description of the POI Pluggable Network Module YANG Module  

## Model Structure

{#fig5}
~~~ yang 
module: poi-pluggable-ntw
  +--rw poi-pluggable-ntw
     +--rw optical-services
        +--rw optical-service* [service-id]
          +--rw service-id               service-id-type
           +--rw global-parameters
           ... 
           +--rw optical-service-nodes
              +--rw optical-service-node* [node-id]
                 ...
                 +--rw port-optical-configuration
                 ... 
                 +--rw port-packet-configuration
                 ... 
                    +--rw lag-interface-config
~~~


## Optical Service and Global Parameters 

The 'optical service" container and its 'global-parameters" is used to define
the optical service and common optical parameters which MUST match between the
packet nodes participating in the service. 

The operational-mode is used similarly to [L1CSM]. It abstracts several optical
configuration parameters to a single identifier.  As an alternative the model
user may explicitly define optical configuration. The parameter values are
passed to the node by the P-PNC without modification. The node SHOULD utilize
the explicit configuration when both explicit configuration and an
operational-mode are defined.

{#fig6}
~~~ yang 
module: poi-pluggable-ntw
  +--rw poi-pluggable-ntw
     +--rw optical-services
        +--rw optical-service* [service-id]
           +--rw service-id               service-id-type
           +--rw global-parameters
           |  +--rw admin-state?                      admin-state-type
           |  +--rw circuit-name?                     string
           |  +--rw circuit-description?              string
           |  +--rw customer-name?                    string
           |  +--rw explicit-optical-configuration
           |  |  +--rw symbol-rate?     uint32
           |  |  +--rw host-channels?   identityref
           |  |  +--rw port-rate?       identityref
           |  |  +--rw modulation?      identityref
           |  |  +--rw fec-type?        identityref
           |  |  +--rw pulse-shaping?   enumeration
           |  +--rw central-frequency?                frequency-type
           |  +--rw (operational-mode-config)?
           |     +--:(organization-defined)
           |     |  +--rw organization-identifier?    string
           |     |  +--rw operational-identifier?     string
           |     +--:(standard-defined)
           |        +--rw operational-mode?           uint16
           +--rw optical-service-nodes
~~~ 

## Packet Node Configuration

The optical-service-nodes list defines the nodes participating in the 
service. The list can contain one or two elements. 

### Port Optical Configuration 

The node-configuration container has two child containers. The
port-optical-configuration defines the configuration applied to the specific
port on a node. The port-id is a required component and is not defined in a
standard format, it will be defined by the node naming conventions. 

### Port Packet Configuration 
The port-packet-configuration defines packet-layer configuration to be
optionally applied to the node. Currently to support the LAG requirements
defined in [POI-Pluggable], the LAG interface ID can be defined. The
configuration for the LAG ID MUST be translated by the P-PNC into the node
configuration. If no LAG has been defined a new LAG will be created.


{#fig7}
~~~ yang
          +--rw optical-service-nodes
              +--rw optical-service-node* [node-id]
                 +--rw node-id                       -> ../node-configuration/node-id
                 +--rw node-configuration
                 |  +--rw description?   string
                 |  +--rw node-id        string
                 +--rw port-optical-configuration
                 |  +--rw port-id                string
                 |  +--rw description?           string
                 |  +--rw target-output-power?   uint16
                 |  +--rw node-port-id           string
                 |  +--rw local-plug-id?         plug-id-type
                 |  +--rw remote-plug-id?        plug-id-type
                 |  +--rw cd-range-min?          uint32
                 |  +--rw cd-range-max?          uint32
                 +--rw port-packet-configuration
                    +--rw lag-interface-config
                       +--rw lag-interface-id?   string

~~~

# Additional Info?  

## Multi-Domain Service Controller (MDSC)

The MDSC can expose the model to allow multi-layer circuit provisioning.  The
MDSC will consume the model to then render pluggable configuration intent to a
downstream Packet Provisioning Network Controller (P-PNC) and communicate
photonic circuit configuration intent to the Optical Provisioning Network
Controller (O-PNC). Each of these downstream configuration intents is optional,
allowing the same model to be used by the MDSC to perform three provisioning use
cases. 

- Multi-Layer provisioning towards P-PNC and O-PNC 
- Pluggable DCO DWDM endpoint configuration only towards P-PNC 
- Photonic optical circuit only towards O-PNC 

## Packet Provisioning Network Controller (P-PNC)  

The P-PNC can expose the model to allow configuration of pluggable optics within 
its scope of control. The model exposed by the P-PNC is a subset of the complete 
model, containing the portions of the model applicable to the configuration of 
the pluggable. The P-PNC will not consume the components of the model used for 
photonic optical line system provisioning.  

## Optical Provisioning Network Controller (O-PNC)  

The models utilized for optical circuit provisioning to support are already well-defined 

Packet network controller model for pluggable management? 

Model has following components and leaf values aligned to other models like L1CSM 

circuit-id RW 
operational-mode RW 
encapsulation (Ethernet|OTN)
frequency?  
bandwidth? 


In summary the pluggable parameters exchanged from O-PNC to MDSC to
   P-PNC for end to end service provisioning are:

    - Pluggable Service source port-ID
    - Pluggable Service destination port-ID
    - Central Frequency (Lambda) (common to source and destination)
    - TX Output power (source port-ID)
    - TX Output power (destination port-ID)
    - Operational-mode (compatible)
    - Vendor OUI
    - Pluggable part number (if the operational mode in not standard)
    - Admin-state (common ?)



https://datatracker.ietf.org/doc/draft-ietf-ccamp-l1csm-yang/

Reference to the optical impairment model for pluggable discovery using RFC 8345 
https://datatracker.ietf.org/doc/draft-lee-ccamp-optical-impairment-topology-yang/

Reference to current ACTN POI architecture draft: 
https://datatracker.ietf.org/doc/html/draft-ietf-teas-actn-poi-applicability

Reference to the current ACTN POI pluggable draft:   
https://datatracker.ietf.org/doc/draft-poidt-ccamp-actn-poi-pluggable/


# Security Considerations

No current security considerations.  

# IANA Considerations

This document has no IANA actions.

{backmatter}
