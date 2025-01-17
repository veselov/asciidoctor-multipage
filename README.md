# asciidoctor-multipage

A multipage HTML converter for Asciidoctor—extends the Html5Converter to
generate multiple HTML pages from the document tree.

See also:

- <https://rubygems.org/gems/asciidoctor-multipage>
- <https://github.com/asciidoctor/asciidoctor/issues/626>
- <https://github.com/asciidoctor/asciidoctor-extensions-lab/pull/96>

## Features

- Generates a root (top level) landing page with a list of child sections.
- Generates branch (intermediate level) landing pages as required, each with
  a list of child sections.
- Generates leaf (content level) pages with the actual content.
- Allows the chunking depth to be configured with the `multipage-level`
  document attribute (the default is 1—split into chapters).
- Supports variable chunking depth between sections in the document (by
  setting the `multipage-level` attribute on individual sections).
- Uses section IDs to name each page (eg. "introduction.html").
- Supports cross-references between pages.
- Generates a full Table of Contents for each page, but with relevant entries
  only (the TOC collapses as required for each page).
- Includes a description for each section on the branch/leaf landing pages
  (from the `desc` attribute, if set).
- Generates previous/up/home/next navigation links for each page.
- Allows the TOC entry for the current page to be styled with CSS.
- Supports standalone and embedded (--no-header-footer) HTML output.
- Retains correct section numbering throughout.

## Notes and limitations

Footnotes are currently not supported. See [issue
#3](https://github.com/owenh000/asciidoctor-multipage/issues/3).

## Installation

Install the gem by adding it to your project's Gemfile or gemspec and running Bundler, or install it directly:

```
$ gem install asciidoctor-multipage
```

(Run `gem install --user-install asciidoctor-multipage` to install the gem in
your user's home directory.)

## Usage

Be sure to use Asciidoctor v2.0.11 or later.

```
$ asciidoctor -V
Asciidoctor 2.0.12 [https://asciidoctor.org]
```

Use Asciidoctor's `-r` option to require `asciidoctor-multipage` and the `-b`
option to select the `multipage_html5` backend. The following command generates
HTML output from a sample document; view the output by loading
`test/out/sample.html` in a browser.

```
$ asciidoctor -r asciidoctor-multipage -b multipage_html5 \
    -D test/out test/black-box-docs/sample/sample.adoc
```

## Adjusting behavior

The following attributes can be used to adjust the extension's behavior:

- The `multipage-level` *document attribute* specifies the section level at
  which the book is split into separate pages. The value should be an integer
  and matches the Asciidoctor levels:
  - `0` splits into parts,
  - `1` splits into chapters (the default),
  - `2` splits into sections, etc.
- The `multipage-level` *section attribute* specifies the section level to use
  for splitting the children of that section only. The integer given must be
  equal to or greater than the values of all parent levels.
- The `desc` attribute can be used for any section to provide a description
  when the section is listed on its parent landing page.

For an example of the above attributes in use, see
`test/black-box-docs/sample/sample.adoc`.

## Sponsors and contributing

To my current sponsor, thank you for your support! ✨

To anyone else interested in contributing, please consider:

- sharing this project with someone else who may be interested;
- contributing a fix for a currently open
  [issue](https://github.com/owenh000/asciidoctor-multipage/issues) (if any)
  using a GitHub pull request (please discuss before working on any large
  changes);
- opening a new issue for a problem you've discovered or a possible
  enhancement; and/or
- sponsoring my work through [GitHub Sponsors](https://github.com/owenh000)
  (and see also [owenh.net/support](https://owenh.net/support)).

## Development

- To install dependencies, run `bundler install`.
- To run tests, run `bundler exec rake`.
- To run tests against multiple versions of Asciidoctor:
  1. run `bundler exec appraisal install` to install dependencies and
  2. run `bundler exec appraisal rake` to run the tests.
- To execute Asciidoctor with the extension (in its present local state) for
  testing, run `bundler exec asciidoctor -r asciidoctor-multipage -b
  multipage_html5 -D test/out test/black-box-docs/sample/sample.adoc` (for
  example).
- To build the current version, run `bundler exec rake build`; the gem will be
  placed in the `pkg/` directory.
- To release a new version:
  1. update the date in `asciidoctor-multipage.gemspec`, remove `.dev` from the
     version in `lib/asciidoctor-multipage/version.rb`, run `bundler lock`, and
     commit the changes;
  2. run `bundler exec rake release`; and
  3. increment the version in `lib/asciidoctor-multipage/version.rb` (adding
     `.dev`), run `bundler lock`, and commit the changes.
- To change versions of Asciidoctor to test against:
  1. update `Appraisals` as required,
  2. run `bundler exec appraisal generate --travis`,
  3. update `.travis.yml` using the output from the previous command, and
  4. commit the changes.

## Copyright and License

Copyright 2019-2021 Owen T. Heisler. MIT license.

This program is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

This source code may be used according to the terms of the MIT license. You
should have received a copy of this license along with this program (see
`LICENSE`).
