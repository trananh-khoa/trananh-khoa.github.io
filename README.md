# trananh-khoa.github.io <!-- omit in toc -->

![Deployment](https://github.com/trananh-khoa/trananh-khoa.github.io-source/workflows/cd/badge.svg)
![Integration](https://github.com/trananh-khoa/trananh-khoa.github.io-source/workflows/ci/badge.svg)

This repository [trananh-khoa.github.io](https://github.com/trananh-khoa/trananh-khoa.github.io) is a Github Actions `external_repository` deployment target for [trananh-khoa.github.io-source](https://github.com/trananh-khoa/trananh-khoa.github.io-source). The build results are hosted on Github Pages: [https://trananh-khoa.github.io](https://trananh-khoa.github.io).

**NOTE**: This `README.md` is located at `/static/README.md` in [trananh-khoa.github.io-source](https://github.com/trananh-khoa/trananh-khoa.github.io-source). Some additional notes about the `static` directory are:
- **This directory is not required, you can delete it if you don't want to use it.**
- This directory contains your static files.
- Each file inside this directory is mapped to `/`.
- Thus you'd want to delete this README.md before deploying to production.
- Example: `/static/robots.txt` is mapped as `/robots.txt`.
- More information about the usage of this directory in [the documentation](https://nuxtjs.org/guide/assets#static).

**NOTE**: The following information is primarily relevant for [trananh-khoa.github.io-source](https://github.com/trananh-khoa/trananh-khoa.github.io-source).

## Table of Contents <!-- omit in toc -->
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
    - [Install Homebrew package manager for MacOS](#install-homebrew-package-manager-for-macos)
    - [Install git and yarn packages](#install-git-and-yarn-packages)
  - [Installing](#installing)
    - [Clone source repository](#clone-source-repository)
    - [Install necessary packages and dependencies](#install-necessary-packages-and-dependencies)
- [Development and Production](#development-and-production)
  - [VSCode Setup](#vscode-setup)
  - [Nuxt Commands](#nuxt-commands)
  - [Source Control](#source-control)
- [Deployment](#deployment)
  - [Generation of Deployment Keys](#generation-of-deployment-keys)
- [Built With](#built-with)
- [Authors](#authors)

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

What things you need to install the software and how to install them (on MacOS).

#### Install Homebrew package manager for MacOS

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

#### Install git and yarn packages

```bash
brew install git yarn
```

### Installing

A step by step series of examples that tell you how to get a development environment running.

#### Clone source repository

```bash
git clone https://github.com/trananh-khoa/trananh-khoa.github.io-source.git
```

#### Install necessary packages and dependencies

```
cd trananh-khoa.github.io-source
yarn install
```

## Development and Production

### VSCode Setup

Include the following as workspace settings:

```json
{
    "css.validate": false,
    "stylelint.enable": true,
    "vetur.validation.style": false
}
```

Commmands used for development and production.
### Nuxt Commands

```bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn dev

# build for production and launch server
$ yarn build
$ yarn start

# generate static project
$ yarn generate
```

For detailed explanation on how things work, check out [Nuxt.js docs](https://nuxtjs.org).

### Source Control

All commits and pull requests must adhere to the `semantic-pull-request` guidelines outlined in [semantic-pull-requests](https://github.com/zeke/semantic-pull-requests).

Branch naming should adhere to the following convention: `<type>/<name>`, where the `type` is one of the following listed in [conventional-commits](https://www.conventionalcommits.org/en/v1.0.0/#summary).

## Deployment

Deployment setup on Github Pages using Github Actions is achieved using the [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages) action.

### Generation of Deployment Keys

```bash
ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""
```

Copy the contents of `gh-pages.pub (public key)` to `Deploy Keys` in the [trananh-khoa.github.io](https://github.com/trananh-khoa/trananh-khoa.github.io) repository settings.

Copy the contents of `gh-pages (private key)` to `Secrets` as `ACTIONS_DEPLOY_KEY` in the [trananh-khoa.github.io-source](https://github.com/trananh-khoa/trananh-khoa.github.io-source) repository settings.

Finally, include the following Github Actions workflow to [trananh-khoa.github.io-source](https://github.com/trananh-khoa/trananh-khoa.github.io-source):

```yml
name: cd

on: [push, pull_request]

jobs:
  cd:
    runs-on: ${{ matrix.os }}

    strategy:
      matrix:
        os: [ubuntu-latest]
        node: [14]

    steps:
      - name: Checkout
        uses: actions/checkout@master

      - name: Setup node env
        uses: actions/setup-node@v2.1.2
        with:
          node-version: ${{ matrix.node }}

      - name: Install dependencies
        run: yarn

      - name: Generate
        run: yarn run generate

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
          publish_dir: ./dist
          external_repository: trananh-khoa/trananh-khoa.github.io
```

## Built With

* [NuxtJS](https://nuxtjs.org/) - The Intuitive Vue Framework used
* [windicss](https://github.com/windicss/windicss) - The utility-first CSS framework used
* [semantic-pull-requests](https://github.com/zeke/semantic-pull-requests) - GitHub status check used to ensure semantic pull requests and commits
* [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages) - GitHub Action used for deployment

## Authors

* **Khoa Tran** - *Owner* - [trananh-khoa](https://github.com/trananh-khoa)