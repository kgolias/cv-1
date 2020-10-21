# Konstantinos Astropekakis
## Business Intelligence / Data Warehouse Analyst at Forthnet Group

### About
<img align="right" width="200" height="299" src="https://github.com/std138001/desktop-tutorial/blob/master/profile_photo_scaled.gif">
Experienced Business Intelligence Analyst with a demonstrated history of working in the telecommunications industry. Skilled in Oracle Database, PL/SQL, MySQL, Sybase, Oracle ODI, BI Publisher and Infoview. Strong research professional. Currenltly studying in a Master of Science - MS focused in Pervasive and Mobile Computing Systems Msc from Hellenic Open University.

[mail](mailto:std138001@ac.eap.gr) [LinkedIn](https://www.linkedin.com/in/konstantinos-astropekakis-b0ba74143/)

## Table of Contents

<details>
<summary>Click to expand</summary>

- [About](#about)
- [Install](#install)
- [Usage](#usage)
  * [API](#api)
  * [Configuration Options](#configuration-options)
- [CLI Usage](#cli-usage)
- [Transforms](#transforms)
  * [CODE](#code)
  * [REMOTE](#remote)
  * [TOC](#toc)
- [Running Async transforms](#running-async-transforms)
- [🔌 Third Party Plugins](#%F0%9F%94%8C-third-party-plugins)
- [Adding Custom Transforms](#adding-custom-transforms)
- [Plugin Example](#plugin-example)
- [Other usage examples](#other-usage-examples)
- [Custom Transform Demo](#custom-transform-demo)
- [Prior Art](#prior-art)
- [License](#license)

</details>


## Install

```bash
npm install markdown-magic --save-dev
```

## Usage
<!-- ⛔️ MD-MAGIC-EXAMPLE:START (CODE:src=./examples/basic-usage.js) -->
<!-- The below code snippet is automatically added from ./examples/basic-usage.js -->
```js
import path from 'path'
import markdownMagic from 'markdown-magic'

const markdownPath = path.join(__dirname, 'README.md')
markdownMagic(markdownPath)
```
<!-- ⛔️ MD-MAGIC-EXAMPLE:END *-->


<!-- ⛔️ MD-MAGIC-EXAMPLE:START (RENDERDOCS:path=./index.js)
- Do not remove or modify this section -->
### API
```js
markdownMagic(filePath, config, callback)
```
- `filePaths` - *String or Array* - Path or glob pattern. Uses [globby patterns](https://github.com/sindresorhus/multimatch/blob/master/test.js)
- `config` - See configuration options below
- `callback` - callback to run after markdown updates
<!-- ⛔️ MD-MAGIC-EXAMPLE:END - Do not remove or modify this section -->

<!-- ⛔️ MD-MAGIC-EXAMPLE:START (RENDERDOCS:path=./lib/processFile.js)
- Do not remove or modify this section -->
### Configuration Options

- `transforms` - *object* - (optional) Custom commands to transform block contents, see transforms & custom transforms sections below.

- `outputDir` - *string* - (optional) Change output path of new content. Default behavior is replacing the original file

- `matchWord` - *string* - (optional) Comment pattern to look for & replace inner contents. Default `AUTO-GENERATED-CONTENT`

- `DEBUG` - *Boolean* - (optional) set debug flag to `true` to inspect the process
<!-- ⛔️ MD-MAGIC-EXAMPLE:END - Do not remove or modify this section -->

## CLI Usage

You can use `markdown-magic` as a CLI command. Run `markdown --help` to see all available CLI options

```bash
markdown --help
# or
md-magic
```

This is useful for adding the package quickly to your `package.json` npm scripts

CLI usage example with options

```bash
md-magic --path '**/*.md' --config ./config.file.js
```

In NPM scripts, `npm run docs` would run the markdown magic and parse all the `.md` files in the directory.

```json
"scripts": {
  "docs": "md-magic --path '**/*.md' --ignore 'node_modules'"
},
```

If you have a `markdown.config.js` file where `markdown-magic` is invoked, it will automatically use that as the configuration unless otherwise specified by `--config` flag.

<!-- ⛔️ MD-MAGIC-EXAMPLE:START (CODE:src=./markdown.config.js) -->
<!-- The below code snippet is automatically added from ./markdown.config.js -->
```js
/* CLI markdown.config.js file example */
module.exports = {
  transforms: {
    /* Match <!-- AUTO-GENERATED-CONTENT:START (LOLZ) --> */
    LOLZ(content, options) {
      return `This section was generated by the cli config markdown.config.js file`
    }
  },
  callback: function () {
    console.log('done')
  }
}
```
<!-- ⛔️ MD-MAGIC-EXAMPLE:END *-->

## Transforms

Markdown Magic comes with a couple of built in transforms for you to use or you can extend it with your own transforms. See 'Custom Transforms' below.

<!-- ⛔️ MD-MAGIC-EXAMPLE:START (RENDERDOCS:path=./lib/transforms/index.js) - Do not remove or modify this section -->
### CODE

Get code from file or URL and put in markdown

**Options:**
- `src`: The relative path to the code to pull in, or the `URL` where the raw code lives
- `syntax` (optional): Syntax will be inferred by fileType if not specified
- `header` (optional): Will add header comment to code snippet. Useful for pointing to relative source directory or adding live doc links

**Example:**
```md
<!-- AUTO-GENERATED-CONTENT:START (CODE:src=./relative/path/to/code.js) -->
This content will be dynamically replaced with code from the file
<!-- AUTO-GENERATED-CONTENT:END -->
```

Default `MATCHWORD` is `AUTO-GENERATED-CONTENT`

---

### REMOTE

Get any remote Data and put in markdown

**Options:**
- `url`: The URL of the remote content to pull in

**Example:**
```md
<!-- AUTO-GENERATED-CONTENT:START (REMOTE:url=http://url-to-raw-md-file.md) -->
This content will be dynamically replace from the remote url
<!-- AUTO-GENERATED-CONTENT:END -->
```

Default `MATCHWORD` is `AUTO-GENERATED-CONTENT`

---

### TOC

Generate table of contents from markdown file

**Options:**
- `firsth1` - *boolean* - (optional): Show first h1 of doc in table of contents. Default `false`
- `collapse` - *boolean* - (optional): Collapse the table of contents in a detail accordian. Default `false`
- `collapseText` - *string* - (optional): Text the toc accordian summary
- `excludeText` - *string* - (optional): Text to exclude in the table of contents. Default `Table of Contents`

**Example:**
```md
<!-- AUTO-GENERATED-CONTENT:START (TOC) -->
toc will be generated here
<!-- AUTO-GENERATED-CONTENT:END -->
```

Default `MATCHWORD` is `AUTO-GENERATED-CONTENT`

---
<!-- ⛔️ MD-MAGIC-EXAMPLE:END - Do not remove or modify this section -->


## Running Async transforms

Markdown magic was designed to work synchronously. Mainly for simplicity & because it's not a serverside app and speed is not really an issue.

You can use async transforms too though! To use async transforms, you will need to force them into a sync mode. [Example](https://github.com/DavidWells/middy-example/blob/7f72fc1afaa8699daf8df77f7d50c0576859b261/scripts/docs.js#L3-L4)

[Forcing async functions to run sync in node.js](https://davidwells.io/snippets/forcing-async-functions-to-sync-in-node/)

Version 2.0 of markdown magic will likely be async first [see issue](https://github.com/DavidWells/markdown-magic/issues/3)

## 🔌 Third Party Plugins

* [wordcount](https://github.com/DavidWells/markdown-magic-wordcount/) - Add wordcount to markdown files
* [github-contributors](https://github.com/DavidWells/markdown-magic-github-contributors) - List out the contributors of a given repository
* [directory-tree](https://github.com/camacho/markdown-magic-directory-tree) - Add directory tree to markdown files
* [install-command](https://github.com/camacho/markdown-magic-install-command) - Add install command to markdown files with `peerDependencies` included
* [subpackage-list](https://github.com/camacho/markdown-magic-subpackage-list) - Add list of all subpackages (great for projects that use [Lerna](https://github.com/lerna/lerna))
* [version-badge](https://github.com/camacho/markdown-magic-version-badge) - Add a badge with the latest version of the project
* [template](https://github.com/camacho/markdown-magic-template) - Add Lodash template support
* [dependency-table](https://github.com/camacho/markdown-magic-dependency-table) - Add a table of dependencies with links to their repositories, version information, and a short description
* [package-scripts](https://github.com/camacho/markdown-magic-package-scripts) - Add a table of `package.json` scripts with descriptions
* [prettier](https://github.com/camacho/markdown-magic-prettier) - Format code blocks with [`prettier`](https://github.com/prettier/prettier)
* [engines](https://github.com/camacho/markdown-magic-engines) - Print engines list from `package.json`
* [jsdoc](https://github.com/bradtaylorsf/markdown-magic-jsdoc) - Adds jsdoc comment support
* [build-badge](https://github.com/rishichawda/markdown-magic-build-badge) - Update branch badges to auto-magically point to current branches.
* [package-json](https://github.com/forresst/markdown-magic-package-json) - Add the package.json properties to markdown files
* [local-image](https://github.com/stevenbenisek/markdown-magic-local-image)
* [figlet](https://github.com/lafourchette/markdown-magic-figlet) - Add FIGfont text to markdown files
* [local-image](https://github.com/stevenbenisek/markdown-magic-local-image) - plugin to add local images to markdown

## Adding Custom Transforms

Markdown Magic is extendable via plugins.

Plugins allow developers to add new transforms to the `config.transforms` object. This allows for things like using different rendering engines, custom formatting, or any other logic you might want.

Plugins run in order of registration.

The below code is used to generate **this markdown file** via the plugin system.

<!-- ⛔️ MD-MAGIC-EXAMPLE:START (CODE:src=./examples/generate-readme.js) -->
<!-- The below code snippet is automatically added from ./examples/generate-readme.js -->
```js
const fs = require('fs')
const path = require('path')
const execSync = require('child_process').execSync
const markdownMagic = require('../index') // 'markdown-magic'

const config = {
  matchWord: 'MD-MAGIC-EXAMPLE', // default matchWord is AUTO-GENERATED-CONTENT
  transforms: {
    /* Match <!-- AUTO-GENERATED-CONTENT:START (customTransform:optionOne=hi&optionOne=DUDE) --> */
    customTransform(content, options) {
      console.log('original content in comment block', content)
      console.log('options defined on transform', options)
      // options = { optionOne: hi, optionOne: DUDE}
      return `This will replace all the contents of inside the comment ${options.optionOne}`
    },
    /* Match <!-- AUTO-GENERATED-CONTENT:START (RENDERDOCS:path=../file.js) --> */
    RENDERDOCS(content, options) {
      const fileContents = fs.readFileSync(options.path, 'utf8')
      const docBlocs = require('dox').parseComments(fileContents, { raw: true, skipSingleStar: true })
      let updatedContent = ''
      docBlocs.forEach((data) => {
        updatedContent += `${data.description.full}\n\n`
      })
      return updatedContent.replace(/^\s+|\s+$/g, '')
    },
    /* Match <!-- AUTO-GENERATED-CONTENT:START (pluginExample) --> */
    pluginExample: require('./plugin-example')({ addNewLine: true }),
    /* Plugins from npm */
    // count: require('markdown-magic-wordcount'),
    // github: require('markdown-magic-github-contributors')
  }
}

/* This example callback automatically updates Readme.md and commits the changes */
const callback = function autoGitCommit(err, output) {
  // output is array of file information
  output.forEach(function(data) {
    const mdPath = data.outputFilePath
    if(!mdPath) return false
    const gitAdd = execSync(`git add ${mdPath}`, {}, (error) => {
      if (error) console.warn(error)
      const msg = `${mdPath} automatically updated by markdown-magic`
      const gitCommitCommand = `git commit -m '${msg}' --no-verify`
      execSync(gitCommitCommand, {}, (err) => {
        if (err) console.warn(err)
        console.log('git commit automatically ran. Push up your changes!')
      })
    })
  })
}

const markdownPath = path.join(__dirname, '..', 'README.md')
markdownMagic(markdownPath, config, callback)
```
<!-- ⛔️ MD-MAGIC-EXAMPLE:END -->

## Plugin Example

Plugins must return a transform function with the following signature.

```js
return function myCustomTransform (content, options)
```

<!-- ⛔️ MD-MAGIC-EXAMPLE:START (CODE:src=./examples/plugin-example.js) -->
<!-- The below code snippet is automatically added from ./examples/plugin-example.js -->
```js
/* Custom Transform Plugin example */
const merge = require('deepmerge')
module.exports = function customPlugin(pluginOptions) {
  // set plugin defaults
  const defaultOptions = {
    addNewLine: false
  }
  const userOptions = pluginOptions || {}
  const pluginConfig = merge(defaultOptions, userOptions)
  // return the transform function
  return function myCustomTransform (content, options) {
    const newLine = (pluginConfig.addNewLine) ? '\n' : ''
    const updatedContent = content + newLine
    return updatedContent
  }
}
```
<!-- ⛔️ MD-MAGIC-EXAMPLE:END -->

[View the raw file](https://raw.githubusercontent.com/DavidWells/markdown-magic/master/README.md) file and run `npm run docs` to see this plugin run
<!-- ⛔️ MD-MAGIC-EXAMPLE:START (pluginExample) DO not edit ⛔️ -->
This content is altered by the `pluginExample` plugin registered in `examples/generate-readme.js`

<!-- ⛔️ MD-MAGIC-EXAMPLE:END -->

## Other usage examples

- [Serverless Plugin Repo](https://github.com/serverless/plugins/blob/master/generate-docs.js) this example takes a `json` file and converts it into a github flavored markdown table
- [MochaJS](https://github.com/mochajs/mocha/blob/4cc711fa00f7166a2303b77bf2487d1c2cc94621/scripts/markdown-magic.config.js)
- [tc39/agendas](https://github.com/tc39/agendas#agendas) - [code](https://github.com/tc39/agendas/blob/65945b1b6658e9829ef95a51bf2632ff44f951e6/scripts/generate.js)
- [moleculerjs/moleculer-addons](https://github.com/moleculerjs/moleculer-addons/blob/7cf0f72140717c52621b724cd54a710517106df0/readme-generator.js)
- [good-first-issue](https://github.com/bnb/good-first-issue/blob/e65513a1f26167dea3c137008b8796640d8d5303/markdown.config.js)
- [navikt/nav-frontend-moduler](https://github.com/navikt/nav-frontend-moduler/blob/20ad521c27a43d3203eab4bc32121e5b8270c077/_scripts/generateReadmes.js)
- [country-flags-svg](https://github.com/ronatskiy/country-flags-svg/blob/cfb2368c7e634ebc1679855e13cc3e26ca11187f/markdown.config.js)
- [react-typesetting](https://github.com/exogen/react-typesetting/blob/7114cdc8c4cb1b0d59ebc8b5364e808687419889/markdown.config.js)
- [and many more!](https://github.com/search?o=desc&p=1&q=markdown-magic+filename%3Apackage.json+-user%3Ah13i32maru+-user%3Aesdoc+-user%3Aes-doc&s=indexed&type=Code)

## Custom Transform Demo

View the raw source of this `README.md` file to see the comment block and see how the `customTransform` function in `examples/generate-readme.js` works

<!-- ⛔️ MD-MAGIC-EXAMPLE:START (customTransform:optionOne=hi&optionOne=DUDE) - Do not remove or modify this section -->
This will replace all the contents of inside the comment DUDE
<!-- ⛔️ MD-MAGIC-EXAMPLE:END - Do not remove or modify this section -->

## Prior Art

This was inspired by [Kent C Dodds](https://twitter.com/kentcdodds) and [jfmengels](https://github.com/jfmengels)'s [all contributors cli](https://github.com/jfmengels/all-contributors-cli) project.

<!-- MD-MAGIC-EXAMPLE:START (LOLZ)-->
This section was generated by the cli config markdown.config.js file
<!-- MD-MAGIC-EXAMPLE:END (LOLZ)-->

## License

[MIT][mit] © [DavidWells][author]

[npm-badge]:https://img.shields.io/npm/v/markdown-magic.svg?style=flat-square
[npm-link]: http://www.npmjs.com/package/markdown-magic
[mit]:      http://opensource.org/licenses/MIT
[author]:   http://github.com/davidwells
