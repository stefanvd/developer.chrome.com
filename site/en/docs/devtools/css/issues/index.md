---
layout: "layouts/doc-post.njk"
title: "Find invalid, overridden, inactive, and other CSS"
authors:
  - sofiayem
date: 2022-11-15
description: "Discover issues with CSS properties at a glance."
tags:
  - css
  - find-issues
---

This guide assumes that you're familiar with inspecting CSS in Chrome DevTools. See [View and change CSS][1] to learn the basics.

## Inspect the CSS you author {: #styles }

Suppose that you added some CSS to an element and want to make sure the new styles are
applied properly. When you refresh the page, the element looks the same as before. Something is wrong.

The first thing to do is [inspect the element][2] and make sure that your new CSS is actually applied to the element.

Sometimes, you'll see your new CSS in the **Elements** > **Styles** pane but your new CSS is in <span style="opacity: 60%;">pale text</span>, non-editable, crossed out, or has a warning or hint icon next to it.

## Understand CSS issues {: #css-issues }

The **Styles** pane recognizes many kinds of CSS issues and highlights them in different ways.

### Invalid {: #invalid }

The **Styles** pane crosses out properties with invalid syntax and displays {% Img src="image/NJdAV9UgKuN8AhoaPBquL7giZQo1/hJxGejygnlBfMvhMtcCX.svg", alt="Warning.", width="24", height="24" %} warning icons next to them.

{% Img src="image/NJdAV9UgKuN8AhoaPBquL7giZQo1/HSQATBEOt6MholSjoji2.png", alt="Invalid CSS properties.", width="800", height="542" %}

### Overridden {: #overridden }

The **Styles** pane crosses out properties that are overridden by other properties according to the [Cascading order][3].

{% Video src="video/NJdAV9UgKuN8AhoaPBquL7giZQo1/rFzkt37FiDQ2kaxus8HD.mp4", autoplay=false, controls=true, loop=true, class="screenshot" %}

In this example, the `width: 300px;` style attribute on the element overrides `width: 100%` on the `.youtube` class.

### Inactive {: #inactive }

The **Styles** pane displays in <span style="opacity: 60%;">pale text</span> and puts {% Img src="image/NJdAV9UgKuN8AhoaPBquL7giZQo1/2OyGRodvgk9neTIEAH4g.svg", alt="Information.", width="24", height="24" %} information icons next to properties that are valid but have no effect because of other properties.

These pale properties are inactive because of CSS logic, not the [Cascading order][3].

{% Aside 'gotchas' %}
- The pale inactive properties differ from pale [non-inherited properties](#inherited-and-non-inherited). Inactive properties have icons.

- Hover over the {% Img src="image/NJdAV9UgKuN8AhoaPBquL7giZQo1/2OyGRodvgk9neTIEAH4g.svg", alt="Information.", width="24", height="24" %} icon to get a hint at what went wrong.
{% endAside %}

{% Img src="image/NJdAV9UgKuN8AhoaPBquL7giZQo1/pe6BdDcVryWmOuT3YjLz.png", alt="Inactive CSS declaration with a hint.", width="800", height="498" %}

In this example, the `display: block;` property disables `justify-content` and `align-items` that control flex or grid layouts.

### Inherited and non-inherited {: #inherited-and-non-inherited }

The **Styles** pane lists properties in `Inherited from <element-name>` sections depending on their [default inheritance](https://developer.mozilla.org/en-US/docs/Web/CSS/inheritance):

- Inherited by default are in regular text.
- Non-inherited by default are in <span style="opacity: 60%;">pale text</span>.

{% Aside 'gotchas' %}
- The pale non-inherited properties differ from pale [inactive properties](#inactive). Non-inherited properties don't have icons and are in the corresponding sections.
- [Overriding default inheritance](https://developer.mozilla.org/docs/Web/CSS/inheritance#overriding_inheritance_an_example) *doesn't* affect the way the **Styles** pane displays the properties: pale or not.
{% endAside %}

{% Img src="image/NJdAV9UgKuN8AhoaPBquL7giZQo1/pkBcVUnT1QLxEZkhq9P9.png", alt="The 'Inherited from body' section listing inherited and non-inherited CSS.", width="800", height="431" %}

### Shorthand {: #shorthand }

The **Styles** pane displays [shorthand properties](https://developer.mozilla.org/docs/Web/CSS/Shorthand_properties) as {% Img src="image/NJdAV9UgKuN8AhoaPBquL7giZQo1/VPpFJAIWgNSaTmnYrqNP.svg", alt="Drop-down.", width="24", height="24" %} drop-down lists that contain all the properties that are shortened.

{% Img src="image/NJdAV9UgKuN8AhoaPBquL7giZQo1/vzJiWOkmE0WeeZoJtdah.png", alt="The shorthand property with a drop-down list.", width="800", height="597" %}

### Non-editable {: #non-editable}

The **Styles** pane displays properties that can't be edited in *italic text*. For example, the CSS from the following sources can't be edited:

- `user agent stylesheet`—Chrome's default stylesheet.

  {% Img src="image/NJdAV9UgKuN8AhoaPBquL7giZQo1/yhDE5n8UWyu2T2CwKGOZ.png", alt="The CSS from user agent stylesheet.", width="800", height="445" %}

- Style attributes on the element. You can edit them in the DOM tree and this updates the CSS in the **Styles** pane, but not the other way around.

  {% Video src="video/NJdAV9UgKuN8AhoaPBquL7giZQo1/TwzSI9igPEbgELGEyAzU.mp4", autoplay=false, controls=true, loop=true, class="screenshot" %}

## Inspect an element that still isn't styled the way you think  {: #computed }

Use the **Computed** pane to see the "final" [CSS applied to an element](/docs/devtools/css/reference/#computed).

The **Elements** > **Styles** pane displays the exact set of CSS rules as they are written in various stylesheets. On the other hand, the **Elements** > **Computed** pane lists resolved CSS values that Chrome uses to render an element:

- CSS derived from [inheritance](https://developer.mozilla.org/docs/Web/CSS/inheritance)
- [Cascade](https://developer.mozilla.org/docs/Web/CSS/Cascade) winners
- Longhand properties (precise), not shorthand (concise)
- Computed values, for example, `font-size: 14px` instead of `font-size: 70%`

## Search for duplicates {: #filter }

To investigate a specific property and its potential duplicates, type that property name in the **Filter** textbox. You can do this both in the **Styles** and **Computed** panes.

{% Img src="image/NJdAV9UgKuN8AhoaPBquL7giZQo1/YF2aohEStRzAOvSQfWVj.png", alt="The Filter textboxes on Styles and Computed panes.", width="800", height="339" %}

See [Search and filter an element's CSS](/docs/devtools/css/reference/#filter).

## Find unused CSS {: #coverage }

See [Coverage: Find unused JavaScript and CSS](/docs/devtools/coverage/).

[1]: /docs/devtools/css
[2]: /docs/devtools/css/reference#select
[3]: https://developer.mozilla.org/docs/Web/CSS/Cascade#cascading_order