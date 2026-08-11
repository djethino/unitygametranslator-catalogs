# UnityGameTranslator Catalogs

Reference data shared by the UnityGameTranslator mod, installer and website: languages, AI models
and mod loaders.

## Why this is a repository and not a file in the code

Three programs need to agree on the same facts, and they cannot share code: the mod and the
installer are C#, the website is PHP. Data is the only thing all three can read.

There is already a shared **code** library — [unitygametranslator-common][common] — and the line
between the two is not "what is convenient to put where":

| | holds | changes when |
|---|---|---|
| `common` | rules — how to compare two versions, how a secret is stored | **we** decide |
| this repo | facts — what languages exist, what DeepL accepts, what loaders are out | **somebody else** decides |

A rule that changes on its own is a bug. A fact that only changes when we edit code is a fact
going stale without anyone noticing. That is the whole reason for the split.

## What is in here

| document | what it settles |
|---|---|
| languages | which languages a translation can be published in, their BCP 47 tags, how to display and search for them, and which of them each provider accepts |
| models | AI models a user may choose, and what their publishers claim — a claim, never our measurement |
| loaders | mod loader versions and where to get them |

## How it is consumed

The same way the loader catalogue already works, and for the same reasons:

1. fetched from GitHub — first, so that a launch does not put an IP in our own server logs;
2. our website as a mirror;
3. a local cache;
4. **a copy baked into the program.**

The last step is not a nicety. It means the tool works offline and on a locked-down network: the
data can be out of date, it can never be missing.

## Two ways to consume this, and the difference is not a preference

Some of these documents are **fetched at run time**. Others are **compiled into a program**. Which
one applies is decided by a single question:

> If this data goes stale, does the program break, or does it merely miss something new?

**Fetched** — loaders, models. A new mod loader ships, a new model appears. Missing one costs a
novelty, never a failure: the tool still installs every loader it already knows. Those change
without us, often, so waiting for a release to carry them would make the release the bottleneck.

**Compiled in** — languages. The mod runs inside a game, frequently with no network at all, and the
language name it sends IS the upload contract: the website validates against exactly that list. A
list fetched at run time could disagree with the code that uses it, and a fetch that simply failed
would leave someone with no language to pick. None of that is a risk worth taking for a list that
moves once in a blue moon, and that moves with a website release anyway.

⚠ **A fetched catalogue still ships an embedded copy**, and it is the SAME FILE that is served — not
a snapshot of it. Offline is not a degraded mode to be tolerated; it is the ordinary condition of a
game machine. Two files would eventually describe two different sets of loaders, and the one that
answered would depend on the network.

⚠ **Compiled in does not mean unwatched.** The language list exists three times — here, in the
website's PHP, in the shared C# library — because none of those runtimes can read the others: the
library targets netstandard2.0, which has no JSON parser and takes no packages, and the website is
PHP. So this repository is not the single source they all *read*; it is the single source they are
all *checked against*. The project keeps a script that compares the three and fails on any
divergence. Run it after touching the list, on every side.

## Rules that are not negotiable

**A language is identified by its name, not its code.** The website stores a name, its upload
endpoint validates against a list of names, the mod resolves to a name and a game's config holds a
name. Codes serve a purpose — a system locale, a provider request, a setting — but they are never
the identity. Five of the languages here have no ISO 639-1 code at all and are perfectly valid
targets.

**The language list is a controlled vocabulary.** Not to keep anyone out: without it, "French",
"french", "FR" and "Francais" become four different languages and every search fragments. Adding a
language is a line in a file here, which is the point of this repository — it used to be a change
in three programs.

**What a provider accepts is a fact about that provider.** Google and DeepL publish what they
support and it changes without us. Any list of theirs written by hand is wrong the day they add a
language, and the person who finds out is a user whose translation request was refused.

## Related

- [unitygametranslator][mod] — the mod
- [unitygametranslator-manager][manager] — the desktop tool
- [unitygametranslator-common][common] — shared rules, as code

[mod]: https://github.com/djethino/unitygametranslator
[manager]: https://github.com/djethino/unitygametranslator-manager
[common]: https://github.com/djethino/unitygametranslator-common
