# Developer Documentation

Developer documentation about all things Git-Mastery

This repository contains the source for the Git-Mastery developer docs site, built with Jekyll and the Just the Docs theme.

## Prerequisites

Install the following tools first:

- Ruby 3.2.2

   Use one of the following OS-specific options.

   macOS (Homebrew + rbenv):

   ```bash
   brew install rbenv ruby-build
   rbenv install 3.2.2
   echo 'eval "$(rbenv init - zsh)"' >> ~/.zshrc
   source ~/.zshrc
   rbenv global 3.2.2
   ruby -v
   ```

   Ubuntu/Debian (rbenv):

   ```bash
   sudo apt update
   sudo apt install -y git curl build-essential libssl-dev zlib1g-dev \
      libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev \
      libxslt1-dev libcurl4-openssl-dev libffi-dev libgdbm-dev libncurses5-dev
   curl -fsSL https://github.com/rbenv/rbenv-installer/raw/main/bin/rbenv-installer | bash
   export PATH="$HOME/.rbenv/bin:$PATH"
   eval "$(rbenv init - bash)"
   rbenv install 3.2.2
   rbenv global 3.2.2
   ruby -v
   ```

   Windows (RubyInstaller + ridk):

   1. Download and install Ruby 3.2.x from [RubyInstaller](https://rubyinstaller.org/downloads/).
   2. Choose the installer named `Ruby+Devkit 3.2.x (x64)` for most systems.
      Use `x86` only if your Windows is 32-bit.
   3. During install, keep the option to run `ridk install` enabled.
   4. Open a new PowerShell and verify:

   ```powershell
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

- `bundle: command not found`: ensure Ruby and Bundler are installed, then open a new shell and run `gem install bundler`.
- Ruby version is not 3.2.2 on macOS/Linux: confirm with `ruby -v`, then set with `rbenv global 3.2.2`.
- Ruby version is not 3.2.2 on Windows: reinstall Ruby 3.2.x from RubyInstaller and reopen PowerShell.
- Port `4000` already in use: run `bundle exec jekyll serve --port 4001`.
- Styling or content not updating: restart `jekyll serve` and hard refresh your browser.
