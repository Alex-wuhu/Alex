# Content Guide for Your Website

This guide explains how to add and manage content on your new website.

## Adding a New Blog Post

1.  **Create a new file:** Add a new Markdown file (`.md`) inside the `pages/posts/` directory. The name of the file will be used to create the URL for the post. For example, `my-awesome-post.md` will be accessible at `/posts/my-awesome-post`.

2.  **Add Front Matter:** At the very top of your new file, you need to add a section with metadata for your post, written in YAML format. Here is an example:

    ```yaml
    ---
    title: My Awesome Post
    date: YYYY-MM-DD
    lang: en
    duration: 5min
    tags:
      - Tech
      - Guide
    ---
    ```

    - `title`: The title of your post.
    - `date`: The date of publication. Use the `YYYY-MM-DD` format.
    - `lang`: The language of the post (e.g., `en` for English, `zh` for Chinese).
    - `duration`: An estimate of how long it takes to read the post (e.g., `5min`, `10min`).
    - `tags`: A list of tags for your post. This is used for filtering.

3.  **Write Your Content:** Below the front matter, you can start writing your post using Markdown.

## Adding a New Note

Notes are similar to blog posts but are for shorter, more specific pieces of information. They are distinguished by their front matter.

1.  **Create a new file:** Just like a blog post, add a new Markdown file (`.md`) inside the `pages/posts/` directory.

2.  **Add Front Matter:** The front matter for a note should include `type: note` and should **not** include a `duration`.

    ```yaml
    ---
    title: My Quick Note
    date: YYYY-MM-DD
    type: note
    tags:
      - Tip
    ---
    ```

    - `title`: The title of your note.
    - `date`: The date of the note.
    - `type`: Must be set to `note`.
    - `tags`: A list of tags for your note.

## Tagging Policy

Please do not add tags to posts or notes unless they are explicitly provided. This helps maintain a clean and organized tag system.

## Polishing Guidelines

When polishing or improving draft documents, please adhere to the following guideline:

- **Do not translate content:** Do not translate content from one language to another (e.g., from Chinese to English). The website supports mixed languages, so please preserve the original language of the content.

## Custom Markdown Features

This website supports special Markdown features to make your content richer. Please refer to the `MARKDOWN_EXAMPLES.md` file (also in this `.github` folder) for a full list and examples of all the custom syntax you can use.

## Important Reminders

- After making changes to configuration files (like `vite.config.ts`), you will need to restart the development server for the changes to take effect.
- Images should be placed in the `public/images/` directory. You can create subdirectories for organization (e.g., `public/images/my-awesome-post/`). When you reference them in your Markdown, use a path starting from the root, like `![](/images/my-awesome-post/image.png)`.

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
