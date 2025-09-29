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

## Custom Markdown Features

This website supports special Markdown features to make your content richer. Please refer to the `MARKDOWN_EXAMPLES.md` file (also in this `.github` folder) for a full list and examples of all the custom syntax you can use.

## Important Reminders

- After making changes to configuration files (like `vite.config.ts`), you will need to restart the development server for the changes to take effect.
- Images should be placed in the `public/images/` directory. You can create subdirectories for organization (e.g., `public/images/my-awesome-post/`). When you reference them in your Markdown, use a path starting from the root, like `![](/images/my-awesome-post/image.png)`.
