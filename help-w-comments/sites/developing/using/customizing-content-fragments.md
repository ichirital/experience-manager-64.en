---
title: Customizing and Extending Content Fragments
seo-title: Customizing and Extending Content Fragments
description: A content fragment extends a standard asset.
seo-description: A content fragment extends a standard asset.
uuid: 4bf16fec-c598-4740-ab7c-3c9200fc4635
contentOwner: User
products: SG_EXPERIENCEMANAGER/6.4/SITES
topic-tags: extending-aem
content-type: reference
discoiquuid: 64fc4265-3301-45dc-87a3-9dde65131564
index: y
internal: n
snippet: y
---

# Customizing and Extending Content Fragments{#customizing-and-extending-content-fragments}

>[!CAUTION]
>
>Some Content Fragment functionality requires the application of [AEM 6.4 Service Pack 2 (6.4.2.0)](../../../release-notes/sp-release-notes.md).

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-03-15T04:18:48.477-0400
<p>Links need to be checked have ones pointing to 6.4 and 6.3</p>
-->

<!--
Comment Type: remark
Last Modified By: unknown unknown (ims-author-57F1056A4CD116590A746C15@AdobeID)
Last Modified Date: 2018-01-18T11:19:30.027-0500
<p>https://docs-author.corp.adobe.com/content/docs/en/aem/6-2/administer/sites/translation/tc-manage.html</p>
-->

A content fragment extends a standard asset; see:

* [Creating and Managing Content Fragments](../../../assets/using/content-fragments.md) and [Page Authoring with Content Fragments](../../../sites/authoring/using/content-fragments.md) for further information about content fragments.

* [Managing Assets](../../../assets/using/managing-assets-touch-ui.md) and [Customizing and Extending Assets](../../../assets/using/extending-assets.md) for further information about standard assets.

## Architecture {#architecture}

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-03-20T05:36:43.036-0400
<p>some changes to this section since the last review - mainly models/templates</p>
-->

The basic [constituent parts](../../../assets/using/content-fragments.md#constituent-parts-of-a-content-fragment) of a content fragment are:

* A *Content Fragment,*
* consisting of one or more *Content Element*s,
* and which can have one or more *Content Variation*s.

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-03-20T05:34:18.330-0400
<p>is the following still valid? for structured fragments where elements == fields (oder?)<br /> </p>
-->

<!--
Comment Type: draft

<ul>
<li>Where:
<ul>
<li>Variations are managed globally; i.e. all elements have the same number of variations with the same name and description, but with (potentially) different content.</li>
</ul> </li>
</ul>
-->

Depending on the type of fragment, either models or templates are also used:

>[!CAUTION]
>
>[Content fragment models](../../../assets/using/content-fragments-models.md) are now recommended for creating all your fragments. 
>
>Content fragment models are used for all examples in We.Retail.

<!--
Comment Type: draft

<note type="caution">
<p><a href="../../../assets/using/content-fragments-models.md">Content fragment models</a> are now recommended for creating all your fragments. See:</p>
<ul>
<li>Customizing Content Fragment Models</li>
<li>Customizing Data Types for Content Fragment Models</li>
</ul>
<p>Content fragment models are used for all examples in We.Retail.</p>
</note>
-->

* Content Fragment Models:

    * Used for defining content fragments that hold structured content.
    * Content fragment models define the structure of a content fragment when it is created.
    * A fragment references the model; so changes to the model may/will impact any dependent fragments.  
    * Models are built-up of data types.  
    * Functions to add new variations, etc., have to update the fragment accordingly.

  <!--
  Comment Type: draft

  <p>Content Fragment Models:</p>
  <ul>
  <li>Used for defining content fragments that hold structured content.</li>
  <li>Content fragment models define the structure of a content fragment when it is created.</li>
  <li>A fragment references the model; so changes to the model may/will impact any dependent fragments.<br /> </li>
  <li>Models are built-up of data types.<br /> </li>
  <li>Functions to add new variations, etc., have to update the fragment accordingly.</li>
  </ul>
  -->

  >[!CAUTION]
  >
  >Any changes to an existing content fragment model can impact dependent fragments; this can lead to orphan properties in those fragments.

* Content Fragment Templates:

    * Used for defining simple content fragments.
    * Templates define the (basic, text-only) structure of a content fragment when it is created.
    * The template is copied to the fragment when it is created; so further changes to the template will not be reflected in existing fragments.
    * Functions to add new variations, etc., have to update the fragment accordingly.
    * [Content fragment templates](../../../sites/developing/using/content-fragment-templates.md) operate in a different manner to that of other templating mechanisms within the AEM ecosystem (e.g. page templates, etc.). Therefore they should be considered separately.
    * When based on a template the MIME type of the content is managed on the actual content; this means that each element and variation can have a different MIME type.

### Integration with Assets {#integration-with-assets}

Content Fragment Management (CFM) is part of AEM Assets as:

* Content fragments are assets.
* They use existing Assets functionality.
* They are fully integrated with Assets (admin consoles, etc.).

#### Mapping Structured Content Fragments to Assets {#mapping-structured-content-fragments-to-assets}

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-03-16T09:18:41.678-0400
<p>new sub-section</p>
-->

![](assets/fragment-to-assets-structured.png)

Content fragments with structured content (i.e. based on a content fragment model) are mapped to a single asset:

* All content is stored under the `jcr:content/data` node of the asset:

    * The element data is stored under the master sub-node:  
      `jcr:content/data/master`
    
    * Variations are stored under a sub-node that carries the name of the variation:  
      e.g. `jcr:content/data/myvariation`
    
    * The data of each element is stored in the respective sub-node as a property with the element name:  
      e.g. the content of element `text` is stored as property `text` on `jcr:content/data/master`

* Metadata and associated content is stored below `jcr:content/metadata`  
  Except for the title and description, which are not considered traditional metadata and stored on `jcr:content`

#### Mapping Simple Content Fragments to Assets {#mapping-simple-content-fragments-to-assets}

![](assets/chlimage_1-264.png)

Simple content fragments (based on a template) are mapped to a composite consisting of a main asset and (optional) sub-assets:

* All non-content information of a fragment (such as title, description, metadata, structure) is managed on the main asset exclusively.
* The content of the first element of a fragment is mapped to the original rendition of the main asset.

    * The variations (if there are any) of the first element are mapped to other renditions of the main asset.

* Additional elements (if existing) are mapped to sub-assets of the main asset.

    * The main content of these additional elements map to the original rendition of the respective sub-asset.
    * Other variations (if applicable) of any additional elements map to other renditions of the respective sub-asset.

#### Asset Location {#asset-location}

As with standard assets, a content fragment is held under:

`/content/dam`

#### Asset Permissions {#asset-permissions}

For further details see [Content Fragment - Delete Considerations](../../../assets/using/content-fragments-delete.md).

#### Feature Integration {#feature-integration}

* The Content Fragment Management (CFM) feature builds on the Assets core, but should be as independent of it as possible.
* CFM provides its own implementations for items in the card/column/list views; these plug into the existing Assets content rendering implementations.
* Several Assets components have been extended to cater for content fragments.

### Using Content Fragments in Pages {#using-content-fragments-in-pages}

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-03-16T09:18:11.708-0400
<p>some changes since last review - mainly models/templates</p>
-->

>[!CAUTION]
>
>The [Content Fragment Core Component](https://helpx.adobe.com/experience-manager/core-components/using/content-fragment-component.html) is now recommended. See [Developing Core Components](https://helpx.adobe.com/experience-manager/core-components/using/developing.html) for more details.

Content fragments can be referenced from AEM pages, just as any other asset type. AEM provides the [**Content Fragment** core component](https://helpx.adobe.com/experience-manager/core-components/using/content-fragment-component.html) - a [component that allows you to include content fragments on your pages](../../../sites/authoring/using/content-fragments.md#adding-a-content-fragment-to-your-page). You can also extend, this **Content Fragment** core component.

* The component uses the `fragmentPath` property to reference the actual content fragment. The `fragmentPath` property is handled in the same manner as similar properties of other asset types; for example, when the content fragment is moved to another location.

* The component allows you to select the variation to be displayed.
* Additionally, a range of paragraphs can be selected to restrict the output; for example, this can be used for multi-column output.
* The component allows [in-between content](../../../sites/developing/using/components-content-fragments.md#in-between-content):

    * Here the component allows you to place other assets (images, etc.) in between the paragraphs of the referenced fragment.
    * For in-between content you need to:

        * be aware of the possibility of unstable references; in-between content (added when authoring a page) has no fixed relationship to the paragraph it is positioned next to, inserting a new paragraph (in the content fragment editor) before the position of the in-between content can lose the relative position
        * consider the additional parameters (such as like variation and paragraph filters) to avoid false positives in search results

>[!NOTE]
>
>**Content Fragment Model:**
>
>When using a content fragment that has been based on a content fragment model on a page, the model is referenced. This means that if the model has not been published at the time you publish the page, this will be flagged and the model added to the resources to be published with the page.
>
>**Content Fragment Template:**
>
>When using a content fragment that has been based on a content fragment template on a page, there is no reference as the template was copied when creating the fragment.

#### Configuration using OSGi console {#configuration-using-osgi-console}

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-03-16T09:18:52.112-0400
<p>new sub-section</p>
-->

The backend implementation of content fragments is, for example, responsible for making instances of a fragment used on a page searchable, or for managing mixed media content. This implementation needs to know which components are used for rendering fragments and how the rendering is parameterized.

The parameters for this can be configured in the [Web Console](../../../sites/deploying/using/configuring-osgi.md#osgi-configuration-with-the-web-console), for the OSGi bundle **DAM Content Fragments Configuration**.

* **Resource types** 
  A list of `sling:resourceTypes` can be provided to define components that are used for rendering content fragments and where the background processing should be applied to.

* **Reference Properties** 
  A list of properties can be configured to specify where the reference to the fragment is stored for the respective component.

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-03-20T06:06:37.548-0400
<p>note below - which property in particular - or both?</p>
-->

>[!NOTE]
>
>There is no direct mapping between property and component type. 
>
>AEM simply takes the first property that can be found on a paragraph. So you should choose the properties carefully.

![](assets/osgi-config.png) 

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-03-15T08:35:00.926-0400
<p style="margin-left: 40px;">if paragraphScope == range then the property paragraphRange defines the range of paragraphs to be rendered (format: see component documentation)</p>
<p>needs a link</p>
-->

There are still some guidelines you must follow to ensure your component is compatible with the content fragment background processing:

* The name of the property where the element(s) to be rendered is defined must be either `element` or `elementNames`.

* The name of the property where the variation to be rendered is defined must be either `variation` or `variationName`.

* If the output of multiple elements is supported (by using `elementNames` to specify multiple elements), the actual display mode is defined by property `displayMode`:

    * If the value is `singleText` (and there is only one element configured) then the element is rendered as a text with in-between content, layout support, etc. This is the default for fragments where only one single element is rendered.
    * Otherwise, a much more simple approach is used (could be called "form view"), where no in-between content is supported and the fragment content is rendered "as is".

* If the fragment is rendered for `displayMode` == `singleText` (implicitly or explicitly) the following additional properties come into play:

    * `paragraphScope` defines whether all paragraphs, or only a range of paragraphs, should be rendered (values: `all` vs. `range`)
    
    * if `paragraphScope` == `range` then the property `paragraphRange` defines the range of paragraphs to be rendered

### Integration with other Frameworks {#integration-with-other-frameworks}

Content fragments can be integrated with:

* 

  <!--
  Comment Type: remark
  Last Modified By: Stefan Grimm (sgrimm)
  Last Modified Date: 2018-03-13T12:46:28.572-0400
  <p>I don't think this is true (unfortunately translations popped up rather late, and we were not able to fully integrate with the translation framework).</p>
  <p>AFAIK:</p>
  <ul>
  <li>Models are not automatically included in translation packages.</li>
  <li>Not sure if they can be translated separately (please ask Mathias, he should be aware)</li>
  <li>There's also a known issue with the We.Retail component (which shouldn't have an impact on this docu though, as the customer is not supposed to base his own work on sample code/content.</li>
  </ul>
  -->

  <!--
  Comment Type: remark
  Last Modified By: Alison Heimoz (aheimoz)
  Last Modified Date: 2018-03-16T04:36:25.169-0400
  <p>"As the model (which also includes lanugage-specific texts such as title, description, etc.) is copied from the template, it can be included in the translation process as well."</p>
  <p>reword to something like?</p>
  <p>As the fragment (which also includes lanugage-specific texts such as title, description, etc.) is dependent on the underlying content fragment model, it can be included in the translation process as well.</p>
  -->

  <!--
  Comment Type: remark
  Last Modified By: Alison Heimoz (aheimoz)
  Last Modified Date: 2018-03-14T06:26:13.548-0400
  <p>following relates to templates/simple fragments (not models)<br /> </p>
  -->

  <!--
  Comment Type: remark
  Last Modified By: Alison Heimoz (aheimoz)
  Last Modified Date: 2018-03-15T08:53:35.732-0400
  <p>have removed the bullet point about models - is the rest generic about the fragments themselves?<br /> </p>
  -->

  <!--
  Comment Type: remark
  Last Modified By: Alison Heimoz (aheimoz)
  Last Modified Date: 2018-03-20T03:28:56.119-0400
  <p>response from Mathias:</p>
  <ol>
  <li>&gt;&gt;&gt; cf models would generally not be translated, just like page templates. Both are stored in /conf. Translation workflows pick up content for translation only in /content.</li>
  <li>&lt;&lt;&lt; think the worry might have been because of field labels, etc.<br /> &gt;&gt;&gt; Those could probably be translated with i18n/dictionary mechanism</li>
  </ol>
  -->

  **Translations**

  Content Fragments are fully integrated with the [AEM translation workflow](../../../sites/administering/using/tc-manage.md). On an architectural level, this means:

    * The individual translations of a content fragment are actually separate fragments; for example:

        * they are located under different language roots:  
          `/content/dam/<*path*/en/<*to*>/<*fragment*>`  
          vs.  
          `/content/dam/<*path*>/de/<*to*>/<*fragment*>`
        
        * but they share exactly the same relative path below the language root:  
          `/content/dam/<*path*>/en/<*to*>/<*fragment*>`  
          vs.  
          `/content/dam/<*path*>/de/<*to*>/<*fragment*>`

    * Besides the rule-based paths, there is no further connection between the different language versions of a content fragment; they are handled as two separate fragments, although the UI provides the means to navigate between the language variants.

  >[!NOTE]
  >
  >The AEM translation workflow works with `/content`:
  >
  >    
  >    
  >    * As the content fragment models reside in `/conf`, these are not included in such translations. You can [internationalize the UI strings](../../../sites/developing/using/i18n-dev.md).  
  >    
  >    * Templates are copied to create the fragment so this is implicit.  
  >    
  >

* **Metadata schemas**

    * Content fragments (re)use the [metadata schemas](../../../assets/using/metadata-schemas.md), that can be defined with standard assets.
    * CFM provides its own, specific schema:  
      `/libs/dam/content/schemaeditors/forms/contentfragment`  
      this can be extended if required.
    
    * The respective schema form is integrated with the fragment editor.

## The Content Fragment Management API - Server-Side {#the-content-fragment-management-api-server-side}

You can use the server-side API to access your content fragments; see:

` [com.adobe.cq.dam.cfm](/sites/developing/using/reference-materials/javadoc/com/adobe/cq/dam/cfm/package-summary.md)`

>[!CAUTION]
>
>It is strongly recommended to use the server-side API instead of directly accessing the content structure.

### Key Interfaces {#key-interfaces}

The following three interfaces can serve as entry points:

* **Fragment Template** ( ` [FragmentTemplate](/sites/developing/using/reference-materials/javadoc/com/adobe/cq/dam/cfm/FragmentTemplate.md)`)

  Use `FragmentTemplate.createFragment()` for creating a new fragment.

  ```
  Resource templateOrModelRsc = resourceResolver.getResource("...");
  FragmentTemplate tpl = templateOrModelRsc.adaptTo(FragmentTemplate.class);
  ContentFragment newFragment = tpl.createFragment(parentRsc, "A fragment name", "A fragment description.");
  
  ```

  <!--
  Comment Type: remark
  Last Modified By: Stefan Grimm (sgrimm)
  Last Modified Date: 2018-03-13T13:07:46.428-0400
  <p>In general this is still valid, but we used "data model" for template-based fragments already - which is somehow different from the "model" of structured fragments.</p>
  <p>Suggestion is to replace</p>
  <p style="margin-left: 40px;"><em>Get the model for a given element/variation</em></p>
  <p>with</p>
  <p style="margin-left: 40px;"><em>Get structural information for a given element/variation</em></p>
  <p>and mention that a FragmentTemplate is used to represent both template and model (naming may be confusing; but it is as it is due to historical reasons - no change to avoid compatibility issues). Same is true for ElementTemplate - despite the naming they reflect both element templates/fields.</p>
  <p>VariationTemplate is (more or less) unrelated to the notion of model vs. template (as it means the same for both types of fragments).</p>
  -->

  <!--
  Comment Type: remark
  Last Modified By: Alison Heimoz (aheimoz)
  Last Modified Date: 2018-03-16T05:18:26.158-0400
  <p>...and use of template was ambiguous as well.....joy</p>
  -->

  <!--
  Comment Type: remark
  Last Modified By: Alison Heimoz (aheimoz)
  Last Modified Date: 2018-03-20T03:37:27.071-0400
  <p>changes made since first review</p>
  -->

  This interface represents:

    * either a content fragment model or content fragment template from which to create a content fragment,  
    * and (after the creation) the structural information of that fragment

  This information can include:

    * Access basic data (title, description)
    * Access templates/models for the elements of the fragment:

        * List element templates
        * Get structural information for a given element
        * Access the element template (see `ElementTemplate`)

    * Access templates for the variations of the fragment:

        * List variation templates
        * Get structural information for a given variation
        * Access the variation template (see `VariationTemplate`)

    * Get initial associated content

  Interfaces that represent important information:

    * `ElementTemplate`

        * Get basic data (name, title)
        * Get initial element content

    * `VariationTemplate`

        * Get basic data (name, title, description)

* **Content Fragment** ( ` [ContentFragment](/sites/developing/using/reference-materials/javadoc/com/adobe/cq/dam/cfm/ContentFragment.md)`)

  This interface allows you to work with a content fragment in an abstract way.

  >[!CAUTION]
  >
  >It is strongly recommended to access a fragment through this interface. Changing the content structure directly should be avoided.

  The interface provides you with the means to:

    * Manage basic data (e.g. get name; get/set title/description)
    * Access meta data
    * Access elements:

        * List elements
        * Get elements by name
        * Create new elements (see [Caveats](#caveats))  
        
        * Access element data (see `ContentElement`)

    * List variations defined for the fragment
    * Create new variations globally
    * Manage associated content:

        * List collections
        * Add collections
        * Remove collections

    * Access the fragment's model or template

  Interfaces that represent the prime elements of a fragment are:

    * **Content Element** ( ` [ContentElement](/sites/developing/using/reference-materials/javadoc/com/adobe/cq/dam/cfm/ContentElement.md)`)

        * Get basic data (name, title, description)
        * Get/Set content
        * Access variations of an element:

            * List variations
            * Get variations by name
            * Create new variations (see [Caveats](#caveats))
            * Remove variations (see [Caveats](#caveats))
            * Access variation data (see `ContentVariation`)

        * Shortcut for resolving variations (applying some additional, implementation-specific fallback logic if the specified variation is not available for an element)

    * **Content Variation** ( ` [ContentVariation](/sites/developing/using/reference-materials/javadoc/com/adobe/cq/dam/cfm/ContentVariation.md)`)

        * Get basic data (name, title, description)
        * Get/Set content
        * Simple synchronization, based on last modified information

  All three interfaces ( `ContentFragment`, `ContentElement`, `ContentVariation`) extend the `Versionable` interface, which adds versioning capabilities, required for content fragments:

    * Create new version of the element
    * List versions of the element
    * Get the content of a specific version of the versioned element

### Adapting - Using adaptTo() {#adapting-using-adaptto}

The following can be adapted:

* `ContentFragment` can be adapted to:

    * `Resource` - the underlying Sling resource; note that updating the underlying `Resource` directly, requires rebuilding the `ContentFragment` object.
    
    * `Asset` - the DAM `Asset` abstraction that represents the content fragment; note that updating the `Asset` directly, requires rebuilding the `ContentFragment` object.

* `ContentElement` can be adapted to:

    * `ElementTemplate` - for accessing the element's structural information.

* `FragmentTemplate` can be adapted to:

  <!--
  Comment Type: remark
  Last Modified By: Alison Heimoz (aheimoz)
  Last Modified Date: 2018-03-14T06:38:55.441-0400
  <p>add link to template and model pages</p>
  -->

    * `Resource` - the `Resource` determining the referenced model or the original template that was copied;

        * changes made through the `Resource` are not automatically reflected in the `FragmentTemplate`.

* `Resource` can be adapted to:

    * `ContentFragment`
    * `FragmentTemplate`

### Caveats {#caveats}

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-03-15T10:17:28.683-0400
<p>updates made since the first review</p>
-->

It should be noted that:

* The API is implemented to provide functionality supported by the UI.
* The entire API is designed to **not** persist changes automatically (unless otherwise noted in the API JavaDoc). So you will always have to commit the resource resolver of the respective request (or the resolver you are actually using).
* Tasks that might require additional effort:

    * Creating/removing new elements will not update the data structure of simple fragments (based on a fragment template).
    * Creating new variations from `ContentElement` will not update the data structure (but creating them globally from `ContentFragment` will).
    
    * Removing existing variations will not update the data structure.

## The Content Fragment Management API - Client-Side {#the-content-fragment-management-api-client-side}

<!--
Comment Type: remark
Last Modified By: Stefan Grimm (sgrimm)
Last Modified Date: 2018-03-14T04:27:34.547-0400
<p>The entire clientside admin part has been marked "internal" (besides a single rendercondition), so there is no such thing anymore. So for 6.4, this entire chapter can be removed.</p>
<p>Things that are not below /libs/dam/cfm/admin may be excluded from that rule, but I will add remarks at the respective content.</p>
<p>I would still mention that the old API is still supported IF 6.4 is run in compat mode (with the compat package installed). And provide a link back to the 6.3 version of the documentation.</p>
-->

>[!CAUTION]
>
>For AEM 6.4 the client-side API is internal.

<!--
Comment Type: draft

<note type="caution">
<p>For AEM 6.4 the client-side API is internal.</p>
<p>If you are using AEM 6.4 in backwards compatibility mode you can refer to the AEM 6.3 documentation.<br /> </p>
</note>
-->

### Additional Information {#additional-information}

<!--
Comment Type: remark
Last Modified By: Stefan Grimm (sgrimm)
Last Modified Date: 2018-03-14T04:28:44.445-0400
<p>I would keep this, except for some areas. See my respective remarks.</p>
-->

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-03-16T05:19:59.383-0400
<p>would it make sense to relocate this....now that it's down to one point...but where does it best belong?</p>
-->

See the following:

* `filter.xml`

  The `filter.xml` for content fragment management is configured so that it does not overlap with the Assets core content package.

## Edit Sessions {#edit-sessions}

<!--
Comment Type: remark
Last Modified By: Stefan Grimm (sgrimm)
Last Modified Date: 2018-03-14T04:33:38.438-0400
<p>Not sure about this section.</p>
<p>I think it's worth keeping with some changes, as it provides some useful background information.</p>
<p>Changes:</p>
<ul>
<li>Remove reference to cfm:block</li>
<li>Entering a page/content change: In 6.4, the edit session is always started when the user enters a page. The system doesn't wait for the first edit. (There's a JIRA to get back to the previous behavior, but we didn't get that far for 6.4.)</li>
</ul>
-->

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-03-16T05:23:45.784-0400
<ul>
<li>have removed the mention cfm:block (haven't replaced it with anything else)</li>
<li>have updated the paragraph about editing session, have left the original in draft for if/when it does revert<br /> </li>
</ul>
-->

An editing session is started when the user opens a content fragment in one of the editor pages. The editing session is finished when the user leaves the editor by selecting either **Save** or **Cancel**.

<!--
Comment Type: draft

<p>An editing session is started when the user opens a content fragment in one of the editor pages and starts editing; for example, by entering text or removing a variation. The editing session is finished when the user leaves the editor by selecting either <strong>Save</strong> or <strong>Cancel</strong>.</p>
-->

#### Requirements {#requirements}

Requirements for controlling an editing session are:

* Editing a content fragment, which can span multiple views (= HTML pages), should be atomic.
* The editing should also be *transactional*; at the end of the edit session the changes must either be committed (saved) or rolled back (cancelled).
* Edge cases should be handled properly; these include situations such as when the user leaving the page by entering a URL manually or using global navigation.
* A periodic auto save (every x minutes) should be available to prevent data loss.
* If a content fragment is edited by two users concurrently, they should not overwrite each other's changes.

#### Processes {#processes}

The processes involved are:

* Starting a session

    * A new version of the content fragment is created.
    * Auto save is started.
    * Cookies are set; these define the currently edited fragment and that there is an edit session open.

* Finishing a session

    * Auto save is stopped.
    * Upon commit:

        * The last modified information is updated.
        * Cookies are removed.

    * Upon rollback:

        * The version of the content fragment that was created when the edit session was started is restored.
        * Cookies are removed.

* Editing

    * All changes (auto save included) are done on the active content fragment - not in a separated, protected area.
    * Therefore, those changes are reflected immediately on AEM pages that reference the respective content fragment

#### Actions {#actions}

The possible actions are:

* Entering a page

    * Check if an editing session is already present; by checking the respective cookie.

        * If one exists, verify that the editing session was started for the content fragment that is currently being edited

            * If the current fragment, reestablish the session.
            * If not, try to cancel editing for the previously edited content fragment and remove cookies (no editing session present afterwards).

        * If no edit session exists, wait for the first change made by the user (see below).

    * Check if the content fragment is already referenced on a page and display appropriate information if so.

* Content change

    * Whenever the user changes content and there is no edit session present, a new edit session is created (see [Starting a session](#processes)).

* Leaving a page

    * If an editing session is present and the changes have not been persisted, a modal confirmation dialog is shown to notify the user of potentially lost content and allow them to stay on the page.

## Examples {#examples}

### Example: Accessing an existing content fragment {#example-accessing-an-existing-content-fragment}

To achieve this you can adapt the resource that represents the API to:

`com.adobe.cq.dam.cfm.ContentFragment`

For example:

```java
// first, get the resource
Resource fragmentResource = resourceResolver.getResource("/content/dam/fragments/my-fragment");
// then adapt it
if (fragmentResource != null) {
    ContentFragment fragment = fragmentResource.adaptTo(ContentFragment.class);
    // the resource is now accessible through the API
} 
```

### Example: Creating a new content fragment {#example-creating-a-new-content-fragment}

To create a new content fragment programmatically, you need to use:

`com.adobe.cq.dam.cfm.ContentFragmentManager#create`

For example:

```java
Resource templateOrModelRsc = resourceResolver.getResource("...");
FragmentTemplate tpl = templateOrModelRsc.adaptTo(FragmentTemplate.class);
ContentFragment newFragment = tpl.createFragment(parentRsc, "A fragment name", "A fragment description.");

```

### Example: Specifying the auto-save interval {#example-specifying-the-auto-save-interval}

<!--
Comment Type: remark
Last Modified By: Alison Heimoz (aheimoz)
Last Modified Date: 2018-02-15T06:39:47.749-0500
<p>need a link to confmgr docu</p>
<p>see https://jira.corp.adobe.com/browse/DOC-6654</p>
<p>now https://jira.corp.adobe.com/browse/CQDOC-6654</p>
-->

<!--
Comment Type: remark
Last Modified By: Stefan Grimm (sgrimm)
Last Modified Date: 2018-03-14T04:42:53.795-0400
<p>Technical documentation for confmgr is: https://git.corp.adobe.com/Granite/com.adobe.granite.confmgr (unfortunately private, so you can't link it).</p>
<p>Unfortunately, there doesn't seem to be "regular" doc available (but I might have missed something - search on docs.adobe.com is not really useful for terms like "configuration manager" ...)</p>
-->

The auto save interval (measured in seconds) can be defined using the configuration manager (ConfMgr):

* Node: `<*conf-root*>/settings/dam/cfm/jcr:content`
* Property Name: `autoSaveInterval`
* Type: `Long`  

* Default: `600` (10 minutes); this is defined on `/libs/settings/dam/cfm/jcr:content`

If you want to set an auto save interval of 5 minutes you need to define the property on your node; for example:

* Node: `/conf/global/settings/dam/cfm/jcr:content`
* Property Name: `autoSaveInterval`  

* Type: `Long`  

* Value: `300` (5 minutes equates to 300 seconds)

## Content Fragment Templates {#content-fragment-templates}

See [Content Fragment Templates](../../../sites/developing/using/content-fragment-templates.md) for full information.

<!--
Comment Type: draft

<h2>Content Fragment Models</h2>
-->

<!--
Comment Type: draft

<p>For further information see:</p>
<ul>
<li>Customizing Content Fragment Models<br /> </li>
<li>Customizing Data Types for Content Fragment Models </li>
</ul>
-->

## Components for Page Authoring {#components-for-page-authoring}

For further information see

* [Core Components - Content Fragment Component](https://helpx.adobe.com/experience-manager/core-components/using/content-fragment-component.html) (recommended)
* [Content Fragment Components - Components for Page Authoring](../../../sites/developing/using/components-content-fragments.md#components-for-page-authoring)

>[!MORE_LIKE_THIS]
>
>* [kt](https://helpx.adobe.com/experience-manager/kt.html)
>* [Assets](https://helpx.adobe.com/experience-manager/kt/assets.html)
>* [Internal](https://helpx.adobe.com/experience-manager/kt/assets/internal.html)
>* [AEM 6.3 Assets: Assets management, metadata, and Insights enhancements](https://helpx.adobe.com/experience-manager/kt/assets/internal/beta-management-metadata-insights-enhancements.html)
>* [AEM 6.3 Assets: Smart tags, templates, and catalog enhancements](https://helpx.adobe.com/experience-manager/kt/assets/internal/beta-tags-templates-catalogs-enhancements.html)
>* [AEM 6.3 What's New - What’s New in AEM Desktop App 1.3 and 1.4](https://helpx.adobe.com/experience-manager/kt/assets/internal/whats-new-assets-smarttags.html)
>* [AEM 6.3 What's New - Smart Tags](https://helpx.adobe.com/experience-manager/kt/assets/internal/whats-new-assets-desktopapp13and14.html)
>* [AEM 6.3 What's New - Related Assets and Multilingual Asset Enhancements](https://helpx.adobe.com/experience-manager/kt/assets/internal/whats-new-assets-related-assets-and-multilingual-enhancements.html)
>* [AEM 6.3 What's New - Asset Templates and Catalog Enhancements](https://helpx.adobe.com/experience-manager/kt/assets/internal/whats-new-assets-templates-catalog-enhancements.html)
>* [AEM 6.3 What's New - Asset Insights Overview and Enhancements](https://helpx.adobe.com/experience-manager/kt/assets/internal/whats-new-assets-insights-overview.html)
>* [AEM 6.3 What's New - Assets Usability and Performance Improvements](https://helpx.adobe.com/experience-manager/kt/assets/internal/whats-new-assets-usability-perf-improvements.html)
>* [AEM 6.3 What's New - General Asset Management Enhancements](https://helpx.adobe.com/experience-manager/kt/assets/internal/whats-new-assets-general-asset-mgmt-enhance.html)
>* [AEM 6.3 Assets: Integration with Livefyre and other enhancements](https://helpx.adobe.com/experience-manager/kt/assets/internal/livefyre-integration-user-experience-improvements.html)
>* [Sizing AEM Assets Deployments](https://helpx.adobe.com/experience-manager/kt/assets/internal/deployment-sizing-calculator-tutorial-use.html)
>* [AEM Assets Brand Portal Enablement Sessions](https://helpx.adobe.com/experience-manager/kt/assets/internal/enablement-sessions-brand-portal-video.html)
>* [KB](https://helpx.adobe.com/experience-manager/kt/assets/kb.html)
>* [Using](https://helpx.adobe.com/experience-manager/kt/assets/using.html)
>* [Using Annotations in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/annotations-feature-video-use.html)
>* [Using Desktop App with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/aem-desktop-app-sync-status-technical-video-use.html)
>* [Set up Asset Insights with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/asset-insights-tutorial-setup.html)
>* [Using Search in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/search-feature-video-use.html)
>* [Understanding Smart Tags in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/smart-tags-feature-video-understand.html)
>* [Using Smart Tags with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/smart-tags-feature-video-use.html)
>* [Set up Smart Tags with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/smart-tags-technical-video-setup.html)
>* [Using the Video Player in AEM Dynamic Media](https://helpx.adobe.com/experience-manager/kt/assets/using/dynamic-media-video-player-feature-video-use.html)
>* [Using Interactive Video with AEM Dynamic Media](https://helpx.adobe.com/experience-manager/kt/assets/using/dynamic-media-interactive-video-feature-video-use.html)
>* [Understanding the Asset Viewer with AEM Dynamic Media](https://helpx.adobe.com/experience-manager/kt/assets/using/dynamic-media-viewer-feature-video-understand.html)
>* [Using Custom Video Thumbnail with AEM Dynamic Media](https://helpx.adobe.com/experience-manager/kt/assets/using/dynamic-media-video-thumbnails-feature-video-use.html)
>* [Understand Search Boosting in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/search-boost-technical-video-understand.html)
>* [Set up Asset Templates with AEM Assets and InDesign Server](https://helpx.adobe.com/experience-manager/kt/assets/using/asset-templates-technical-video-setup.html)
>* [Using Asset Templates with AEM Assets and InDesign Server](https://helpx.adobe.com/experience-manager/kt/assets/using/asset-templates-feature-video-use.html)
>* [Understanding Color Management with AEM Dynamic Media](https://helpx.adobe.com/experience-manager/kt/assets/using/dynamic-media-color-management-technical-video-setup.html)
>* [Using Review Task to compare assets in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/review-task-compare-feature-video-use.html)
>* [Set up Brand Portal with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/brand-portal-technical-video-setup.html)
>* [Using Brand Portal with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/brand-portal-feature-video-use.html)
>* [Understanding Brand Portal with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/brand-portal-article-understand.html)
>* [Using Source File Translation with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/source-file-translation-feature-video-use.html)
>* [Set up 3D with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/3d-assets-technical-video-setup.html)
>* [Using 3D with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/3d-assets-feature-video-use.html)
>* [Developing for Brand Portal with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/brand-portal-technical-video-develop.html)
>* [Understanding InDesign files and Asset Templates in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/asset-templates-tutorial-understand.html)
>* [Using Image Sharpening with AEM Dynamic Media](https://helpx.adobe.com/experience-manager/kt/assets/using/dynamic-media-image-sharpening-feature-video-use.html)
>* [Understanding Multitenancy and Concurrent Development](https://helpx.adobe.com/experience-manager/kt/assets/using/multitenancy-concurrent-development.html)
>* [Understanding Asset Share Commons](https://helpx.adobe.com/experience-manager/kt/assets/using/asset-share-commons-article-understand.html)
>* [Set up Asset Share Commons on local AEM](https://helpx.adobe.com/experience-manager/kt/assets/using/asset-share-commons-article-understand/asset-share-commons-feature-video-setup.html)
>* [Understanding the User Experience of Asset Share Commons](https://helpx.adobe.com/experience-manager/kt/assets/using/asset-share-commons-article-understand/asset-share-commons-user-experience-feature-video-understand.html)
>* [Introduction to Theming in Asset Share Commons](https://helpx.adobe.com/experience-manager/kt/assets/using/asset-share-commons-article-understand/asset-share-commons-feature-video-theming.html)
>* [Using Cascading Metadata in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/cascade-metadata-feature-video-use.html)
>* [Using Brand Portal with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/brand-portal-improvements-feature-video-use.html)
>* [Set up Smart Translation Search with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-technical-video-setup.html)
>* [Using Smart Translation Search with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html)
>* [Using Metadata Import and Export in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/metadata-import-feature-video-use.html)
>* [Using Smart Crop with AEM Assets Dynamic Media](https://helpx.adobe.com/experience-manager/kt/assets/using/smart-crop-feature-video-use.html)
>* [Using Experience Fragments with AEM Assets Dynamic Media](https://helpx.adobe.com/experience-manager/kt/assets/using/dynamic-media-experience-fragments-feature-video-use.html)
>* [Using Reports in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/asset-reports-feature-video-use.html)
>* [Using Closed User Groups with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/closed-user-groups-feature-video-use.html)
>* [Using Enhanced Smart Tags with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/enhanced-smart-tags-feature-video-use.html)
>* [Using Adobe Asset Link Extension with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/adobe-asset-link-feature-video-use.html)
>* [Using Asset Catalog with AEM Commerce and InDesign Server](https://helpx.adobe.com/experience-manager/kt/assets/using/asset-catalog-template-feature-video-use.html)
>* [Understanding Adobe Asset Link authentication with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/adobe-asset-link-authentication-article-understand.html)
>* [Setup Enhanced Smart Tags in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/enhanced-smart-tags-technical-video-setup.html)
>* [Understanding Enhanced Smart Tags with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/enhanced-smart-tags-article-understand.html)
>* [Using Panorama and Vertical Image Viewer with AEM Assets Dynamic Media](https://helpx.adobe.com/experience-manager/kt/assets/using/panorama-vertical-image-viewer-feature-video-use.html)
>* [Using Adobe Stock assets with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/stock-assets-feature-video-use.html)
>* [Set up Adobe Stock with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/adobe-stock-aem-assets-technical-video-setup.html)
>* [Using Check-in/Check-out in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/checkin-checkout-feature-video-use.html)
>* [Understanding Check-in/Check-out in AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/checkin-checkout-technical-video-understand.html)
>* [Set up Asset Insights with AEM Assets and Launch, By Adobe](https://helpx.adobe.com/experience-manager/kt/assets/using/asset-insights-launch-tutorial-setup.html)
>* [Using Remote DAM with AEM Assets](https://helpx.adobe.com/experience-manager/kt/assets/using/remote-dam-feature-video-use.html)
>* [Index](https://helpx.adobe.com/experience-manager/kt/assets/index.html)
>* [AEM Assets 6.3 Videos](https://helpx.adobe.com/experience-manager/kt/assets/index/aem-6-3-assets.html)
>* [AEM Assets 6.4 Videos](https://helpx.adobe.com/experience-manager/kt/assets/index/aem-6-4-assets.html)
>* [AEM Assets 6.4 Videos](https://helpx.adobe.com/experience-manager/kt/assets/index/aem-6-5-assets.html)
>* [Beta](https://helpx.adobe.com/experience-manager/kt/assets/beta.html)
>* [AEM Assets Remote DAM](https://helpx.adobe.com/experience-manager/kt/assets/beta/remote-dam-feature-video-understand.html)
>* [Commerce](https://helpx.adobe.com/experience-manager/kt/commerce.html)
>* [Internal](https://helpx.adobe.com/experience-manager/kt/commerce/internal.html)
>* [KB](https://helpx.adobe.com/experience-manager/kt/commerce/kb.html)
>* [Using](https://helpx.adobe.com/experience-manager/kt/commerce/using.html)
>* [Understanding Architecture of Demandware Integration in AEM](https://helpx.adobe.com/experience-manager/kt/commerce/using/demandware-technical-video-understand.html)
>* [Understanding Demandware Integration in AEM](https://helpx.adobe.com/experience-manager/kt/commerce/using/demandware-feature-video-understand.html)
>* [Index](https://helpx.adobe.com/experience-manager/kt/commerce/index.html)
>* [Videos highlighting the new commerce features for the AEM 6.3 release.](https://helpx.adobe.com/experience-manager/kt/commerce/index/aem-6-3-commerce.html)
>* [Communities](https://helpx.adobe.com/experience-manager/kt/communities.html)
>* [Internal](https://helpx.adobe.com/experience-manager/kt/communities/internal.html)
>* [AEM 6.3 Communities: What's New and Release Overview](https://helpx.adobe.com/experience-manager/kt/communities/internal/beta-what-new-release-overview.html)
>* [AEM 6.3 Communities: Scoring, badging, and ideation](https://helpx.adobe.com/experience-manager/kt/communities/internal/beta-scoring-badging-leaderboards-ideation.html)
>* [AEM 6.3 Communities: Enablement and learning use cases](https://helpx.adobe.com/experience-manager/kt/communities/internal/beta-enablement-learning-usecases.html)
>* [AEM 6.3 Communities: Key features](https://helpx.adobe.com/experience-manager/kt/communities/internal/beta-key-features.html)
>* [KB](https://helpx.adobe.com/experience-manager/kt/communities/kb.html)
>* [Using](https://helpx.adobe.com/experience-manager/kt/communities/using.html)
>* [New Features Overview](https://helpx.adobe.com/experience-manager/kt/communities/using/whats-new-feature-video-understand.html)
>* [Understanding Groups](https://helpx.adobe.com/experience-manager/kt/communities/using/groups-feature-video-understand.html)
>* [Creating a New Community Site](https://helpx.adobe.com/experience-manager/kt/communities/using/create-new-community-site-feature-video-use.html)
>* [How AEM Communities Works](https://helpx.adobe.com/experience-manager/kt/communities/using/overview-feature-video-understand.html)
>* [How to Access Learning Resources](https://helpx.adobe.com/experience-manager/kt/communities/using/viewing-learning-resources-feature-video-use.html)
>* [Calendar Functionality Overview](https://helpx.adobe.com/experience-manager/kt/communities/using/calendar-overview-feature-video-understand.html)
>* [Understanding Blogging Functionality](https://helpx.adobe.com/experience-manager/kt/communities/using/blogs-feature-video-undertstand.html)
>* [Using Reports and Analytics](https://helpx.adobe.com/experience-manager/kt/communities/using/reports-analytics-feature-video-use.html)
>* [Publish Learning Resources for Enablement](https://helpx.adobe.com/experience-manager/kt/communities/using/publish-learning-resources-enablement-feature-video-use.html)
>* [Editing Pages](https://helpx.adobe.com/experience-manager/kt/communities/using/edit-pages-feature-video-use.html)
>* [New Features Overview](https://helpx.adobe.com/experience-manager/kt/communities/using/updates-feature-video-understand.html)
>* [Index](https://helpx.adobe.com/experience-manager/kt/communities/index.html)
>* [AEM 6.3 Communities](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-4/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_915934575/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_915934575/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_915934575/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_915934575/tutorial-card-4/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_1497782182/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_1497782182/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card_1372438351/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card_1372438351/step-with-card-pars/tutorial_cards/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card_1372438351/step-with-card-pars/tutorial_cards/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-3-communities/jcr:content/main-pars/step_with_card_1372438351/step-with-card-pars/tutorial_cards/tutorial-card-4/file.html)
>* [AEM 6.4 Communities](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-4/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_915934575/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_915934575/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_915934575/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_915934575/tutorial-card-4/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_1497782182/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards_1497782182/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card_1372438351/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card_1372438351/step-with-card-pars/tutorial_cards/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card_1372438351/step-with-card-pars/tutorial_cards/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/communities/index/aem-6-4-communities/jcr:content/main-pars/step_with_card_1372438351/step-with-card-pars/tutorial_cards/tutorial-card-4/file.html)
>* [Forms](https://helpx.adobe.com/experience-manager/kt/forms.html)
>* [Internal](https://helpx.adobe.com/experience-manager/kt/forms/internal.html)
>* [AEM 6.3 Forms: New features and enhancements](https://helpx.adobe.com/experience-manager/kt/forms/internal/beta-what-new.html)
>* [AEM 6.3 Forms: Authoring enhancements (Part 1)](https://helpx.adobe.com/experience-manager/kt/forms/internal/beta-authoring-enhancements-1.html)
>* [AEM 6.3 Forms: Authoring enhancements (Part 2)](https://helpx.adobe.com/experience-manager/kt/forms/internal/beta-authoring-enhancements-2.html)
>* [AEM 6.3 Forms: Data Integration](https://helpx.adobe.com/experience-manager/kt/forms/internal/beta-data-integration.html)
>* [AEM 6.3 Forms: Form Workflow on OSGi](https://helpx.adobe.com/experience-manager/kt/forms/internal/beta-forms-workflow-osgi.html)
>* [AEM 6.3 What's New - Theme Editor Correspondence Management Improvements](https://helpx.adobe.com/experience-manager/kt/forms/internal/whats-new-forms-theme-editor.html)
>* [AEM 6.3 What's New - Authoring Improvements Secure and Simplify Authoring](https://helpx.adobe.com/experience-manager/kt/forms/internal/whats-new-forms-authoring-mprovements.html)
>* [AEM 6.3 What's New - Workflow Integration](https://helpx.adobe.com/experience-manager/kt/forms/internal/whats-new-forms-workflow-intergration.html)
>* [AEM 6.3 Forms: AEM Workflow Forms Technical Review](https://helpx.adobe.com/experience-manager/kt/forms/internal/aem-workflow-formstechnicalreview.html)
>* [KB](https://helpx.adobe.com/experience-manager/kt/forms/kb.html)
>* [Using](https://helpx.adobe.com/experience-manager/kt/forms/using.html)
>* [Set up Data Integration with AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/data-integration-technical-video-setup.html)
>* [Using User Profile Data Integration with AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/user-profile-data-integration-feature-video-use.html)
>* [Using JDBC-based Form Data Models with AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/jdbc-data-model-technical-video-use.html)
>* [Using Association Data Models with AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/association-data-model-technical-video-use.html)
>* [Using Service Data Models with AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/service-data-model-technical-video-use.html)
>* [Using CAPTCHAs with AEM Adaptive Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/forms-captcha-feature-video-use.html)
>* [Using Correspondence Manager API via POST invocation in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/correspondence-mamangement-api-article-setup.html)
>* [Using Automated Tests with AEM Adaptive Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/calvin-sdk-test-adaptive-forms-article-use.html)
>* [Using Adobe Sign with AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/adobe-sign-integration-feature-video.html)
>* [Developing with Service Users in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/service-user-tutorial-develop.html)
>* [Using API to generate Document of Record with AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/document-of-record-api-tutorial-use.html)
>* [Using AEM Workflow routing with AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-workflow-integration-technical-video-use.html)
>* [Developing with Useful Utility Functions in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/useful-utility-functions-article-develop.html)
>* [Developing with Output and Forms Services in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/output-and-forms-services-article-develop.html)
>* [Using PDFG in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/using-pdfg-in-aem-forms.html)
>* [Using POST mechanism to launch AEM Forms Create Correspondence Management Inteface](https://helpx.adobe.com/experience-manager/kt/forms/using/using-post-to-open-cm-ui.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/forms/using/using-post-to-open-cm-ui/jcr:content/main-pars/download_section/download-1/file.sftmp)
>* [Configure Adobe Sign for AEM](https://helpx.adobe.com/experience-manager/kt/forms/using/adobe-sign-technical-video-setup.html)
>* [Using Correspondence Management in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/using-correspondence-management-in-aem-forms.html)
>* [Using Assembler Service in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/using-assembler-service-in-aem-forms.html)
>* [Restricting the Rule Editor to specific groups in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/restricting-rule-editor-aem-forms-technical-video-use.html)
>* [Theme Editor Improvements in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/theme-editor-improvements-feature-video-use.html)
>* [Form Editor Improvements in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/form-editor-improvements-feature-video-use.html)
>* [Storing Adaptive Form Data](https://helpx.adobe.com/experience-manager/kt/forms/using/storing_adaptive_form_data_in_db.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/forms/using/storing_adaptive_form_data_in_db/jcr:content/main-pars/image_2073616316/file.sftmp)
>* [List CM Letters in AEM Page](https://helpx.adobe.com/experience-manager/kt/forms/using/listing-letters-in-portal-aem-page.html)
>* [Rule Editor Improvements in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/rule-editor-improvements-feature-video-use.html)
>* [Understanding Automated Forms Testing with AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/calvin-sdk-test-adaptive-forms-feature-video.html)
>* [Editing Improvements for Correspondence Management in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/editing-improvements-correspondence-mgmt-feature-video-use.html)
>* [AEM Forms Samples](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-samples.html)
>* [Configuring Microsoft Dynamics for AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/config-dynamics-for-aem-forms.html)
>* [Using Dynamic Tables in AEM Forms Correspondence Management](https://helpx.adobe.com/experience-manager/kt/forms/using/Dynamic-Tables-AEM-FORMS-CM.html)
>* [Using Microsoft Dynamics with AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/using-ms-dynamics-with-aem-forms.html)
>* [Using Table Component in AEM Forms Print Channel Document](https://helpx.adobe.com/experience-manager/kt/forms/using/table-in-print-channel-documents-video-use.html)
>* [Creating your first interactive communication for web channel](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms.html)
>* [Creating Document Fragments to hold the recipient name and address](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/five.html)
>* [Creating Web Channel Document Template AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/four.html)
>* [Creating Form Data Model](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/three.html)
>* [Creating DataSource Configuration in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/two.html)
>* [Install and Configure Tomcat](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/one.html)
>* [Create Interactive Communication for Web Channel](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/six.html)
>* [Adding text and image content to web channel document](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/seven.html)
>* [Configuring line chart for your first interactive communication document](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/eight.html)
>* [Adding table to account balance panel](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/nine.html)
>* [Configuring Retirement Outlook Panel](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/ten.html)
>* [Configuring Investment Mix Panel](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/eleven.html)
>* [Setting up the delivery of web channel document](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/twelve.html)
>* [Creating Form Data Model](https://helpx.adobe.com/experience-manager/kt/forms/using/creating-form-data-model-feature-video-use.html)
>* [Creating Document Fragments in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/creating-document-fragment-feature-video-use.html)
>* [Creating Web Channel Document Template AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/creating-web-channel-document-template-feature-video-use.html)
>* [Creating DataSource Configuration in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/creating-datasource-feature-video-use.html)
>* [Using Charts In Print Channel Documents](https://helpx.adobe.com/experience-manager/kt/forms/using/using-charts-in-print-channel-documents-aem-forms-video-use.html)
>* [Preparing DataSource For Form Data Model](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
>* [Using Form Data Model To Post Binary Data](https://helpx.adobe.com/experience-manager/kt/forms/using/form-data-model-to-post-binary-data-tutorial-use.html)
>* [Delivery of Interactive Communication Document - Web Channel AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/delivery-of-web-channel-document-tutorial-use.html)
>* [Using Table in Interactive Communication Documents - Web Channel](https://helpx.adobe.com/experience-manager/kt/forms/using/table-in-web-channel-document-video-use.html)
>* [Using JSON Xpath in AEM Forms Form Data Model](https://helpx.adobe.com/experience-manager/kt/forms/using/json-xpath-in-form-data-model-tutorial-use.html)
>* [Using PieCharts in Interactive Communications Document - Web Channel](https://helpx.adobe.com/experience-manager/kt/forms/using/pie-charts-aem-forms-web-channel-video-use.html)
>* [Simplified Steps for Installing AEM Forms on Windows](https://helpx.adobe.com/experience-manager/kt/forms/using/installing-aem-form-on-windows-tutorial-use.html)
>* [Using Reducer Functions in AEM Forms - Charts](https://helpx.adobe.com/experience-manager/kt/forms/using/reducer-functions-in-charts-aem-forms-video-use.html)
>* [Using setvalue in AEM Forms workflow](https://helpx.adobe.com/experience-manager/kt/forms/using/SetValue-in-Aem-Forms-workflow-tutorial-use.html)
>* [Using setvalue in AEM Forms workflow](https://helpx.adobe.com/experience-manager/kt/forms/using/SetValue-in-Aem-Forms-workflow-tutorial-use/SetValue-in-Aem-Forms-workflow-tutorial-use.html)
>* [Developing with Adobe Sign APIs in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/adobe-sign-api-with-aem-tutorial-use.html)
>* [Using Send Email Step of Forms Workflow](https://helpx.adobe.com/experience-manager/kt/forms/using/email-step-aem-workflow-video-use.html)
>* [Using Form Data Model Service as Step in Workflow](https://helpx.adobe.com/experience-manager/kt/forms/using/form-data-model-service-as-step-in-workflow-video-use.html)
>* [Creating Form Data Model Without Data Source](https://helpx.adobe.com/experience-manager/kt/forms/using/form-data-model-without-data-source-feature-video-use.html)
>* [Generating Interactive Communications Document for print channel using watch folder mechanism](https://helpx.adobe.com/experience-manager/kt/forms/using/generating-interactive-communications-print-document-using-api-tutorial-use.html)
>* [Getting Started With Adaptive Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-forms-getting-started-tutorial-use.html)
>* [Creating Adaptive Form](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-forms-getting-started-tutorial-use/part1.html)
>* [Adding Child Panels to Root Panel](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-forms-getting-started-tutorial-use/part2.html)
>* [Adding components to People panel](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-forms-getting-started-tutorial-use/part3.html)
>* [Adding components to Income panel](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-forms-getting-started-tutorial-use/part4.html)
>* [Adding components to Assets section](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-forms-getting-started-tutorial-use/part5.html)
>* [Using functions and code editor](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-forms-getting-started-tutorial-use/part6.html)
>* [Listing Custom Assets Type in AEM Forms Portal](https://helpx.adobe.com/experience-manager/kt/forms/using/listing-custom-asset-types-tutorial-use.html)
>* [Registering Custom Asset Types](https://helpx.adobe.com/experience-manager/kt/forms/using/listing-custom-asset-types-tutorial-use/part1.html)
>* [Listing Custom Asset Types in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/listing-custom-asset-types-tutorial-use/part2.html)
>* [Using Watched Folders in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/watched-folders-document-services-article-use.html)
>* [Handling Adaptive Form Submission](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-form-submission-tutorial-use.html)
>* [Submitting Adaptive Form to AEM Workflow](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-form-submission-tutorial-use/invoking-aem-workflow-on-form-submission-article-use.html)
>* [Submitting Adaptive Form to External Server](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-form-submission-tutorial-use/submitting-adaptive-forms-to-external-server-article-use.html)
>* [Submitting To Thank You Page](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-form-submission-tutorial-use/submitting-adaptive-forms-thank-you-page-article-use.html)
>* [Sending Email on Adaptive Form Submission](https://helpx.adobe.com/experience-manager/kt/forms/using/adaptive-form-submission-tutorial-use/sending-email-on-adaptive-form-submission.html)
>* [Using ldap with Aem Forms Workflow](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-workflow-with-ldap-article-use.html)
>* [Using Document Services in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/documentservices-aem-forms-tutorial-use.html)
>* [Creating your first interactive communication for the print channel](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms.html)
>* [Install and Configure Tomcat](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms/Part1.html)
>* [Creating DataSource Configuration in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms/Part2.html)
>* [Creating Form Data Model](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms/Part3.html)
>* [Create Layout using Forms Designer](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms/Part4.html)
>* [Create Interactive Communication For Print Channel](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms/Part6.html)
>* [Creating Document Fragments to hold the recipient name and address](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms/Part5.html)
>* [Adding text and image content to print channel document](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms/Part7.html)
>* [Adding table to contributions section](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms/Part9.html)
>* [Configuring line chart for your first interactive communication document](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms/Part8.html)
>* [Generating Print Channel Documents Using Watched Folder](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms/Part10.html)
>* [Opening Agent UI On POST Submission](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms/Part11.html)
>* [Getting Started With AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-getting-started-tutorial-use.html)
>* [Prefill Service in Adaptive Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/prefill-service-adaptive-forms-article-use.html)
>* [Acroform To AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/acroforms-aem-forms-tutorial-use.html)
>* [Two Column Layout in AEM Forms Print Channel Document](https://helpx.adobe.com/experience-manager/kt/forms/using/two-column-layout-aem-forms-article-use.html)
>* [Tagging and Storing AEM Forms DoR in DAM](https://helpx.adobe.com/experience-manager/kt/forms/using/tagging-and-saving-document-of-record-in-dam-article-use.html)
>* [Using Transaction Reporting in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/transaction-reporting-aem-forms-article-use.html)
>* [Data based routing of Adaptive Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/routing-adaptive-forms-based-on-data-aem-forms-article-use.html)
>* [Certifying Document in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/certifying-documents-aem-forms-tutorial.html)
>* [Capturing workflow comments in Adaptive Forms Workflow](https://helpx.adobe.com/experience-manager/kt/forms/using/capturing-workflow-comments-aem-workflow-article.html)
>* [PrePopulate HTML5 Forms using data attribute](https://helpx.adobe.com/experience-manager/kt/forms/using/prepopulating_html5_forms_in_aem_forms-article.html)
>* [Setting value of Json Data Element in AEM Forms Workflow](https://helpx.adobe.com/experience-manager/kt/forms/using/setvalue-json-data-in-aem-forms-workflow-article-use.html)
>* [Configuring DataSource with Salesforce in AEM Forms 6.3 and 6.4](https://helpx.adobe.com/experience-manager/kt/forms/using/using-adaptive-forms-with-sales-force-integration-tutorial.html)
>* [Creating your first interactive communication for the print channel](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial.html)
>* [Install and Configure Tomcat](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial/Part1.html)
>* [Creating DataSource Configuration in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial/Part2.html)
>* [Creating Form Data Model](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial/Part3.html)
>* [Create Layout using Forms Designer](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial/Part4.html)
>* [Create Interactive Communication For Print Channel](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial/Part6.html)
>* [Creating Document Fragments to hold the recipient name and address](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial/Part5.html)
>* [Adding text and image content to print channel document](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial/Part7.html)
>* [Adding table to contributions section](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial/Part9.html)
>* [Configuring line chart for your first interactive communication document](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial/Part8.html)
>* [Generating Print Channel Documents Using Watched Folder](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial/Part10.html)
>* [Opening Agent UI On POST Submission](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-print-channel-aem-forms-tutorial/Part11.html)
>* [Using Geolocation API's in Adaptive Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/using-geolocation-api-in-aem-forms-article.html)
>* [Creating Computed Form Data Model Elements in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/computed-form-data-model-elements-aem-forms-feature-video.html)
>* [Getting Started with AEM Forms and Adobe Campaign Standard](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-with-campaign-standard-getting-started-tutorial.html)
>* [Generating JSON Web Token and Access Token](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-with-campaign-standard-getting-started-tutorial/Part1.html)
>* [Prefilling Adaptive Form using ACS Profile](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-with-campaign-standard-getting-started-tutorial/Part3.html)
>* [Creating Campaign Profile on Adaptive Form Submission](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-with-campaign-standard-getting-started-tutorial/Part2.html)
>* [Create Campaign Profile Using Form Data Model](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-with-campaign-standard-getting-started-tutorial/Part4.html)
>* [Create Form Data Model to fetch ACS profile information](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-with-campaign-standard-getting-started-tutorial/Part5.html)
>* [Merging Adaptive Form Data with Acroform](https://helpx.adobe.com/experience-manager/kt/forms/using/merging-adaptive-form-data-with-acroform-article-use.html)
>* [Generate PDF from HTML5 Form Submission](https://helpx.adobe.com/experience-manager/kt/forms/using/generating-pdf-on-mobile-form-submission-article-use.html)
>* [Translating Adaptive Form](https://helpx.adobe.com/experience-manager/kt/forms/using/translating-adaptive-forms-tutorial-use.html)
>* [Integrate AEM Forms with Microsoft Dynamics 365](https://helpx.adobe.com/experience-manager/kt/forms/using/Integrate-AEM-Forms-With-Dynamics-365.html)
>* [Implementing Custom AEM Process Step](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-process-step-aem-forms-tutorial.html)
>* [AEM Forms with JSON Schema and Data](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-with-json-schema-and-data-tutorial.html)
>* [Create Adaptive Form based on JSON Schema](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-with-json-schema-and-data-tutorial/part1.html)
>* [Storing Submitted Data in Database](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-with-json-schema-and-data-tutorial/part2.html)
>* [Storing JSON Schema in Database](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-with-json-schema-and-data-tutorial/part3.html)
>* [Querying Submitted Data](https://helpx.adobe.com/experience-manager/kt/forms/using/aem-forms-with-json-schema-and-data-tutorial/part4.html)
>* [Writing a Custom Submit in AEM Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)
>* [Rendering XDP into PDF with Usage Rights](https://helpx.adobe.com/experience-manager/kt/forms/using/rendering-and-reader-extending-xdp-templates-article.html)
>* [Ability to modify Data Source Configuration Settings](https://helpx.adobe.com/experience-manager/kt/forms/using/modify-data-source-configuration-settings-article.html)
>* [Create Re-Usable AEM Forms Workflow Models](https://helpx.adobe.com/experience-manager/kt/forms/using/re-usable-aem-forms-workflow-models-article.html)
>* [Ability to add web channel to an existing print channel](https://helpx.adobe.com/experience-manager/kt/forms/using/add-web-channel-to-print-channel-document-article.html)
>* [Barcode Service With Adaptive Forms](https://helpx.adobe.com/experience-manager/kt/forms/using/barcode-service-adaptive-forms-article.html)
>* [Generate PDF from HTM5 Form Submission](https://helpx.adobe.com/experience-manager/kt/forms/using/generate-pdf-from-mobile-form-submission-article.html)
>* [Index](https://helpx.adobe.com/experience-manager/kt/forms/index.html)
>* [AEM 6.3 Forms Tutorials and Videos](https://helpx.adobe.com/experience-manager/kt/forms/index/aem-6-3-forms.html)
>* [AEM 6.4 Forms Tutorials and Videos](https://helpx.adobe.com/experience-manager/kt/forms/index/aem-6-4-forms.html)
>* [AEM 6.5 Forms Tutorials and Videos](https://helpx.adobe.com/experience-manager/kt/forms/index/aem-6-5-forms.html)
>* [Beta](https://helpx.adobe.com/experience-manager/kt/forms/beta.html)
>* [Form Data Model Enhancements](https://helpx.adobe.com/experience-manager/kt/forms/beta/form-data-model-enhancements-feature-video-understand.html)
>* [Interactive Communication Enhancements](https://helpx.adobe.com/experience-manager/kt/forms/beta/interactive-communication-enhancements-feature-video-understand.html)
>* [Using Adobe Sign Cloud Signatures with Adaptive Forms](https://helpx.adobe.com/experience-manager/kt/forms/beta/adobe-sign-cloud-signatures-adaptive-forms-feature-video-understand.html)
>* [Dynamically Populate Adaptive Form Controls](https://helpx.adobe.com/experience-manager/kt/forms/beta/dynamic-population-adaptive-forms-controls-feature-video-understand.html)
>* [Workflow Enhancements](https://helpx.adobe.com/experience-manager/kt/forms/beta/workflow-enhancements-feature-video-understand.html)
>* [Mobile](https://helpx.adobe.com/experience-manager/kt/mobile.html)
>* [Internal](https://helpx.adobe.com/experience-manager/kt/mobile/internal.html)
>* [KB](https://helpx.adobe.com/experience-manager/kt/mobile/kb.html)
>* [Using](https://helpx.adobe.com/experience-manager/kt/mobile/using.html)
>* [Index](https://helpx.adobe.com/experience-manager/kt/mobile/index.html)
>* [AEM Foundation](https://helpx.adobe.com/experience-manager/kt/platform-repository.html)
>* [Internal](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal.html)
>* [AEM 6.3 Platform: Online revision cleanup on TarMK](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/beta-online-revision-cleanup-tarmk.html)
>* [AEM 6.3 Platform: Projects, workflows, and inbox](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/beta-projects-workflows-inbox.html)
>* [AEM 6.3 What's New - We.Retail Reference Site](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/whats-new-platform-weretailreferencesite.html)
>* [AEM 6.3 What's New - Integrations with Marketing Cloud Solutions](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/whats-new-platform-integrationswithMCsolutions.html)
>* [AEM 6.3 What's New - MongoMK Cross-geo Deployment and Production Readiness](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/whats-new-platform-mongomkcrossgeodeployment.html)
>* [AEM 6.3 What's New - HTL Enhancements](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/whats-new-platform-htl-enhancements.html)
>* [AEM 6.3 What's New Videos](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_770649399/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_770649399/step-with-card-pars/tutorial_cards/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_770649399/step-with-card-pars/tutorial_cards/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_770649399/step-with-card-pars/tutorial_cards/tutorial-card-4/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_770649399/step-with-card-pars/tutorial_cards_505605962/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_770649399/step-with-card-pars/tutorial_cards_505605962/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_770649399/step-with-card-pars/tutorial_cards_505605962/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-4/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_1138826323/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_1138826323/step-with-card-pars/tutorial_cards/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_1138826323/step-with-card-pars/tutorial_cards/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_1138826323/step-with-card-pars/tutorial_cards/tutorial-card-4/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_1138826323/step-with-card-pars/tutorial_cards_769783197/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_35738720/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_35738720/step-with-card-pars/tutorial_cards/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/aem-6-3-whats-new-videos/jcr:content/main-pars/step_with_card_35738720/step-with-card-pars/tutorial_cards/tutorial-card-3/file.html)
>* [AEM 6.3 Platform: Upgrade](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/beta-upgrade.html)
>* [AEM 6.3 Platform: Operations dashboard](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/beta-operations-dashboard.html)
>* [AEM 6.3 What's New - UI Updates](https://helpx.adobe.com/experience-manager/kt/platform-repository/internal/whats-new-platform-uiupdates.html)
>* [KB](https://helpx.adobe.com/experience-manager/kt/platform-repository/kb.html)
>* [Using](https://helpx.adobe.com/experience-manager/kt/platform-repository/using.html)
>* [Developing for the OSGi HTTP Whiteboard in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/osgi-http-whiteboard-code-sample-develop.html)
>* [Developing OAuth Scopes in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/oauth-code-sample-develop.html)
>* [Developing Sling Model Exporters in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-model-exporter-tutorial-develop.html)
>* [Understanding Sling Model Exporters in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-model-exporter-tutorial-understand.html)
>* [Using Calendar View with AEM Projects and Inbox](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/projects-and-inbox-calendar-view-feature-video-use.html)
>* [Using the Inbox in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/inbox-feature-video-use.html)
>* [Developing Projects in AEM - Part 1](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/projects-part1-tutorial-develop.html)
>* [Developing Projects in AEM - Part 2](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/projects-part2-tutorial-develop.html)
>* [Developing for Task Management in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/task-management-api-code-sample-develop.html)
>* [Using Online Revision Clean-up in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/revision-cleanup-technical-video-use.html)
>* [Set up AEM Dispatcher on macOS](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/dispatcher-macos-technical-video-setup.html)
>* [Set up Sling Dynamic Include in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-dynamic-include-technical-video-setup.html)
>* [Understanding Content Fragments and Experience Fragments in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/content-fragments-experience-fragments-article-understand.html)
>* [Using Project Masters in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/projects-masters-feature-video-use.html)
>* [Using the SSL Wizard in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html)
>* [Developing for AEM 6.3](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/aem-6-3-article-develop.html)
>* [Developing for Cross-Origin Resource Sharing (CORS) with AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/cors-security-technical-video-develop.html)
>* [Understanding Cross-Origin Resource Sharing (CORS) with AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/cors-security-article-understand.html)
>* [Understanding Authentication support in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/authentication-support-article-understand.html)
>* [Developing a new project in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/new-aem-project-tutorial-develop.html)
>* [Set up an AEM Project using the AEM Maven Project Archetype](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/maven-archetype-technical-video-setup.html)
>* [User Experience Enhancements in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/enhancements-ux-feature-video-use.html)
>* [Using the System Overview Dashboard in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/system-overview-feature-video-use.html)
>* [Using oak-run.jar to Manage Indexes in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/oak-run-index-technical-video-use.html)
>* [Understanding Reasons to Upgrade AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/upgrade-aem-article-understand.html)
>* [Understanding Reasons to Upgrade to AEM 6.3](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/upgrade-aem-article-understand/upgrade-aem-6-3-article-understand.html)
>* [Understanding Reasons to Upgrade to AEM 6.3](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/upgrade-aem-6-3-article-understand.html)
>* [Developing for AEM 6.4](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/aem-article-develop.html)
>* [Developing for AEM 6.3](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/aem-article-develop/aem-6-3-article-develop.html)
>* [Using the Workflow Editor in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/workflow-editor-feature-video-use.html)
>* [Using the CI/CD Pipeline in Cloud Manager for AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/cloud-manager-cicd-pipeline-feature-video-use.html)
>* [AEM Security Notification (November 2018)](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/aem-security-notification-article.html)
>* [Understanding Java API preference in AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/java-apis-article-understand.html)
>* [Set up a Local AEM Development Environment](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)
>* [Understanding Adobe IMS authentication with AEM on Adobe Managed Services](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/adobe-ims-authentication-technical-video-understand.html)
>* [Setup public and private keys for use with AEM and Adobe I/O](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/public-private-keys-tutorial-setup.html)
>* [Index](https://helpx.adobe.com/experience-manager/kt/platform-repository/index.html)
>* [AEM 6.3 tutorials videos new platform foundation Adobe Experience Manager](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-3-platform.html)
>* [AEM 6.3 feature videos new projects Adobe Experience Manager](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-3-projects.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-3-projects/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-3-projects/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-3-projects/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-3-projects/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-4/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-3-projects/jcr:content/main-pars/step_with_card_892877700/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [AEM 6.4 tutorial videos new platform foundation Adobe Experience Manager](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-4-foundation.html)
>* [AEM 6.3 feature videos new projects Adobe Experience Manager](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-4-projects.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-4-projects/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-4-projects/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-2/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-4-projects/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-3/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-4-projects/jcr:content/main-pars/step_with_card/step-with-card-pars/tutorial_cards/tutorial-card-4/file.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/platform-repository/index/aem-6-4-projects/jcr:content/main-pars/step_with_card_892877700/step-with-card-pars/tutorial_cards/tutorial-card-1/file.html)
>* [Sites](https://helpx.adobe.com/experience-manager/kt/sites.html)
>* [Internal](https://helpx.adobe.com/experience-manager/kt/sites/internal.html)
>* [AEM 6.3 Sites: Admin and page editor improvements](https://helpx.adobe.com/experience-manager/kt/sites/internal/beta-site-admin-page-editor-improvements.html)
>* [AEM 6.3 Sites: Core components and template editor improvements](https://helpx.adobe.com/experience-manager/kt/sites/internal/beta-core-components-template-editor-improvements.html)
>* [AEM 6.3 What's New - Core Components](https://helpx.adobe.com/experience-manager/kt/sites/internal/whats-new-sites-core-components.html)
>* [AEM 6.3 What's New - Experience Fragments](https://helpx.adobe.com/experience-manager/kt/sites/internal/whats-new-sites-experience-fragments.html)
>* [AEM 6.3 What's New - Content Fragments](https://helpx.adobe.com/experience-manager/kt/sites/internal/whats-new-sites-content-fragments.html)
>* [AEM 6.3 What's New - Sites Admin and Page Authoring Improvements](https://helpx.adobe.com/experience-manager/kt/sites/internal/whats-new-sites-admin-page-authoring.html)
>* [AEM 6.3 Sites: Content Fragments](https://helpx.adobe.com/experience-manager/kt/sites/internal/beta-content-fragments.html)
>* [AEM 6.3 Sites: Experience Fragments](https://helpx.adobe.com/experience-manager/kt/sites/internal/beta-experience-fragments.html)
>* [KB](https://helpx.adobe.com/experience-manager/kt/sites/kb.html)
>* [Using](https://helpx.adobe.com/experience-manager/kt/sites/using.html)
>* [Using Mixed-media with AEM Content Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-mixed-media-feature-video-use.html)
>* [Using Content Fragments in AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
>* [Using Content Fragments in AEM 6.3](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use/aem-63.html)
>* [Understanding AEM Content Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html)
>* [Using AEM Experience Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
>* [Using Translation with AEM Content Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-translation-feature-video-use.html)
>* [Touch UI Authoring Improvements in AEM 6.3](https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html)
>* [Introducing Multi Site Manager Touch UI Interfaces](https://helpx.adobe.com/experience-manager/kt/sites/using/multi-site-manager-feature-video-use.html)
>* [Using Publication Management with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/publication-management-feature-video-use.html)
>* [Understanding AEM Content Fragment Delivery Considerations](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-delivery-considerations-article-understand.html)
>* [Using Language Copy with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/language-copy-feature-video-use.html)
>* [Set up Social Posting with AEM Experience Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-social-technical-video-setup.html)
>* [Developing Component Icons in AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/component-icons-technical-video-develop.html)
>* [Using the Components Console with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/components-console-feature-video-use.html)
>* [Set up Adobe Analytics Activity Map with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/activity-map-feature-video-setup.html)
>* [Using Adobe Analytics Activity Map with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/activity-map-feature-video-use.html)
>* [Troubleshooting Adobe Analytics Activity Map with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/activity-map-feature-video-troubleshoot.html)
>* [Using Timewarp with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/timewarp-feature-video-use.html)
>* [Template Editor Enhancements in AEM 6.3](https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html)
>* [Using Page Difference with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/page-diff-feature-video-use.html)
>* [Simple search implementation guide](https://helpx.adobe.com/experience-manager/kt/sites/using/search-tutorial-develop.html)
>* [Developing Resource Statuses in AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/resource-status-tutorial-develop.html)
>* [Understanding Mixed-Media with AEM Content Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-mixed-media-technical-video-understand.html)
>* [Setup Translation Rules in AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/translation-rules-editor-technical-video-setup.html)
>* [Understanding Publication Management with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/publication-management-feature-video-understand.html)
>* [Developing for Page Difference in AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/page-diff-technical-video-develop.html)
>* [Using Social Media Sharing in AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/social-media-sharing-feature-video-use.html)
>* [Understanding AEM Experience Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
>* [Understanding Core Components](https://helpx.adobe.com/experience-manager/kt/sites/using/core-components-feature-video-understand.html)
>* [Getting Started with AEM Sites - WKND Tutorial](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop.html)
>* [Getting Started with AEM Sites Chapter 1 - Project Setup](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/part1.html)
>* [Getting Started with AEM Sites Chapter 2 - Creating a Base Page and Template](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/part2.html)
>* [Getting Started with AEM Sites Chapter 3 - Client-Side Libraries and Responsive Grid](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/part3.html)
>* [Getting Started with AEM Sites Chapter 4 - Developing with the Style System](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/part4.html)
>* [Getting Started with AEM Sites Chapter 5 - Navigation](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/part5.html)
>* [legacy](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/legacy.html)
>* [Getting Started with AEM Sites Part 1 - Project Setup](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/legacy/part1.html)
>* [Getting Started with AEM Sites Part 2 - Creating a Base Page and Template](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/legacy/part2.html)
>* [Getting Started with AEM Sites Part 3 - Client-Side Libraries and Responsive Grid](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/legacy/part3.html)
>* [Getting Started with AEM Sites Part 4 - Developing with the Style System](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/legacy/part4.html)
>* [Getting Started with AEM Sites Part 5 - Navigation](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/legacy/part5.html)
>* [Getting Started with AEM Sites Part 6 - Sling Models and Card Component](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/legacy/part6.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/legacy/part6/jcr:content/main-pars/procedure_236510881/proc_par/step_174669634/step_par/download_section/download-1/file.sftmp)
>* [Getting Started with AEM Sites Chapter 6 - Creating a new AEM Component](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/part6.html)
>* [Getting Started with AEM Sites Chapter 7 - Teaser and Carousel Components](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/part7.html)
>* [Getting Started with AEM Sites Chapter 8 - Unit Testing](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-wknd-tutorial-develop/part8.html)
>* [Extending Page Properties in AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/page-properties-technical-video-develop.html)
>* [Extending Page Properties in AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/page-properties-technical-video-develop/6-3-page-properties-technical-video-develop.html)
>* [Understanding Responsive Layout with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/responsive-layout-feature-video-understand.html)
>* [Using the Simple project with AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/simple-code-use.html)
>* [Using the Simple Plus PJAX project with AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/simple-code-use/simple-plus-pjax-code-use.html)
>* [Using the Simple Plus Bootstrap 3 project with AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/simple-code-use/simple-plus-bootstrap-3-code-use.html)
>* [Using Content Fragments and Content Services in AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
>* [Set up Content Fragments and Content Services in AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-content-fragments-content-services-article-setup.html)
>* [Using AEM Experience Fragments with Adobe Target](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragment-target-feature-video-use.html)
>* [Getting Started with AEM Content Services](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html)
>* [Getting Started with AEM Content Services - Part 1 - AEM Content Services Set up](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use/part1.html)
>* [Getting Started with AEM Content Services - Part 2 - Defining FAQ Content Fragment Models](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use/part2.html)
>* [Getting Started with AEM Content Services - Part 3 - Authoring FAQ Content Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use/part3.html)
>* [Getting Started with AEM Content Services - Part 4 - Defining Content Services Templates](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use/part4.html)
>* [Getting Started with AEM Content Services - Part 5 - Authoring Content Services Pages](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use/part5.html)
>* [Getting Started with AEM Content Services - Part 6 - Exposing the Content on AEM Publish](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use/part6.html)
>* [Getting Started with AEM Content Services - Part 7 - Consuming AEM Content Services from a 3rd Party App](https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use/part7.html)
>* [Using the Style System with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-feature-video-use.html)
>* [Using the Style System with AEM 6.3 Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-feature-video-use/aem-63.html)
>* [Using Building Blocks with AEM Experience Fragments](https://helpx.adobe.com/experience-manager/kt/sites/using/building-blocks-experience-fragment-feature-video-use.html)
>* [Translation Enhancements in AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/translation-enhancements-feature-video-use.html)
>* [Understanding how to code for the AEM Style System](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-technical-video-understand.html)
>* [Understanding Content Fragments and Content Services in AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-content-services-feature-video-understand.html)
>* [Getting Started with Core Components and the AEM Style System](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop.html)
>* [Getting Started with Core Components and the AEM Style System - Part 1 - AEM Core Components](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part1.html)
>* [Getting Started with Core Components and the AEM Style System - Part 0 - Set up](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part0.html)
>* [Getting Started with Core Components and the AEM Style System - Part 2 - Preparing the Page Template](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part2.html)
>* [Getting Started with Core Components and the AEM Style System - Part 3 - Content-first Authoring](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part3.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part3/jcr:content/main-pars/procedure/proc_par/step_1319053040/step_par/image_365829580/file.sftmp)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part3/jcr:content/main-pars/procedure/proc_par/step_431396762/step_par/image/file.sftmp)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part3/jcr:content/main-pars/procedure/proc_par/step_126729473/step_par/image/file.sftmp)
>* [Getting Started with Core Components and the AEM Style System - Part 4 - Organizing Client Libraries, CSS & JavaScript](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part4.html)
>* [Getting Started with Core Components and the AEM Style System - Part 5 - Applying the Basic Dopetrope Style](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part5.html)
>* [Getting Started with Core Components and the AEM Style System - Part 6 - Applying the Teaser Component CSS-based Styles](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part6.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part6/jcr:content/main-pars/procedure/proc_par/step_0/step_par/image/file.sftmp)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part6/jcr:content/main-pars/procedure/proc_par/step_4/step_par/image/file.sftmp)
>* [Getting Started with Core Components and the AEM Style System - Part 7 - Applying the Teaser Component JavaScript-based Styles](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part7.html)
>* [Getting Started with Core Components and the AEM Style System - Part 8 - Exploration of How Other Components Were Styled](https://helpx.adobe.com/experience-manager/kt/sites/using/style-system-core-components-tutorial-develop/part8.html)
>* [Using the SPA Editor framework with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/spa-editor-framework-feature-video-use.html)
>* [Overview of AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/overview-feature-video-understand.html)
>* [Set Up Experience Fragments and Adobe Target Integration in AEM](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragment-target-feature-video-setup.html)
>* [Using AEM Experience Fragment Offers within Adobe Target](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragment-target-offer-feature-video-use.html)
>* [Overview of Single Page Applications (SPA)](https://helpx.adobe.com/experience-manager/kt/sites/using/spa-overview-feature-video.html)
>* [Getting Started with the AEM SPA Editor - Hello World Tutorial](https://helpx.adobe.com/experience-manager/kt/sites/using/spa-editor-helloworld-tutorial-use.html)
>* [Understanding SPA components in AEM SPA Editor](https://helpx.adobe.com/experience-manager/kt/sites/using/spa-editor-components-technical-video-understand.html)
>* [Understanding style organization with the AEM Style System](https://helpx.adobe.com/experience-manager/kt/sites/using/style-organization-style-system-understand-article.html)
>* [Set up ContextHub with AEM Sites](https://helpx.adobe.com/experience-manager/kt/sites/using/context-hub-technical-video-setup.html)
>* [Getting Started with the AEM SPA Editor - WKND Events Tutorial](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop.html)
>* [Getting Started with React and AEM SPA Editor](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/react.html)
>* [do not publish](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/react/chapter-0-project-setup.html)
>* [Template DO NOT PUBLISH](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/react/template.html)
>* [Getting Started with React and AEM SPA Editor - Chapter 1](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/react/chapter-1.html)
>* [Getting Started with React and AEM SPA Editor - Chapter 2](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/react/chapter-2.html)
>* [Getting Started with React and AEM SPA Editor - Chapter 0](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/react/chapter-0.html)
>* [Getting Started with React and AEM SPA Editor - Chapter 3](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/react/chapter-3.html)
>* [Getting Started with Angular and AEM SPA Editor](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/angular.html)
>* [Getting Started with Angular and AEM SPA Editor - Chapter 0](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/angular/chapter-0.html)
>* [Getting Started with Angular and AEM SPA Editor - Chapter 1](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/angular/chapter-1.html)
>* [Getting Started with Angular and AEM SPA Editor - Chapter 2](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/angular/chapter-2.html)
>* [Getting Started with Angular and AEM SPA Editor - Chapter 3](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/angular/chapter-3.html)
>* [Getting Started with Angular and AEM SPA Editor - Chapter 4](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/angular/chapter-4.html)
>* [Getting Started with Angular and AEM SPA Editor - Chapter 5](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/angular/chapter-5.html)
>* [Getting Started with Angular and AEM SPA Editor - Chapter 6](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/angular/chapter-6.html)
>* [Getting Started with Angular and AEM SPA Editor - Chapter 7](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/angular/chapter-7.html)
>* [Getting Started with Angular and AEM SPA Editor - Chapter 8](https://helpx.adobe.com/experience-manager/kt/sites/using/getting-started-spa-wknd-tutorial-develop/angular/chapter-8.html)
>* [Index](https://helpx.adobe.com/experience-manager/kt/sites/index.html)
>* [AEM 6.3 Sites Tutorials and Videos](https://helpx.adobe.com/experience-manager/kt/sites/index/aem-6-3-sites.html)
>* [Simple for AEM](https://helpx.adobe.com/experience-manager/kt/sites/index/simple.html)
>* [AEM 6.4 Sites Tutorials and Videos](https://helpx.adobe.com/experience-manager/kt/sites/index/aem-6-4-sites.html)
>* [AEM 6.5 Sites Tutorials and Videos](https://helpx.adobe.com/experience-manager/kt/sites/index/aem-6-5-sites.html)
>* [Beta](https://helpx.adobe.com/experience-manager/kt/sites/beta.html)
>* [Architecture of the AEM Style System](https://helpx.adobe.com/experience-manager/kt/sites/beta/style-system-architecture-technical-video-understand.html)
>* [How to Customize Component Element Names in the AEM Style System](https://helpx.adobe.com/experience-manager/kt/sites/beta/style-system-overview-feature-video-understand.html)
>* [Overview of Style System Authoring Workflow](https://helpx.adobe.com/experience-manager/kt/sites/beta/style-system-workflow-feature-video-understand.html)
>* [Overview of the AEM Style System](https://helpx.adobe.com/experience-manager/kt/sites/beta/style-system-customize-element-names-feature-video-understand.html)
>* [Using Core Components with the AEM Style System](https://helpx.adobe.com/experience-manager/kt/sites/beta/style-system-component-requirements-technical-video-understand.html)
>* [Overview of Content Fragments and Content Services](https://helpx.adobe.com/experience-manager/kt/sites/beta/content-fragments-content-services-overview-feature-video-understand.html)
>* [Summary of Content Fragments and Content Services Enhancements](https://helpx.adobe.com/experience-manager/kt/sites/beta/content-fragments-content-services-enhancements-feature-video-understand.html)
>* [Authoring Experience Using Structured Content Fragments](https://helpx.adobe.com/experience-manager/kt/sites/beta/content-fragments-authoring-feature-video-use.html)
>* [Creating Content Fragment Models](https://helpx.adobe.com/experience-manager/kt/sites/beta/content-fragments-create-models-technical-video-use.html)
>* [Extracting Data Using Content Services](https://helpx.adobe.com/experience-manager/kt/sites/beta/content-fragments-content-services-output-technical-video-use.html)
>* [Understand Content Services Data Output](https://helpx.adobe.com/experience-manager/kt/sites/beta/content-fragments-output-scenarios-technical-video-understand.html)
>* [Content Fragment Enhancements](https://helpx.adobe.com/experience-manager/kt/sites/beta/content-fragment-enhancements-feature-video-understand.html)
>* [Experience Fragment Enhancements](https://helpx.adobe.com/experience-manager/kt/sites/beta/experience-fragments-enhancements-feature-video-understand.html)
>* [Translation Enhancements](https://helpx.adobe.com/experience-manager/kt/sites/beta/translation-enhancements-feature-video-understand.html)
>* [Templates (Do Not Publish)](https://helpx.adobe.com/experience-manager/kt/templates--do-not-publish-.html)
>* [Video Page](https://helpx.adobe.com/experience-manager/kt/templates--do-not-publish-/video-page.html)
>* [Diagram Page](https://helpx.adobe.com/experience-manager/kt/templates--do-not-publish-/diagram-page.html)
>* [Code Page](https://helpx.adobe.com/experience-manager/kt/templates--do-not-publish-/code-page.html)
>* [Aggregate Page](https://helpx.adobe.com/experience-manager/kt/templates--do-not-publish-/aggregrate-page.html)
>* [Helpx AEM Tech Marketing Standards](https://helpx.adobe.com/experience-manager/kt/templates--do-not-publish-/standards.html)
>* [POC CCL Tutorial Page for AEM Technical Marketing](https://helpx.adobe.com/experience-manager/kt/templates--do-not-publish-/poc-ccl-tutorial-page.html)
>* [Tutorial Page](https://helpx.adobe.com/experience-manager/kt/templates--do-not-publish-/tutorial-page.html)
>* [Internal](https://helpx.adobe.com/experience-manager/kt/livefyre/kb-internal.html)
>* [Using](https://helpx.adobe.com/experience-manager/kt/livefyre/using.html)
>* [KB](https://helpx.adobe.com/experience-manager/kt/livefyre/kb.html)
>* [Index](https://helpx.adobe.com/experience-manager/kt/livefyre/index.html)
>* [Livefyre](https://helpx.adobe.com/experience-manager/kt/livefyre.html)
>* [Internal](https://helpx.adobe.com/experience-manager/kt/integration/kb-internal.html)
>* [Using](https://helpx.adobe.com/experience-manager/kt/integration/using.html)
>* [Launch by Adobe Reference Architectures](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-guides.html)
>* [Integrate Launch by Adobe with AEM Sites](https://helpx.adobe.com/experience-manager/kt/integration/using/adobe-launch-integration-tutorial-understand.html)
>* [Understanding AEM Integration with DTM, Analytics and Target](https://helpx.adobe.com/experience-manager/kt/integration/using/aem-dtm-integration-tutorial-understand.html)
>* [Understanding AEM Integration with Launch By Adobe, Analytics and Target](https://helpx.adobe.com/experience-manager/kt/integration/using/aem-launch-integration-tutorial-understand.html)
>* [Using Launch by Adobe to Implement Analytics, Target, and Audience Manager in Single Page Applications (SPA)](https://helpx.adobe.com/experience-manager/kt/integration/using/launch-reference-architecture-SPA-tutorial-implement.html)
>* [Understanding Single Page Application Content Insights using Launch by Adobe](https://helpx.adobe.com/experience-manager/kt/integration/using/aem-analytics-spa-tutorial-understand.html)
>* [KB](https://helpx.adobe.com/experience-manager/kt/integration/kb.html)
>* [Index](https://helpx.adobe.com/experience-manager/kt/integration/index.html)
>* [Integration](https://helpx.adobe.com/experience-manager/kt/integration.html)
>* [Index](https://helpx.adobe.com/experience-manager/kt/index.html)
>* [AEM 6.3 Tutorials and Videos](https://helpx.adobe.com/experience-manager/kt/index/aem-6-3-videos.html)
>* [AEM 6.4 Tutorials and Videos](https://helpx.adobe.com/experience-manager/kt/index/aem-6-4-videos.html)
>* [A collection of tutorials focused on using, developing and integrating AEM with other Adobe products.](https://helpx.adobe.com/experience-manager/kt/index/aem-tutorials.html)
>* [AEM 6.5 Tutorials and Videos](https://helpx.adobe.com/experience-manager/kt/index/aem-6-5-videos.html)
>* [eSeminars](https://helpx.adobe.com/experience-manager/kt/eseminars.html)
>* [Adobe Experience Manager: Desktop App](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-desktop-app.html)
>* [Adobe Analytics: Data Feed Management Tips & Tricks](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-data-feeds-tips-and-tricks.html)
>* [Adobe Customer Care Office Hours](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-index.html)
>* [Adobe Customer Care Office Hours](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-index/ccoo-index-AEM.html)
>* [Adobe Customer Care Office Hours](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-index/ccoo-index-Camapign.html)
>* [Adobe Campaign: Success Mantra for Email Delivery in Adobe Campaign v6](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-email-delivery-campaignv6.html)
>* [Adobe Experience Manager: Desktop App Advanced Tips](https://helpx.adobe.com/experience-manager/kt/eseminars/cc00-aem-desktop-app-advanced.html)
>* [Adobe Campaign: Success Mantra for Email Delivery in Adobe Campaign v6](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-registernow.html)
>* [AEM GEMS : Sites and Experience Fragments](https://helpx.adobe.com/experience-manager/kt/eseminars/gem-template-page.html)
>* [Adobe Campaign: Success Mantra for Email Delivery in Adobe Campaign v6](https://helpx.adobe.com/experience-manager/kt/eseminars/coco-template-registration.html)
>* [Adobe Campaign: Message Center - Real Time Messaging](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-june-register.html)
>* [Adobe Experience Manager: Indexing - Best Practices and Troubleshooting](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-june-register.html)
>* [Adobe Analytics: Visitor ID Service Overview Registration](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-june-register.html)
>* [Adobe Analytics: Marketing Cloud ID Recording](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-marketing-cloud-id.html)
>* [Adobe Campaign: Messaging Center Recording](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-message-center-recording.html)
>* [Adobe Experience Manager: Indexing](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-indexing-recording.html)
>* [Adobe Analytics: Marketing Cloud Administration Overview Registration](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-july-register.html)
>* [Adobe Experience Manager: Key Features of AEM 6.3 and Upgrade Best Practices Registration](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-july-register.html)
>* [Adobe Campaign: Making the best of Campaign Solution Registration](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-july-register.html)
>* [Adobe Analytics: Marketing Cloud Administration Overview](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-july-recording.html)
>* [Adobe Experience Manager: Key Features of AEM 6.3 and Upgrade Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-july-recording.html)
>* [Adobe Campaign: Making the best of Campaign Solution](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-july-recording.html)
>* [GEMS](https://helpx.adobe.com/experience-manager/kt/eseminars/gems.html)
>* [AEM SPA Editor](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/jcr:content/aem-spa-editor.html)
>* [AEM Indexing and JCR Query](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-indexing-jcr-query.html)
>* [Troubleshooting AEM Replication](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-troubleshooting-aem-replication.html)
>* [AEM GEMS](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-index.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-index/jcr:content/hero/file.html)
>* [Granite GEMS](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-index.html)
>* [Building Health Checks for AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-building-health-checks-for-aem.html)
>* [Toughday2 - A new and improved stress testing and benchmarking tool](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-toughday2-stress-testing-benchmarking-tool.html)
>* [AEM Sustenance - Best Practices for deploying AEM Maintenance Releases](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-sustenance-best-practices-deploying-maintenance-releases.html)
>* [Search forms made easy with the AEM querybuilder](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-search-forms-using-querybuilder.html)
>* [Into the tar pit: a TarMK deep dive](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-tarmk-deepdive.html)
>* [Tools to use for testing Adobe Experience Manager applications](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-testing-tools-for-aem-apps.html)
>* [Introduction to AEM Screens](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-introduction-to-aem-screens.html)
>* [Configuring the DAM for Enterprise](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-configuring-dam-for-enterprise.html)
>* [Managing your content with the template editor of Adobe Experience Manager](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-managing-content-with-template-editor.html)
>* [Setup and Configure AEM Dynamic Media](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-setup-and-configure-aem-dynamic-media.html)
>* [Utilizing SAML in Adobe Experience Manager deployments](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-utilizing-saml-in-aem-deployments.html)
>* [AEM Web Performance](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-web-performance.html)
>* [Technical Sneak Peek](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-technical-sneak-peek.html)
>* [Running AEM on MongoDB](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-running-aem-on-mongodb.html)
>* [Oak Lucene Indexes](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-oak-lucene-indexes.html)
>* [Track quality metrics of your Javascript project](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-track-quality-metrics-of-your-javascript-project.html)
>* [Deep dive into AEM upgrade process](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-deep-dive-into-aem-upgrade-process.html)
>* [Customizing Dialog Fields in Touch UI](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-customizing-dialog-fields-in-touch-ui.html)
>* [AEM 6.1 Translation Integration & Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-6_1-translation-integration-and-best-practices.html)
>* [IBM WebSphere Commerce Integration for AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-ibm-websphere-commerce-integration-for-aem.html)
>* [Inside ACS AEM Commons & Tools](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-inside-acs-aem-commons-and-tools.html)
>* [Oak's External Login Module - Authenticating with LDAP and Beyond](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-oak-external-login-module-authenticating-with-ldap-and-beyond.html)
>* [Creating online Communities with AEM 6.1](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-creating-online-communities-with-aem-6_1.html)
>* [Tips and tricks for AEM Sites Touch UI](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-tips-and-tricks-for-aem-sites-touch-ui.html)
>* [Sonar - A key element to improve product quality](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-sonar-a-key-element-to-improve-product-quality.html)
>* [AEM Forms Feature Pack 1 introduction and technical samples](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-forms-feature-pack-1-introduction-and-technical-samples.html)
>* [Dispatcher Caching - New Features and Optimizations](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-dispatcher-caching-new-features-and-optimizations.html)
>* [AEM Tech Sneak Peek](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-tech-sneak-peek.html)
>* [Machine Translation in AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-machine-translation-in-aem.html)
>* [AEM 6 Oak: MongoMK and Queries](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-oak-mongomk-and-queries.html)
>* [How to deploy Adobe Analytics on a local AEM instance by using the Dynamic Tag Management cloud service](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-adobe-analytics-dynamic-tag-management.html)
>* [AEM Developer Tools for Eclipse](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-developer-tools-for-eclipse.html)
>* [Delivering Managed Content to your Native Apps](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-delivering-managed-content-to-your-native-apps.html)
>* [OAuth Server functionality in AEM - Embrace Federation and unleash your REST APIs!](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-oauth-server-functionality-in-aem.html)
>* [Social Component Framework in AEM 6](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-social-component-framework-in-aem-6.html)
>* [AEM 6.0 Developer Mode](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-developer-mode.html)
>* [Efficiently Build Reusable Components](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-efficiently-build-reusable-components.html)
>* [Introduction to HTL](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-introduction-to-htl.html)
>* [Technical Deep Dive into the AEM 6 Platform](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-technical-deep-dive-into-the-aem-6-platform.html)
>* [Technical Overview of the AEM 6 Platform](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-technical-overview-of-the-aem-6-platform.html)
>* [User Interface Customization for AEM 6](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-user-interface-customization-for-aem6.html)
>* [How to get the most out of your DAM Feature Pack](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-dam-feature-pack.html)
>* [AEM Dynamic Media 6.3 Architecture](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-dynamic-media-architecture.html)
>* [SharePoint Connector - Setup and Configuration](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-sharepoint-connector-setup-and-configuration.html)
>* [Metadata Management in AEM DAM](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-metadata-management-in-aem-dam.html)
>* [Streamlining multilingual content process](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-streamlining-multilingual-content-process.html)
>* [CQ/AEM 5.6 Troubleshooting](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-cq-aem-5-6-troubleshooting.html)
>* [Mobile-First Development with CQ Made Easy](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-mobile-first-development-with-cq-made-easy.html)
>* [MSM and Translation: Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-msm-and-translation-best-practices.html)
>* [Introduction of Job Handling and Offloading in AEM 5.6.1.](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-job-handling-and-offloading.html)
>* [Launches: concurrent preparation of multiple versions of a website (AEM 5.6)](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-launches.html)
>* [AEM 5.6 upgrade mechanisms](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-upgrade-mechanisms.html)
>* [hybris/AEM 5.6 eCommerce framework integration](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-hybris-ecommerce-framework-integration.html)
>* [Architecture of the AEM 5.6 Platform](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-architecture-of-the-aem-5-6-platform.html)
>* [AEM 5.6 Media Publisher Deep Dive](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-media-publisher-deep-dive.html)
>* [eCommerce Integration Framework](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-ecommerce-integration-framework.html)
>* [AEM Upgrade to major version - Backwards compatibility continued: Testing for BC compliance; Pattern detection](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-upgrade-major-version-pattern-detector-testing.html)
>* [Developing OSGi Bundles and Services for AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-developing-osgi-bundles-services-for-aem.html)
>* [AEM Upgrade to major version - Backwards compatibility](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-upgrade-to-major-version-backwards-compatibility.html)
>* [Assuming the worst - AEM Customer orientation and long-term CritSit reduction](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-assuming-the-worst-aem-customer-orientation-and-long-term-critsit-reduction.html)
>* [Evergreen Sprouts - Make it green from the start](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-evergreen-sprouts-make-it-green-from-the-start.html)
>* [Content Package Validation Improvements](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-content-package-validation-improvements.html)
>* [AEM Cumulative Fix Pack process](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-cumulative-fix-pack-process.html)
>* [HTTP API Framework](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-http-api-framework.html)
>* [Project evergreen and how to un-ring a bell - a required process change coming with AEM 6.3 M3 (load 9)](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-project-evergreen.html)
>* [AEM Forms and Mobile Apps](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-aem-forms-and-mobile-apps.html)
>* [AEM 2016/2017 program and operation](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-program-and-operation.html)
>* [Snapshotting and Restoring AEM using Docker and CRIU](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-snapshotting-and-restoring-aem.html)
>* [AEM Integrations - a solid foundation goes a long way](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-integrations.html)
>* [AEM 6.3 Ready for the World - Translation Integration & Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-translation-integration.html)
>* [Managing AEM DataStore](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-managing-aem-datastore.html)
>* [AEM Fluid Experiences for headless usecases](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-headless-usecases.html)
>* [AEM 6.3 Ready for the World - Translation Integration & Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-translation-best-practices.html)
>* [Major Brand Portal Release and new reference implementation for Asset Share](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-brand-portal.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-brand-portal/jcr:content/main-pars/download_section/download-1/file.sftmp)
>* [Dispatcher - New features and best practices](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-dispatcher.html)
>* [Troubleshooting Sling Content Distribution](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-troubleshooting-sling.html)
>* [The Digital Asset Explosion & AEM Assets](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-asset-explosion.html)
>* [Experiments in AEM Author Scalability](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-author-scalability.html)
>* [The Digital Asset Explosion & AEM Assets](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-digital-asset-explosion.html)
>* [Experiments in AEM Author Scalability](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-author-scalability1.html)
>* [Deep Dive into Adobe Experience Manager 6.4](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Deep-Dive-into-Adobe-Experience-Manager-6-4.html)
>* [Deep Dive into Adobe Experience Manager 6.4](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-deep-dive-6-4.html)
>* [The Digital Asset Explosion & AEM Assets](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Deep-Dive-into-Adobe-Experience-Manager-6-41.html)
>* [AEM Managed Services IMS Integration: Architecture and Operation](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-ams-ms-ims-integration-1.html)
>* [Deep Dive into Adobe Experience Manager 6.4](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-6_4_technical_sneak_peek.html)
>* [Mapping AEM - automatic analysis of AEM and its usage - a possible help in your daily decision-making](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-mapping-aem.html)
>* [Cognifide's Zen Garden: What? Why? When?](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-cognifide-zen-garden.html)
>* [New Release Model for AEM 2018](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-new-release-model.html)
>* [How to improve your patent submission](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-patent-submission.html)
>* [Machine Learning in AEM: Enhanced Smart Tags, Smart Layout and more](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Machine-Learning-in-AEM-Enhanced-Smart-Tags-Smart-Layout-and-more.html)
>* [Machine Learning in AEM: Enhanced Smart Tags, Smart Layout and more](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-machine-learning.html)
>* [AEM Managed Services IMS Integration - End to End Demo](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-ims-demo.html)
>* [Real-time and lightweight: build event-driven integrations with AEM using I/O Events](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/event-driven-integrations-with-aem-using-IO-Events.html)
>* [Principles of the new AEM Engineering Operations Framework](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-aem-engineering-framework.html)
>* [Configuring AEM deployments for Adobe Asset Link (fka Project Europa)](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-asset-link.html)
>* [Real-time and lightweight: build event-driven integrations with AEM using I/O Events](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-adobe-io.html)
>* [Solr as an Oak index for AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Solr-as-an-Oak-index-for-AEM.html)
>* [Adobe I/O Events - Analytics Triggers](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-analytics-triggers.html)
>* [Solr as an Oak index for AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Solr-as-an-Oak-index-for-AEM1.html)
>* [Machine Learning in AEM: Enhanced Smart Tags, Smart Layout and more](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Adobe-Cloud-Platform---The-Heart-of-Experience-Cloud.html)
>* [AEM Query and Index Troubleshooting](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/AEM-Query-and-Index-Troubleshooting2.html)
>* [Adobe Cloud Platform - The Heart of Experience Cloud](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-acp.html)
>* [AEM Query and Index troubleshooting](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/AEM-Query-and-Index-Troubleshooting.html)
>* [AEM Query and Index Troubleshooting](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/A-Blank-registration-template.html)
>* [Best Practices for Test Automation with Selenium](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-selenium.html)
>* [Maintaining Open Source While Maintaining Your Sanity](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Maintaining-Open-Source-While-Maintaining-Your-Sanity.html)
>* [Metrics reporter in AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/granite-metrics-reporter.html)
>* [Maintaining Open Source While Maintaining Your Sanity](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-maintaining-open-source.html)
>* [Introduction to ContextHub in AEM 6.4](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-intro-to-contexthub.html)
>* [Using OSGi R7 in AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Using-OSGi-R7-in-AEM.html)
>* [AEM SPA Editor](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/aem-spa-editor.html)
>* [AEM SPA Editor](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/AEM-SPA-Editor.html)
>* [SPA Editor SDK Deep Dive - Part 1 - React](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/SPA-Editor-SDK-Deep-Dive-React.html)
>* [SPA Editor SDK Deep Dive - Part 2 - Angular](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/SPA-Editor-SDK-Deep-Dive-Angular.html)
>* [AEM Core Components](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/AEM-Core-Components.html)
>* [AskTheExpert](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert.html)
>* [AEM Ask the AEM Community Expert](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/atace-index.html)
>* [AEM Dispatcher](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-develop-with-dispatcher-in-mind.html)
>* [AEM Assets and Dynamic Media](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-assets-and-dynamic-media.html)
>* [Using Lazybones and Editable template in AEM projects](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-using-lazybones-and-editable-template-in-aem-projects.html)
>* [Building responsive layouts using Bootstrap and Angular JS in Experience Manager](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-building-responsive-layouts-using-bootstrap-and-angular-js-in-aem.html)
>* [Getting the most out of digital interactions with AEM and Analytics](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-getting-the-most-out-of-digital-interactions-with-aem-and-analytics.html)
>* [Best Practices for using Experience Manager and Adobe Campaign](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-best-practices-for-using-experience-manager-and-adobe-campaign.html)
>* [Integrating Test and Target with Adobe Experience Manager for Personalization use cases](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-integrating-test-and-target-with-aem-for-personalization-use-cases.html)
>* [Comparative Architecture Analysis of large scale Experience Manager Installations](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-comparative-architecture-analysis-of-large-scale-experience-manager-installations.html)
>* [Best Practices for Experience Manager and AEM Assets](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-best-practices-for-experience-manager-and-aem-assets.html)
>* [Working with Experience Manager and eCommerce](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-working-with-experience-manager-and-ecommerce.html)
>* [Preparing for Adobe Experience Manager Developer Exam](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-preparing-for-adobe-experience-manager-developer-exam.html)
>* [AEM Forms dive into Document Services](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-forms-dive-into-document-services.html)
>* [Deep Dive into AEM Communities](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-deep-dive-into-aem-communities.html)
>* [Working with AEM Forms](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-working-with-aem-forms.html)
>* [Deep Dive into developing AEM components using HTL](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-deep-dive-into-developing-aem-components-using-htl.html)
>* [Developing AEM Sling Components using Brackets](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-developing-aem-sling-components-using-brackets.html)
>* [Deep Dive into AEM and translations](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-deep-dive-into-aem-and-translations.html)
>* [AEM Apps Deep Dive](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-apps-deep-dive.html)
>* [AEM & integration with other Digital Marketing products](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-integration-with-other-digital-marketing-products.html)
>* [Personalization & segmentation with AEM & Adobe Campaign](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-personalization-and-segmentation-with-aem-and-adobe-campaign.html)
>* [Adobe Experience Manager Technical Documentation](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-technical-documentation.html)
>* [Adobe Experience Manager & Sling](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-adobe-experience-manager-and-sling.html)
>* [AEM & Dispatcher](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-dispatcher.html)
>* [Advanced AEM component development](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-advanced-aem-component-development.html)
>* [Touch UI components](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-touch-ui-components.html)
>* [AEM Workflows](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-workflows.html)
>* [Building login-based sites in AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-building-login-based-sites-in-aem.html)
>* [Replication](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-replication.html)
>* [Getting Started with AEM Apps](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-getting-started-with-aem-apps.html)
>* [Testing AEM Applications with ease](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-testing-aem-applications-with-ease.html)
>* [Developing custom services to customize AEM](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-developing-custom-services-to-customize-aem.html)
>* [Working with Experience Manager Core Components](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-working-with-experience-manager-core-components1.html)
>* [Creating custom components using HTL](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-custom-components-HTL.html)
>* [Working with Experience Manager Core Components](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-contexthub.html)
>* [AEM Component Development Strategies](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-component-development-strategies.html)
>* [Understanding AEM Communities](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-understanding-communities.html)
>* [AEM Sites - Multi-Site Management](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-multi-site-management.html)
>* [AEM Component Development Strategies](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-multi-site-management1.html)
>* [Working with AEM Workflows and Workflow API's](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-workflows-apis.html)
>* [Working with AEM Workflows and Workflow API's](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-workflows1.html)
>* [AEM Content Services: What, Why, and How?](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
>* [Easy Access to Critical Information: Content Reports in Adobe Experience Manager](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-reports.html)
>* [Adobe Experience Manager and Audience Manager](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-audience-manager1.html)
>* [Adobe Experience Manager and Creative Cloud](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-creative-cloud.html)
>* [Developing AEM component using Vue.js](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-components-vue.html)
>* [Explore AEM Assets and Tags by their APIs](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-assets-tags.html)
>* [Adobe Experience Manager and Adobe Sensei](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-sensei.html)
>* [Developing AEM component using Vue.js](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-vue.html)
>* [Enterprise Search Solution for AEM using Apache Solr](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-apache-solr.html)
>* [Creating a site structure to support your global business](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-site-structure.html)
>* [Working with Single Page applications for Experience Manager](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-single-page-application.html)
>* [Enterprise Search Solution for AEM using Apache Solr](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-single-page-application1.html)
>* [Adobe Analytics: First Party Cookies & Using Adobe Managed Certificates](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-Aug-register.html)
>* [Adobe Campaign: Tips and Tricks to a healthy Campaign Server and Delivery Monitoring](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-Aug-register.html)
>* [Adobe Experience Manager: AEM 6.x Maintenance Tasks](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-Aug-register.html)
>* [Adobe Analytics: Marketing Channels - Common Pitfalls](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-sept-register.html)
>* [Adobe Experience Manager: Integrating SAML & LDAP with AEM 6.x](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-sept-register.html)
>* [Adobe Analytics: First Party Cookies & Using Adobe Managed Certificates](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-aug-recording.html)
>* [Adobe Campaign: Tips and Tricks to a healthy Campaign Server and Delivery Monitoring](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-Aug-recording.html)
>* [Adobe Experience Manager: AEM 6.x Maintenance Tasks](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-Aug-recording.html)
>* [Adobe Experience Manager: Integrating SAML and LDAP with AEM 6.x](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-AEM-Sept-recording.html)
>* [Adobe Analytics: Marketing Channels - Common Pitfalls](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-sept-recording.html)
>* [Adobe Analytics: Classifications - Get the most out of your Analytics Data](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-oct-register.html)
>* [Adobe Experience Manager: AEM Assets - Implementation Guidelines & Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-oct-register.html)
>* [Adobe Campaign: Delivery Architecture and Performance](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-oct-register.html)
>* [Adobe Campaign: Getting the most from your Campaign solution](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign1-sept-recording.html)
>* [Adobe Analytics: Classifications - Get the most out of your Analytics Data](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-oct-recording.html)
>* [Adobe Experience Manager: AEM Assets - Implementation Guidelines & Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-oct-recording.html)
>* [Adobe Campaign: Delivery Architecture and Performance](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-oct-recording.html)
>* [Adobe Analytics: Data Availability & Latency](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-Nov-register.html)
>* [Adobe Campaign Standard: Technical Workflows, Branding , Notification, Monitoring and Reports](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-Nov-register.html)
>* [Adobe Experience Manager: AEM 6.x Performance Tuning and Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-Nov-register.html)
>* [Adobe Target: Visual Experience Composer](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-target-Nov-register.html)
>* [Adobe Primetime: TVE Dashboard User Interface](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-primetime-Nov-register.html)
>* [Adobe Analytics: Data Processing & Latency](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-nov-recording.html)
>* [Adobe Campaign Standard: Technical Workflows, Branding , Notification, Monitoring and Reports](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-Nov-recording.html)
>* [Adobe Experience Manager: AEM 6.x Performance Tuning and Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-nov-recording.html)
>* [Adobe Primetime: TVE Dashboard User Interface](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-primetime-nov-recording.html)
>* [Adobe Target: Visual Experience Composer](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-target-nov-recording.html)
>* [Adobe Experience Manager: Adaptive Forms - Authoring and Processing](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-jan-register.html)
>* [Adobe Analytics: Using Raw Data](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-jan-register.html)
>* [Adobe Target: Analytics/Target-A4T](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-target-jan18-register.html)
>* [Adobe Campaign: Mobile delivery channels in Campaign](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-jan18-register.html)
>* [Experience Insider](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider.html)
>* [Experience Insider](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/experience-insider-index.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/experience-insider-index/jcr:content/hero/file.html)
>* [Experience Insider News](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/experience-insider-news.html)
>* [What’s New in AEM6.4](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/whats-new-aem-6_4.html)
>* [Experience Insider](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/experience-insider-oss-index.html)
>* [N/A](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/experience-insider-oss-index/jcr:content/hero/file.html)
>* [Need for Speed - Accelerate time to value with new Cloud Manager](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/aem-cloud-manager.html)
>* [AEM Assets Part 1: Dynamic Media Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-dynamic-media.html)
>* [AEM Sites: Best Practices for Organizational Readiness](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-aem-sites.html)
>* [Mobile Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-mobile-best-practices.html)
>* [AEM Assets Part 2: DAM Implementation Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-dam-best-practices.html)
>* [AEM Sites 6.3 What’s New and Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-aem-sites-6-3.html)
>* [AEM Assets 6.3 What’s New and Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-aem-assets-6-3.html)
>* [AEM Assets 6.3 Dynamic Media: What’s New and Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-aem-dynamic-media-6-3.html)
>* [AEM Forms 6.3: What’s New and Best Practices](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-aem-forms-6-3.html)
>* [AEM 6.3 What’s New for Integrations](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-aem-integrations-6-3.html)
>* [Need for Speed - Accelerate time to value with new Cloud Manager](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-cloud-manager.html)
>* [Insider tips to empower your omnichannel business](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-sites-64.html)
>* [Unlocking More Powerful Asset Analytics With AEM 6.4](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-asset-analytics.html)
>* [Unlocking More Powerful Asset Analytics With AEM 6.4](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-asset-analytics-64.html)
>* [GDPR - An opportunity to provide outstanding customer experiences](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-gdpr-customer-experience.html)
>* [Optimizing Creative Operations with AEM Assets](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-creative-ops-assets1.html)
>* [Harness the Power of Touch UI To Deliver Amazing Experiences at Scale](https://helpx.adobe.com/experience-manager/kt/eseminars/experience-insider/exp-touch-ui.html)
>* [Adobe Campaign: Mobile Delivery Channels](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-campaign-Jan-recording.html)
>* [Adobe Target: Visual Experience Composer](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-target-Jan-recording.html)
>* [Adobe Experience Manager: Adaptive Forms - Authoring and Processing](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-aem-Jan-recording.html)
>* [Adobe Analytics: Using RAW data](https://helpx.adobe.com/experience-manager/kt/eseminars/ccoo-analytics-Jan-recording.html)
>* [Marketing](https://helpx.adobe.com/experience-manager/kt/marketing.html)
>* [dm-test](https://helpx.adobe.com/experience-manager/kt/marketing/dm-test.html)
>* [AEM Assets 6.3 Videos](https://helpx.adobe.com/experience-manager/kt/marketing/sampleindex.html)
>* [Sample Article](https://helpx.adobe.com/experience-manager/kt/marketing/samplearticle.html)
>* [Playground](https://helpx.adobe.com/experience-manager/kt/playground.html)
>* [test](https://helpx.adobe.com/experience-manager/kt/playground/test.html)
>* [test](https://helpx.adobe.com/experience-manager/kt/playground/foo.html)
>* [footwo](https://helpx.adobe.com/experience-manager/kt/playground/footwo.html)
>* [foothree](https://helpx.adobe.com/experience-manager/kt/playground/foothree.html)
>* [fourfive](https://helpx.adobe.com/experience-manager/kt/playground/fourfive.html)
>* [AEM 6.3 Feature Videos](https://helpx.adobe.com/experience-manager/kt/playground/max-foo-videos.html)
>* [Experience Business on Adobe Cloud Platform](https://helpx.adobe.com/experience-manager/kt/playground/experience-business-acp-presentation-learn.html)
>* [WWSC 2019 Pre-conference Enablement](https://helpx.adobe.com/experience-manager/kt/playground/wwsc-preconference-enablement.html)
>* [SC Content Three](https://helpx.adobe.com/experience-manager/kt/playground/wwsc-sc-3.html)
>* [SC Content Two](https://helpx.adobe.com/experience-manager/kt/playground/wwsc-sc-2.html)
>* [SC Content One](https://helpx.adobe.com/experience-manager/kt/playground/wwsc-sc-1.html)
>* [CSM Content One](https://helpx.adobe.com/experience-manager/kt/playground/wwsc-csm-1.html)
>* [CSM Content One](https://helpx.adobe.com/experience-manager/kt/playground/wwsc-csm-2.html)
>* [CSM Content Three](https://helpx.adobe.com/experience-manager/kt/playground/wwsc-csm-3.html)
>* [Description text SEO title](https://helpx.adobe.com/experience-manager/kt/playground/foo-feature-video-use.html)
>* [Description text SEO title](https://helpx.adobe.com/experience-manager/kt/playground/foo-feature2-video-use.html)
>* [Screens](https://helpx.adobe.com/experience-manager/kt/screens.html)
>* [Internal](https://helpx.adobe.com/experience-manager/kt/screens/internal.html)
>* [KB](https://helpx.adobe.com/experience-manager/kt/screens/kb.html)
>* [Using](https://helpx.adobe.com/experience-manager/kt/screens/using.html)
>* [REDIRECT Understanding AEM Screens](https://helpx.adobe.com/experience-manager/kt/screens/using/screens-concepts-feature-video-understand.html)
>* [REDIRECT Extending an AEM Screens Component](https://helpx.adobe.com/experience-manager/kt/screens/using/extending-component-tutorial-develop.html)
>* [REDIRECT Developing a Custom Component for AEM Screens](https://helpx.adobe.com/experience-manager/kt/screens/using/developing-custom-component-tutorial-develop.html)
>* [Index](https://helpx.adobe.com/experience-manager/kt/screens/index.html)
>* [AEM 6.4 Screens Videos](https://helpx.adobe.com/experience-manager/kt/screens/index/aem-6-4-screens.html)