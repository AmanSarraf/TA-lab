# Sprint 04 Landing Page Design Decisions

**Date:** 2026-08-02

**Status:** Proposed for review in `TA-site`

**Artifact type:** Personal research and decision rationale

## Purpose

This note records the reasoning behind the proposed Talent Angels landing page.
It is not the product specification or a second implementation. The shared,
runnable landing page belongs in
[`LFX-Talent-Angels/TA-site`](https://github.com/LFX-Talent-Angels/TA-site).

The decision becomes a shared project decision only after it is reviewed and
accepted through a `TA-site` pull request.

## Repository boundary

| Artifact | Owner |
| --- | --- |
| Research, alternatives, measurements, QA notes, and learnings | This Sprint 04 mentee folder |
| Landing-page source, configuration, assets, copy, and tests | `TA-site` |
| MVP chat interface | `TA-app` |
| Assistant runtime and API edge | `TA-agents` |
| Taxonomy ingestion and graph schemas | `TA-taxonomies` |
| Installed dependencies (`node_modules`), build output, local caches, and secrets | Never committed |

The existing personal prototype is reference material only. Useful ideas may be
reimplemented selectively in `TA-site`; its repository history and redundant
files will not be transferred.

## Product boundary

The landing page is the project's public front door. It explains the problem,
the Talent Angels direction, and how people can follow or contribute to the
work.

It will be:

- A static, public, single-page website.
- Independent from the MVP chat interface.
- Responsive and accessible across mobile and desktop layouts.
- Deployable without a backend, database, authentication, or secrets.
- Honest about work in progress without inventing adoption, users, partners,
  integrations, or results.

It will not call the assistant API, query taxonomy graphs, embed the MVP chat,
or collect personal data during the initial delivery.

## Visual direction

The proposed experience is one continuous page with anchored navigation. It
takes inspiration from the editorial rhythm of the Learning Tokens landing page
without copying its identity, wording, or project-specific motifs.

The design should use:

- Strong typographic hierarchy and generous spacing.
- A restrained technical/editorial visual language rather than generic SaaS
  cards.
- Graph, node, and pathway motifs that support the project story.
- A considered light and dark theme, not a simple colour inversion.
- Accessible contrast, visible focus states, reduced-motion support, and
  keyboard-operable controls.
- Final project language without demo-only disclaimers.

The exact content, palette, typography, illustrations, and page sections will
be reviewed in the later layout and content pull requests.

## Technical approaches considered

### Plain HTML, CSS, and JavaScript

This has the smallest toolchain and produces a static site naturally. It is a
reasonable choice for a very small page, but maintaining repeated sections,
shared layout behavior, theme logic, and future content changes would become
less structured as the site grows.

### React with Vite

This offers a large ecosystem and familiar component patterns. However, the
landing page does not need a client-side application runtime, routing, or
application state. React would add more browser JavaScript and blur the boundary
between the static site and `TA-app`.

### Astro with Tailwind CSS — recommended

Astro matches the content-first, static requirement while still providing
components for a maintainable one-page layout. It can ship HTML and CSS with
client JavaScript added only where interaction requires it. Tailwind provides a
consistent design-token and responsive-utility layer for implementing the
shared visual system.

The fresh scaffold should use Tailwind CSS 4 through its Vite plugin. Astro's
older `@astrojs/tailwind` integration is deprecated, so the earlier prototype's
Tailwind 3 configuration should not be copied unchanged. See the official
[Astro styling guide](https://docs.astro.build/en/guides/styling/) and
[`@astrojs/tailwind` deprecation notice](https://docs.astro.build/en/guides/integrations-guide/tailwind/).

## Proposed stack

- Current stable Astro with static output.
- Astro components without a React, Vue, or Svelte integration.
- TypeScript checking.
- Tailwind CSS 4 through `@tailwindcss/vite`.
- npm with a committed `package-lock.json` for reproducible installs.
- GitHub Pages as the deployment target.
- GitHub Actions for build and deployment in a dedicated deployment PR.

GitHub Pages keeps the public site close to the open-source repository and does
not introduce another hosting account. Astro documents an official GitHub
Action for static Pages deployments, while GitHub recommends an Actions workflow
when a static-site generator needs a build step. See the
[Astro GitHub Pages guide](https://docs.astro.build/en/guides/deploy/github/)
and
[GitHub Pages custom-workflow guide](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages).

## Contribution sequence

The shared implementation should land through small, reviewable `TA-site` pull
requests:

1. **Foundation:** record the stack rationale and add a minimal Astro, Tailwind,
   TypeScript, and npm scaffold that passes checks and builds static output.
2. **Layout:** add the single-page structure, design tokens, responsive layout,
   and theme behavior.
3. **Content:** add reviewed project copy, public links, and necessary assets.
4. **Deployment:** add and verify the GitHub Pages workflow and repository-path
   configuration.
5. **QA:** record and address accessibility, responsive, performance, and
   interaction findings.

Each implementation commit must use DCO sign-off, and each pull request should
target the organization repository from a contributor fork.

## Validation evidence to retain here

This mentee folder will record evidence gathered while the shared implementation
is reviewed:

- Links to the `TA-site` pull requests.
- Build and type-check results.
- Static output and dependency observations.
- Desktop and mobile visual-review findings.
- Keyboard, contrast, reduced-motion, and theme-behavior findings.
- Performance measurements and changes made in response.
- Mentor feedback and resulting lessons.

No shared pull request has been opened as of 2026-08-02. Links and measured
results will be added only after they exist.

## References

- Sprint 04 MVP and Landing Page brief.
- [Talent Angels `TA-site`](https://github.com/LFX-Talent-Angels/TA-site).
- [Learning Tokens landing page](https://hyperledger-labs.github.io/learning-tokens/).
- [Astro styling and Tailwind guidance](https://docs.astro.build/en/guides/styling/).
- [Astro deployment to GitHub Pages](https://docs.astro.build/en/guides/deploy/github/).
- [GitHub Pages custom workflows](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages).
