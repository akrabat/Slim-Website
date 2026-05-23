# The Slim Framework Website

This is the repository for the Slim website ([slimframework.com][slimframework-url]).

## Want to contribute?

If you spot any errors, typos, or missing information, please submit [a pull request][pr-url].

## The documentation style guide

Unless otherwise stated, the documentation follows [the AP Stylebook][ap-stylebook-url].

## Run locally to test your changes

When making changes, it's helpful to run the site locally so you can see the changes or
read your new works within the context of the site.

### Developing with Docker

If you would rather not install Ruby and the gem toolchain on your machine,
you can run the site in a container instead. All you need is [Docker]
[docker-url].

From the repository root, start the site:

```bash
$ docker compose up
```

The first run installs the gems into a cached volume and takes a few minutes;
later runs start in seconds. Once Jekyll reports `Server running...`, browse
to http://localhost:4000.

The site rebuilds automatically as you edit files, and the browser refreshes
via LiveReload. Press `Ctrl-C` to stop the server; run `docker compose down`
afterwards if you also want to remove the container.

To run a one-off command in the same environment, such as a production build,
use `docker compose run`:

```bash
$ docker compose run --rm jekyll bundle exec jekyll build
```


### Developing directly on your local computer

You can also run this site directly on your local computer to test your changes.

Firstly, ensure you have Ruby installed. If you are a Microsoft Windows user, you
need to make sure you have [Ruby Devkit Installed (MSYS2)](https://rubyinstaller.org/add-ons/devkit.html).


```bash
$ sudo gem install bundler
$ bundle install
```

Now, run the local [Jekyll][jekyll-url] installation:

```bash
$ bundle exec jekyll serve
```

Then, browse to http://localhost:4000 in your browser of choice.

## Editing the site's CSS

The CSS uses Less and is managed by `grunt`.

Install the tool chain:

```bash
$ npm install -g grunt-cli
$ npm install
```

To change any CSS, edit the appropriate files in `assets\less` and then run:

```bash
grunt
```

You can also run `grunt watch` to automatically rebuild when you make CSS changes.

If you would rather not install NodeJS and grunt on your machine, the
`docker-compose.yml` includes a `css` service that runs the same toolchain
in a container. To start `grunt watch`:

```bash
$ docker compose up css
```

For a one-off build, run:

```bash
$ docker compose run --rm css npx grunt
```

The first run installs the npm packages into a cached volume; later runs
start almost instantly.

### Build instructions for deployment

```bash
bundle exec jekyll build
```

Merging a pull request to the `gh-pages` branch will trigger a GitHub Action
that builds and deploys the site to GitHub Pages.


### Update the Algolia search database

Ensure you set the environment variable `ALGOLIA_API_KEY` before running these commands. 
See [algolia docs](https://community.algolia.com/jekyll-algolia/getting-started.html) for more information.

```bash
bundle install
bundle exec jekyll algolia
```

[ap-stylebook-url]: https://www.apstylebook.com
[docker-url]: https://docs.docker.com/get-docker/
[jekyll-url]: https://jekyllrb.com
[pr-url]: https://github.com/slimphp/Slim-Website/compare
[slimframework-url]: https://slimframework.com
