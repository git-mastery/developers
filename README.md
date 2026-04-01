# Developer Documentation

Developer documentation about all things Git-Mastery

This repository contains the source for the Git-Mastery developer docs site, built with Jekyll and the Just the Docs theme.

## Prerequisites

Install the following tools first:

- Ruby 3.2.2

  | Note: the installation commands below are for macOS using Homebrew only. Adjust as needed for your OS and package manager.

  ```bash
  brew install rbenv ruby-build
  rbenv install 3.2.2
  echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
  source ~/.zshrc
  rbenv global 3.2.2
  ruby -v
  ```

- Bundler

  ```bash
  gem install bundler
  ```

## Run locally

From the repository root:

1. Install dependencies:

   ```bash
   bundle install
   ```

2. Start the local docs server:

   ```bash
   bundle exec jekyll serve --livereload
   ```

3. Open the site at:

   ```text
   http://127.0.0.1:4000/developers/
   ```

Note: this repository uses `baseurl: "/developers"`, so the local path includes `/developers/`.

## Build for production

To generate a static build in `_site/`:

```bash
bundle exec jekyll build
```

## Authoring notes

- Add or edit docs in `docs/`.
- Use frontmatter fields like `title`, `parent`, and `nav_order` so pages appear correctly in navigation.
- Keep links and examples consistent with the current Git-Mastery workflows.

## Troubleshooting

- `bundle: command not found`: install Bundler with `gem install bundler`.
- Shell still reports wrong Ruby version: run `rbenv version` to confirm 3.2.2 is active; if not, run `rbenv global 3.2.2` and restart the terminal.
- Port `4000` already in use: run `bundle exec jekyll serve --port 4001`.
- Styling or content not updating: restart `jekyll serve` and hard refresh your browser.
