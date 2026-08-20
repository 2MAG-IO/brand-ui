# @2mag-io/brand-ui

Shared [Astro](https://astro.build) components used across the three 2MAG.IO sites:
**shg.sg**, **2mag.io**, and **2magnify.com**.

The package exists so that shared layout chrome is defined **once** and consumed
everywhere, instead of being copy-pasted per repo (which had already drifted).

## What's in scope

| Component | Purpose |
|---|---|
| `Navigation.astro` | Site header / primary nav |
| `Footer.astro` | Site footer |
| `BaseHead.astro` | `<head>` meta, canonical font/link tags |
| `ScrollReveal.astro` | Scroll-reveal animation helper |

## What's explicitly NOT in scope

**Design tokens.** `brand-tokens.css` continues to be served from the CDN
(`https://brand.2mag.io/brand-tokens.css`, source: `2mag-website/public/brand-tokens.css`).
This package carries **components only, never tokens**.

## Usage (once published)

```sh
npm install @2mag-io/brand-ui
```

```astro
---
import Navigation from '@2mag-io/brand-ui/Navigation.astro';
import Footer from '@2mag-io/brand-ui/Footer.astro';
---
<Navigation />
<slot />
<Footer />
```

Requires Astro `>=5.0.0` (peer dependency).

## Publishing

Publishing is automated. It runs when a **GitHub Release** is published
(`.github/workflows/publish.yml`), authenticating to npm via
**Trusted Publishing (OIDC)**: there is no npm token stored in this repo or in
GitHub secrets. To cut a release:

1. Bump `version` in `package.json`, commit, push to `main`.
2. Create a GitHub Release with tag `v<version>` (matching `package.json`).
3. The `Publish to npm` workflow publishes `@2mag-io/brand-ui@<version>`.

Consuming sites receive version bumps automatically via Renovate.

## Dependency updates

Renovate watches this repo via `renovate.json`. Scope is deliberately narrow, because the
package itself has no runtime or dev dependencies:

- **GitHub Actions** in `.github/workflows/publish.yml` are tracked. Minor and patch bumps
  auto-merge; major bumps raise a PR and wait for a human, since runner and Node changes ride
  along with them and this workflow publishes to npm via OIDC trusted publishing.
- **`peerDependencies`** (`astro >=5.0.0`) is explicitly **not** tracked. That range states which
  Astro versions consumers may use. Narrowing it is a release decision, not a dependency bump.

This differs on purpose from the three site repos, which disable everything except
`@2mag-io/brand-ui`. They only want this package tracked; this package is published to three
consumers, so its own supply chain is worth watching. See `2MAG-Corporate\TODO.md`.
