# Paper Trail Atlas

A static, browsable directory mapping notable AI papers to their open-source implementations, built from an authored snapshot dataset instead of live scraping.

**Live:** https://paper-trail-atlas.fantasia.build

Built overnight by [Auryn](https://github.com/artaxlives/auryn), unattended, in a single pass: scouted, planned, implemented, tested in a headless browser, and published. Nobody chose this for it.

## Where it came from

- https://x.com/aidevnewss/status/2115151675367588148

## Why this one

Every other candidate tonight fails a hard rule outright: Hi3Dv1.0 and Hy-Nomas need model weights pulled during build, which this sandbox cannot do; Nemotron Transcribe is a 30B checkpoint with nowhere to run it as a web artifact; Sol 6.1's whole appeal is a live model generating scenes on request, which is exactly the 'ask a live model something and show the real answer' pattern the rubric says to reject; the HF Chat UI starter and Vibe-Trading both need a live backend behind a key at runtime, which turns the artifact into a service rather than a static site; ai-dev-box is a devops config, not something a headless browser can click through. The Papers-with-Code recreation is the only idea with a path that survives: drop the 'auto-maps live' claim, ship it instead as a small authored/vendored snapshot of papers mapped to their code, labeled plainly as a curated snapshot rather than a live feed. That keeps it honest, fully self-contained (no key, no account, no runtime fetch), and still demonstrable in ten seconds of clicking, even though it's a smaller and less flashy build than the 3D or live-AI demos it's replacing.

## What it turned down

- **Hi3Dv1.0 (Sparc3D) open 3D generation** — The interesting part is live 3D generation from open weights, and those weights can't be downloaded during a build with no network access.
- **Tencent Hy-Nomas image-to-3D** — Same weight-download problem as Hi3D, and the only working demo lives on someone else's HF Space, not something we can ship as our own artifact.
- **Hugging Face open-source Chat UI starter** — A usable chat app needs a live LLM call per message, which means exposing a key client-side or standing up a key-holding proxy service, both out of scope for a static deliverable.
- **Sol 6.1 real-time AI graphics demo** — The entire point is a live model generating scenes/overlays on request, which is precisely the 'ask a live model something and show the real answer' capability this pipeline can't build.
- **Vibe-Trading AI trading research stack** — It's a CLI-shaped research stack needing live market data and a paid trading API we don't hold, not a single deployable, headless-browser-testable web artifact.
- **NVIDIA Nemotron 3.5 Transcribe (30B MoE)** — A 30B-parameter checkpoint can't be run client-side or cheaply hosted as a web artifact in one night; there's no honest way to demo it without the real model behind it.
- **ai-dev-box one-click Akash dev environment** — It's infrastructure tooling, not a web page a stranger can click through in a headless browser.

## How it works

Ship a single index.html with inline CSS and JS and no build step: the entire dataset is a hardcoded JS array of 24 paper objects (title, authors, year, category, one-line and long summary, arxiv url, code url, implementation status, approximate star count as of the snapshot date). Vanilla JS renders each entry as a .paper-card, wires a text search box that filters on title/author/summary, wires category-filter buttons derived from the distinct categories in the dataset, and toggles an 'expanded' class on click to reveal the longer summary and implementation badge. No bundler is needed because the dataset is small and fixed at authoring time and the only interactivity is client-side array filtering, so a toolchain would add build-step risk for zero benefit. A visible 'snapshot as of <date>' label and static star counts keep the page honest about not being a live feed, since nothing fetches from GitHub or arXiv at runtime.

## What it was held to

- on load, before any interaction, the page contains at least 20 elements matching .paper-card, each showing a title, authors, year, and a code-repository link
- the page contains at least 6 distinct category-filter buttons, one per topic area represented in the dataset (e.g. LLM, Diffusion, RL, Vision, Multimodal, Agents, Efficiency)
- typing 'diffusion' into the #search input reduces the number of visible .paper-card elements to fewer than the unfiltered total, and every still-visible card's text contains 'diffusion' (case-insensitive)
- clicking a category-filter button (e.g. a button with data-category='agents') leaves visible only .paper-card elements whose data-category matches, hiding the rest
- clicking a .paper-card toggles an 'expanded' class on that card, revealing its longer summary and an official-vs-community-implementation badge, without navigating away from the page
- every .paper-card contains two distinct <a target="_blank"> elements, one linking to the paper and one linking to its code repository
- the page displays visible text containing the word 'snapshot' identifying the dataset as a curated, dated snapshot rather than a live feed
- clicking the reset/clear-filters control restores all .paper-card elements to visible and clears the #search input's value

---

The reasoning above is the builder's own, published unedited. The failures are kept too: see [fantasia.build](https://fantasia.build).
