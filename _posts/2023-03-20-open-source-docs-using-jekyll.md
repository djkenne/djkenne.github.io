---
layout: post
title: "Open source your documentation using Jekyll"
date: 2023-03-20 10:00:00 -0500
categories: Tutorial
tags: jekyll documentation github
image:
 path: /assets/img/headers/octojekyll.png
---

Jekyll is a static site generator that takes text written in your favourite markup language and uses layouts to create beautiful static websites. It can be used for a documentation site, a blog, an event site, or even a simple business site (e.g. one you might link to from a business card). It's fast, secure, easy and open source!

I have been using Jekyll since 2016 and heavily customized a theme called [Mediator](https://jekyllthemes.io/theme/mediator). However, I was recently inspired by [Techno Tim](https://technotim.live) and decided to switch to the [Chirpy theme](http://jekyllthemes.org/themes/jekyll-theme-chirpy/) to maintain my open source technical documentation.

So let's get started! We'll install Jekyll and its dependencies, configure a new site based on the Chirpy theme and create some pages using Markdown. Finally, we'll host it for free using [GitHub Pages](https://pages.github.com). If you'd prefer to not host it in the cloud, you could alternatively self-host this in your own infrastructure.

If you like the project, please be sure to ⭐ the [Jekyll repo on GitHub](https://github.com/jekyll/jekyll) and the [Chirpy theme repo](https://github.com/cotes2020/jekyll-theme-chirpy).

## Prerequisites
### Install Dependencies (macOS)
Install [Homebrew](https://brew.sh):
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Install the latest version of Ruby supported by Jekyll:
```bash
brew install chruby ruby-install xz
ruby-install ruby 3.1.3
```

Configure your shell to automatically use `chruby`:
```bash
echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc
echo "chruby ruby-3.1.3" >> ~/.zshrc # run 'chruby' to see actual version
```
Note: If you're using Bash, replace `.zshrc` with `.bash_profile`.

Quit and relaunch Terminal, and verify everything is working:
```bash
❯ ruby -v
ruby 3.1.3p185 (2022-11-24 revision 1a6b16756e) [arm64-darwin22]
```

Full installation instructions for Jekyll on macOS are [available here](https://jekyllrb.com/docs/installation/macos/).


### Install Dependencies (Linux)

```bash
sudo apt update
sudo apt install ruby-full build-essential zlib1g-dev git
```

Avoid installing RubyGems packages (called gems) as the root user. Instead, set up a gem installation directory for your user account. The following commands will add environment variables to your `~/.zshrc` file to configure the gem installation path:
```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.zshrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.zshrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```
Note: If you're using Bash, replace `.zshrc` with `.bash_profile`.

Full installation instructions for Jekyll on Linux are [available here](https://jekyllrb.com/docs/installation/other-linux/).

## Installation
### Install Jekyll and Bundler

```bash
gem install jekyll bundler
```

That's it! You're ready to start using Jekyll.

### Create a new site based on the Chirpy theme

Visit <https://chirpy.cotes.page/posts/getting-started/#option-1-using-the-chirpy-starter>

After creating a site based on the template, clone your repository:
```bash
git clone git@<YOUR-USERNAME>/<YOUR-REPO-NAME>.git
```

Before running a local server for the first time:
```bash
cd repo-name
bundle
```

Make changes to your site as you see fit, and then commit and push these to Git:
```bash
git add .
git commit -m "My awesome changes"
git push
```

## Building and Testing
### Local Testing

Serve the site for local testing from a terminal:
```bash
bundle exec jekyll serve
```

Or serve the site for local testing using Docker:
```bash
docker run -it --rm \
    --volume="$PWD:/srv/jekyll" \
    -p 4000:4000 jekyll/jekyll \
    jekyll serve
```
Open <http://localhost:4000> in a browser of your choice.

### Building Site Manually
Build the site in production mode:
```bash
JEKYLL_ENV=production bundle exec jekyll build
```

This will output the production site to `_site`.

### Building Site with GitHub

This site already works with GitHub Actions. See the section below on deploying using GitHub Pages.

### Building Site with Docker (Optional)

Create a `Dockerfile` with the following contents:
```Dockerfile
FROM nginx:stable-alpine
COPY _site /usr/share/nginx/html
```

Build site in production mode:
```bash
JEKYLL_ENV=production bundle exec jekyll build
```

Then build your image:

`docker build .`

## Usage

### Naming Conventions

Jekyll uses a naming convention for [pages](https://jekyllrb.com/docs/pages/) and [posts](https://jekyllrb.com/docs/posts/).

Create a file in `_posts` with the format:
```file
YEAR-MONTH-DAY-title.md
```

For example:

```file
2022-12-31-new-years-eve-is-awesome.md
2023-01-07-cool-thing-i-learned.md
```

> Jekyll can delay posts which have the date/time set for a point in the future determined by the "front matter" section at the top of your post file. Check the date & time as well as time zone if you don't see a post appear shortly after re-build.
{: .prompt-tip }

### Local Linking of Files

Image from asset:

```markdown
... which is shown in the screenshot below:
![A screenshot](/assets/screenshot.jpg)
```

Linking to a file

```markdown
... you can [download the PDF](/assets/diagram.pdf) here.
```

See more post formatting rules on the [Jekyll site](https://jekyllrb.com/docs/posts/)

### Markdown Examples

If you need some help with markdown, check out the [markdown cheat sheet](https://www.markdownguide.org/cheat-sheet/)

For more neat syntax for the Chirpy theme check their demo page on making posts <https://chirpy.cotes.page/posts/write-a-new-post/>

## Deployment
### Host Your Site For Free Using GitHub Pages

Sites on [GitHub Pages](https://pages.github.com) are powered by Jekyll behind the scenes, so if you’re looking for a zero-hassle, zero-cost solution, GitHub Pages are a great way to [host your Jekyll-powered website for free](https://jekyllrb.com/docs/github-pages/).

* If you’re on the GitHub Free plan, keep your site repository public.
* If you have committed Gemfile.lock to the repository, and your local machine is not running Linux, go the the root of your site and update the platform list of the lock-file:
```bash
$ bundle lock --add-platform x86_64-linux
```

Next, configure the Pages service.

Browse to your repository on GitHub. Select the tab Settings, then click Pages in the left navigation bar. Then, in the Source section (under Build and deployment), select GitHub Actions from the dropdown menu.

Push any commits to GitHub to trigger the Actions workflow. In the Actions tab of your repository, you should see the workflow Build and Deploy running. Once the build is complete and successful, the site will be deployed automatically.

At this point, you can go to the URL indicated by GitHub to access your site.

## References

[Jekyll Documentation](https://jekyllrb.com/docs/)
