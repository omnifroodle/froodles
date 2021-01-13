---
title: Adding some Tailwind (css)
date: "2021-01-13T14:30:00.000Z"
description: "Changing our CSS framework over to Tailwind"
---

I'm a fan of Tailwind CSS.  Is it the best CSS framework? I have no idea. But it does let me iterate quickly with small discrete building blocks. It's the [Unix Philosophy](https://en.wikipedia.org/wiki/Unix_philosophy) in CSS and I'm running with it.

Tailwind has a good set of instructions on integrating with Gatsby.  [Saving you a Google](https://tailwindcss.com/docs/guides/gatsby). In future times, if that link is gone you can check my tailwind commit here, (https://github.com/omnifroodle/froodles/commit/2f2ad3c89c52a1914794c0f3c78fe81e4d71678c). That can serve as a handy reference for this post as well.

Once we have tailwind up and intergrated, we need to clean out the old styles.  Starting with the `gatsby-browser.js`, I removed the `normalize.css` and the `styles.css` (you could also just empty this and keep it, it may be useful later).

Here's the updated version

```js
// custom typefaces
import "typeface-montserrat"	import "typeface-montserrat"
import "typeface-merriweather"	import "typeface-merriweather"
// normalize CSS across browsers	
import "./src/normalize.css"	import './src/styles/global.css'
// custom CSS styles	
import "./src/style.css"	
// Highlighting for code blocks	// Highlighting for code blocks
import "prismjs/themes/prism.css"	import "prismjs/themes/prism.css"
```

You can now delete `normalize.css` and `styles.css` from your project.

A note on `normalize`, we don't need it, because Tailwind has it's own approach.  However, if you look at the site now even the header styles are gone.  That's by design, you can find out more here about [prefight normalization](https://tailwindcss.com/docs/preflight).

Let's layer in some base style into our `global.css`.

```css
@tailwind base;
@layer base {
  h1 {
    @apply text-2xl font-black;
  }
  h1 > a, h2 > a {
    @apply no-underline;
    color: inherit;
  }
  h2 {
    @apply text-xl font-black;
  }
  h3 {
    @apply text-lg;
  }
  a {
    @apply text-blue-600 underline;
  }
  h1,
  h2,
  h3,
  h4,
  h5,
  h6 {
    font-family: Montserrat, system-ui, -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans", sans-serif,
    "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
  }
}
@tailwind components;

@tailwind utilities; 
```

This gives our headers some style and uses the typeface from the original Gatsby starter.

The next step is either 
1) Implementing the existing style classes in our `styles.css` and making use of tailwind.
1) Hacking some Tailwind into our components directly.

I choose the later, and just made a couple changes in the `layout.js` to re-implement the layout of the site with Tailwind primatives.

Here's an excerpt.
```js
  return (	  return (
    <div className="global-wrapper" data-is-root-path={isRootPath}>	    <div className="container mx-auto space-y-8" data-is-root-path={isRootPath}>
      <header className="global-header">{header}</header>	      <header className="global-header">{header}</header>
      <main>{children}</main>	      <main>{children}</main>
      <footer>	      <footer class="text-sm text-center">
        © {new Date().getFullYear()}, Built with	        © {new Date().getFullYear()}, Built with
        {` `}	        {` `}
        <a href="https://www.gatsbyjs.com">Gatsby</a>	        <a href="https://www.gatsbyjs.com">Gatsby</a>
```

That's basically it, if you have questions or I skipped a step take a look at this commit in my git repo, (https://github.com/omnifroodle/froodles/commit/2f2ad3c89c52a1914794c0f3c78fe81e4d71678c).