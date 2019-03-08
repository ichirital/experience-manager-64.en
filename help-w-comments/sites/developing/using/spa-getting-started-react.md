---
title: Getting Started with SPAs in AEM - React
seo-title: Getting Started with SPAs in AEM - React
description: This article presents a sample SPA application, explains how it is put together, and allows you to get up-and-running with your own SPA quickly using the React framework.
seo-description: This article presents a sample SPA application, explains how it is put together, and allows you to get up-and-running with your own SPA quickly using the React framework.
uuid: 2b9a291e-8fb6-4e13-a1f7-5b5f0aa44d80
contentOwner: bohnert
products: SG_EXPERIENCEMANAGER/6.4/SITES
topic-tags: spa
content-type: reference
discoiquuid: 76df8de7-a889-42cb-8612-04d373783a4e
index: y
internal: n
snippet: y
---

# Getting Started with SPAs in AEM - React{#getting-started-with-spas-in-aem-react}

Single page applications (SPAs) can offer compelling experiences for website users. Developers want to be able to build sites using SPA frameworks and authors want to seamlessly edit content within AEM for a site built using SPA frameworks.

The SPA authoring feature offers a comprehensive solution for supporting SPAs within AEM. This article presents a simplified SPA application on the React framework, explains how it is put together, allowing you to get up-and-running with your own SPA quickly.

>[!NOTE]
>
>This article is based on the React framework. For the corresponding document for the Angular framework see [Getting Started with SPAs in AEM - Angular](../../../sites/developing/using/spa-getting-started-angular.md).

## Introduction {#introduction}

This article summarizes the basic functioning of a simple SPA and the minimum that you need to know to get yours running.

For more detail on how SPAs work in AEM, see the following documents:

* [SPA Introduction and Walkthrough](../../../sites/developing/using/spa-walkthrough.md)  

* [SPA Authoring Introduction](../../../sites/developing/using/spa-overview.md)  

* [SPA Blueprint](../../../sites/developing/using/spa-blueprint.md)

>[!NOTE]
>
>In order to be able to author content within an SPA, the content must be stored in AEM and be exposed by the content model.
>
>An SPA developed outside of AEM will not be authorable if it does not respect the content model contract.

This document will walk through the structure of a simplified SPA created using the React framework and illustrate how it works so you can apply this understanding to your own SPA.

## Dependencies, Configuration, and Building {#dependencies-configuration-and-building}

In addition to the expected React dependency, the sample SPA can leverage additional libraries to make the creation of the SPA more efficient.

### Dependencies {#dependencies}

The `package.json` file defines the requirements of the overall SPA package. The minimum AEM dependencies for a working SPA are listed here.

```
  "dependencies": {
    "@adobe/cq-react-editable-components": "~1.0.3",
    "@adobe/cq-spa-component-mapping": "~1.0.3",
    "@adobe/cq-spa-page-model-manager": "~1.0.4"
  }
```

Because this example is based on the React framework, there are two React-specific dependencies which are obligatory in the `package.json` file:

```
react
 react-dom
```

<!--
Comment Type: annotation
Last Modified By: fauchere
Last Modified Date: 2018-12-10T10:56:28.858-0500
Doesn't bring enough value and hard to maintain
-->

The `aem-clientlib-generator` is leveraged to make the creation of client libraries automatic as part of the build process.

`"aem-clientlib-generator": "^1.4.1",`

Further details about it can be found [on GitHub here](https://github.com/wcm-io-frontend/aem-clientlib-generator).

>[!CAUTION]
>
>The minimum version of the `aem-clientlib-generator` required is 1.4.1.

The `aem-clientlib-generator` is configured in the `clientlib.config.js` file as follows.

```
module.exports = {
    // default working directory (can be changed per 'cwd' in every asset option)
    context: __dirname,

    // path to the clientlib root folder (output)
    clientLibRoot: "./../content/jcr_root/apps/my-react-app/clientlibs",

    libs: {
        name: "my-react-app",
        allowProxy: true,
        categories: ["my-react-app"],
        embed: ["my-react-app.responsivegrid"],
        jsProcessor: ["min:gcc"],
        serializationFormat: "xml",
        assets: {
            js: [
                "dist/**/*.js"
            ],
            css: [
                "dist/**/*.css"
            ]
        }
    }
};
```

### Building {#building}

Actually building the app leverages [Webpack](https://webpack.js.org/) for transpilation in addition to the aem-clientlib-generator for automatic client library creation. Therefore the build command will resemble:

`"build": "webpack && clientlib --verbose"`

Once built, the package can be uploaded to an AEM instance.

### Maven Archetype for SPA Starter Kit {#maven-archetype-for-spa-starter-kit}

Adobe recommends leveraging the [Maven Archetype for SPA Starter Kit](https://github.com/adobe/aem-spa-project-archetype) to help you start your own SPA project for AEM.

## Application Structure {#application-structure}

Including the dependencies and building your app as described previously will leave you with a working SPA package which you can upload to your AEM instance.

The next section of this document will take you through how a SPA in AEM is structured, the important files which drive the application, and how they work together.

A simplified image component is used as an example, but all components of the application are based on the same concept.

### index.js {#index-js}

The entry point into the SPA is of course the `index.js` file shown here simplified to focus on the important content.

```
import ReactDOM from 'react-dom';
import App from './App';
import { ModelManager, Constants } from "@adobe/cq-spa-page-model-manager";

...

ModelManager.initialize().then((pageModel) => {
ReactDOM.render(
    <App cqChildren={pageModel[Constants.CHILDREN_PROP]} cqItems={pageModel[Constants.ITEMS_PROP]} cqItemsOrder={pageModel[Constants.ITEMS_ORDER_PROP]} cqPath={ModelManager.rootPath} locationPathname={ window.location.pathname }/>
, document.getElementById('page'));

});
```

The main function of `index.js` is to leverage the `ReactDOM.render` function to determine where in the DOM to inject the application.

This is a standard use of this function, not unique to this example app.

#### Static Instantiation {#static-instantiation}

When the component is instantiated statically using the component template (e.g. JSX), the value must be passed from the model to the properties of the component.

### App.js {#app-js}

By rendering the app, `index.js` calls `App.js`, which is shown here in a simplified version to focus on the important content.

```
import {Page, withModel } from '@adobe/cq-react-editable-components';

...

class App extends Page {
...
}

export default withModel(App);
```

`App.js` primarially serves to wrap the root components that compose the app. The entry point of any app is the page.

### Page.js {#page-js}

By rendering the page, `App.js` calls `Page.js` listed here in a simplfied version.

```
import {Page, MapTo, withComponentMappingContext } from "@adobe/cq-react-editable-components";

...

class AppPage extends Page {
...
}

MapTo('my-react-app/components/structure/page')(withComponentMappingContext(AppPage));

```

In this example the `AppPage` class extends `Page`, which contains the inner-content methods that can then be used.

The `Page` ingests the JSON representation of the page model and processes the content to wrap/decorate each element of the page. Further details on the `Page` can be found in the document [SPA Blueprint](../../../sites/developing/using/spa-blueprint.md#main-pars-header-1694932501).

### Image.js {#image-js}

With the page rendered, the components such as `Image.js` as shown here can be rendered.

```
import React, {Component} from 'react';
import {MapTo} from '@adobe/cq-react-editable-components';

require('./Image.css');

const ImageEditConfig = {

    emptyLabel: 'Image',

    isEmpty: function() {
        return !this.props || !this.props.src || this.props.src.trim().length < 1;
    }
};

class Image extends Component {

    render() {
        return (<img src={this.props.src}>);
    }
}

MapTo('my-react-app/components/content/image')(Image, ImageEditConfig);
```

The central idea of SPAs in AEM is the idea of mapping SPA components to AEM components and updating the component when the content is modified (and vice versa). See the document [SPA Editor Overview](../../../sites/developing/using/spa-overview.md) for an summary of this communication model.

`MapTo('my-react-app/components/content/image')(Image, ImageEditConfig);`

The `MapTo` method maps the SPA component to the AEM component. It supports the use of a single string or an array of strings.

`ImageEditConfig` is a configuration object that contributes to enabling the authoring capabilities of a component by providing the necessary metadata for the editor to generate placeholders

If there is no content, labels are provided as placeholders to represent the empty content.

#### Dynamically Passed Properties {#dynamically-passed-properties}

The data coming from the model are dynamically passed as properties of the component.

## Exporting Editable Content {#exporting-editable-content}

You can export a component and keep it editable.

```
import React, { Component } from 'react';
import { MapTo } from '@cq/cq-react-editable-components';

...

const EditConfig = {...}

class PageClass extends Component {...};

...

export default MapTo('my-react-app/react/components/structure/page')(PageClass, EditConfig);
```

The `MapTo` function returns a `Component` which is the result of a composition that extends the provided `PageClass` with the class names and attributes that enable the authoring. This component can be exported to later be instantiated in the markup of your application.

When exported using the `MapTo` or `withModel` functions, the `Page` component, is wrapped with a `ModelProvider` component which provides standard components access to the latest version of the page model or a precise location in that page model.

For more information see the [SPA Blueprint document](../../../sites/developing/using/spa-blueprint.md#main-pars-header-329251743).

>[!NOTE]
>
>By default you receive the entire model of the component when using the `withModel` function.

## Next Steps {#next-steps}

For a step-by-step guide to creating your own SPA, see the [Getting Started with the AEM SPA Editor - WKND Events Tutorial](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html).

For further information about how to oraganize yourself to develop SPAs for AEM see the article [Developing SPAs for AEM](../../../sites/developing/using/spa-architecture.md).

For further details about the dynamic model to component mapping and how it works within SPAs in AEM, see the article [Dynamic Model to Component Mapping for SPAs](../../../sites/developing/using/spa-dynamic-model-to-component-mapping.md).

If you wish to implement SPAs in AEM for a framework other than React or Angular or simply wish to take a deep dive into how the SPA SDK for AEM works, refer to the [SPA Blueprint](../../../sites/developing/using/spa-blueprint.md) article.

>[!MORE_LIKE_THIS]
>
>* [Using the SPA Editor framework with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/spa-editor-framework-feature-video-use.html)
>* [Overview of Single Page Applications (SPA)](https://helpx.adobe.com/experience-manager/kt/sites/using/spa-overview-feature-video.html)
>* [Getting Started with the AEM SPA Editor - Hello World Tutorial](https://helpx.adobe.com/experience-manager/kt/sites/using/spa-editor-helloworld-tutorial-use.html)
>* [Understanding SPA components in AEM SPA Editor](https://helpx.adobe.com/experience-manager/kt/sites/using/spa-editor-components-technical-video-understand.html)
>* [do not publish](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/react/chapter-0-project-setup.html)
>* [Template DO NOT PUBLISH](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/react/template.html)