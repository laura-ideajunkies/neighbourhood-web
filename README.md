# The neighbourhood web

**An open sketch for decentralised publishing with declared addresses and computed maps.**

Status: thought experiment, version 0.1
Author: Laura Richards, Idea Junkies Studio
First published: October 2026
Licence: CC BY 4.0 (see LICENSE)
Companion essay: [link to Frontier Philosophies piece - added on publish]

This document exists to date and describe an idea, and to invite people to poke holes in it. Nobody is building this at the time of writing, least of all me. If you build on it, attribution under CC BY is all that's asked.

## Premise

The early web solved discovery with neighbourhoods and webrings, and never solved money. The platform web solved money with advertising, and broke discovery on purpose to do it. If AI agents move the transactional web onto machine-readable pages that humans never see, the advertising subsidy under the human-facing web drains away - and discovery becomes the problem worth solving again.

This spec sketches a discovery layer for independently owned sites. It borrows one idea from GeoCities (a writer-declared neighbourhood address), one from robots.txt (openness by default, refusal by exception), one from RSS (publish once, read anywhere), and adds a modern layer underneath: semantic mapping that orders sites within their declared neighbourhood, with the reader controlling how far from home they browse.

## Design principles

1. **The writer declares the address.** A neighbourhood is chosen by the person publishing, at the moment they set up their site. It is never inferred, assigned, or computed. A classifier can order sites within a neighbourhood; it can never move a site between neighbourhoods.
2. **Shared territory, plural routes.** The list of neighbourhoods is short and human-scale, and it changes slowly. It is the same for every reader - that sameness is what makes an address an address. Everything computed (orderings, distances, recommendations) may differ per indexer and per reader.
3. **Open by default, refusal by exception.** The declaration file grants no permission and names no gatekeeper, because a public fact about a site needs neither. Any indexer may read any declaration. A writer refuses a specific indexer by naming it in an exclusion list, exactly as robots.txt names user-agents.
4. **No canonical map.** Every indexer computes its own map from the same public declarations, with whatever embedding model it chooses. Indexers must version and date their embedding sets, and treat a model change as an announced event, so drift caused by the territory changing can be told apart from drift caused by re-measurement.
5. **The address book is public.** Every indexer must publish the full set of declarations it knows about, as a downloadable artefact. A new indexer bootstraps from an existing one's list. Knowing who exists is not a moat.
6. **The reader holds the dial.** Discovery distance is a setting the reader controls - same street, next street over, far side of town, or surprise me. Nothing widens or narrows a reader's exposure without the reader choosing it.
7. **The same file that makes a site findable makes it licensable.** The declaration may carry machine-readable terms for automated use of the site's content - indexing, retrieval, training - so that being discovered by a human and being licensed by a machine run through one declaration under the writer's control. This spec should not invent a terms vocabulary: the declaration should carry or reference terms expressed in an existing standard such as Really Simple Licensing (RSL), which already defines machine-readable licensing, attribution, and compensation terms with robots.txt-based discovery.

## Architecture: four layers

**Layer 1 - the site.** A normal, independently owned website. Static site generators and static hosts already make this free or nearly free. This layer needs no new infrastructure and no relationship with anyone.

**Layer 2 - the declaration.** One small JSON file at a well-known URI (RFC 8615) on the writer's own server:

`https://example.com/.well-known/neighbourhood.json`

The file declares the neighbourhood, the feed location, and optionally terms and exclusions. See `neighbourhood.example.json` in this repo. Deleting the file is leaving. No account, no registration, no platform.

**Layer 3 - the indexer.** A crawler that reads declarations and feeds, computes embeddings for each site, stores them in a vector index, and serves an API: sites in a neighbourhood, nearest neighbours to a site, sites at a requested distance. Anyone can run one. Obligations on an indexer that wants to be considered compliant with this spec: publish your address list (principle 5), version your embeddings (principle 4), honour exclusions (principle 3), and never reassign a declared neighbourhood (principle 1).

**Layer 4 - the reader.** Any application built on one or more indexer APIs. Renders sites editorially, respects each site's own design where it can, and exposes the radius control (principle 6). Readers compete on rendering and route-making; they inherit the same territory.

## The declaration file

Minimum viable declaration:

```json
{
  "version": "0.1",
  "name": "Example Kitchen",
  "neighbourhood": "food",
  "feed": "https://example.com/feed.xml"
}
```

Full example with optional keys - human-readable description, language, terms for automated use, and indexer exclusions - in `neighbourhood.example.json`.

Open questions this version does not resolve: who stewards the neighbourhood list and by what process it changes; whether a site may hold one address only (this sketch says yes, singularity is what makes the choice carry information); and how terms in the declaration bind legally without a contract layer behind them. Contributions and criticism on any of these are the point of publishing.

## Prior art and influences

GeoCities neighbourhoods and Community Leaders; webrings; RSS and OPML; robots.txt; RFC 8615 well-known URIs; the IndieWeb and microformats; Maximal Marginal Relevance (Carbonell and Goldstein, 1998) for diversity-aware retrieval; Really Simple Licensing (RSL, 2025) for machine-readable content licensing terms. None of the pieces here is new. The claim is that the combination, with the declared/computed split and the reader-held radius, has not been assembled.

## What this is not

Not a platform, not a product, not a token, not a company, and not a roadmap commitment by the author. It is a dated, attributable statement of an idea, published so that it exists as prior art and so that better-qualified people can improve it or demolish it.

---

(c) 2026 Laura Richards, Idea Junkies Studio. Licensed under [CC BY 4.0](LICENSE.md).

Suggested attribution: "The neighbourhood web" by Laura Richards (Idea Junkies Studio, 2026), licensed under CC BY 4.0.
