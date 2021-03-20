# 0000: ThunderNet HTML Semantics
| Property | Value |
|----------|-------|
| Status | Awaiting review |
| Product(s) (if any) or area | ThunderNet; ThunderNet Browser |
| Author(s) | james@devicefuture.org |
| Reviewer(s) (if any) | (N/A) |
| Submission date | 2021-03-19 |
| Acceptance date | [YYYY-MM-DD[G1]] |
| Addresses issue(s) (if any) | (N/A) |
| Review discussion | [Link to review discussion[G4]] |

## Synopsis
This document standardises the formatting of HTML content sent through the ThunderNet network to ensure efficient data transmission.

## Details
The need for fast and efficient transmission is paramount on the ThunderNet network. We propse a lightweight subset of the HTML standards[F1] which is designed to only convey critical information about a web page. This includes the omission of purely aesthetic content such as CSS and JavaScript. Much of the HTML language will be accepted as valid and renderable content on the ThunderNet Browser. There are, however, some minor changes which ensures that any unnecessary syntax is not sent.

The most noticeable difference between standard HTML and HTML served through the ThunderNet network is that the `<html>`, `<head>` and `<body>` tags will not be used. Instead, all content will reside as either root nodes or descendents of each other. The `<!DOCTYPE html>` declaration will also be excluded to further save on the amount of data transferred.

### Article layout semantics
All main article content must reside in a `<main>` element. This includes article headers, as well as paragraphs. Additional information about an article (such as further reading lists) should not reside in the `<main>` element. Such information should be stored in `<nav>` or `<aside>` elements instead.

Main site links should be part of a single `<nav>` element. A single image is allowed in `<nav>` to represent a website's branding logo. Links to other pages on the site can be supplied with `<a>` elements. Likewise, further reading links and other information can reside in `<aside>` elements. Presentation-wise, `<nav>` elements should show in their entirety on a desktop computer (with overflow links being relegated to a dropdown menu), with only the site's logo or site title showing on mobile (a hamburger menu is to be used for site links).

`<aside>` elements can either be containers for general information, or can hold a list of links to other pages. For the latter, the `<aside>` element must have the `list` attribute present. This should format each link on a line of its own.

Refer to **Figure 1** and **Figure 2** for examples of usage for both `<nav>` and `<aside>` elements.

**Figure 1:** Usage of `<nav>` element
```html
<nav>
    <a href="/"><img src="https://example.com/logo.png" alt="Example Website"></a>
    <a href="/">Home</a>
    <a href="/international">International News</a>
    <a href="/latest">Latest Updates</a>
</nav>
```

**Figure 2:** Usage of `<aside>` elements
```html
<aside>
    <h2>Consider donating to us</h2>
    <p>We create high-quality content from over 20 regional locations, for free. Donate to us to show your support and keep us running.</p>
    <a button href="/donate">Donate now</a>
</aside>
<aside list>
    <h2>Further reading</h2>
    <a href="/articles/read-todays-editorial-picks">Read Today's Editorial Picks</a>
    <a href="/articles/uk-budget-plan">The UK's Budget Plan, Explained</a>
    <a href="/articles/your-questions-answered">Your Questions, Answered</a>
</aside>
```

Footers can be contained in a `<footer>` element at the bottom of the page. `<a>` elements contained in the footer that do not reside in `<p>` elements will be placed on a line for each link, and each link may be resized to fit multiple on a row (for a desktop layout).

Links can be customised to make them appear as buttons that reside in `<aside>` elements. To do this, the `<a>` element for a link must have the `button` attribute present.

### Listing multiple articles
A prominent feature of many post-based sites is the use of a home page to allow visitors to explore the articles featured on the website. To display a list of articles to the visitor, `<a>` elements with the attribute `article` present will be used within the `<main>` element. Each `<a article>` element can contain a header, image and short paragraph. See **Figure 3** for an example of multiple `<a article>` tags.

**Figure 3:** Usage of `<a article>` tag
```html
<main>
    <h1>Today's News</h1>
    <a article href="/articles/read-todays-editorial-picks">
        <h2>Read Today's Editorial Picks</h2>
        <img src="/media/read-todays-editorial-picks.png" alt="Graphic of multiple newspaper articles">
        <p>Join our Editor-in-Chief for a detailed analysis of today's news.</p>
    </a>
    <a article href="/articles/uk-budget-plan">
        <h2>The UK's Budget Plan, Explained</h2>
        <img src="/media/uk-budget-plan.png" alt="Graphic of money on a table">
        <p>We've unpicked the complete plan for the Uk Government's budgeting.</p>
    </a>
    <h1>Have Your Say</h1>
    <a article href="/articles/your-questions-answered">
        <h2>Your Questions, Answered</h2>
        <img src="/media/read-todays-editorial-picks.png" alt="Graphic of people chatting">
        <p>Let us know about the thoughts you've had today.</p>
    </a>
</main>
```

### Styling options
Since opportunities for styling are limited due to the absence of CSS, `<meta>` tags will be used instead:
* To choose a main accent colour for a site (which affects the `<nav>` element, and may also affect link/button colours if `tn:accent2` is absent), use meta name `tn:accent1` and set content to be a valid CSS colour. A matching foreground colour (either black or white) will be picked to ensure a high contrast ratio.
* To choose a secondary accent colour for a site (which affects the colour of `<a>` and `<a button>` elements), use meta name `tn:accent2` and set content to be a valid CSS colour.
* To choose a background colour for the `<aside>` and `<footer>` elements, use meta name `tn:accent3` and set content to be a valid CSS colour. A matching foreground colour (either black or white) will be picked to ensure a high contrast ratio.
* To choose a corner radius value for container elements (such as `<aside>` elements, in addition to `<a button>` elements), use meta name `tn:radius` and set content to be an integer that matches a pixel value.
* To make `<a button>` elements have fully rounded corners, use meta name `tn:rounded` and set content to be `true`.
* To set the font family of the site, use meta name `tn:font` and set content to be a valid CSS font stack.
* To set the font family of headers, use meta name `tn:headerfont` and set content to be a valid CSS font stack.

See **Figure 4** for a complete example of the aforementioned styling options.

**Figure 4:** Usage of `<meta>` tags
```html
<meta name="tn:accent1" content="#2cee90">
<meta name="tn:accent2" content="#5863f8">
<meta name="tn:accent3" content="#ddf0e9">
<meta name="tn:radius" content="5">
<meta name="tn:rounded" content="true">
<meta name="tn:font" content="sans-serif">
<meta name="tn:headerfont" content="serif">
```

### Restricted elements
Certain elements are restricted or not allowed so that only the core functionality of HTML is used. Below is a list of elements which are restricted. Elements that are not supported in HTML5 are unavailable[F2].

| Tag          | Status      | Restriction                                                                                    |
|--------------|-------------|------------------------------------------------------------------------------------------------|
| `<audio>`    | Restricted  | Content not loaded on ThunderNet, but instead through the internet. Cannot be autoplayed.      |
| `<button>`   | Unavailable | JavaScript is not supported. Use `<a button>` instead since it is better for accessibility.    |
| `<canvas>`   | Unavailable | JavaScript is not supported.                                                                   |
| `<dialog>`   | Unavailable | Popups and dialogs are not allowed for anti-distraction purposes.                              |
| `<embed>`    | Unavailable | Embedded content is not supported.                                                             |
| `<form>`     | Unavailable | JavaScript is not supported. Web forms are not supported at this time.                         |
| `<iframe>`   | Unavailable | Iframes are not allowed for anti-tracking purposes.                                            |
| `<img>`      | Restricted  | Images will not load by default unless disabled. User must explicitly choose to view an image. |
| `<input>`    | Unavailable | JavaScript is not supported. Web forms are not supported at this time.                         |
| `<link>`     | Restricted  | Only `rel="icon"` is supported at this time.                                                   |
| `<meter>`    | Unavailable | JavaScript is not supported.                                                                   |
| `<noscript>` | Unavailable | JavaScript is not supported, but `<noscript>` tags are hidden.                                 |
| `<object>`   | Unavailable | Embedded content is not supported.                                                             |
| `<optgroup>` | Unavailable | JavaScript is not supported. Web forms are not supported at this time.                         |
| `<option>`   | Unavailable | JavaScript is not supported. Web forms are not supported at this time.                         |
| `<param>`    | Unavailable | Embedded content is not supported.                                                             |
| `<progress>` | Unavailable | JavaScript is not supported.                                                                   |
| `<script>`   | Unavailable | JavaScript is not supported.                                                                   |
| `<select>`   | Unavailable | JavaScript is not supported. Web forms are not supported at this time.                         |
| `<style>`    | Unavailable | CSS is not supported.                                                                          |
| `<textarea>` | Unavailable | JavaScript is not supported. Web forms are not supported at this time.                         |
| `<video>`    | Restricted  | Content not loaded on ThunderNet, but instead through the internet. Cannot be autoplayed.      |

### Extra page detail inference
Certain `<meta>` tags may be interpreted to show extra metadata or detail about a specific page. This information may be presented to the visitor of a site if it is not already included within the main content of the web page.

| Meta name                | Usage                                                |
|--------------------------|------------------------------------------------------|
| `author`                 | Display name of article author                       |
| `og:email`               | Contact email address for site                       |
| `og:phone_number`        | International phone number for site                  |
| `og:locality`            | Town or city where article has originated from       |
| `og:region`              | Province or region where article has originated from |
| `article:published_time` | Time of publication (in ISO 8601 format[F3])         |

## Footnotes
[F1] See https://html.spec.whatwg.org/ for the full HTML standard.

[F2] See https://www.w3schools.com/tags/default.asp for a list of all HTML tags, including tags that are unavailable in HTML5.

[F3] See https://www.iso.org/iso-8601-date-and-time-format.html for more information about the ISO time standard.
