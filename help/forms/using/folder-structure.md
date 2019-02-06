---
title: Understanding the folder structure
seo-title: Understanding the folder structure
description: How to understand the folder structure of AEM Forms workspace source code to customize.
seo-description: How to understand the folder structure of AEM Forms workspace source code to customize.
uuid: 84ec0029-c8f5-4c22-b304-46f9d40f8b36
contentOwner: robhagat
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4/FORMS
topic-tags: forms-workspace
discoiquuid: cd935fc9-3523-4663-8984-4ff00843def7
index: y
internal: n
snippet: y
---

# Understanding the folder structure{#understanding-the-folder-structure}

AEM Forms workspace components are designed on MVC architecture using Backbone. Each component has a file for:

* Model, that contains business logic.
* Template, that is an HTML file containing interface controls.
* View, that acts as a Controller class to Template.

The assets for all the components are placed in the folder structure described below. To access the assets, log in to CRXDE Lite and browse to `/libs/ws/js/runtime/`.

**models** Contains backbone models.

**views** Contains backbone views.

**templates** Contains only the HTML templates for the components.

**routes** Contains universal routes. Templates folder inside routes contains the HTML code and the references to the components.

**services** Contains service interface to call Adobe Experience Manager server APIs on REST endpoint.

**util** Contains generic utilities usable by multiple components.

[**Contact Support**](https://www.adobe.com/account/sign-in.supportportal.html)

>[!MORE_LIKE_THIS]
>
>* [Backbone interaction](../../forms/using/backbone-interaction.md)
>* [Description of the reusable components](../../forms/using/description-reusable-components.md)
>* [Understanding the folder structure](../../forms/using/folder-structure.md)
>* [Document details for renderer](../../forms/using/document-details-renderer.md)
>* [Integrating AEM Forms workspace components in web applications](../../forms/using/integrating-html-ws-components-web.md)
>* [New Render and Submit service](../../forms/using/new-render-submit-service.md)