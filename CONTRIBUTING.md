# Contributing

This repository holds data, not code. There is nothing to build and nothing to run: a contribution
is an edit to a document, and the bar is that the edit is **true**.

By contributing you agree that your contribution is released under [CC0](LICENSE), like everything
else here.

## Adding a language

This is the most common contribution, and it is deliberately cheap — one entry.

It used to require a change in three programs, which is why languages that people actually speak
are missing. If yours is one of them, that is an omission and not a decision.

An entry needs:

| field | |
|---|---|
| `name` | the **identity**. English, one spelling, and it never changes afterwards — the website stores it, the mod sends it, a game's config holds it. Renaming it orphans every translation already published under the old spelling. |
| `tag` | the [BCP 47][bcp47] tag. For most languages this is the familiar two-letter code, which is already a valid BCP 47 tag. |
| `parent` | only for a variety: `Egyptian Arabic` has `Arabic` as its parent, so a picker can group them instead of scattering them alphabetically. |
| `aliases` | what someone would actually type. An Egyptian says "Egyptian", not "Egyptian Arabic". |

### Two things to know before you open the pull request

**The name is a contract with the website.** Its upload endpoint validates against exactly this
list. A name added here and not there — or spelled differently — does not degrade anything
visibly: it makes uploads fail with a validation error the contributor did not cause and cannot
read. The two move together, in the same change.

**A language with no ISO 639-1 code is welcome.** That standard is a closed set of about 184
two-letter codes; it has no room for Cantonese or for the Arabic varieties, which have hundreds of
millions of speakers between them. BCP 47 covers them (`yue`, `ar-EG`, `ary`). Being absent from
639-1 is a fact about a 1967 standard, not about a language.

## Recording what a provider accepts

Google Translate and DeepL each publish the languages they support, and those lists change without
us. So:

- take it from their endpoint, do not type it from their documentation page;
- record **when** it was taken, so a stale entry can be recognised as stale;
- record refusals as plainly as acceptances. Knowing DeepL will not do a language lets a program
  say so before someone starts a translation, instead of after a request comes back rejected.

## Recording a model

What a publisher claims about a model is **their claim**, and it stays labelled as one. We do not
measure how well a model translates, and a number written here as if we had would mislead exactly
the person it is meant to help.

In particular: how many languages a model handles is a **count**, and it stays a count. Turning it
into "which languages", and filtering what a user is offered by what they are translating into, is
something this project does not do anywhere.

## What does not belong here

Anything that changes only when we decide it does. How versions compare, how a secret is stored,
how a merge resolves — those are rules, they live in
[unitygametranslator-common][common], and they are code.

The test: if this data can go out of date without anyone touching it, it belongs here.

[bcp47]: https://www.rfc-editor.org/info/bcp47
[common]: https://github.com/djethino/unitygametranslator-common
