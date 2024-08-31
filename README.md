# AStro Example

Features:

- ✅ All features from Astro's default starter
- ✅ List comments on blog posts
- ✅ Add comments to blog posts

This example uses Vue to list and add comments to blog posts, however, statically rendering comments is also possible with Astro.

## 🚀 Getting Started

To get started with this site, you'll need to add your Headless Comments API key and Site ID to /src/comments/index.ts, or as environment variables.

```ts
const api = new HeadlessComments("YOUR_API_KEY_HERE", "YOUR_SITE_ID_HERE");
```

## 🚀 Project Structure

Inside of your Astro project, you'll see the following folders and files:

```text
├── public/
├── src/
|   ├── comments/
│   ├── components/
│   ├── content/
│   ├── layouts/
│   └── pages/
├── astro.config.mjs
├── README.md
├── package.json
└── tsconfig.json
```

Astro looks for `.astro` or `.md` files in the `src/pages/` directory. Each page is exposed as a route based on its file name.

## 👀 Want to learn more?

Check out [our documentation](https://docs.headlesscomments.io) or jump into our [Discord server](https://discord.gg/invite/bekhDgtPjP).

