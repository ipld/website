
!!!

The website source repo has **moved**.

We're now building the website out sources of the [ipld/ipld](https://github.com/ipld/ipld/) meta-repo,
where it's handled in a unified way with all the docs and specs content that needs the publishing!

All documentation, fixtures, specifications, and web content is now gathered into that repo.
Please update your links, and direct new contributions there.

The website is still published at https://ipld.io/ , as before.

!!!

---

# IPLD website

[![](https://img.shields.io/badge/made%20by-Protocol%20Labs-blue.svg?style=flat-square)](http://ipn.io)
[![](https://img.shields.io/badge/project-IPLD-blue.svg?style=flat-square)](http://github.com/ipld/ipld)
[![](https://img.shields.io/badge/freenode-%23ipfs-blue.svg?style=flat-square)](http://webchat.freenode.net/?channels=%23ipfs)
[![standard-readme compliant](https://img.shields.io/badge/standard--readme-OK-green.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)
[![build status](https://img.shields.io/circleci/project/github/ipld/website/master.svg?style=flat-square)](https://circleci.com/gh/ipld/website)


![ipld logo animation](https://cloud.githubusercontent.com/assets/58871/26447582/074ed6cc-4141-11e7-9d4d-a28a58597772.gif)

> Official website for IPLD http://ipld.io

This repository contains the source code for the IPLD website available at http://ipld.io

This project builds out a static site to explain IPLD, ready for deployment on ipfs. It uses [`hugo`](https://github.com/gohugoio/hugo) to glue the html together. It provides an informative, public-facing website. The most important things are the words, concepts and links it presents.

The implementation aims to progressively enhance the content. The styling is done with [tachyons](http://tachyons.io/) to keep things light. The animations are done in SVG and CSS. There is very little JavaScript and the interactive elements that use JS should degrade gracefully on browsers without it.

## Install

```sh
git clone https://github.com/ipld/website
```

We use `make` to build the site, and `npm` to fetch our build tool dependencies. Running `make` or `make dev` will fetch the dependencies from `npm` for you the first time you run it.

If you have any problems with building the site, try removing your local `node_module` director and re-run `make`.

## Usage

To work on the site locally with a hot-reloading dev site served at http://localhost:1313/

```sh
make dev
```

When you submit a PR, the site will be built and pinned on IPFS Cluster and a preview link will be added as PR status item.

When the `master` branch changes, site will be built, pinned to cluster, and the [DNSlink](https://docs.ipfs.io/guides/concepts/dnslink/) for https://ipld.io will be updated with the latest hash.

To deploy the site to https://ipld.io, run:

```sh
# Build out the optimised site to ./public, where you can check it locally.
make

# Add the site to your local ipfs, you can check it via /ipfs/<hash>
make deploy

# Save your dnsimple api token as auth.token
cat "<api token here>" > auth.token

# Update the dns record for ipld.io to point to the new ipfs hash.
make publish-to-domain
```

The following commands are available:

### `make`

Build the optimised site to the `./public` dir

### `make dev`

Start a hot-reloading dev server on http://localhost:1313 _(requires `hugo` on your `PATH`)_

### `make serve`

Preview the production ready site at http://localhost:1313 _(requires `hugo` on your `PATH`)_

### `make minfy`

Optimise all the things!

### `make deploy`

Build the site in the `public` dir and add to `ipfs` _(requires `hugo` & `ipfs` on your `PATH`)_

### `make publish-to-domain` :rocket:

Update the DNS record for `libp2p.io`.  _(requires an `auto.token` file to be saved in the project root.)_

If you'd like to update the dnslink TXT record for another domain, pass `DOMAIN=<your domain here>` like so:

```sh
make publish-to-domain DOMAIN=tableflip.io
```

---

See the `Makefile` for the full list or run `make help` in the project root. You can pass the env var `DEBUG=true` to increase the verbosity of your chosen command.

## Dependencies

* `Node.js` and `npm` for build tools
* `ipfs` to add to IPFS during `make deploy`
* `dnslink-deploy` to update dns `make publish-to-domain`

## Contribute

Please do! Check out [the issues](https://github.com/ipld/website/issues), or open a PR!

Check out our [contributing document](https://github.com/ipld/ipld/blob/master/contributing.md) for more information on how we work, and about contributing in general. Please be aware that all interactions related to IPLD are subject to the IPFS [Code of Conduct](https://github.com/ipfs/community/blob/master/code-of-conduct.md).

Small note: If editing the README, please conform to the [standard-readme](https://github.com/RichardLitt/standard-readme) specification.

## License

[MIT](LICENSE) © 2016 Protocol Labs Inc.
