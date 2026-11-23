# Contributing to Koi-Code-Aquarium 🐟

Thanks for helping the pond grow! Every kind of contribution is welcome: bug reports, ideas, docs, code snippets for a new language, themes, or showing off your own aquarium.

## Quick ways to help

| Time you have | What you can do |
| --- | --- |
| 1 minute | ⭐ Star the repo, or share your aquarium with `#KoiCodeAquarium` |
| 5 minutes | Add your profile to [SHOWCASE.md](SHOWCASE.md) |
| 15 minutes | Add code snippets for a language that's missing from `SNIPPETS` in [`koi-aquarium.mjs`](koi-aquarium.mjs) |
| An evening | Pick an issue labelled [`good first issue`](../../issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22) or [`help wanted`](../../issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22) |

## Set up

You need:
- Node.js 18 or newer
- the [GitHub CLI](https://cli.github.com), signed in with `gh auth login`
- the [git-koi 1.0.2](https://github.com/yalongwastaken/git-koi/releases/tag/v1.0.2) binary for your system

```bash
git clone https://github.com/<you>/Koi-Code-Aquarium.git
cd Koi-Code-Aquarium
node koi-aquarium.mjs --user <your-login> --git-koi /path/to/git-koi --out koi/aquarium.svg
```

Open `koi/aquarium.svg` in a browser to see the result. There are no npm packages to install.

## Making a change

1. Fork the repo and create a branch: `git checkout -b feat/short-name`.
2. Keep the generator **dependency-free**, using only Node's built-in modules.
3. Keep the SVG **GitHub-safe**:
   - Animate with SMIL (`<animate>`, `<animateTransform>`, `<animateMotion>`). CSS animations freeze when GitHub shows the SVG as an `<img>`.
   - Don't use external images, fonts, or scripts. GitHub blocks them inside SVG images.
4. Check your change:
   ```bash
   node --check koi-aquarium.mjs
   node koi-aquarium.mjs --help
   node koi-aquarium.mjs --user <your-login> --git-koi /path/to/git-koi --out koi/aquarium.svg
   ```
   Then view the SVG through an `<img>` tag, not only by opening the file. That's how GitHub shows it.
5. Commit with a clear message, for example `feat: add Kotlin snippets` or `fix: fish swim backwards on dark coats`.
6. Open a pull request and fill in the template. Include a screenshot or GIF if the picture changes.

## Ideas we'd love help with

- More code snippets for more languages
- New themes, such as `retro`, `sunset` or `matrix`, behind a `--theme` option
- Fish name labels that stay readable while swimming
- Light-mode version (`prefers-color-scheme`)
- Smaller output files: smarter rectangle merging
- Tests for the PNG decoder and the head-direction detection

## Licence of contributions

By submitting a contribution, you agree it's licensed under the project's [Apache License 2.0](LICENSE), as described in section 5 of the licence.

## Code of conduct

Please be kind. This project follows the [Code of Conduct](CODE_OF_CONDUCT.md).
