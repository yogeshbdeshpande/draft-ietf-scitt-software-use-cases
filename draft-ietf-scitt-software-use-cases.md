---
v: 3

title: Detailed Software Supply Chain Uses Cases for SCITT
abbrev: SCITT SW Supply Chain
docname: draft-ietf-scitt-software-use-cases-latest
keyword: Internet-Draft
cat: info
stream: IETF

venue:
  group: SCITT
  mail: scitt@ietf.org
  github: ietf-wg-scitt/draft-ietf-scitt-software-use-cases

pi: [toc, sortrefs, symrefs, compact, comments]

author:
  - ins: H. Birkholz
    name: Henk Birkholz
    org: Fraunhofer Institute for Secure Information Technology
    abbrev: Fraunhofer SIT
    email: henk.birkholz@sit.fraunhofer.de
    street: Rheinstrasse 75
    code: '64295'
    city: Darmstadt
    country: Germany
  - ins: Y. Deshpande
    name: Yogesh Deshpande
    org: ARM
    email: yogesh.deshpande@arm.com
  - ins: D. Brooks
    name: Dick Brooks
    org: REA
    email: dick@reliableenergyanalytics.com
  - ins: R. Martin
    name: Robert Martin
    org: MITRE
    email: ramartin@mitre.org
  - ins: B. Knight
    name: Brian Knight
    org: Microsoft
    email: brianknight@microsoft.com

normative:

informative:
  RFC9019: SUIT-ARCH
  RFC9124: SUIT-IM
  TUF: TUF
    target: https://theupdateframework.io/overview/

--- abstract

This document includes a collection of representative Software Supply Chain Use Cases.
These use cases aim to identify software supply chain problems that the industry faces today and act as a guideline for developing a comprehensive security architecture  and solutions for these scenarios.

--- middle

# Introduction {#intro}

Modern software applications are an intricate mix of first-party and third-party code, development practices and tools, deployment methods and infrastructure, and interfaces and protocols.
The software supply chain comprises all elements associated with a system's design, development, build, integration, deployment, and maintenance throughout its entire lifecycle.
The complexity of software, coupled with a lack of lifecycle visibility, increases the risks associated with system attack surface and the number of cyber threats capable of harmful impacts, such as exfiltration of data, disruption of operations, and loss of reputation, intellectual property, and financial assets.
There is a need for an architecture that will allow consumers to know that suppliers maintained appropriate security practices without requiring access to proprietary intellectual property.
SCITT-enabled products assist in managing compliance with often distinct, but overlapping and interconnected, legal, regulatory, and technical requirements, assessing risks, and detecting supply chain attacks across the software lifecycle while prioritizing data privacy.

## Terminology {#terms}

{::boilerplate bcp14-tagged}

# Generic Problem Statement

Supply chain security is a prerequisite to protecting consumers and minimizing economic, public health, and safety threats.
Supply chain security has historically focused on risk management practices to safeguard logistics, meet compliance regulations, forecast demand, and optimize inventory.
While these elements are foundational to a healthy supply chain, an integrated cyber security-based perspective of the software supply chains remains broadly undefined.
Recently, the global community has experienced numerous supply chain attacks targeting weaknesses in software supply chains. As illustrated in {{lifecycle-threats}}, a software supply chain attack may leverage one or more lifecycle stages and directly or indirectly target the component.

~~~~aasvg
      Dependencies        Malicious 3rd-party package or version
           |
           |
     +-----+-----+
     |           |
     |   Code    |        Compromise source control
     |           |
     +-----+-----+
           |
     +-----+-----+
     |           |        Malicious plug-ins;
     |  Commit   |        Malicious commit
     |           |
     +-----+-----+
           |
     +-----+-----+
     |           |        Modify build tasks or build environment;
     |   Build   |        Poison build agent/compiler;
     |           |        Tamper with build cache
     +-----+-----+
           |
     +-----+-----+
     |           |        Compromise test tools;
     |    Test   |        Falsification of test results
     |           |
     +-----+-----+
           |
     +-----+-----+
     |           |        Use bad package;
     |  Package  |        Compromise package repository
     |           |
     +-----+-----+
           |
     +-----+-----+
     |           |        Modify release tasks;
     |  Release  |        Modify build drop prior to release
     |           |
     +-----+-----+
           |
     +-----+-----+
     |           |
     |  Deploy   |        Tamper with versioning and update process
     |           |
     +-----------+
~~~~
{: #lifecycle-threats title="Example Lifecycle Threats"}

DevSecOps often depends on third-party and open-source software.
These dependencies can be quite complex throughout the supply chain and render the checking of lifecycle compliance difficult.
There is a need for manageable auditability and accountability of digital products.
Typically, the range of types of statements about digital products (and their dependencies) is vast, heterogeneous, and can differ between community policy requirements.
Taking the type and structure of all statements about digital and products into account might not be possible.
Examples of statements may include commit signatures, build environment and parameters, software bill of materials, static and dynamic application security testing results, fuzz testing results, release approvals, deployment records, vulnerability scan results, and patch logs.
In consequence, instead of trying to understand and describe the detailed syntax and semantics of every type of statement about digital products, the SCITT architecture focuses on ensuring statement authenticity, visibility/transparency, and intends to provide scalable accessibility.
The following use cases illustrate the scope of SCITT and elaborate on the generic problem statement above.

## Software Supply Chain Use Cases

## Verification That Signing Certificate Is Authorized by Supplier

Consumers wish to verify the authenticity and integrity of software they use before installation.
To do this today, they rely on the digital signature of the software.
This can be misleading, however, as there is no guarantee that the certificate used to sign the software is authorized by the Supplier for signing.
For example, a malicious actor may obtain a signing certificate from a reputable organization and use that certificate to sign malicious software.
The consumer, believing the software originated from the reputable organization, would then install malicious software.

A consumer of software wants to:

- verify the authenticity and integrity of software they use before installation.

There is no standardized way to:

- enable the consumer to verify that software originated from a 'duly authorized signing party' on behalf of the supplier, and is still valid.

## Multi Stakeholder Evaluation of a Released Software Product

In the IT industry, it is a common practice that once a software product is released, it is evaluated on various aspects.
For example, an auditing company, a code review company or a government body will examine the software product and issue authoritative reports about the product.
The end users (consumers or distribution entities) use these report to make an accurate assessment as to whether the software product is deemed fit to use.

There are multiple such authoritative bodies that make such assessments.
There is no assurance that all the bodies may be aware of statements from other authoritative entities or actively acknowledge them.
Discovery of all sources of such reports and/or identities of the authoritative bodies adds a significant cost to the end user or consumer of the product.

A consumer of released software product wants to:

- offload the burden of identifying all relevant authoritative entities to an entity who does it on their behalf
- offload the burden to filter from and select all statements that are applicable to a particular version of a multi release software product, to an entity who does this on their behalf
- make an informed decisions on which authoritative entities to believe

There is no standardized way to:

- aggregate large numbers of related statements in one place and discover them
- referencing other statements via a statement
- identifying or discover all (or at least a critical mass) of relevant authoritative entities

## Security Analysis of a Software Product

This use case is a specialization of the use case above.

A released software product is often accompanied by a set of complementary statements about it's security compliance.
This gives enough confidence to both producers and consumers that the released software has a good security standard and is suitable to use.

Subsequently, multiple security researchers often run sophisticated security analysis tools on the same product.
The intention is to identify any security weaknesses or vulnerabilities in the package.

Initially, a particular analysis can identify a simple weakness in a software component.
Over a period of time, a statement from a third-party illustrates that the weakness is exposed in a way that represents an exploitable vulnerability.
The producer of the software product provides a statement that confirms the linking of software component vulnerability with the software product and also issues an advisory statement on how to mitigate the vulnerability.
At first, the producer provides an updated software product that still uses the vulnerable software component but shields the issue in a fashion that inhibits exploitation.
Later, a second update of the software product includes a security patch to the affected software component from the software producer.
Finally, a third update includes a new release (updated version) of the formerly insecure software component.
For this release, both the software product and the affected software component are deemed secure by the producer and consumers.

A consumer of a released software wants to:

- know where to get these security statements from producers and third-parties related to the software product in a timely and unambiguous fashion
- attribute them to an authoritative issuer
- associate the statements in a meaningful manner via a set of well-known semantic relationships
- consistently, efficiently, and homogeneously check their authenticity

There is no standardized way to:

- know the various sources of statements
- express the provenance and historicity of statements
- relate and link various heterogeneous statements in a simple fashion
- check that the statement comes from a source with authority to issue that statement

## Promotion of a Software Component by Multiple Entities

A software component (e.g., a library) released by a certain original producer is becoming popular.
The released software component is accompanied by a statement of authenticity (e.g., a detached signature).
Over time, due to its enhanced applicability to various products, there has been an increasing amount of multiple providers of the same software component version on the internet.

Some providers include this particular software component as part of their release package bundle and provide the package with proof of authenticity using their own issuer authority.
Some packages include the original statement of authenticity, and some do not.
Over time, some providers no longer offer the exact same software component source code but pre-compiled software component binaries.
Some sources do not provide the exact same software component, but include patches and fixes produced by third-parties, as these emerge faster than solutions from the original producer.
Due to complex distribution and promotion lifecycle scenarios, the original software component takes myriad forms.

A consumer of a released software wants to:

- understand if a particular provider is actually the original provider or a promoter
- know if and how the source, or resulting binary, of a promoted software component differs from the original software component
- check the provenance and history of a software component's source back to its origin
- assess whether to trust a promoter or not

There is no standardized way to:

- reliably discern if a provider is the original producer or is a trustworthy promoter or is an illegitimate provider
- track the provenance path from an original producer to a particular provider
- check the trustworthiness of a provider
- check the integrity of modifications or transformations applied by a provider

## Post-Boot Firmware Provenance

In contrast to operating systems or user space software components of a large and complex systems, firmware components are often already executed during boot-cycles before there is an opportunity to authenticate them.

Authentication takes place, for example, by validating a signed artifact against a Reference Integrity Manifest (RIM), such as IETF's Concise Reference Integrity Manifest, TCG Reference Integrity Manifest (RIM) Information Model, or another specification as applicable.
Corresponding procedures are often called authenticated, measured, or secure boot.
The output of these high assurance boot procedures is often used as input to more complex verifications known as remote attestation procedures.

If measurements before execution are not possible, static after-the-fact analysis is required, typically by examining artifacts.
When best practices are followed, measurements (e.g., a hash or digests) are stored in a protected or shielded environment (e.g., TEEs or TPMs).
After finishing a boot sequence, these measurements about foundational firmware are retrieved after-the-fact from shielded locations and must be compared to reference values that are part of RIMs.
A verifying system appraising the integrity of a boot sequence must identify, locate, retrieve, and authenticate corresponding RIMs.

A consumer of published software wants to:

- easily identify sources for RIMs
- select appropriate RIMs and download them for the appraisal of measurements
- assure the authenticity, applicability, and freshness of RIMs over time

There is no standardized way to:

- identify, locate, retrieve and authenticate RIMs in a uniform fashion
- uniquely identify and filter multiple potential available RIMs (e.g., by age, source, signing authority, etc.)
- store RIMs in a secure fashion that enables their usage in appraisal procedures years after they were created

## Auditing of Software Products

An organization has established procurement requirements and compliance policies for software use.
In order to allow the acquisition and deployment of software in certain security domains of the organization, a check of software quality and characteristics must succeed.
Compliance and requirement checking includes audits of the results of organizational procedures and technical procedures, which can originate from checks conducted by the organization itself or checks conducted by trusted third parties.
Consequently, consumers of statements about released software can be auditors.
Examples of procedure results important to audits include:

- available fresh and applicable code reviews
- certification documents (e.g., FIPS or Common Criteria)
- virus scans
- vulnerability disclosure reports (fixed or not fixed)
- security impact or applicability justification statements

Relevant documents (such as compliance, requirements or procedure results) originate from various sources and include a wide range of representations and formats.

A producer of released software wants to:

- match disclosures related to released software to the needs of an auditor
- provide documents that enable efficient audit procedures
- minimize the cost of audits

There is no standardized way to:

- discover and associate relevant documents, data, and validation results required for various types of audits
- assert the authenticity and provenance of documents relevant to audits in a deterministic and uniform fashion
- check the validity of identity statements about relevant documents after the fact (when they were made) in a consistent, long-term fashion
- allow for more than one level of disclosure of audit procedures (potentially depending on criticality)

## Authentic Software Components in Air-Gapped Infrastructure

Some software is deployed on systems not connected to the Internet.
Authenticity checks for off-line systems can occur at time of deployment of released software.
Off-line systems require appropriate configuration and maintenance to be able to conduct useful authenticity checks.
If the off-line systems in operation are part of constrained node environments, they do not possess the capabilities to process and evaluate all the authenticity proofs that come with the released software.

A consumer of released software wants:

- a proof of authenticity that can be checked by an off-line system for vast periods of time after system deployment
- a proof of authenticity to be small and as uniform as possible to allow for application in constrained node environments
- a simple and low cost way to update the configuration of a system component in charge of validity or authenticity checking

There is no standardized way to:

- provide an authenticity proof that can be checked by off-line systems in a simple and uniform fashion
- enable high performance, and constrained systems to conduct authenticity checks
- verify the authenticity and integrity of software in a fashion that scales

## Firmware Delivery to Large Set of Devices

Firmware is a critical component of constrained IoT devices and general purpose computers.
Firmware is often the bedrock on which the security story of a device is built.
For example, personal health monitoring devices (eHealth devices) are generally battery driven and offer health telemetry monitoring, such as temperature, blood pressure, and pulse rate.
These devices typically connect to the Internet through an intermediary base station using wireless technologies.
Through this connection, the telemetry data and analytics are transfered, and the device receives firmware updates published by vendors.
During initialization, general purpose computers can also have resource constraints like that of constrained IoT devices.
Verification of hardened configuration of the computer's chipset for ongoing telemetry is increasingly important.
After initialization, even if not constrained similarly to IoT devices, the computer's operating system can facilitate telemetry about telemetry settings and measure differences at scale.
The public network, open distribution system, and firmware update process create several security challenges.

Consumers and other interested parties of a firmware update ecosystem want to:

- know that the received firmware for system update is not faulty or malicious
- know if the signing identity used to assert the authenticity of the firmware is somehow used to sign unintended updates
- ascertain that the released firmware is not subverted or compromised due to an insider risk - be it malicious or otherwise
- confirm that the publishers knows if their deliverable has been compromised. For example, can they trust their key protection or audit logging?
- know how the update client on an instance of a health monitoring system discerns a general update from one specially crafted for just a small subset of a fleet of devices
- know if the firwmare has effectively maintained or changed applicable hardware settings after installation

There is no standardized way to:

- provide an update framework that allows validation of authenticity of firmware revisions (in addition to existing approaches, such as {{-SUIT-ARCH}}, {{-SUIT-IM}}, or {{-TUF}})
- to verify that the firmware update seen by a single device, is indeed the same as seen by all the devices
- reliably discern an update that has been signed by the appropriate and intended signing identity
- make an informed judgement on all available information about firmware at the install time.
- implement an update framework with the ability to measure hardware configuration

## Software Integrator Assembling a Software Product for a Smart Car

Software Integration is a complex activity.
This typically involves getting various software components from multiple suppliers, producing an integrated package deployed as part of device assembly.
For example, car manufacturers source integrated software for their autonomous vehicles from third parties that integrates software components from various sources.
Integration complexity creates a higher risk of security vulnerabilities to the delivered software.

Consumers of integrated software want:

- all components presents in a software product listed
- the ability to identify and retrieve all components from a secure and tamper-proof location
- to receive an alert when a vulnerability scan detects a known security issue on a running software component
- verifiable proofs on build process and build environment with all supplier tiers to ensure end to end build quality and security

There is no standardized way to:

- provide a tiered and transparent framework that allows for verification of integrity and authenticity of the integrated software at both component and product level before installation
- notify software integrators of vulnerabilities identified during security scans of running software
- provide valid annotations on build integrity to ensure conformance

## Identify Statements and Updates to Specific Versions of Released Software

Software producers often have multiple and concurrent supported versions of a product.
The versions may represent major feature or compatibility differentiating releases (1.0, 2.0), or implementations for different Operating System Platforms and their respective Instruction Set Architectures (AMD, ARM, x86, x64 for Linux, Mac, and Windows).

For each release, the software producer must be capable of providing statements, unique to that version.
Producers may provide patches to upgrade specific versions and not others.
Consumers need to know which updates are compatible with their environment.
Third parties that provide statements of quality need to know how to differentiate supported version bands, avoiding the recommendation to upgrade to an incompatible version.

As versions lose recency and freshness and vulnerabilities are discovered, consumers need to know the latest version of a particular product.
Software producers implement versioned updates, however there are no standards for consumers and third parties to apply across software producers.

Consumers of related software components want to:

- discover information based on certain aspects of software, such as version, platform architecture, or associated vulnerabilities
- assess the applicability of patches when planning update campaigns

There is no standardized way to:

- associate vulnerability information, statements of quality, statements of support and end of life (EOL) with a specific version of a product
- identify a patched version, specific to their Operating System and Platform
- differentiate major and minor version upgrades
- provide concurrent versioned updates

--- back

<!--
Contributors
============
{: numbered="no"}

 -->

<!--
Acknowledgements
================
{: numbered="false"}
-->
