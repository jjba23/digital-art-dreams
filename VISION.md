# Vision

Digital Art Dreams is where the art this project's author has collected over the
years lives: a plain git repository of image files, dedicated to the public
domain under CC0. It is not a program and will not become one — it is an
archive, and an archive is judged by the questions a stranger can ask it. The
direction below is about turning a folder of images into a collection that
answers those questions — what this is, where it came from, what it is licensed
under, what it would look like on your screen — while keeping the property that
makes it worth having: `git clone` and nothing else.

## Where Digital Art Dreams is today

The repository holds 74 images, about 509 MB, in five top-level groups and three
levels of nesting at most:

- `mac/` — 25 wallpapers named after macOS releases, from the 10.4 Tiger era
  through 15 Sequoia, most carrying a resolution hint in the name (`6k`), a
  vendor-style download name (`wp4555285-…`) or a day and night pairing.
- `catppuccin/`, `sss/dracula/` and `sss/solarized-light.jpg` — art grouped by
  the palette it was drawn from rather than by subject.
- `space/` and `sss/space/` — astrophotography, some of it clearly from a public
  science archive, recognisable by the `heic`/`potw` naming that archive uses.
- `sss/` — 35 files, the largest group and the least structured: camera dumps
  with `PXL_` prefixes, Imgur-style short identifiers (`0mar2ygf59je1`), game fan
  art (`inazuma`, `diluc-tree`), forests, one Japanese-flavoured set, and the
  palette subgroups above.
- Root-level singles: `bergen-1/2`, `kanagawa`, `wild-hunt`.

Resolutions are high and inconsistent — 3840×2160 for a PNG, 6016×3384 and
6000×3845 for JPEGs, 4000×3000 for archive photographs — and EXIF is intact,
including the software that last touched a file, dates, and image descriptions
written by an archive. Names are the only metadata, and they are not a schema.

`README.org` is three paragraphs: what the repository is, the CC0 dedication,
and a pointer to the shared AI policy. There is no `COPYING` file, no index, no
manifest, no build, no CI, and no tooling of any kind. The only automation in
the tree is `.github/FUNDING.yml`.

Four decisions are load-bearing, and each one constrains the options:

1. **The bytes live in git, not beside it.** No LFS, no annex, no release
   tarballs. Every clone is the whole collection, which is a feature until the
   collection doubles.
2. **CC0 for everything, "unless otherwise stated", with nowhere to state it.**
   The licence claim is repository-wide and unverifiable per file, and there is
   no mechanism for an exception.
3. **Directory names are the taxonomy.** Palette, platform and subject are all
   expressed as paths, mixed freely, with no second axis and no cross-reference.
4. **Filenames are the catalogue.** There is no record of provenance, author,
   palette, resolution or content, so nothing can be searched, filtered or
   verified without opening the file.

## The vision

1. **Every image is described by a machine-readable record.** A generated
   catalogue — resolution, aspect ratio, dominant colours, provenance, licence,
   subject — makes the collection queryable, so selecting wallpapers for a
   palette or a screen size is a query and not an afternoon of `file` and
   `feh`.
2. **The catalogue is generated from the tree and checked in CI.** The index is
   a build product of the images plus a small hand-written provenance file, and
   a stale index is a build failure rather than a slowly rotting document.
3. **Nothing is anonymous.** Every `PXL_` dump and every short identifier has
   either been named for what it shows or carries a record saying the origin is
   unknown by choice. "Unknown" is an acceptable answer; "no record" is not.
4. **The size question has an answer that is written down.** Either the large
   originals move to a mechanism built for large files, or the collection states
   that plain git is the point and ships resolution tiers (the 6k original, plus
   generated 4k and 1080p renditions) so that a screen never needs the biggest
   copy.
5. **The licence is auditable file by file.** A `COPYING` plus a provenance
   record per image, where CC0 is the default and a per-file exception is the
   only way anything differs — including for the archive photographs whose
   upstream terms deserve a line of their own.

## Features that are in scope and not built yet

The tree already anticipates these, because the layout implies an axis that
nothing yet reads:

- **A catalogue task.** The directory groups imply a taxonomy, but nothing emits
  it. The hard part is the shape of the record: a sidecar per image is
  reviewable in a diff but 74 files to keep in step; one manifest is tidy but
  merges badly. A generated `index.org` with a hand-written `provenance.scm`
  as its only input is the honest compromise, and it still needs a decision on
  where new facts go when someone adds an image.
- **A generated index page.** `README.org` lists no images, so a visitor sees
  filenames. The hard part is thumbnails: the tree has no small derivatives, so
  either they are committed (doubling the review surface) or generated on demand
  (which means the index is only as good as the machine that built it).
- **Palette grouping that means something.** `catppuccin/`, `dracula/` and
  `solarized-light.jpg` name palettes without defining them, so a file placed in
  a palette folder is a claim nobody can check. The hard part is the source of
  truth for the palette values, which lives in another project entirely.
- **Archive-era metadata.** The `mac/` names encode release, day/night variant
  and resolution in one string, which is a schema in disguise. The hard part is
  parsing it without inventing facts: `10-5-Server-6k` and `12-Monterey-Dark`
  do not decompose the same way.
- **Duplicate detection.** `sss/` almost certainly repeats images that appear
  elsewhere in spirit if not in file. The hard part is near-duplicates at
  different resolutions: a perceptual hash answers it, and the threshold is the
  whole design.

## New features that would be worth having

- **A Guix or Nix packaging of a selection.** A derivation that installs a named
  subset (or all of it, by palette) into a home profile, so a machine describes
  its wallpapers declaratively and rotation is a rebuild rather than a copy. This
  is the single change that makes the collection *usable* rather than *browsable*.
- **Metadata hygiene on copies.** The camera dumps carry EXIF that includes
  device and date information; publishing scrubbed renditions next to the
  originals is good archival practice and costs one `exiftool` invocation per
  file.
- **Screenshot compositions.** A small script that renders a wallpaper behind a
  mock window and a bar, in the style of the desktop screenshots projects use to
  advertise themes. It is a genuine aid to choosing, and it is presentation work
  that fits an art collection.
- **A palette extractor that writes the record, not a picture.** Eight dominant
  colours per image, in a stable order, as data — which is what makes
  "wallpapers that go with this theme" a query.
- **A rotation manifest for wallpaper daemons.** Most daemons read a directory;
  a generated `wallpapers.conf`/`swww` script per collection would let a
  wallpaper change on a timer without a second tool.
- **Parametric pieces.** This is speculative and not yet in scope, but the honest
  long-term answer to 6k files is a few small programs that *draw* a wallpaper
  for a given palette and resolution, so the archive grows in hundreds of bytes.

## Contributions that would move the needle

1. **A generated catalogue plus a staleness check.** Highest value per hour: one
   Scheme or shell script over `identify`/`magick`, a small manifest, and a check
   that fails when the two disagree. Overnight, the collection becomes queryable
   and the "what is this file?" question has an answer that does not require
   opening it.
2. **A provenance pass, one image at a time.** Small, parallelisable, needs no
   code: for each file, record what it shows and where it came from, or record
   that the origin is unknown. It turns the licence claim from repository-wide
   assertion into per-file fact, and it is the thing that would make an archival
   donation possible.
3. **`COPYING` with the CC0 text and the exception rule.** Ten minutes of work,
   and it removes the only legal ambiguity in the repository.
4. **Renaming the opaque filenames.** Mechanical with `git mv`, but it should
   follow the record from contribution 2, not lead it, or the names invent
   provenance the archive does not have.
5. **Thumbnail generation in CI.** Larger, because it needs a decision about
   where derivatives live, but it unlocks the index page and the screenshot
   compositions.
6. **Packaging for a desktop profile.** The most useful change for a user and the
   most work for a maintainer: a derivation, a selection format, and the
   documentation that explains how to choose.

## Compared to related projects

| Project | What it is | Where Digital Art Dreams differs |
|---------|------------|----------------------------------|
| nixos-artwork | the NixOS distribution's artwork repository: logos and wallpapers as image files in git, installed by Nix derivations | This is not distro artwork and ships no derivation; today it is a personal collection, and its one structural advantage is that everything in it is CC0 |
| Catppuccin | a palette project with per-port repositories, including wallpaper sets, all derived from four defined colour schemes | Files here are grouped under palette names but the palettes are not defined here, so a folder is a claim rather than an application of a defined scheme |
| Dracula theme | a theme collection with wallpaper assets across many applications | Dracula is a design system with an identity to enforce; this archive has no opinions about how the art is used |
| wallhaven.cc | a hosted wallpaper index with tags, resolution filters, ratings and collections | That is searchable and this is not, but this is forkable, versioned, account-free, and cannot change its terms or disappear |
| Unsplash | a photographic library under its own licence with an API and a search index | CC0 here means no attribution obligation and no API key, at the cost of having no catalogue at all |
| Wikimedia Commons | a media archive with structured data, per-file licensing and full provenance for every file | Commons is the model this repository should steal from: structured data and per-file provenance, without the wiki |
| git-annex | a tool for versioning large files whose contents live outside the repository | This repository keeps 509 MB in git proper, which is a decision to be defended rather than an oversight |

What the others do that this could still add: structured per-file records with a
licence and a source (Commons), a searchable index with resolution and colour
facets (wallhaven), and installation as a package rather than a manual copy
(nixos-artwork). Where this is ahead and should stay ahead: the licence is one
line for the whole collection and needs no attribution tracking, the collection
is one clone with no account and no service, and it documents its AI policy —
which is more governance than most wallpaper collections have.

## In the long run

The multi-year bet is that an archive should stop being a pile of finished
pixels. Two consequences follow, and both are speculative today:

- **Generation over storage.** Once a catalogue knows the palette and the
  resolution of every piece, the interesting artefact is a renderer that produces
  a wallpaper matching a theme at whatever size the screen is. The 6k JPEG then
  becomes one output among several rather than the thing itself.
- **Archival respectability.** A CC0 collection with per-file provenance and
  structured data is something a public archive can accept, mirror or import
  wholesale. That is a better fate for thirty years of collecting than a
  repository nobody can search.

A third, smaller bet: wallpapers are how a desktop introduces itself, so a
collection with a rotation manifest and screenshot compositions becomes part of
how a machine is set up, not a folder visited once.

## Getting there from here

Start with the catalogue and the staleness check, then the provenance pass, then
`COPYING`. Those three steps need no new dependencies, no hosting decision and no
agreement about taste, and together they turn the tree into an archive that can
be asked questions. Everything after that — thumbnails, packaging, parametric
pieces — is a matter of how much the collection wants to be a product.

*This document describes intent, not commitments: nothing here is promised for a
version, a date or a release, and the collection is complete as it stands.*
