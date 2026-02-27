# vinext HackerNews on Upsun

A deployment of the [cloudflare/vinext HackerNews example](https://github.com/cloudflare/vinext/tree/main/examples/hackernews) on [Upsun](https://upsun.com), demonstrating that vinext applications are not locked to Cloudflare Workers.

## What is vinext?

[vinext](https://blog.cloudflare.com/vinext/) is a drop-in Next.js replacement built on top of Vite by Cloudflare engineers. It implements the full Next.js API surface — App Router, React Server Components, server actions, caching, and middleware — while delivering significantly better performance:

- **4.4x faster builds** (1.67s vs 7.38s for Next.js)
- **57% smaller client bundles** (72.9KB vs 168.9KB gzipped)
- **94% Next.js API coverage**

Because ~95% of vinext's code is pure Vite with no Cloudflare-specific dependencies, it runs on any Node.js-capable platform — including Upsun.

## How this repo works

This repo contains no application source code. Instead, it holds only the Upsun configuration and two deployment-specific override files:

```
.upsun/config.yaml   # Platform config and build hook
package.json         # Node.js-targeted scripts and deps (overrides upstream)
vite.config.ts       # vinext Vite plugin config (overrides upstream)
```

At build time, the hook:
1. Sparse-clones the `examples/hackernews` directory from `cloudflare/vinext`
2. Restores the Upsun-targeted `package.json` and `vite.config.ts`
3. Patches the server info component to display "Upsun" branding
4. Runs `pnpm install` and `vinext build`

The upstream example targets Cloudflare Workers (via Wrangler). The `package.json` and `vite.config.ts` in this repo swap that out for the `vinext` Node.js server, which listens on a standard HTTP port.

## Deploying to Upsun

1. Fork or clone this repo
2. Create a new Upsun project and link it to the repo:
   ```bash
   upsun project:create
   upsun push
   ```
3. Upsun will run the build hook automatically on each push.

## Local development

To develop locally, clone the upstream source alongside the config overrides:

```bash
git clone --depth 1 --filter=blob:none --sparse https://github.com/cloudflare/vinext.git
cd vinext && git sparse-checkout set examples/hackernews
cd examples/hackernews
pnpm install
pnpm dev
```

Replace `package.json` and `vite.config.ts` with the versions from this repo if you want the Node.js server instead of the Cloudflare Workers target.

## References

- [cloudflare/vinext HackerNews example](https://github.com/cloudflare/vinext/tree/main/examples/hackernews)
- [Introducing vinext — Cloudflare Blog](https://blog.cloudflare.com/vinext/)
