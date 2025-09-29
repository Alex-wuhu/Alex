# Custom Markdown Examples

This website uses an enhanced version of Markdown with several custom features. Here are examples of how to use them.

## Table of Contents

To automatically generate a table of contents based on the headings in your post, simply add the following line where you want the TOC to appear:

```markdown
[[toc]]
```

## GitHub-style Alerts

You can create alerts that look like the ones on GitHub.

**Syntax:**

```markdown
> [!NOTE]
> This is a note.

> [!TIP]
> This is a tip.

> [!IMPORTANT]
> This is an important message.

> [!WARNING]
> This is a warning.

> [!CAUTION]
> This is a caution.
```

**Result:**

> [!NOTE]
> This is a note.

> [!TIP]
> This is a tip.

> [!IMPORTANT]
> This is an important message.

> [!WARNING]
> This is a warning.

> [!CAUTION]
> This is a caution.

## Magic Links

Certain keywords are automatically converted into links. Just type the keyword in your text.

**Supported Keywords:**
`NuxtLabs`, `Vitest`, `Slidev`, `VueUse`, `UnoCSS`, `Elk`, `Apus network`, `Samsung Electrions`, `Type Challenges`, `Vue`, `Nuxt`, `Vite`, `Shiki`, `Twoslash`, `ESLint Stylistic`, `Unplugin`, `Nuxt DevTools`, `Vite PWA`, `i18n Ally`, `ESLint`, `Astro`, `TwoSlash`, `Netlify`, `Stackblitz`.

**Example:**
Writing `Vite` will automatically be rendered as a link to the Vite website.

## Full-Width Layout

If you want a specific post to use the full width of the page instead of the standard centered prose layout, you can add the following to your content:

```markdown
@layout-full-width
```

This is useful for pages with wide tables, images, or custom components.
