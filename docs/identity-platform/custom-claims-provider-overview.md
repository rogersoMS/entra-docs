---
title: Custom claims provider overview
description: Conceptual article describing the custom claims provider as part of the custom authentication extension framework.
author: cilwerner
manager: CelesteDG
ms.author: cwerner
ms.custom: 
ms.date: 04/10/2023
ms.reviewer: JasSuri
ms.service: identity-platform

ms.topic: conceptual
titleSuffix: Microsoft identity platform
#Customer intent: As a developer, I want to learn about custom claims provider so that I can augment tokens with claims from an external identity system or role management system.
---

# Custom claims provider (preview)

This article provides an overview to the Microsoft Entra custom claims provider.
When a user authenticates to an application, a custom claims provider can be used to add  claims into the token. A custom claims provider is made up of a custom authentication extension that calls an external REST API, to fetch claims from external systems. A custom claims provider can be assigned to one or many applications in your directory.

Key data about a user is often stored in systems external to Microsoft Entra ID. For example, secondary email, billing tier, or sensitive information. Some applications may rely on these attributes for the application to function as designed. For example, the application may block access to certain features based on a claim in the token.

The following short video provides an excellent overview of the Microsoft Entra custom authentication extensions and custom claims providers:

> [!VIDEO https://www.youtube.com/embed/1tPA7B9ztz0]

Use a custom claims provider for the following scenarios:

- **Migration of legacy systems** - You may have legacy identity systems such as Active Directory Federation Services (AD FS) or data stores (such as LDAP directory) that hold information about users. You'd like to migrate these applications, but can't fully migrate the identity data into Microsoft Entra ID. Your apps may depend on certain information on the token, and can't be rearchitected.
- **Integration with other data stores that can't be synced to the directory** - You may have third-party systems, or your own systems that store user data. Ideally this information could be consolidated, either through [synchronization](~/identity/hybrid/cloud-sync/what-is-cloud-sync.md) or direct migration, in the Microsoft Entra directory. However, that isn't always feasible. The restriction may be because of data residency, regulations, or other requirements.

## Token issuance start event listener

An event listener is a procedure that waits for an event to occur. The custom authentication extension uses the **token issuance start** event listener. The  event is triggered when a token is about to be issued to your application. When the event is triggered the custom authentication extension REST API is called to fetch attributes from external systems.

For an example using a custom claims provider with the **token issuance start** event listener, check out the [get started with custom claims providers](custom-extension-get-started.md) article.

## Next steps

- Learn how to [create and register a custom claims provider](custom-extension-get-started.md) with a sample OpenID Connect application.
- If you already have a custom claims provider registered, you can configure a [SAML application](custom-extension-configure-saml-app.md) to receive tokens with claims sourced from an external store.
- Learn more about custom claims providers with the [custom claims provider reference](custom-claims-provider-reference.md) article.
