---
title: Rendering form template for HTML5 forms
seo-title: Rendering form template for HTML5 forms
description: HTML5 forms profiles are associated with profile renders. Profile Renders are JSP pages responsible for generating HTML representation of the form by calling the Forms OSGi service.
seo-description: HTML5 forms profiles are associated with profile renders. Profile Renders are JSP pages responsible for generating HTML representation of the form by calling the Forms OSGi service.
uuid: aea72700-9dad-4e02-902c-4ec6273808fb
content-type: reference
products: SG_EXPERIENCEMANAGER/6.4/FORMS
topic-tags: hTML5_forms
discoiquuid: 0547ed7c-6318-4022-b571-54c5d4aaa8e4
index: y
internal: n
snippet: y
---

# Rendering form template for HTML5 forms{#rendering-form-template-for-html-forms}

### Render Endpoint {#render-endpoint}

HTML5 forms have the notion of **Profiles **which are exposed as REST Endpoints to enable Mobile Rendering of Form Templates. These Profiles have associated **Profile Renderer**. They are JSP pages responsible for generating HTML representation of the form by calling the Forms OSGi service. The JCR path of the Profile node determines the URL of the render end point. The default render end point of the form pointing to 'default' profile looks like:

http://&lt;*host*&gt;:&lt;*port*&gt;/content/xfaforms/profiles/default.html?contentRoot=&lt;*path of the folder containg form xdp*&gt;&template=&lt;*name of the xdp*&gt;

For example, `http://localhost:4502/content/xfaforms/profiles/default.html?contentRoot=c:/xdps&template=sampleForm.xdp`

For a custom profile, the endpoint changes accordingly. For example, the end point for the custom profile with the name hrforms is:

`http://localhost:4502/content/xfaforms/profiles/hrforms.html?contentRoot=c:/xdps&template=sampleForm.xdp`

If your template resides in the AEM repository in an application called FormSubmission, the URI is:

```
http://localhost:4502/content/xfaforms/profiles/default.html?
 contentRoot=crx:///content/dam/formsanddocuments/FormSubmission/1.0
 &template=sampleForm.xdp

```

### Render Parameters {#render-parameters}

The request parameters supported while rendering form as HTML are:

<table border="1" cellpadding="1" cellspacing="0" width="100%"> 
 <tbody> 
  <tr> 
   <th><strong>Parameter </strong></th> 
   <th><strong>Description</strong></th> 
  </tr> 
  <tr> 
   <td>template<br /> </td> 
   <td>This parameter specifies the name of the template file.<br /> </td> 
  </tr> 
  <tr> 
   <td>contentRoot<br /> </td> 
   <td>This parameter specifies the path where the template and the associated resources reside. This path can be the server file system path or a repository path or http or an ftp path.<br /> </td> 
  </tr> 
  <tr> 
   <td>submitUrl<br /> </td> 
   <td>This parameter specifies the url to which the form data xml is posted.<br /> </td> 
  </tr> 
 </tbody> 
</table>

#### Merge Data with form template {#merge-data-with-form-template}

| Parameter  |Description |
|---|---|
| dataRef |This parameter specifies** absolute path **of the data file that is merged with the template. This parameter can be a URL to a rest service returning the data in xml format. |
| data |This parameter specifies the UTF-8 encoded data bytes that are merged with the template. If this parameter is specified, the HTML5 form ignores dataRef parameter. |

#### Passing the render parameter {#passing-the-render-parameter}

HTML5 forms support three methods for passing the render parameters. You can pass parameters via URLs, key-value pairs, and profile node. In the render parameter, key-value pair holds highest precedence followed by profile node. The URL Request parameter holds least precedence.

* **URL request parameters**: You can specify the render parameters in the URL. In the URL request parameters, the parameters are visible to the end user. For example, the following submit URL contains template parameter in the URL: `http://localhost:4502/content/xfaforms/profiles/default.html?contentRoot=/Applications/FormSubmission/1.0&template=sampleForm.xdp`

* **SetAttribute request parameters**: You can specify the render parameters as a key-value pair. In the SetAttribute request parameters, the parameters are not visible to the end user. You can forward a request from any other JSP to HTML5 form profile renderer JSP and use *setAttribute* on request object to pass all the render parameters. This method has highest precedence.

* **Profile node request parameters:** You can specify the render parameters as node properties of a profile node. In the profile node request parameters, the parameters are not visible to the end user. Profile node is the node where request is sent. To specify parameters as node properties, use CRXDE lite.

#### Submit Parameters {#submit-parameters}

HTML5 forms submit data; execute server-sided scripts and web-services on AEM servers. For detailed information on parameters used to execute server-sided scripts and web-services on AEM servers, see [HTML5 forms Service Proxy](../../forms/using/service-proxy.md).

[**Contact Support**](https://www.adobe.com/account/sign-in.supportportal.html)

<!--
<related-links>
<a href="../../forms/using/wip/best-practices-design-html5-forms.md">Best practices to design a Mobile form</a>
<a href="../../forms/using/preview-xdp-forms-html.md">Previewing your XDP form in HTML</a>
<a href="../../forms/using/scribble-signature.md">Using Scribble Signature</a>
<a href="../../forms/using/rendering-form-template.md">Rendering Form Template</a>
<a href="../../forms/using/designing-form-template.md">Designing form templates</a>
</related-links>
-->
