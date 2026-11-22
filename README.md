# crestbrowser.com

The website for [Crest](https://github.com/crest-browser/Crest), served at
[crestbrowser.com](https://crestbrowser.com).

| Folder | What it holds |
| --- | --- |
| `site/` | The pages, styles and assets, published as they are |
| `scripts/` | The generators for the release notes and credits pages |
| `data/credits.json` | The people and supporters the credits page lists |
| `marketing/` | The App Store listing text |
| `tests/` | Checks the deploy runs before it publishes |

## How it deploys

`.github/workflows/deploy.yml` runs on every push to `main`, and Crest starts
it after each release. It checks out Crest, then:

1. exports Crest's release history and renders `/releases/` from it,
2. builds `/credits/` from `data/credits.json`, GitHub's contributor list and Patreon,
3. builds the Help Center from Crest's `HelpCenter/` into `/guides/`,
4. publishes `site/` to GitHub Pages.

Release notes and Help Center articles are written in Crest, not here.

## Preview

```sh
git clone https://github.com/crest-browser/Crest.git ../Crest
python3 ../Crest/Scripts/export-release-history.py --repository ../Crest --output build/release-history.json
python3 scripts/build-release-pages.py --history build/release-history.json
python3 scripts/build-credits-page.py
python3 -m http.server --directory site 4818
```

## License

The site's code is available under the [Mozilla Public License 2.0](LICENSE).
The Crest name, icon and visual identity are not; see [NOTICE](NOTICE).
