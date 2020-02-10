# Welcome to serverless-rollup-plugin 👋

[![Build Status](https://travis-ci.org/drg-adaptive/serverless-rollup-plugin.svg)](https://travis-ci.org/drg-adaptive/serverless-rollup-plugin)
[![Maintainability](https://api.codeclimate.com/v1/badges/79e200bf72d884691c7a/maintainability)](https://codeclimate.com/github/drg-adaptive/serverless-rollup-plugin/maintainability)
[![npm version](https://badge.fury.io/js/serverless-rollup-plugin.svg)](https://badge.fury.io/js/serverless-rollup-plugin)
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fdrg-adaptive%2Fserverless-rollup-plugin.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fdrg-adaptive%2Fserverless-rollup-plugin?ref=badge_shield)

> A plugin for the serverless framework to bundle lambda code using rollup

## Install

```sh
yarn install serverless-rollup-plugin
```

## Usage

Add the plugin to your serverless config:

```yaml
plugins:
  - serverless-rollup-plugin
  - ...any other plugins
```

Under the custom property, add a section for rollup. The only required property to run rollup is the `config` property:

```yaml
custom:
  rollup:
    config: ./rollup.config.js
```

For each function that you would like to use rollup option, just define the handler option as normal. You can
optionally define the `dependencies` property as a list of packages to be installed in the `node_modules` folder
in your lambda.

```yaml
testFunction:
  handler: src/functions/testFunction/index.handler
  dependencies:
    - aws-xray-sdk-core
  copyFiles:
    - some/glob/**/*.pattern
```

### Using Yarn

By default if you specify function dependencies `npm` will be used. You can override this by setting the `installCommand` property, like this:

```yaml
custom:
  rollup:
    installCommand: yarn add
```

### Add Global Dependency

If you want to add a dependency for every lambda function (for example [adding source map support](#adding-sourcemap-support)), you can add them to the rollup `dependencies` property:

```yaml
custom:
  rollup:
    dependencies:
      - some-package-name
```

### Output Options

If you don't specify `output` settings in your rollup config, the following defaults will be used:

```json
{
  "format": "cjs",
  "sourcemap": true
}
```

### Adding Sourcemap Support

You can easily get your lambda stack traces to show correct file/line information using the `source-map-support` package.
To use this with the `serverless-rollup-plugin`, first install the package and add it to the universal dependencies:

```yaml
custom:
  rollup:
    dependencies:
      - source-map-support
```

Then in your rollup config, set the output banner to install it:

```typescript
export default {
  output: {
    format: "cjs",
    sourcemap: true,
    banner: "require('source-map-support').install();"
  }
};
```

If you do specify `output` settings, they will be used and only the `file` property will be overwritten.

### Copying Resource Files

To copy a static file into your function deployment, use the `copyFiles` parameter. This
parameter is an array of glob patterns.

## Author

👤 **Ben Force**

- Twitter: [@theBenForce](https://twitter.com/theBenForce)
- Github: [@theBenForce](https://github.com/theBenForce)

## 🤝 Contributing

Contributions, issues and feature requests are welcome!

Feel free to check [issues page](https://github.com/drg-adaptive/serverless-rollup-plugin/issues).

## Show your support

Give a ⭐️ if this project helped you!

---

_This README was generated with ❤️ by [readme-md-generator](https://github.com/kefranabg/readme-md-generator)_
