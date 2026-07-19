# TML Flow Third-Party Notices

**Table of contents**

- [Bundled map data](#bundled-map-data)
- [Bundled Bible text](#bundled-bible-text)
  - [King James Version](#king-james-version)
  - [World English Bible](#world-english-bible)
- [Production software](#production-software)


**Last reviewed:** 2026-07-18

TML Flow is a private, unlicensed application. The `UNLICENSED` package metadata does not change the licenses or public-domain status of the third-party software and data listed below.

## Bundled map data

The offline world map uses the following Natural Earth 1:50m GeoJSON-derived datasets in `public/maps/`:

- countries
- lakes
- rivers

Natural Earth states that all of its raster and vector map data is in the public domain. Suggested attribution: "Made with Natural Earth."

Source and terms: <https://www.naturalearthdata.com/about/terms-of-use/>

Natural Earth boundaries and names are contextual cartographic data. They are not authoritative statements about disputed borders, sovereignty, or current political status.

## Bundled Bible text

### King James Version

The bundled KJV text is generated from the standardized 1769 JSON data published by `farskipper/kjv`:

<https://github.com/farskipper/kjv>

That repository identifies the text as public domain and distributes its data under the Unlicense. eBible.org likewise identifies the 1769 KJV as public domain outside the United Kingdom. Crown-prerogative and letters-patent restrictions may apply to publication or import in the United Kingdom.

Reference: <https://ebible.org/bible/details.php?id=eng-kjv2006>

### World English Bible

The bundled WEB text is generated from JSON published by `TehShrike/world-english-bible`:

<https://github.com/TehShrike/world-english-bible>

The World English Bible is dedicated to the public domain. "World English Bible" is a trademark of eBible.org and may be used to identify faithful copies of the translation. TML Flow concatenates source verse segments and normalizes whitespace; it does not intentionally revise the translation.

Source status and trademark terms: <https://ebible.org/details.php?id=eng-web>

The ESV is not bundled. Selecting ESV uses Crossway's external API and therefore requires network access and remains subject to the API provider's terms.

## Production software

The packaged application includes license files from its production dependencies. The principal runtime projects are:

| Project | Version in 0.9.0-beta.1 | License |
|---|---:|---|
| Electron | 42.3.0 | MIT; bundled Chromium notices also apply |
| React and React DOM | 19.2.5 | MIT |
| React Flow (`@xyflow/react`) | 12.10.2 | MIT |
| PDF.js (`pdfjs-dist`) | 5.7.284 | Apache-2.0 |
| Zustand | 4.5.7 | MIT |
| D3 runtime modules used by React Flow | 3.x | ISC |
| `@napi-rs/canvas` | 0.1.100 | MIT |

Electron distributions include `LICENSE.electron.txt` and `LICENSES.chromium.html`. Other dependency license files are retained in the packaged `app.asar` under their respective `node_modules` directories.

This notice is informational and is not legal advice. Before broader geographic distribution, the release owner should review jurisdiction-specific Bible-text, trademark, and software-license obligations.
