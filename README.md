![Community-Project](https://gitlab.com/softbutterfly/open-source/open-source-office/-/raw/master/assets/dynova/dynova-open-source--banner--community-project.png)
[![MIT License][badge-license]][repository] [![Microsoft Dynamics NAV C/AL][badge-language]][repository] [![Visual Studio Code][badge-tool]][repository]

# New Relic Query Language (NRQL) for Visual Studio Code

This package provides simple syntax highlighting for New Relic Query Language
(NRQL) in Visual Studio Code.

New Relic already provides syntax highlighting as part of their [Code Stream Extension ↗][vscode-codestream], but this extension provides a lightweight alternative for those who want to use NRQL in their local development environment without the need to log into New Relic.

> This extension is not affiliated with *New Relic, Inc.* in any way.

![NRQL Syntax Highlighting](https://github.com/dynovaio/newrelic-sb-nrql-vscode/raw/HEAD/images/_nrql_sample.png)

## ✨ Features

- ✅ Syntax highlighting for:
  - Clauses
  - Functions
  - Operators
  - Comments (//, /* ... */, --)
  - Strings (quoted & backticked)
- ✅ Markdown embedding support
- ✅ Custom file icon for `.nrql` files

## 📦 Installation

1. Open Visual Studio Code.
2. Go to the Extensions view by clicking on the Extensions icon in the Activity Bar on the side of the window or by pressing `Ctrl+Shift+X`.
3. Search for "New Relic Query Language" or "NRQL".
4. Click on the "Install" button for the extension named "New Relic Query Language (NRQL) for Visual Studio Code".
5. Once installed, you can start using NRQL syntax highlighting in your `.nrql` files.
6. Optionally, you can set the default language for `.nrql` files by adding the following to your `settings.json`:

```json
"files.associations": {
    "*.nrql": "nrql"
}
```

## Release Notes

All changes are listed in our [change log ↗][changelog].

## Development

Check out our [contribution guide ↗][contributing].

## Contributors

See the list of contributors in our [contributors page ↗][contributors].

## License

This project is licensed under the terms of the MIT license. See the
[LICENSE ↗][license] file.

[badge-license]: https://img.shields.io/badge/License-MIT-blue.svg?maxAge=2592000&style=flat-square
[badge-language]: https://img.shields.io/badge/Language-NRQL-blue.svg?maxAge=2592000&style=flat-square
[badge-tool]: https://img.shields.io/badge/Tool-Visual%20Studio%20Code-blue.svg?maxAge=2592000&style=flat-square
[repository]: https://github.com/dynovaio/newrelic-sb-nrql-vscode
[vscode-codestream]: https://marketplace.visualstudio.com/items?itemName=CodeStream.codestream
[contributing]: https://github.com/dynovaio/newrelic-sb-nrql-vscode/blob/master/CONTRIBUTING.md
[contributors]: https://github.com/dynovaio/newrelic-sb-nrql-vscode/graphs/contributors
[changelog]: https://github.com/dynovaio/newrelic-sb-nrql-vscode/blob/master/CHANGELOG.md
[license]: https://github.com/dynovaio/newrelic-sb-nrql-vscode/blob/master/LICENSE.txt
