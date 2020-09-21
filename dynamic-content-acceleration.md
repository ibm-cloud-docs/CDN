---

copyright:
  years: 2019
lastupdated: "2020-04-08"

keywords: DCA, dynamic, detection path, prefetching, image compression, ttl, cache

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# Configuring Dynamic Content Acceleration
{: #dynamic-content-acceleration}
{: help}
{: support}

Dynamic Content Acceleration (DCA) is a technology to accelerate the dynamic web content.
It provides improved reliability, offload, and network performance over your original web infrastructure, while handling the specific requirements of dynamically generated content — without a costly hardware build-out. Using real-time network optimizations and advanced caching techniques, it speeds and secures interactive websites.
{:shortdesc}

## Working with DCA configurations
{: #dca-configurations}

To enable DCA, follow these steps:

1. Open the page of a specified CDN mapping, click **Settings** from the navigation pane. Then, select **Dynamic content acceleration** from the **Optimize for** list menu.

   ![Configure origin](images/settings-dca-switch.png)

2. Enable DCA:

   - Download the route detection 'test object', or use your own test file, and upload it to your origin server. For more information about the criteria of the test object, see [Detection Path](#detection-path).
   - Type the path of the object in the **Detection path** field. Click **Test** to verify that the object can be reached.
   - Enable/Disable [Prefetching](#prefetching) and [Image compression](#image-compression).

   ![Configure origin](images/settings-dca-input.png)

3. Click the **Save** button to save the settings. If the **`Detection path`** is unreachable, you are prompted by a warning. You can continue to save your settings and update the **`Detection path`** later.

   ![Configure origin](images/settings-dca-save.png)

## Understanding DCA concepts
{: #dca-concepts}

### Detection Path
{: #detection-path}

**`Detection Path`** is used by Akamai Edge servers to find the best optimized route from Edge servers to origin.
The best path to origin must be known at the time a user’s request arrives at an Edge server because any inline analysis or detection would defeat the purpose of speeding up things.

To accomplish this, you are asked to place a test object on your origin. Edge servers periodically fetch the test object from the origin using each of the candidate paths, including the direct path (the default path through the internet from Edge to Origin).

The fetches of the test object are called the races. When a real request comes in, the Edge consults the most recent race data to send that request over the fastest path to the origin.
You can download the provided [test object](https://public.dhe.ibm.com/cloud/bluemix/network/cdn/route-detection-test-object.html){:external} and upload it to the origin server, or you can use your own test object.

A valid test object must:
* Get HTTP 200 response without authentication
* Not contact a database or do any back-end processing
* Be a static text file of content-type text/HTML
* Be approximately the size of an average page, but no less than 8 KB


**`Detection Path`** is designed to work only for HTTPS domain mapping (SAN HTTPS or Wildcard HTTPS).
{:note}

### Prefetching
{: #prefetching}

**`Prefetching`** is to inspect HTML responses and prefetch embedded objects in the HTML files. Prefetch works on any page that includes **`\<img\>`**, **`\<script\>`**, or **`\<link\>`** tags that specify relative paths. It also works when the resource hostname matches the request domain in the HTML file, and it is part of a fully qualified URI. When it is set to **On**, Edge servers prefetch objects with the following file extensions:  
aif, aiff, au, avi, bin, bmp, cab, carb, cct, cdf, class, css, doc, dcr, dtd, exe, flv, gcf, gff, gif, grv, hdml, hqx, ico, ini, jpeg, jpg, js, mov, mp3, nc, pct, pdf, png, ppc, pws, swa, swf, txt, vbs, w32, wav, wbmp, wml, wmlc, wmls, wmlsc, xsd, and zip.

### Image compression
{: #image-compression}

Serving compressed images reduces the amount of content that is required to load a page. This helps offset less robust connections, such as those formed with mobile devices. If your site visitors have slow network speeds, Image Compressing technology can automatically increase compression of JPEG images to speed up loading. However, Image Compressing results in lossy compression or irreversible compression, and might affect the quality of the images on your site.  

Image Compression supported file extensions: .jpg, .jpeg, .jpe, .jif, .jfif, and .jfi

In order for the feature **`Image Compression`** to work for DCA, you must make sure that the path of the image files is cacheable. See [Caching](#caching-cache-content) to set the images cacheable.
{:note}

## Caching
{: #caching}

When DCA is enabled for your CDN mapping or origin, the Akamai Edge servers cache (or prevent caching of) contents based on your settings:

* To enable caching of contents under the path, set `Respect header` to `ON` and set the caching headers in your origin (for example, `Cache-Control:public, max-age=31536000`).

* To prevent caching of contents, do one of the following:

   - Set `Respect header` to `ON` (default) and include headers in your origin, which prevents caching (for example, `Cache-Control: no-cache, no-store`).
   - Set `Respect header` to `OFF`, create a TTL with a path to match your contents, and set a `0` value.

In some cases, you might want to mix static (for example, images, CSS, JS) and dynamic assets under the same path, so you might need to make some assets cacheable. You can do this with the following options:

- Set `Respect header` to `ON` and set the caching headers in your origin (for example, `Cache-Control:public, max-age=31536000`).
- Set `Respect header` to `OFF`, create a TTL with path to match your static contents, and set the value as the wanted caching time.
