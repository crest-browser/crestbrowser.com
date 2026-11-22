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

## Credits

`/credits/` lists the developer, the community, the code contributors GitHub
lists for Crest without bots, any other roles in `data/credits.json`, and the
supporters by tier, each marked with the service they give through. When the
repository secret `PATREON_ACCESS_TOKEN` holds the Patreon campaign's creator
access token, its active patrons are listed at the highest tier their pledge
reaches, by their first names and the initial of their last.

- A file entry whose `patreon` holds a patron's member ID gives them the name
  and link they asked for, without putting their Patreon name in the file, and
  drops out once that membership ends.
- Ko-fi offers no way to list members, so Ko-fi supporters are file entries
  marked `"source": "ko-fi"`.
- Anyone `supporters.hidden` names, by the name the page shows, their Patreon
  name or their member ID, never appears. `tests/test_credits_page.py` holds
  that, and the deploy runs it before it publishes.

The page saves each avatar and site icon beside it, so it loads nothing from
other sites, and `credits.json` beside it lists the same people.

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
