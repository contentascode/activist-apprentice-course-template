[![Travis](https://img.shields.io/travis/contentascode/activist-apprentice.svg)](https://travis-ci.org/contentascode/activist-apprentice) [![npm](https://img.shields.io/npm/dt/activist-apprentice.svg)](https://www.npmjs.com/package/activist-apprentice) [![GitHub license](https://img.shields.io/github/license/contentascode/activist-apprentice.svg)](https://github.com/contentascode/activist-apprentice/blob/master/LICENSE)

# Activist Apprentice Content as Code CLI tool

Activist Apprentice is...

Command line tool to manage content workflows for the Apprentice project. It uses the content as code framework.

- [Features](#features)
- [Install](#install)
- [Usage](#usage)
- [Links](#links)

## Features

A quick walk through...

[![IMAGE ALT TEXT HERE]()]()

## Install

Prerequisites:
 - [node v6](https://nodejs.org/en/)
 - npm v5.3.0
   -  `npm i -g npm`
 - docsmith v0.6.0:
   -  `npm i -g docsmith`
 - pandoc 1.19
   - `brew install pandoc`
   - `apt-get install pandoc`

You can now install safetag with:
```
npm i -g activist-apprentice
```

In order to open local urls with Atom by clicking the edit link in previews you need a handler for the `atm://` local url scheme:
 - OSX: https://github.com/WizardOfOgz/atom-handler
 - Linux: https://github.com/eclemens/atom-url-handler

## Usage

```
mkdir workspace
cd workspace
apprentice init
```

This will prompt you with a few questions about configuration. The defaults should work in the majority of cases.

```
apprentice start
```

This will start the preview server and watch your local files for changes. You can open your browser at `http://localhost:8081` to browse the preview.

## Links

- Related repositories
  - `activist-apprentice-course-template` content package https://github.com/contentascode/activist-apprentice-course-template
  - Content as code https://github.com/iilab/contentascode
  - CLI Framework https://github.com/docsmith/docsmith
