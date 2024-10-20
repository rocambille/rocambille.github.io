---
ref: 2024-10-20-a-css-razor
lang: en
title: "A CSS razor"
tags: css beginners
authors:
  - romain-guillemot
---

![](/assets/2024-10-20-a-css-razor.webp){: width="768"}{: height="453" }

A "razor" in philosophy is a methodological principle that helps simplify complex choices by eliminating unnecessary hypotheses or options.

<!--more-->

The most famous one is [Occam's Razor](https://en.wikipedia.org/wiki/Occam%27s_razor), which advises not to multiply entities or hypotheses beyond necessity: choose the simplest explanation that works.

Applied to CSS, this idea would suggest streamlining our style property choices to design pages in a simple and effective manner, adopting techniques that solve layout problems without unnecessary complexity.

To apply the philosophical razor to CSS, it's about choosing the simplest and most effective solutions to solve layout problems, without overloading the code with unnecessary rules. Here's how you can structure your CSS property choices efficiently, adopting a progressive approach to maintain simplicity while handling complex layout requirements:

## Prioritize the Normal Flow

The **normal flow** is the natural way HTML elements are arranged on the page without any specific intervention. It is the simplest foundation and should be your starting point for building a layout.

- **Blocks**: Block-level elements (such as `<section>`, `<p>`, `<h1>`, etc.) stack vertically, taking up the full available width.
- **Inline**: Inline elements (such as `<strong>`, `<a>`) line up next to each other, following the horizontal flow of text.

Always start by seeing if the basic layout can be accomplished simply by working with these natural behaviors. For example:

- Adjust dimensions using `max-width` or `max-height` for containers or images only after the content is in place.
- Use typography properties (`font-size`, `line-height`, etc.) to organize content.

## Switch to Flexbox or Grid When Necessary

When the normal flow isn’t enough, **Flexbox** and **CSS Grid** are powerful tools for handling more complex layouts. Use them thoughtfully, avoiding unnecessary complexity in the structure:

- **Flexbox** is ideal for **one-dimensional** layouts (either a row or a column):
  - For instance, to center an element both horizontally and vertically within a container, `display: flex` and `justify-content: center; align-items: center;` will suffice.
  - Flexbox excels at simple layouts where the relationship between elements is linear (e.g., navigation bars, aligned cards, etc.).

- **CSS Grid** is better suited for **two-dimensional** layouts (arranging elements in rows and columns):
  - Use Grid for more complex layouts, like image galleries or data tables.
  - Grid is more powerful than Flexbox for layouts where you need to control both rows and columns simultaneously.

The idea is to introduce Flexbox or Grid only when you reach the limits of the normal flow, avoiding applying them everywhere without real need.

For more details, check out these excellent guides by Josh Comeau:

* [An Interactive Guide to Flexbox](https://www.joshwcomeau.com/css/interactive-guide-to-flexbox/).
* [An Interactive Guide to CSS Grid](https://www.joshwcomeau.com/css/interactive-guide-to-grid/).

## Handle Spacing with `padding` and `margin`

To organize the spaces between elements, it's essential to understand the differences between `padding` and `margin` and to apply these properties methodically:

- **Padding**: Manages the space **inside** the element, between its content and its border. Use `padding` to add space between internal content and the edge of a container, like in a button or card.

- **Margin**: Manages the space **outside** the element, between the element’s border and the surrounding elements. Use `margin` to space elements apart from one another within the flow.

In general, use `padding` for **internal** space and `margin` for **external** space. It’s often clearer to use `margin` to control spacing between independent elements and reserve `padding` for adjusting space inside container elements.

See this article by Nathan Curtis for visual proof: [Space in Design Systems](https://medium.com/eightshapes-llc/space-in-design-systems-188bcbae0d62).

## Use `position` Values for Layering

Positioning in CSS allows for more dynamic layouts, but it’s important to avoid overusing them. Here's how to choose between the different `position` values:

- **`position: static`** (default): Elements are positioned based on the normal flow.

- **`position: relative`**: The element stays in the normal flow but can be offset from its original position. Use it when you want to move an element slightly without affecting the flow of other elements.

- **`position: absolute`**: The element is removed from the normal flow and positioned relative to its first positioned ancestor (one with `position: relative`, `absolute`, or `fixed`). It’s useful for layering elements or positioning something precisely within a container without influencing others.

- **`position: fixed`**: Similar to `absolute`, but the element is positioned relative to the browser window and remains fixed while scrolling (e.g., sticky navigation bars, pop-ups).

- **`position: sticky`**: A mix between `relative` and `fixed`, it allows an element to stay in the flow until a certain condition is met (e.g., when it reaches a specific scroll point, it becomes fixed). It's useful for things like navigation bars that need to remain visible after some scrolling.

Use positioning wisely for specific cases where the normal flow and Flexbox/Grid cannot meet the requirements.

A concrete example: [sticky footer solved by Flexbox](https://philipwalton.github.io/solved-by-flexbox/demos/sticky-footer/).

## Choose Appropriate Sizes for a Fluid and Responsive Layout

To ensure that the layout remains fluid and responsive, use flexible units like:

- **`%`**: Percentages are relative to the parent container's size, allowing elements to adapt to different screen sizes.
- **`em` and `rem`**: These units are relative to the parent element’s font size (or the root element’s size for `rem`). They are ideal for creating adaptive sizes, especially for spacing (`margin`, `padding`) and dimensions other than `100%` (`width`, `height`).
- **`vw` and `vh`**: These units are relative to the browser window size (1 `vw` = 1% of the window's width, 1 `vh` = 1% of the height). Use them for layouts that adapt to the entire screen size.

Avoid fixed units like `px` unless absolutely necessary, to ensure the design stays fluid across devices.

A great use case: [fluid typography](https://www.smashingmagazine.com/2022/01/modern-fluid-typography-css-clamp/).

## My CSS Razor in short

1. **Start with the normal flow**: Build a layout foundation using block and inline elements, simply adjusting `display`, `width`, `height`.
2. **Use Flexbox or Grid sparingly**: Switch to Flexbox for one-dimensional layouts or Grid for more complex two-dimensional layouts when the normal flow is insufficient.
3. **Handle spacing intelligently**: Use `margin` to space elements apart and `padding` to manage space inside containers.
4. **Position elements as needed**: Use `relative` for minor adjustments, `absolute` or `fixed` for elements outside the normal flow, and keep everything else `static`.
5. **Prioritize fluid units**: Use relative units like `%`, `em`, `rem`, `vw`, and `vh` to ensure a fluid and adaptable layout.

By following this methodical approach and simplifying as much as possible, you’ll be able to design effective pages without falling into the traps of overcomplexity while ensuring code maintainability.
