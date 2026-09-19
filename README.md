# Playlist style dashboard

An interactive dashboard that reads `playlist.xlsx` in the browser every time the page opens
(and every 45 seconds while it stays open). Edit the Excel file, publish it, and the dashboard follows.
Nothing else needs rebuilding.

## Files

| File | Purpose |
|---|---|
| `index.html` | The dashboard (all code and styling in one file) |
| `playlist.xlsx` | Your data. This is the only file you edit |
| `vendor/xlsx.mini.min.js` | SheetJS, the library that reads Excel files (Apache-2.0, licence included) |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are |

## Publish on GitHub Pages

1. Create a repository on GitHub and upload these files at the root of the repository.
2. Open **Settings > Pages**, set **Source** to *Deploy from a branch*, choose `main` and `/ (root)`, then save.
3. After about a minute the dashboard is live at `https://<your-user>.github.io/<repository>/`.

To update it, replace `playlist.xlsx` in the repository (**Add file > Upload files** on github.com,
or commit it with git). GitHub takes about a minute to publish the new version. An open dashboard then
picks it up within 45 seconds and redraws, keeping your current selection.

The Excel file is downloadable by anyone who can open the site. Keep the repository private
(GitHub Pages on private repositories needs a paid plan) if the playlist should not be public.

## The Excel file

**Sheet `Tracks`** (or the first sheet if there is no sheet with that name)

| Column | Notes |
|---|---|
| Title | Required |
| Artist | Required. For a collaboration, the first artist listed is the main one |
| Album | Optional |
| Style | Optional. Dropdown of the styles in the `Styles` sheet; you can also type a new one |
| Certainty | Optional: `Confident`, `Likely` or `Best guess` (best guesses show as hollow squares). Empty means Confident |

- A plain export with no header row also works: columns are read as Title, Artist, Album, Style, Certainty.
- **Rows with no Style** are filled in from what the dashboard already knows: the same track, then other tracks
  by the same artist. These are labelled "inferred". Otherwise they show as **Unclassified** (grey) with a
  notice, and stay outside the diversity measures until you give them a Style.
- After correcting a style by hand, clear its Certainty cell so it stops showing as a best guess.

**Sheet `Styles`** (optional) defines the styles and families: `Style`, `Family`, `Description`, and an optional
`Color` as a hex code such as `#E0187F`. To add a style, add a row here, or just type a new name in the Style
column of `Tracks`: it appears right away under the family "Other" until you give it a family in `Styles`.

## Options

Add these to the address: `?file=other.xlsx` to read another file, `?poll=20` to check every 20 seconds.

Click **Open an Excel file** (or drop a file on the page) to preview a local file before publishing it.

## Trying it on your computer

Browsers do not let a page opened from disk read the file next to it. Run this in the folder,
then open http://localhost:8000

    python -m http.server 8000
