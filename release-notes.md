We're _**so**_ excited to release Remix v2 to you and we really hope this upgrade is one of the smoothest framework upgrades you've ever experienced! That was our primary goal with v2 - something we aimed to achieve through a heavy use of deprecation warnings and [Future Flags](https://remix.run/blog/future-flags) in Remix v1. If you are on the latest `1.x` version and you've enabled all future flags and addressed all console warnings, then our hope is that you are 90%+ of the way to being upgraded for v2! There are always going to be a few things that we _can't_ put behind a flag (like breaking types changes) or come up at the very last moment and don't have time to add as a warning or flag in `1.x`.

If you're _not_ yet on the latest 1.x version we'd recommend first upgrading to that and resolving any flag/console warnings: `npx upgrade-remix 1.19.3`

## Breaking Changes

Please read the [**Preparing for v2**](https://remix.run/docs/en/dev/guides/v2) guide which provides a comprehensive walkthrough of the breaking changes that come along with v2 - and instructions on how to adapt your application to handle them.

For clarity, here's a _very concise_ list of the breaking changes in v2:

- Thw following future flags were removed and their behavior is now the default:
  - [`v2_dev`][v2dev] - New dev server with HMR+HDR ([#7002](https://github.com/remix-run/remix/pull/7002))
  - [`v2_errorBoundary`][v2errorboundary] - Removed `CatchBoundary` in favor of a singular `ErrorBoundary` ([#6906](https://github.com/remix-run/remix/pull/6906))
  - [`v2_headers`][v2headers] - Altered the logic for `headers` in nested route scenarios ([#6979](https://github.com/remix-run/remix/pull/6979))
  - [`v2_meta`][v2meta] - Altered the return format of `meta()` ([#6958](https://github.com/remix-run/remix/pull/6958))
  - [`v2_normalizeFormMethod`][v2normalizeformmethod] - Normalize `formMethod` APIs to uppercase ([#6875](https://github.com/remix-run/remix/pull/6875))
  - [`v2_routeConvention`][v2routeconvention] - Routes use a flat route convention by default now ([#6969](https://github.com/remix-run/remix/pull/6969))
- Breaking Changes/API Removals
  - `@remix-run/react`
    - Removed [`useTransition`][usetransition] ([#6870](https://github.com/remix-run/remix/pull/6870))
    - Removed [`fetcher.type`][usefetcher] and flattened [`fetcher.submission`][usefetcher] ([#6874](https://github.com/remix-run/remix/pull/6874))
      - `<fetcher.Form method="get">` is now more accurately categorized as `state:"loading"` instead of `state:"submitting"` to better align with the underlying GET request
    - Require camelCased versions of [`imagesrcset`/`imagesizes`][imagesrcsetsizes] ([#6936](https://github.com/remix-run/remix/pull/6936))
    - Remove `unstable_shouldReload` in favor of `shouldRevalidate` ([#6865](https://github.com/remix-run/remix/pull/6865))
  - `remix.config.js`
    - Renamed [`browserBuildDirectory`][browserbuilddirectory] to `assetsBuildDirectory` ([#6900](https://github.com/remix-run/remix/pull/6900))
    - Removed [`devServerBroadcastDelay`][devserverbroadcastdelay] ([#7063](https://github.com/remix-run/remix/pull/7063))
    - Renamed [`devServerPort`][devserverport] to `dev.port` ([#7078](https://github.com/remix-run/remix/pull/7078))
      - ⭕️ Preparing for v2 shows `v2_dev.port` - what's the best way to communicate this nuance?
    - Changed [`serverBuildDirectory`][serverbuilddirectory] to `serverBuildPath` ([#6897](https://github.com/remix-run/remix/pull/6897))
    - Removed [`serverBuildTarget`][serverbuildtarget] ([#6896](https://github.com/remix-run/remix/pull/6896))
    - Changed the default [`serverModuleFormat`][servermoduleformat] from `cjs` to `esm` ([#6949](https://github.com/remix-run/remix/pull/6949))
    - Node built-ins are no longer polyfilled by default, you must opt-into polyfills via [`browserNodeBuiltinsPolyfill`][browsernodebuiltinspolyfill] and [`serverNodeBuiltinsPolyfill`][servernodebuiltinspolyfill] ([#6911](https://github.com/remix-run/remix/pull/6911), [#7269](https://github.com/remix-run/remix/pull/7269))
    - PostCSS/Tailwind will be enabled by default if config files exist in your app, you may disable this via the [`postcss` and `tailwind`][postcsstailwind] flags ([#6909](https://github.com/remix-run/remix/pull/6909))
  - `@remix-run/node`
    - `fetch` is no longer polyfilled by default - apps must call [`installGlobals()`][installglobals] to install the polyfills ([#7009](https://github.com/remix-run/remix/pull/7009))
    - Apps must call [`sourceMapSupport.install()`][sourcemapsupport] to setup source map support
  - The [`@remix-run/netlify`][netlifyadapter] adapter has been removed in favor of the Netlify official adapters
  - The [`@remix-run/vercel`][verceladapter] adapter has been removed in favor of out of the box functionality provided by Vercel
  - ⭕️ Removed magic exports ([#6895](https://github.com/remix-run/remix/pull/6895))
  - `@remix-run/dev`
    - ⭕️ Removed `REMIX_DEV_HTTP_ORIGIN` in favor of `REMIX_DEV_ORIGIN` ([#6963](https://github.com/remix-run/remix/pull/6963))
    - ⭕️ Removed `REMIX_DEV_SERVER_WS_PORT` in fvor of `dev.port` or `--port` ([#6965](https://github.com/remix-run/remix/pull/6965))
    - ⭕️ Removed `--no-restart`/`restart` flag in favor of `--manual`/`manual` ([#6962](https://github.com/remix-run/remix/pull/6962))
    - ⭕️ Removed `--scheme`/`scheme` and `--host`/`host` in favor of `REMIX_DEV_ORIGIN` instead ([#6962](https://github.com/remix-run/remix/pull/6962))
    - ⭕️ Removed the `codemod` command ([#6918](https://github.com/remix-run/remix/pull/6918))
  - `@remix-run/cloudflare`
    - ⭕️ Remove `createCloudflareKVSessionStorage` ([#6898](https://github.com/remix-run/remix/pull/6898))
    - ⭕️ Drop `@cloudflare/workers-types` v2 & v3 support ([#6925](https://github.com/remix-run/remix/pull/6925))
  - `@remix-run/eslint-config`
    - ⭕️ Remove `@remix-run/eslint-config/jest` config ([#6903](https://github.com/remix-run/remix/pull/6903))
    - ⭕️ Remove magic imports ESLint warnings ([#6902](https://github.com/remix-run/remix/pull/6902))
  - `create-remix`
    - ⭕️ Stop passing `isTypeScript` to `remix.init` script ([#7099](https://github.com/remix-run/remix/pull/7099))
  - `remix-serve`
    - `remix-serve` picks an open port if 3000 is taken and `PORT` is not specified ([#7278](https://github.com/remix-run/remix/pull/7278))
    - Integrate `manual` mode ([#7231](https://github.com/remix-run/remix/pull/7231))
    - Remove undocumented `createApp` Node API ([#7229](https://github.com/remix-run/remix/pull/7229))
    - Preserve dynamic imports in remix-serve for external bundle ([#7173](https://github.com/remix-run/remix/pull/7173))
- Breaking Type Changes
  - ❓
- Minimum version support
  - ⭕️ React 18 ([#7121](https://github.com/remix-run/remix/pull/7121))
  - ⭕️ [Node 18][node-version-support] ([#6939](https://github.com/remix-run/remix/pull/6939), [#7292](https://github.com/remix-run/remix/pull/7292))

## New Features

- New [`create-remix`][createremix] CLI ([#6887](https://github.com/remix-run/remix/pull/6887))
  - Most notably, this removes the dropdown to choose your template/stack in favor of the `--template` flag and our ever-growing list of [available templates][templates]
  - Adds a new `--overwrite` flag ([#7062](https://github.com/remix-run/remix/pull/7062))
  - Supports the `bun` package manager ([#7074](https://github.com/remix-run/remix/pull/7074))
- Detect built mode via `build.mode` ([#6964](https://github.com/remix-run/remix/pull/6964))
- Support polyfilling node globals via `serverNodeBuiltinsPolyfill.globals`/`browserNodeBuiltinsPolyfill.globals` ([#7269](https://github.com/remix-run/remix/pull/7269))
- New `redirectDocument` utility to redirect via a fresh document load ([#7040](https://github.com/remix-run/remix/pull/7040), [#6842](https://github.com/remix-run/remix/pull/6842))
- Add `error` to `meta` params so you can render error titles, etc. ([#7105](https://github.com/remix-run/remix/pull/7105))
- `unstable_createRemixStub` now supports adding `meta`/`links` functions on stubbed Remix routes ([#7186](https://github.com/remix-run/remix/pull/7186))
  - ⚠️ `unstable_createRemixStub` no longer supports the `element`/`errorElement` properties on routes. You must use `Component`/`ErrorBoundary` to match what you would export from a Remix route module.

## Other Notable Changes

- Remix now uses React Router's `route.lazy` method internally to load route modules on navigations ([#7133](https://github.com/remix-run/remix/pull/7133))
- Updated React Router dependencies:
  - [`react-router-dom@6.16.0`](https://github.com/remix-run/react-router/releases/tag/react-router%406.16.0)
  - [`@remix-run/router@1.9.0`](https://github.com/remix-run/react-router/blob/main/packages/router/CHANGELOG.md#190)

## Changes by Package 🔗

- [`create-remix`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/create-remix/CHANGELOG.md#200)
- [`@remix-run/architect`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-architect/CHANGELOG.md#200)
- [`@remix-run/cloudflare`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-cloudflare/CHANGELOG.md#200)
- [`@remix-run/cloudflare-pages`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-cloudflare-pages/CHANGELOG.md#200)
- [`@remix-run/cloudflare-workers`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-cloudflare-workers/CHANGELOG.md#200)
- [`@remix-run/css-bundle`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-css-bundle/CHANGELOG.md#200)
- [`@remix-run/deno`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-deno/CHANGELOG.md#200)
- [`@remix-run/dev`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-dev/CHANGELOG.md#200)
- [`@remix-run/eslint-config`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-eslint-config/CHANGELOG.md#200)
- [`@remix-run/express`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-express/CHANGELOG.md#200)
- [`@remix-run/node`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-node/CHANGELOG.md#200)
- [`@remix-run/react`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-react/CHANGELOG.md#200)
- [`@remix-run/serve`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-serve/CHANGELOG.md#200)
- [`@remix-run/server-runtime`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-server-runtime/CHANGELOG.md#200)
- [`@remix-run/testing`](https://github.com/remix-run/remix/blob/remix%402.0.0/packages/remix-testing/CHANGELOG.md#200)

---

**Full Changelog**: [`1.19.3...2.0.0`](https://github.com/remix-run/remix/compare/remix@1.19.3...remix@2.0.0)

[v2dev]: https://remix.run/docs/en/release-next/guides/v2#remix-dev
[v2errorboundary]: https://remix.run/docs/en/release-next/guides/v2#catchboundary-and-errorboundary
[v2headers]: https://remix.run/docs/en/dev/guides/v2#route-headers
[v2meta]: https://remix.run/docs/en/release-next/guides/v2#route-meta
[v2normalizeformmethod]: https://remix.run/docs/en/release-next/guides/v2#formmethod
[v2routeconvention]: https://remix.run/docs/en/2.0.0/guides/v2#file-system-route-convention
[usetransition]: https://remix.run/docs/en/2.0.0/guides/v2#usetransition
[usefetcher]: https://remix.run/docs/en/2.0.0/guides/v2#usefetcher
[imagesrcsetsizes]: https://remix.run/docs/en/2.0.0/guides/v2#links-imagesizes-and-imagesrcset
[browserbuilddirectory]: https://remix.run/docs/en/2.0.0/guides/v2#browserbuilddirectory
[devserverbroadcastdelay]: https://remix.run/docs/en/2.0.0/guides/v2#devserverbroadcastdelay
[devserverport]: https://remix.run/docs/en/2.0.0/guides/v2#devserverport
[serverbuilddirectory]: https://remix.run/docs/en/2.0.0/guides/v2#serverbuilddirectory
[serverbuildtarget]: https://remix.run/docs/en/2.0.0/guides/v2#serverbuildtarget
[servermoduleformat]: https://remix.run/docs/en/2.0.0/guides/v2#servermoduleformat
[browsernodebuiltinspolyfill]: https://remix.run/docs/en/2.0.0/guides/v2#browsernodebuiltinspolyfill
[servernodebuiltinspolyfill]: https://remix.run/docs/en/2.0.0/guides/v2#servernodebuiltinspolyfill
[installglobals]: https://remix.run/docs/en/2.0.0/guides/v2#installglobals
[sourcemapsupport]: https://remix.run/docs/en/2.0.0/guides/v2#source-map-support
[netlifyadapter]: https://remix.run/docs/en/2.0.0/guides/v2#netlify-adapter
[verceladapter]: https://remix.run/docs/en/2.0.0/guides/v2#vercel-adapter
[postcsstailwind]: https://remix.run/docs/en/2.0.0/guides/v2#built-in-postcsstailwind-support
[node-version-support]: https://remix.run/docs/en/2.0.0/other-api/node#version-support
[createremix]: https://remix.run/docs/en/2.0.0/other-api/create-remix
[templates]: https://remix.run/docs/en/2.0.0/guides/templates
