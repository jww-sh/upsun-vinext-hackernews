# Hacker News on Upsun

A Hacker News clone (App Router, RSC) running on [Upsun](https://upsun.com) via vinext. Based on the [hackernews](../hackernews) example.

## Running locally

```sh
pnpm install
pnpm dev
```

## Deploying to Upsun

1. Install the [Upsun CLI](https://docs.upsun.com/administration/cli.html) and log in.

2. Create a new Upsun project:

```sh
upsun project:create
```

3. Push to deploy:

```sh
upsun push
```

The `.upsun/config.yaml` in this directory configures the Node.js environment, build, and routing. See comments in that file for the pnpm variant.
