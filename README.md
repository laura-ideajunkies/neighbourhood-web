# The neighbourhood web

**A thought experiment about helping people find independent websites, and giving those sites somewhere to belong.**

Status: thought experiment, version 0.1  
Author: Laura Richards, Idea Junkies Studio  
First published: October 2026  
Licence: [CC BY 4.0](LICENSE.md)  
Companion essay: link to the Frontier Philosophies article will be added when it's published.

I've been wondering whether we could borrow something from GeoCities' neighbourhoods to help people find their way around the personal web. This is where I've written down how I think it might work, with enough detail for someone to point out what I've missed.

I'm not building it. It sits firmly in the drawer marked *interesting*, alongside the RSS reader I'd quite like someone to make for me. But if you want to build on it, please do. The sketch is available under CC BY 4.0, with attribution as set out in the licence.

## Where the idea came from

GeoCities gave people a neighbourhood to put their website in, and somewhere to wander once they'd finished looking at one site. Webrings did something similar by linking sites around a shared interest. I've been thinking about what we could do with those ideas now, while keeping the freedom to build a website wherever and however we like.

AI agents have got me thinking about this again. If an agent can do more of our shopping or research on our behalf, we may never visit the websites it uses. That could change the economics for people whose work depends on those visits. I don't think losing advertising income would automatically give us a better internet, but I do wonder what we'd want the places we still visit ourselves to be like.

So this sketch starts with a writer choosing a neighbourhood for their site and making that choice public. Software could read those declarations and suggest sites with something in common. The reader would decide how far from their usual interests to explore. Ideally, I'd be able to start with a subject I know and end up somewhere I wouldn't have thought to search for.

## The choices I'd build into it

### 1. Writers choose their own neighbourhood

The person publishing chooses which neighbourhood their site belongs to. Software can suggest an order for sites within that neighbourhood, but it must never assign a neighbourhood or move a site into a different one. Writers can change their own choice.

### 2. We can browse the same neighbourhood

The list of named neighbourhoods should stay small enough to browse, be shared across readers and indexers, and change slowly. The recommendations could vary depending on my preferences, but we'd both be able to look around the same neighbourhood and see everyone who'd chosen to put their site there. Each service could calculate its own recommendations and distances between sites.

### 3. Participation is open, with a way to exclude particular services

The declaration is a public file on the writer's website. Anyone building an index can read it without the writer having to sign up to their service. A writer can name particular indexers in an exclusion list, and a service following this spec must respect that choice.

Putting that preference in a file doesn't enforce it. We'd need to work out how to deal with services that ignore exclusions. Making the declaration public also doesn't settle the terms for using the site's actual content; those are covered separately below.

### 4. Indexers can make different maps, and must explain when they change

Each indexer builds its own map from the public declarations, using its choice of embedding model. Embeddings represent aspects of a text as numbers, allowing software to compare sites for similarity.

Indexers must version and date their embedding sets, and announce when they change models. Otherwise, a site's suggested neighbours might change and nobody would know whether that reflected new writing or a different way of comparing it.

### 5. Every indexer shares its address list

An indexer must publish the full set of declarations it knows about as a downloadable file. Someone starting another service could use that list to find participating sites. I'd want a new service to have a reasonable place to start, and readers to be able to move between services without asking writers to move their sites.

### 6. Readers choose how far to wander

Some evenings I want more of a subject I'm already absorbed in, but other times I'm bored of my own interests and would quite like to encounter someone else's. The reader should let me adjust that distance, including asking for something surprising. A service must not widen or narrow that setting on my behalf.

### 7. Writers can also publish terms for machine use

The declaration may carry or point to terms for automated use of the site's content. For example, a writer might allow indexing while asking companies to contact them about training on their work.

I'd use an existing standard such as Really Simple Licensing (RSL), which provides a way to express machine-readable terms, including attribution and payment. How those terms connect to the declaration still needs working through. Publishing them would make them easier to find; getting services to respect them is a further problem.

## How it could work

### Layer 1 - the website

A normal, independently owned website, hosted wherever the writer chooses. The proposal doesn't require a new kind of hosting or a move to another platform.

### Layer 2 - the declaration

A small JSON file on the writer's own site, at a predictable address using the well-known URI convention (RFC 8615):

`https://example.com/.well-known/neighbourhood.json`

The file names the neighbourhood and gives the location of the site's RSS feed. It can also include terms for automated use and indexer exclusions. There's an example in [`neighbourhood.example.json`](neighbourhood.example.json).

A writer would join by adding the file and leave by removing it, without creating an account with an indexer. We'd still need to specify how often indexers check for changes, and how they handle a missing declaration, so leaving actually takes effect.

### Layer 3 - the indexer

An indexer reads declarations and feeds, computes embeddings for each site, and stores them in a vector index so it can look up sites with similar writing. It then makes those results available through an API (application programming interface), which reader apps can use.

The API would need to return the sites in a neighbourhood, find neighbours for a particular site, and return sites at a distance requested by the reader. What that distance means in practice still needs testing.

Anyone could run an indexer. To comply with this sketch, it must publish its address list and version its embeddings. It must also honour exclusions and leave each writer's choice of neighbourhood alone, as described above.

### Layer 4 - the reader

A reader is an app that uses one or more indexers to help people explore the sites. I'd want it to keep something of each site's own design where possible, and make the distance control easy to use. Different reader apps could present sites differently and offer different ways through them, while letting people browse the same declared neighbourhoods.

## The declaration file

The smallest declaration would look like this:

```json
{
  "version": "0.1",
  "name": "Example Kitchen",
  "neighbourhood": "food",
  "feed": "https://example.com/feed.xml"
}
```

The [full example](neighbourhood.example.json) adds a description and language, plus suggested fields for terms and indexer exclusions.

The `terms` object in that example is illustrative. It uses placeholder values to show the kinds of choices a writer might make; it isn't an implementation of RSL. Before anyone builds against it, we'd need to agree how the declaration references or includes terms from an existing standard.

## Things I haven't worked out

Who looks after the neighbourhood list, and how do people agree to change it? This sketch assumes one neighbourhood per site, but that choice needs testing with people whose writing covers several subjects. We'd also need to work out what happens when people misuse a category or fill it with spam (someone will 100% try).

There's a question about the people doing the work, too. GeoCities had Community Leaders who helped newcomers and took an interest in their neighbourhoods. Would this need something similar, and how would we support them? A good index might help people find a site without giving them much reason to keep coming back.

The terms for machine use raise further questions about agreements and enforcement. We'd need to agree how services accept those terms and what happens if they ignore them. Payment would need its own arrangements too.

And a file format on its own gets us very little. There would need to be an index with enough interesting sites and a reader worth opening. I'd want to test those together before assuming this was something people would use.

## Ideas this borrows from

GeoCities neighbourhoods and Community Leaders; webrings; RSS and OPML; robots.txt; RFC 8615 well-known URIs; the IndieWeb and microformats; Maximal Marginal Relevance (Carbonell and Goldstein, 1998) for diversity-aware retrieval; Really Simple Licensing (RSL, 2025) for machine-readable content licensing terms.

These are existing ideas I'm trying to bring together. I'm interested in combining a neighbourhood the writer chooses with recommendations the reader can adjust, and I'd like to hear about anyone already doing something similar.

## If you want to take this further

Please point out the holes or build something that tests the idea. Publishing the sketch gives people a dated version to refer to and a way to discuss the details. I haven't committed to developing it myself, but I'd be very interested to see what someone else makes of it.

---

(c) 2026 Laura Richards, Idea Junkies Studio. Licensed under [CC BY 4.0](LICENSE.md).

Suggested attribution: "The neighbourhood web" by Laura Richards (Idea Junkies Studio, 2026), licensed under CC BY 4.0.
