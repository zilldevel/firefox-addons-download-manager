
![Zilldevel Download Fork](https://raw.githubusercontent.com/zilldevel/firefox-addons-downthemall/master/style/icon128.png)

# Zilldevel Download Fork

## About

This is a fork of [DownThemAll](https://github.com/downthemall/downthemall) by Nils Maier. For those unfamiliar with that project, I would strongly encourage you to use the original project and become familiar with that before considering mine.

This fork was mostly started so that I had somewhere to test out my own changes / make my own AMO test builds. I have no intentions of competing with DTA or making this fork into viable long-term alternative. If I have any worthwhile changes, they will probably make their way into PRs in the original project at some point. If I *do* happen to forget to make a PR/get hit by a bus, I would be happy to see anything I add get picked up in the original project. The name and logo changes are just me trying to comply with DTA's [License terms](https://github.com/downthemall/downthemall/blob/master/LICENSE.md) to the best of my ability and avoid stepping on any toes.

Please support the original project.

Note: Most of this Readme file is unchanged from the original project, so "I" will usually be from the perspective of the original author (Nils Maier) unless otherwise noted.

Note \#2: For anybody that doesn't like the new icons... Keep in mind that I (zilldevel) am NOT a graphics designer and likely wouldn't have bothered changing them at all if it hadn't been clearly worded as a requirement in the license. So please envision a poorly drawn smiley face done in MS Paint (or really TuxPaint for me since I'm on Linux) and remember that they could have been much worse. As it was, it took me quite awhile to learn my way around GIMP enough to manage this much. Since it's mostly just for my own use, I don't mind but I guess if someone really hated them enough to provide GPL/CC licensed replacements, then I guess I'd at least consider swapping mine out.


## Functional differences from the DTA WebExtension

Currently none

## Translations

While translations are always welcome, unless it is specific to some change that I (zilldevel) made, I would encourage you to submit any translation work upstream. Please see [DTA's translation guide](https://github.com/downthemall/downthemall/blob/master/_locales/Readme.md).

## Development

### Requirements

- [node](https://nodejs.org/en/)
- [yarn](https://yarnpkg.com/)
- [python3](https://www.python.org/) >= 3.6 (to build zips)
- [web-ext](https://www.npmjs.com/package/web-ext) (for development ease)

### Setup

You will want to run `yarn` to install the development dependencies such as webpack first.

### Making changes

Just use your favorite text editor to edit the files.

You will want to run `yarn watch`.
This will run the webpack bundler in watch mode, transpiling the TypeScript to Javascript and updating bundles as you change the source.

Please note: You have to run `yarn watch` or `yarn build` (at least once) as it builds the actual script bundles.

### Running in Firefox

I recommend you install the [`web-ext`](https://www.npmjs.com/package/web-ext) tools from mozilla. It is not listed as a dependency by design at it causes problems with dependency resolution in yarn right now if installed in the same location as the rest of the dependencies.

If you did, then running `yarn webext` (additionally to `yarn watch`) will run the WebExtension in a development profile. This will use the directory `../dtalite.p` to keep a development profile. You might need to create this directory before you use this command. Furthermore `yarn webext` will watch for changes to the sources and automatically reload the extension.
  
Alternatively, you can also `yarn build`, which then builds an *unsigned* zip that you can then install permanently in a browser that does not enforce signing (i.e. Nightly or the Unbranded Firefox with the right about:config preferences).

### Running in Chrome/Chromium/etc

You have to build the bundles first, of course.

Then put your Chrome into Developement Mode on the Extensions page, and Load Unpacked the directory of your downthemall clone.

### Making release zips

To get a basic unofficial set of zips for Firefox and chrome, run `yarn build`.

If you want to generate release builds like the ones that are eventually released in the extension stores, use `python3 util/build.py --mode=release`.

The output is located in `web-ext-artifacts`.

- `-fx.zip` are Firefox builds
- `-crx.zip` are Chrome/Chromium builds
- `-opr.zip` are Opera builds (essentially like the Chrome one, but without sounds)

### The AMO Editors tl;dr guide

  1. Install the requirements.
  2. `yarn && python3 util/build.py --mode=release`
  3. Have a look in `web-ext-artifacts/dta-*-fx.zip`

### Patches

Before submitting patches, please make sure you run eslint (if this isn't done automatically in your text editor/IDE), and eslint does not report any open issues. Code contributions should favor typescript code over javascript code. External dependencies that would ship with the final product (including all npm/yarn packages) should be kept to a bare minimum and need justification.

Please submit your patches as Pull Requests, and rebase your commits onto the current `master` before submitting.

### Code structure

The code base is comparatively large for a WebExtension, with over 11K sloc of typescript.
It isn't as well organized as it should be in some places; hope you don't mind.

- `uikit/` - The base User Interface Kit, which currently consists of
  - the `VirtualTable` implementation, aka that interactive HTML table with columns, columns resizing and hiding, etc you see in the Manager, Select and Preferences windows/tabs
  - the `ContextMenu` and related classes that drive the HTML-based context menus
- `lib/` - The "backend stuff" and assorted library routines and classes.
- `windows/` - The "frontend stuff" so all the HTML and corresponding code to make that HTML into something interactive
- `style/` - CSS and images
