---
title: "Changes to Microsoft Edge browser TLS server certificate verification"
ms.author: erikan
author: dan-wesley
manager: arvindm
ms.date: 02/08/2023
audience: ITPro
ms.topic: conceptual
ms.prod: microsoft-edge
ms.localizationpriority: high
ms.collection: M365-modern-desktop
description: "An overview of the changes to Microsoft Edge browser TLS server certificate verification"
---

# Changes to Microsoft Edge browser TLS server certificate verification

When establishing connections to an HTTPS server, Microsoft Edge verifies that the server has presented a certificate issued by an entity trusted by the browser. This trust relationship is established via a **certificate trust list** and the component responsible for performing the checks is called the **certificate verifier**.

In past versions of Microsoft Edge, both the default certificate trust list and the certificate verifier logic were provided by underlying operating system (OS) platform.

Starting with Microsoft Edge 111 on Windows and macOS, both the default certificate trust list and the certificate verifier are provided by and shipped with the browser. This decouples the list and verifier from the host operating system's root store for the default verification behavior. See the [rollout timeline and testing guidance](#rollout-timeline-and-testing-guidance) for more detail about the timing of the change.

Even after the change, in addition to trusting the built-in roots that ship with Microsoft Edge, the browser queries the underlying platform for—and trust—locally installed roots that users and/or enterprises installed. As a result, scenarios where a user or enterprise installed additional trusted roots to the host operating system's root store should continue to work.

This change means that certificate verification logic works consistently in Microsoft Edge on Windows and macOS. In a future to-be-determined release, the rollout will also apply to Linux and Android. Due to Apple App Store policies, the Apple-provided root store and certificate verifier will continue to be used on iOS and iPadOS.

## Default certificate trust list source

The root store that ships with Microsoft Edge on Windows and macOS comes from the Certificate Trust List (CTL) defined by the [Microsoft Trusted Root Certificate Program](/security/trusted-root/program-requirements). This is the same root certificate program that defines the list that ships with Microsoft Windows and, as a result, customers should expect to see no user-visible changes.

On macOS, if a certificate was issued by a root certificate trusted by the platform but not by Microsoft's Trusted Root Certificate Program, the certificate will no longer be trusted. This lack of trust isn't expected to be a common situation, since most servers already ensure the TLS certificates that they use are trusted by Microsoft Windows.

Updates will be released on the cadence documented in the [release notes](/security/trusted-root/release-notes) for the Microsoft Trusted Root Program.

## Rollout timeline and testing guidance

Starting in Microsoft Edge 109, an enterprise policy (**MicrosoftRootStoreEnabled**) and a flag in *edge://flags* (“Microsoft Root Store”) is available to control when the built-in root store and certificate verifier are used.

Devices that aren't managed by the enterprise started receiving the feature via a Controlled Feature Rollout (CFR) in Microsoft Edge 109 and 110. For more information, see [Microsoft Edge configurations and experimentation](/deployedge/edge-configuration-and-experiments), which explains how CFRs in Microsoft Edge work. For enterprise-managed devices, the existing platform-provided implementation was used through Microsoft Edge 110.

Starting with Microsoft Edge 111, the default changed for all Windows and macOS devices, including enterprise-managed ones, to use the verifier implementation and CTL shipped with the browser. The **MicrosoftRootStoreEnabled** policy will continue to be available in this release to allow enterprises to revert to the previous behavior if unexpected issues are found and to report the issues to Microsoft.

Microsoft recommends that enterprises that have break-and-inspect proxies or other scenarios involving TLS server certificates issued by roots not in the Microsoft CTL to proactively identiy and report any compatibility issues to Microsoft.

In Microsoft Edge 113, we plan to remove support for the **MicrosoftRootStoreEnabled** policy.


## See also

- [Microsoft Edge security for your business](ms-edge-security-for-business.md)
- [Microsoft Edge Enterprise landing page](https://aka.ms/EdgeEnterprise)
