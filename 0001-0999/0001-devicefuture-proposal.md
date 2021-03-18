<!--
Please delete this block comment before submitting your issue.

Ensure that you replace fields with square brackets (such as `[Spec Name]`) with relevant information. You do not need to specify a specification ID since it is determined when it is accepted.

Numbers in square brackets which are proceeded with a `G` (such as `[G1]`) refer to further guidance which is shown below.

* [G1]: May not be applicable. Please write `(N/A)` if this is the case.
* [G2]: An area can either be `Meta` for discussion about the specifications in general, `Community` for information about community management/events, or `Other` if this spec does not fit into any specific area.
* [G3]: Please link to issues as full URLs. For GitHub issue #123, write:
  ```
  [GH#123](https://github.com/issues/123)
  ```
* [G4]: A reviewer will set this field for you. You can leave it as-is for now.

Dates must be written in the form YYYY-MM-DD, where YYYY is the 4-digit year, MM is the 2-digit month, and DD is the 2-digit day of the month. When the month or day of the month is only 1 digit, then it must be preceeded with a `0`.

The delimiter for multiple items within spec property values is a semicolon followed by a space (`; `).
-->

# 0001: DeviceFuture Proposal
| Property | Value |
|----------|-------|
| Status | Awaiting review |
| Product(s) (if any) or area | Meta |
| Author(s) | james@devicefuture.org |
| Reviewer(s) (if any) | (N/A) |
| Submission date | 2021-03-17 |
| Acceptance date | (N/A) |
| Addresses issue(s) (if any) | (N/A) |
| Review discussion | [GH#1](https://github.com/DeviceFuture/spec/pull/1) |

## Synopsis
This document serves as the founding proposal for DeviceFuture. The products described herein may be expanded or futher adapted in future spec documents. However, this spec simply provides an overview of the DeviceFuture project.

## Details
DeviceFuture is a community who plans to build open-source software and hardware to address the relevant issues of technology with regards to environmental sustainability. We hope to specifically address many issues with web technologies at this time, as will be described herein. Below is a list of products that we will build under the DeviceFuture project.

### ThunderNet
Our main project will be ThunderNet, a decentralised network of independently-operated **nodes** which can be connected to from consumer devices (known as **endpoints**) used by **visitors**. A **node** is defined as any internet-connected device which is capable of acting as a WAN-wide web server. A **visitor** is defined as an indiviudal who connects to nodes via their own **endpoint** device.

The responsibility of a node is to act as a single point in the content delivery network that makes up ThunderNet as a whole. Each node must respond to endpoints by serving content such as websites for them. The node should cache frequently-requested content to reduce the number of requests made to other servers on the internet.

When an endpoint requests a specific internet resource from a node, that node must retrieve that resource from either the internet or their own internal cache, compress the resource for decompression on the endpoint, and encrypt the resource such that it can only be decrypted by the endpoint once received. A node must additionally strip any unnecessary information from retrieved resources before it is relayed onto the endpoint. We will explore how this stripping of information works next.

One of the most effective measures that we can take to strip the contents from resources is through HTML minification, which involves removing any whitespace which does not affect the structure of a HTML page. We also simplify the page by restructuring it so it only contains information that is critical to interpreting the page as a visitor. This generally means excluding non-critical code such as tracking scripts, as well as removing popups (such as privacy consent or newsletter sign-up forms) and advertisements.

The rationale behind stripping the contents of resources is because a large proportion of content in a resource is never interacted with by a visitor, and therefore serves no use within that resource. By determining what parts of a resource is frequently unused, we can reduce the amount of data sent from a node to an endpoint, and therefore reduce the electricity consumption used transmitting that data.

Compressing data before sending it to endpoints is also a powerful way to reduce the amount of data which is transmitted. We plan to use the LZMA encryption/decryption algorithm[F1] to reduce the size of resources by a ratio of around 2:1. The use of compression is also helpful when caching resources to reduce the amount of storage space in use.

One of the fundamental missions of DeviceFuture is to ensure that everybody has access to an internet which is free of large-scale censoring that promotes the interests of one party only. Our combined use of a decentralised, multi-node network and symmetrical encryption can help to guarantee the privacy of a user who would otherwise be using potentially a restricted and monitored network. We plan to use AES encryption since it is built into all modern browsers and therefore does not need external library support (which in turn, reduces data transmission when installing our software on endpoint devices).

We plan to write our node software such that it is lightweight and capable of running on inexpensive hardware such as the Raspberry Pi Zero. We hope to make energy efficiency a priority when writing our software so that it can be run on devices powered by clean, renewable energy such as solar/photovoltaic cells.

### ThunderNet Browser
To access information on ThunderNet, we will build a web browser called ThunderNet Browser, which will be available to install on endpoint devices. The browser will be a Progressive Web App (PWA), which makes it portable (available across multiple devices, such as desktop computers and mobile devices) as well as convenient to install. To maintain our aforementioned goals, the browser will be available in an extremely compact form, which saves space on the endpoint device and also ensures a fast installation on devices connected to a low-bandwidth network.

The browser will function in the same as other internet browsers, in that a visitor enters a URL to a resource into the browser's address bar, and then the ThunderNet Browser will resolve that URL to a resource served through ThunderNet. Similarly, the ThunderNet Browser will cache recently-visited resources locally so that any future visits to a specific page can save on the number of requests made to nodes. A simple checksum system will be implemented so that cached resources do not go stale when the original resource is updated.

The ThunderNet Browser will also decrypt and decompress resources so that the contents of those resources can be presented to the visitor.

### ThunderNet QR
ThunderNet Browser will include a feature to share resources with other endpoints through a completely offline method called ThunderNet QR. Since QR codes have become more and more versatile over the years, it is possible to store kilobytes of data within a single QR matrix. If one visitor wants another visitor to see an article, the first visitor can use the ThunderNet QR feature within ThunderNet Browser to generate a QR code which summarises the article. Then, the second visitor can scan the generated QR code to receive a copy of the resource.

Resources may be split up into a series of QR codes if they are especially large. These QR codes can then be scanned one-at-a-time to receive the resource in parts.

We plan to use a JavaScript port[F2] of ZXing to scan QR codes and QRCode.js[F3] to generate QR codes. Both actions will be done from within ThunderNet Browser on endpoints.

### ThunderSearch
Since ThunderNet nodes perform parsing on various internet resources, we can use this data to create a search index for easy access to resources on the ThunderNet network. We will create a search engine called ThunderSearch which is accessible from the ThunderNet Browser. This engine will allow for simple keyword search, as well as more advanced search options to refine results by factors such as first retrieval date and size.

To ensure that ThunderSearch is efficient, rendering of the search engine will happen within ThunderNet Browser within the endpoint. This means that a ThunderNet node only needs to send search results through a simple API. We plan to use NeDB[F4], a lightweight and fast database system, to store ThunderNet resources in addition to ThunderSearch results.

## Footnotes
[F1] https://github.com/LZMA-JS/LZMA-JS

[F2] https://github.com/nimiq/qr-scanner

[F3] https://github.com/davidshimjs/qrcodejs

[F4] https://github.com/louischatriot/nedb
