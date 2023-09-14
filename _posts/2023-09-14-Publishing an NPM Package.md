---
title: Publishing an NPM Package
date:  2023-09-14 11:33:58 +0800
categories: [Technology, NPM]
tags: [package, typescript, npm]     # TAG names should always be lowercase
---

My programming language track is from Pacal, C++, C, Java, C/C++, php, and Typescript. For me, JavaScript is paused in my life of high school life from 2007 to 2010. I am glad to have Typescript which makes the whole development process much more easier with type checking. And, as I have been working on several data visualization projcets, I learnt ReactJs for UI/UX development. And, now, Typescript is my favourite programming language.

The npm package [hk-bus-eta](https://www.npmjs.com/package/hk-bus-eta) have become open source along with the project [hkbus.app](https://hkbus.app). I originally wrote the package in [CommonJS](https://en.wikipedia.org/wiki/CommonJS) for server-side compatability to older project, while the entire pattern differences from [ECMAScript](https://en.wikipedia.org/wiki/ECMAScript) had really pissed me off. After several months, I decide to rewrite it in ECMAScript and keeping it compatible to both CommonJS and ECMAScript.

Taking a simple project [cantonese-romanisation](https://www.npmjs.com/package/cantonese-romanisation) as an example, the following steps could publish the package to NPM.

## 0. TLDR;

Check out the directory structure and `package.json` in the example [Git repository](https://github.com/chunlaw/cantonese-romanisation), and run the following commands.

```sh
# pull the project and prepare the environment
$ git clone https://github.com/chunlaw/cantonese-romanisation
$ cd cantonese-romanisation
$ yarn

# publish the package to NPM
$ yarn publish
```

## 1. Directory Structure

Below directory structure depicts the files required.

```sh
.
├── package.json        # required
├── README.md           # description to be used in NPM
├── tsconfig.json       # typescript configuration setting
├── README.md           # description to be used in NPM
├── src/                # the folder of source code 
│ └── index.ts          # source code entry point
├── jest.config.js      # optional, jest configuration setting
└── test/               # optional, just for testing the package before publish
  └── index.test.ts     # test cases for jest running 
```

## 2. Config `package.json`

Assume the `package.json` has already been initialized by `yarn init` or `npm init`. We go through several important parameter belows.

The field `name`, `version`, `description`, `author`, `license`, `homepage`, `repository`, `keywords`, and `bugs` are the metadata for indicating other developers what the package is.
```js
{
  ...
  "name": "cantonese-romanisation",
  "version": "1.0.5",
  "description": "Library for mapping Chinese character to Hong Kong Government Cantonese Romanisation, Pingyam (Yale or LSHK)",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/chunlaw/cantonese-romanisation.git"
  },
  "bugs": {
    "url": "https://github.com/chunlaw/cantonese-romanisation/issues"
  },
  "homepage": "https://cantonese-romanisation.chunlaw.io",
  "keywords": [
    "cantonese",
    "pingyam",
    "pingyum",
    "pinyin",
    "chinese",
    "roman",
    "yale",
    "lshk",
    "Hong Kong Government Cantonese Romanisation",
    "romanisation"
  ],
  "author": "chunlaw",
  ...
}
```

## 3. Prepare environment

The field `dependencies` are the packages that are required for your application to run properly, and `devDependencies` are the packages that are required for development and testing purposes only. In this example, the `dependencies` is empty as the package is all fine without third party dependencies.

We will run the following command,

```bash
yarn add -D @types/jest csvtojson jest rimraf ts-jest ts-node typescript
# or 
npm install --save-dev @types/jest csvtojson jest rimraf ts-jest ts-node typescript
```

The package.json will be something like below,

```js
{
  ...
  "dependencies": {
  },
  "devDependencies": {
    "@types/jest": "^29.5.4",
    "csvtojson": "^2.0.10",
    "jest": "^29.6.4",
    "rimraf": "^5.0.1",
    "ts-jest": "^29.1.1",
    "ts-node": "^10.9.1",
    "typescript": "^5.2.2"
  },
  ...
}
```

## 4. Writing Code and test case

The content of `./src/index.ts` is as below.

```typescript
// ./src/index.ts
export function sayHello(): void {
  return "Hello";
}
```

The content of `./test/index.test.ts` is as below,

```typescript
// ./test/index.test.ts
import { sayHello } from "../src/index"

test('expected Hello', () => {
  expect(sayHello()).toBe("Hello")
})
```

## 5. Setup scritps

Add the following lines into `package.json`.

```js
{
  ...
  "scripts": {
    "clean": "rimraf dist",
    "test": "jest",
    "build:esm": "tsc --target es2018 --outDir esm",
    "build:cjs": "tsc --target es2015 --module commonjs --outDir dist",
    "build": "yarn build:cjs && yarn build:esm"
  },
  ...
}
```

After that, `build:esm` will build the script for _ECMAScript_, and `build:cjs` for _CommonJS_

## 6. Entry point for _CommonJS_ and _ECMASCript_

Add the following lines into `package.json` for setting up the entry points for _CommonJS_ and _ECMAScript_.

```js
{
  ...
  "main": "dist/index.js",
  "module": "esm/index.js",
  ...
}
```

## 7. Whitelisting publishing folders

Add the following lines to whitelist the bundle build folders and source code to be published in NPM. These folders will be submitted along with README.md to NPM.

```js
{
  ...
  "files": [
    "dist",
    "esm",
    "src"
  ],
  ...
}
```

Right now, we have finished all the setup for `package.json`, the content should look similar as below.

```json
{
  "name": "cantonese-romanisation",
  "version": "1.0.5",
  "description": "Library for mapping Chinese character to Hong Kong Government Cantonese Romanisation, Pingyam (Yale or LSHK)",
  "main": "dist/index.js",
  "module": "esm/index.js",
  "files": [
    "dist",
    "esm",
    "src"
  ],
  "scripts": {
    "clean": "rimraf dist",
    "test": "jest",
    "build:esm": "tsc --target es2018 --outDir esm",
    "build:cjs": "tsc --target es2015 --module commonjs --outDir dist",
    "build": "yarn build:cjs && yarn build:esm"
  },
  "keywords": [
    "cantonese",
    "pingyam",
    "pingyum",
    "pinyin",
    "chinese",
    "roman",
    "yale",
    "lshk",
    "Hong Kong Government Cantonese Romanisation",
    "romanisation"
  ],
  "author": "chunlaw",
  "license": "MIT",
  "dependencies": {
  },
  "devDependencies": {
    "@types/jest": "^29.5.4",
    "csvtojson": "^2.0.10",
    "jest": "^29.6.4",
    "rimraf": "^5.0.1",
    "ts-jest": "^29.1.1",
    "ts-node": "^10.9.1",
    "typescript": "^5.2.2"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/chunlaw/cantonese-romanisation.git"
  },
  "bugs": {
    "url": "https://github.com/chunlaw/cantonese-romanisation/issues"
  },
  "homepage": "https://cantonese-romanisation.chunlaw.io"
}

```

## 8. Build and submit

Run the following commands to build and submit to NPM

```sh
$ yarn build && yarn publish
# or
$ npm run build && npm publish
```

Have fun.