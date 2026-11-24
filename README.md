<h1 align="center">🐟 Koi-Code-Aquarium</h1>

<p align="center">
  <b>An animated, high-resolution koi aquarium for your GitHub profile README.</b><br>
  Every fish is one of your programming languages, and it grows as you add repositories.
</p>

<p align="center">
  <a href="LICENSE"><img alt="License: Apache-2.0" src="https://img.shields.io/badge/license-Apache--2.0-blue?style=flat-square"></a>
  <a href="https://github.com/azizulabedinazmi/Koi-Code-Aquarium/releases"><img alt="Release" src="https://img.shields.io/github/v/tag/azizulabedinazmi/Koi-Code-Aquarium?label=release&style=flat-square&color=0ea5e9"></a>
  <a href="https://github.com/azizulabedinazmi/Koi-Code-Aquarium/stargazers"><img alt="Stars" src="https://img.shields.io/github/stars/azizulabedinazmi/Koi-Code-Aquarium?style=flat-square&color=facc15"></a>
  <a href="CONTRIBUTING.md"><img alt="PRs welcome" src="https://img.shields.io/badge/PRs-welcome-22c55e?style=flat-square"></a>
  <img alt="Zero dependencies" src="https://img.shields.io/badge/dependencies-0-8b5cf6?style=flat-square">
  <img alt="GitHub Action" src="https://img.shields.io/badge/GitHub%20Action-ready-2088ff?style=flat-square&logo=githubactions&logoColor=white">
</p>

<p align="center">
  <img src="assets/demo.svg" width="100%" alt="Koi-Code-Aquarium demo: animated pixel-art koi, one per programming language, swimming in a coding-themed pond on a GitHub profile README">
</p>

<p align="center">
  <a href="#-quick-start-github-action">Quick start</a> ·
  <a href="#-features">Features</a> ·
  <a href="#%EF%B8%8F-options">Options</a> ·
  <a href="#-how-it-works">How it works</a> ·
  <a href="#-faq">FAQ</a> ·
  <a href="#-contributing">Contributing</a>
</p>

> ⭐ **If your pond makes you smile, star the repo.** Stars help other developers find it.

---

## ✨ Features

- **🐠 One koi per language.** Each fish is a [git-koi](https://github.com/yalongwastaken/git-koi) pixel-art koi spotted in that language's official GitHub colour: Python blue, Rust orange, JavaScript yellow, and so on.
- **📈 Grows with your repos.** A koi's length is proportional to how many public repositories use its language. Ship more Python, and your Python koi gets bigger.
- **🔍 Truly high resolution.** Every pixel becomes a vector square, so the art stays sharp on retina screens and at any zoom.
- **💻 Coding theme.** A terminal bar, a dark pond with a grid, faint code written in *your* languages, glowing circuit traces, rising `{ }` `</>` bubbles, and a `languages.json` legend.
- **🖼️ Custom wallpaper.** Use any image as the pond background.
- **🎞️ Animated on GitHub.** It uses SVG's built-in animation tags (SMIL), which keep playing inside a README `<img>`. The fish swim head-first: a wave runs down each body from head to tail and the side fins paddle.
- **⚡ Zero dependencies.** One Node.js file. Run it as a GitHub Action, with `npx`, or locally.

## 🚀 Quick start (GitHub Action)

**1.** In your profile repository (the one named after your username), create `.github/workflows/koi-aquarium.yml`:

```yaml
name: Koi aquarium
on:
  schedule: [{ cron: "20 0 * * *" }] # nightly
  workflow_dispatch:
permissions:
  contents: write
jobs:
  aquarium:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: azizulabedinazmi/Koi-Code-Aquarium@v1
        with:
          out: koi/aquarium.svg
      - run: |
          git add koi/aquarium.svg
          git diff --cached --quiet && exit 0
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git commit -m "Update koi aquarium" && git push
```

**2.** Add it to your `README.md`:

```html
<img src="koi/aquarium.svg" width="100%" alt="My koi aquarium">
```

**3.** Run the workflow once from the **Actions** tab. 🎉

There's a fuller version in [`examples/workflow.yml`](examples/workflow.yml).

## 💻 Run it locally or with npx

You need:
- Node.js 18 or newer
- the [GitHub CLI](https://cli.github.com), signed in with `gh auth login`
- the [git-koi 1.0.2](https://github.com/yalongwastaken/git-koi/releases/tag/v1.0.2) binary

```bash
npx github:azizulabedinazmi/Koi-Code-Aquarium --user <your-login> --git-koi /path/to/git-koi
```

Or clone the repo and run:

```bash
node koi-aquarium.mjs --user <your-login> --git-koi /path/to/git-koi --out koi/aquarium.svg
```

## ⚙️ Options

| Action input | CLI flag | Default | What it does |
| --- | --- | --- | --- |
| `user` | `--user` | repository owner | Account to draw |
| `out` | `--out` | `koi/aquarium.svg` | Output file |
| `max-fish` | `--max-fish` | `12` | Most languages to show, `1`–`20` |
| `theme` | `--theme` | `code` | `code` adds the grid, code, circuits and bubbles; `plain` leaves just the water |
| `wallpaper` | `--wallpaper` | none | Pond background: `.jpg` `.png` `.webp` `.gif` `.svg` |
| `title` | `--title` | `<user>'s code pond` | Text in the terminal bar |
| — | `--include-forks` | off | Count forked repositories too |
| — | `--git-koi` | auto-detected | Path to the git-koi binary |
| `token` | — | built-in token | Keep the default; only public data is read |

## 🧠 How it works

1. **Count languages.** The GitHub GraphQL API lists your public, non-fork repositories. Each repository counts once, for its primary language.
2. **Collect activity.** `git-koi collect` reads your public activity once. That sets each koi's growth and pose.
3. **One koi per language.** For each language, `git-koi render` draws a koi spotted only in that colour. Pale colours get git-koi's black koi so the spots stay visible.
4. **Vectorise.** The koi PNG is decoded with Node's built-in zlib, and same-coloured pixels are merged into as few vector rectangles as possible.
5. **Size.** Each koi's length is `84 + 176 × (its repos ÷ your top language's repos)`, so your top language is the biggest fish.
6. **Swim.** Each koi follows its own smooth loop with `animateMotion`, always facing the way it swims. The koi is cut into a chain of body segments that bend one after another, so a wave travels from a gently swaying head down to the tail fan, while the side fins paddle in turn.

The same data always draws the same pond, so the file only changes when your languages or activity change.

## 🖼️ Wallpaper tips

- The image is embedded inside the SVG, because GitHub won't load images linked from an SVG. Keep it under 1 MB.
- About **2000 × 725 pixels** matches the pond's shape. Other shapes are cropped to fill it.
- The image is darkened slightly so the fish stand out. Use `theme: plain` to show it without decorations.

## ❓ FAQ

**How do I add an animated aquarium to my GitHub profile README?**
Follow the [quick start](#-quick-start-github-action). It takes about two minutes.

**Does the animation work on GitHub?**
Yes. It uses SMIL animation, which GitHub keeps playing inside README images. CSS animations would freeze there.

**Which languages appear?**
The primary languages of your public, non-fork repositories, biggest first, up to `max-fish`.

**Is my token safe?**
The Action uses the built-in `GITHUB_TOKEN`, which only reaches the repository running the workflow, and only public data is read. See [SECURITY.md](SECURITY.md).

**Can I use my own background?**
Yes. Use the `wallpaper` input or the `--wallpaper` flag.

## 🌊 Showcase

Using Koi-Code-Aquarium? **Add your profile to [SHOWCASE.md](SHOWCASE.md)** with a one-line pull request. You can also share it with **#KoiCodeAquarium**.

## 🤝 Contributing

Contributions of every size are welcome: new language snippets, themes, fixes and docs.
Start with [CONTRIBUTING.md](CONTRIBUTING.md), and look for issues labelled **good first issue**.

<a href="https://github.com/azizulabedinazmi/Koi-Code-Aquarium/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=azizulabedinazmi/Koi-Code-Aquarium" alt="Contributors">
</a>

## 🙏 Credits

- **Koi art and activity model:** [git-koi](https://github.com/yalongwastaken/git-koi) by Anthony Yalong (MIT). Koi-Code-Aquarium runs the official git-koi release and doesn't include its code.
- **Inspiration:** [git-koi-aquarium](https://github.com/reginaldalfret/git-koi-aquarium) (MIT).

## 📄 License

[Apache License 2.0](LICENSE) © 2026 [Azizul Abedin Azmi](https://github.com/azizulabedinazmi).
If you redistribute or build on this project, keep the [NOTICE](NOTICE) file and credit the original.

<p align="center"><sub>Made with 🐟 and code. If you like it, a ⭐ goes a long way.</sub></p>
