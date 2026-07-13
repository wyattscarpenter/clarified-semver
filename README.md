# Clarified Semver

Semantic versioning is a much better idea than everybody-just-versions-how-they-feel-like versioning, which some projects still sadly use. (There are also other viable versioning patterns, like date-based versioning; cf https://date-ver.com/ & https://calver.org/). The most popular expression of the concept of semantic versioning is https://semver.org/ "Semantic Versioning 2.0.0" from the site Semantic Versioning (according to the HTML metadata on that site). That site is presented as a "Specification", but unfortunately leaves some important questions unanswered or even answered wrong. This document, "Clarified Semver", specifies additional details about semver.

<link rel="stylesheet" type="text/css" href="/style.css" /> <!-- This line is merely to style the page correctly in systems that respect such styling; it has no semantic meaning otherwise. -->
 
## Document precedence

This document builds on top of https://semver.org/spec/v2.0.0.html; in all unspecified details, that document should be consulted. In conflicts of contents or implications, this document wins.

## Nomenclature

All of semver, Semver, SemVer, semVer, and SEMVER are correct; as are the same but with a hyphen, an underscore, or a space between the first three letters and the second three letter. All capitalizations of "Semantic Versioning" are acceptable.

You may specify that you follow specifically "Clarified Semver" to indicate that you follow Semver and you believe this document is correct about it.

## Clarifications

### API

The concept of an API is never elaborated upon in the semver spec. As an engineer, you may be tempted to think the API is "any observable behavior of the system". (You may then wonder how PATCH and MAJOR do not contradict each other.) However, this is not how the "API" of semver is. The API in semver is the interface you have agreed to. Often implicitly, but implicitly in the sense of "it was what you intended", not "it was everything you happened to do".

There is a bonus detail here that is hard to characterize, which is that if you change behavior backwards-incompatibly in a way that does not impact what you thought of as your API, but which a significant number[^num] of your dependents will be adversely affected by, you should probably treat that as an API change anyway. (If it pleases you, you can think of this as being truly a part of your API, implying that users get some say in what the API actually is.) See also "Version numbers are free" below.

[^num]: Can a significant number be 1? I don't see why not.

### Non-bug PATCH

PATCH increments may be used for anything that does not require a MAJOR or MINOR increment. Documentation changes and internal refactors fit in here.

## Guidance

Although not binding, here is some advice about semver:

### Git tags and branches

Common convention is that releases are tagged in git as v[version identifier] and branches are named after features/contributors/etc. While it's technically possible to do something else, like use branches for marking releases and tags for marking feature development, that would just confuse everyone and not be helpful. I recommend you do it the standard way. Other version control systems probably have similar conventions.

### Major often

A lot of semver-related advice is implicitly afraid of doing major increments. This makes a certain amount of sense, given that they demand greater downstream-developer attention. And, in general, you should try to maintain backwards compatibility for as long as possible if you are a mature product with dependents. But, if the fiasco of Python 2→3 has taught us anything (python does not use semantic versioning btw[^py]) it's that it can often be worse to gather up all your backwards-incompatible breaking changes at once into one major version bump. It can be less hope-destroying for your dependents if you tear off that bandage a bit at a time, allowing them to incrementally adjust to each breaking change.

[^py]: They *should* — it would be a good idea for them to — but they don't.

### Version numbers are free

There is no reason to be afraid of doing major versions — or any other kind of version increment — other than the actual cost to the users of removing the backwards compatibility. Version numbers are free and you can have as many as you want. That's not the important part of the process. For instance, if you have some change where you're not sure if it will cause trouble for dependents or not, it's best to err on the side of calling it a major increment rather than ambushing those potential users with a minor-or-patch increment that *technically* you weren't *required* to describe as a major.

### Deprecating versions

A number of the FAQ items on semver.org don't really contemplate the possibility of deprecating (or, in some ecosystems, "yanking") a version that is faulty, which you probably can and should do in cases like "What do I do if I accidentally release a backward incompatible change as a minor version?" and "What if I inadvertently alter the public API in a way that is not compliant with the version number change (i.e. the code incorrectly introduces a major breaking change in a patch release)?". See, for example: https://docs.npmjs.com/deprecating-and-undeprecating-packages-or-package-versions, https://docs.pypi.org/project-management/yanking/, https://doc.rust-lang.org/cargo/commands/cargo-yank.html.

While it remains a crucial law of semver that each released version must have its own unique version identifier, it is not actually incumbent upon you to continue *providing* versions that completely break things.

### 0 major

A common practice is that in 0.y.z, y becomes a pseudo-major version indicator and z becomes a pseudo-(union of minor and patch) version indicator. I'm not recommending you do this; I'm just letting you know.

### Changelog

Basically the whole point of doing a major release is to tell downstream developers to pay extra attention, so you'd best have something for them to pay attention to, like a changelog or "here's what's new in version X!" document. Or, possibly, just a suitably detailed commit message with an easy way to find it.

### Pinning ranges

For various reasons, this is a bad idea. Just pin a specific version and advance that later. If you actually know for a fact that you support some range of versions of your deps, go ahead if you want (this had better be a finite set that gets tested in your testing pipeline, or else how do you know?!)

### Tilde range specifiers

NPM `~` (originally called "[spermies](https://github.com/npm/node-semver/commit/49de97e8411ccc30b78fc4d019800985c1233596)") and Python `~=` range specifiers actually do not mean the same thing. None of these range specifiers are part of semver, by the way. Just a tip for you.

## Versions of this document

https://semver.org/ describes itself as 2.0.0; this is presumably semantically versioned itself. You can see the difference between semver 1 and 2 here: https://github.com/semver/semver/compare/v1.0.0...v2.0.0. I have never in my entire life heard anyone specify what version of SemVer they were using. Presumably they were all using 2 🤷.

Clarified Semver, & this document, are not versioned, because they are not expected to significantly change in backwards-incompatible ways. This document may, however, be expanded and clarified from time to time. If you need to refer to a particular revision of this document, you may do so using [its git history](https://github.com/wyattscarpenter/clarified-semver/commits/master/). Yes, this does mean that the Clarified Semver specification is itself date-versioned.

## Epigraph

> If names be not correct, language is not in accordance with the truth of things. If language be not in accordance with the truth of things, affairs cannot be carried on to success. When affairs cannot be carried on to success, proprieties and music do not flourish. When proprieties and music do not flourish, punishments will not be properly awarded. When punishments are not properly awarded, the people do not know how to move hand or foot. Therefore a superior man considers it necessary that the names he uses may be spoken appropriately, and also that what he speaks may be carried out appropriately. What the superior man requires is just that in his words there may be nothing incorrect.

— Confucius, Analects, Book XIII, Chapter 3, verses 5–7, Analect 13.3, translated by James Legge

## License
This document is currently my sole, proprietary property, and if you [contribute to it](https://github.com/wyattscarpenter/clarified-semver/) you agree to give me your changes under those terms.

If it would somehow help you for this document to be freely licensed, we can talk.
