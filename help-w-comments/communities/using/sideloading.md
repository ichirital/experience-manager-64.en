---
title: Component Sideloading
seo-title: Component Sideloading
description: Communities component sideloading is useful when a web page is designed as a simple, single page app that dynamically alters what is displayed depending on what is selected by the site visitor
seo-description: Communities component sideloading is useful when a web page is designed as a simple, single page app that dynamically alters what is displayed depending on what is selected by the site visitor
uuid: 9dd48fb0-3d0d-4d65-b05f-90782f648420
contentOwner: msm-service
products: SG_EXPERIENCEMANAGER/6.4/COMMUNITIES
topic-tags: developing
content-type: reference
discoiquuid: 6c03bc9d-c830-43cb-b0b0-f6ca93970be2
index: y
internal: n
snippet: y
---

# Component Sideloading{#component-sideloading}

## Overview {#overview}

Communities component sideloading is useful when a web page is designed as a simple, single page app that dynamically alters what is displayed depending on what is selected by the site visitor.

This is accomplished when Communities components do not exist in the page template, but instead are dynamically added following a site visitor's selection.

Since the social component framework (SCF) has a lightweight presence, only SCF components that exist at the time of initial page load are registered. For a dynamically added SCF component to be registered after page load, SCF must be invoked to "sideload" the component.

When a page is designed to sideload Communities components, it is possible to cache the entire page.

The steps to dynamically add SCF components are :

1) [add the component to the DOM](#dynamically-add-component-to-dom)

2) [sideload the component](#sideload-by-invoking-scf) using one of two methods :

* [dynamic inclusion](#dynamic-inclusion)

    * boostrap all dynamically added components

* [dynamic loading](#dynamic-loading)

    * add one specific component on-demand

>[!NOTE]
>
>Sideloading of [non-existing resources](../../communities/using/scf.md#add-or-include-a-communities-component) is not supported.

## Dynamically Add Component to DOM {#dynamically-add-component-to-dom}

Whether the component is dynamically included or dynamically loaded, it must first be added to the DOM.

When adding the SCF component, the most common tag to use is the DIV tag, but other tags may be used as well. Because SCF only examines the DOM when the page initially loads, this addition to the DOM will go unnoticed until SCF is explicitly invoked.

Whatever tag is used, at a minimum, the element must conform to the normal SCF root element pattern by containing these two attributes :

* **data-component-id** 
  the effective path to the added component

* **data-scf-component** 
  the resourceType of the component

Following is one example of an added comments component :

```xml
<div
    class="scf-commentsystem scf translation-commentsystem" 
    data-component-id="<%= currentPage.getPath()%>/jcr:content/content-left/comments"
    data-scf-component="social/commons/components/hbs/comments"
>
</div>
```

## Sideload by Invoking SCF {#sideload-by-invoking-scf}

### Dynamic Inclusion {#dynamic-inclusion}

Dynamic inclusion uses a boostrap request that results in SCF examining the DOM and bootsrapping all SCF components found on the page.

To initialize SCF components anytime after page load, simply fire a JQuery event like this:

$(document).trigger(SCF.events.BOOTSTRAP_REQUEST);

### Dynamic Loading {#dynamic-loading}

Dynamic loading provides control over loading SCF components.

Instead of bootstrapping all SCF components found in the DOM, it is possible to specify a specific SCF component to load using this JavaScript method :

SCF.addComponent(document.getElementById(*someId*));

Where *someId* is the value of the **data-component-id** attribute.