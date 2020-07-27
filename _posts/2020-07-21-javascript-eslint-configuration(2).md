---
layout: post
title:  "User Guide - Configuring ESLint (2)"
date:   2020-07-23 21:10:00 +0900
categories: javascript
tags: eslint
---

# Configuring ESLint


## Using Configuration Files

구성 파일을 사용하는 방법에는 두 가지가 있습니다.

구성 파일을 사용하는 첫 번째 방법은`.eslintrc. *` 및 `package.json` 파일을 이용하는 것입니다. ESLint는 lint가 될 파일의 디렉토리와 파일 시스템의 루트 디렉토리까지 연속적인 상위 디렉토리에서 자동으로 파일을 찾습니다 (`'root : true'`가 지정되지 않은 경우). 이 옵션은 프로젝트의 다른 부분에 대해 다른 구성을 원하거나 구성 파일을 전달할 필요 없이 다른 사람들이 ESLint를 직접 사용할 수 있도록하려는 경우에 유용합니다.

두 번째는 파일을 원하는 곳에 저장하고`-c` 옵션을 사용하여 CLI에 위치를 전달하는 것입니다.

```
eslint -c myconfig.json myfiletotest.js
```

하나의 구성 파일을 사용 중이고 ESLint가`.eslintrc. *`파일을 무시하도록하려면`-c` 플래그와 함께`--no-eslintrc`를 사용해야합니다.

각각의 경우 구성 파일의 설정이 기본 설정보다 우선됩니다.


## Configuration File Formats

ESLint는 여러 형식의 구성 파일을 지원합니다.

-   **JavaScript**  - use  `.eslintrc.js`  and export an object containing your configuration.
-   **JavaScript (ESM)**  - use  `.eslintrc.cjs`  when running ESLint in JavaScript packages that specify  `"type":"module"`  in their  `package.json`. Note that ESLint does not support ESM configuration at this time.
-   **YAML**  - use  `.eslintrc.yaml`  or  `.eslintrc.yml`  to define the configuration structure.
-   **JSON**  - use  `.eslintrc.json`  to define the configuration structure. ESLint's JSON files also allow JavaScript-style comments.
-   **Deprecated**  - use  `.eslintrc`, which can be either JSON or YAML.
-   **package.json**  - create an  `eslintConfig`  property in your  `package.json`  file and define your configuration there.

동일한 디렉토리에 여러 구성 파일이있는 경우 ESLint는 하나만 사용합니다. 우선 순위는 다음과 같습니다.

1.  `.eslintrc.js`
2.  `.eslintrc.cjs`
3.  `.eslintrc.yaml`
4.  `.eslintrc.yml`
5.  `.eslintrc.json`
6.  `.eslintrc`
7.  `package.json`


## Configuration Cascading and Hierarchy

When using  `.eslintrc.*`  and  `package.json`  files for configuration, you can take advantage of configuration cascading. For instance, suppose you have the following structure:
`.eslintrc. *`및`package.json` 파일을 구성에 사용할 때 구성 cascading을 활용할 수 있습니다. 예를 들어, 다음 구조를 가지고 있다고 가정하십시오.

```
your-project
├── .eslintrc
├── lib
│ └── source.js
└─┬ tests
  ├── .eslintrc
  └── test.js
```

configuration cascade는 가장 가까운`.eslintrc` 파일을 가장 우선 순위가 높은 파일에 lint한 다음 상위 디렉토리의 구성 파일 등을 사용하여 작동합니다. 이 프로젝트에서 ESLint를 실행하면`lib /`의 모든 파일은 프로젝트의 루트에 있는`.eslintrc` 파일을 구성으로 사용합니다. ESLint가`tests /`디렉토리로 이동하면`your-project / .eslintrc` 외에도`your-project / tests / .eslintrc`가 사용됩니다. 따라서 'your-project / tests / test.js'는 디렉토리 계층 구조에서 두 개의`.eslintrc` 파일의 조합을 기반으로 lint가 있으며 가장 가까운 파일이 우선합니다. 이러한 방식으로 프로젝트 수준 ESLint 설정을 사용하고 디렉토리 별 재정의를 수행 할 수 있습니다.

In the same way, if there is a  `package.json`  file in the root directory with an  `eslintConfig`  field, the configuration it describes will apply to all subdirectories beneath it, but the configuration described by the  `.eslintrc`  file in the tests directory will override it where there are conflicting specifications.
같은 방법으로 루트 디렉토리에`eslintConfig` 필드가있는`package.json` 파일이있는 경우, 설명하는 구성은 그 아래의 모든 하위 디렉토리에 적용되지만, tests 디렉토리에 있는`.eslintrc` 파일에 의해 기술 된 설정은 사양이 상충하는 부분을 덮어 씁니다.

```
your-project
├── package.json
├── lib
│ └── source.js
└─┬ tests
  ├── .eslintrc
  └── test.js
```

같은 디렉토리에`.eslintrc`와`package.json` 파일이 있으면`.eslintrc`가 우선권을 가지며`package.json` 파일은 사용되지 않습니다.

기본적으로 ESLint는 모든 상위 폴더에서 루트 디렉토리까지 구성 파일을 찾습니다. 이것은 모든 프로젝트가 특정 규칙을 따르기를 원할 때 유용하지만 때로는 예기치 않은 결과가 발생할 수 있습니다. ESLint를 특정 프로젝트로 제한하려면`package.json` 파일의`eslintConfig` 필드 또는 프로젝트의 루트 레벨에있는`.eslintrc. *`파일에` "root": true`를 넣으십시오. ESLint는 ` "root": true`로 설정을 찾으면 상위 폴더 검색을 중단합니다.

```
{
    "root": true
}
```

And in YAML:

```
---
  root: true
```

예를 들어, `lib /` 디렉토리의 `.eslintrc` 파일에 ` "root": true`가 설정된 `projectA`를 고려하십시오. 이 경우 `main.js`를 lint하는 동안 `lib /`내의 구성이 사용되지만 `projectA /`의 `.eslintrc` 파일은 사용되지 않습니다.

```
home
└── user
    └── projectA
        ├── .eslintrc  <- Not used
        └── lib
            ├── .eslintrc  <- { "root": true }
            └── main.js
```

가장 높은 우선 순위에서 가장 낮은 우선 순위까지 전체 구성 계층 구조는 다음과 같습니다.

1.  Inline configuration
    1.  `/*eslint-disable*/`  and  `/*eslint-enable*/`
    2.  `/*global*/`
    3.  `/*eslint*/`
    4.  `/*eslint-env*/`
2.  Command line options (or CLIEngine equivalents):
    1.  `--global`
    2.  `--rule`
    3.  `--env`
    4.  `-c`,  `--config`
3.  Project-level configuration:
    1.  `.eslintrc.*`  or  `package.json`  file in same directory as linted file
    2.  루트 디렉토리까지 또는 루트 디렉토리를 포함하여 또는 ""root ":``인 설정이 발견 될 때까지 상위 디렉토리 (부모가 우선 순위가 높고 조부모 등)에서`.eslintrc` 및`package.json` 파일을 계속 검색하십시오.


> 여기까지 읽었습니당


## Extending Configuration Files

A configuration file can extend the set of enabled rules from base configurations.

The  `extends`  property value is either:

-   a string that specifies a configuration (either a path to a config file, the name of a shareable config,  `eslint:recommended`, or  `eslint:all`)
-   an array of strings: each additional configuration extends the preceding configurations

ESLint extends configurations recursively, so a base configuration can also have an  `extends`  property. Relative paths and shareable config names in an  `extends`  property are resolved from the location of the config file where they appear.

The  `rules`  property can do any of the following to extend (or override) the set of rules:

-   enable additional rules
-   change an inherited rule's severity without changing its options:
    -   Base config:  `"eqeqeq": ["error", "allow-null"]`
    -   Derived config:  `"eqeqeq": "warn"`
    -   Resulting actual config:  `"eqeqeq": ["warn", "allow-null"]`
-   override options for rules from base configurations:
    -   Base config:  `"quotes": ["error", "single", "avoid-escape"]`
    -   Derived config:  `"quotes": ["error", "single"]`
    -   Resulting actual config:  `"quotes": ["error", "single"]`

### Using  `"eslint:recommended"`[](https://eslint.org/docs/user-guide/configuring#using-eslint-recommended)

An  `extends`  property value  `"eslint:recommended"`  enables a subset of core rules that report common problems, which have a check mark  on the  [rules page](https://eslint.org/docs/rules/). The recommended subset can change only at major versions of ESLint.

If your configuration extends the recommended rules: after you upgrade to a newer major version of ESLint, review the reported problems before you use the  `--fix`  option on the  [command line](https://eslint.org/docs/user-guide/command-line-interface#fix), so you know if a new fixable recommended rule will make changes to the code.

The  `eslint --init`  command can create a configuration so you can extend the recommended rules.

Example of a configuration file in JavaScript format:

```
module.exports = {
    "extends": "eslint:recommended",
    "rules": {
        // enable additional rules
        "indent": ["error", 4],
        "linebreak-style": ["error", "unix"],
        "quotes": ["error", "double"],
        "semi": ["error", "always"],

        // override default options for rules from base configurations
        "comma-dangle": ["error", "always"],
        "no-cond-assign": ["error", "always"],

        // disable rules from base configurations
        "no-console": "off",
    }
}

```

### Using a shareable configuration package[](https://eslint.org/docs/user-guide/configuring#using-a-shareable-configuration-package)

A  [sharable configuration](https://eslint.org/docs/developer-guide/shareable-configs)  is an npm package that exports a configuration object. Make sure the package has been installed to a directory where ESLint can require it.

The  `extends`  property value can omit the  `eslint-config-`  prefix of the package name.

The  `eslint --init`  command can create a configuration so you can extend a popular style guide (for example,  `eslint-config-standard`).

Example of a configuration file in YAML format:

```
extends: standard
rules:
  comma-dangle:
    - error
    - always
  no-empty: warn

```

### Using the configuration from a plugin[](https://eslint.org/docs/user-guide/configuring#using-the-configuration-from-a-plugin)

A  [plugin](https://eslint.org/docs/developer-guide/working-with-plugins)  is an npm package that usually exports rules. Some plugins also export one or more named  [configurations](https://eslint.org/docs/developer-guide/working-with-plugins#configs-in-plugins). Make sure the package has been installed to a directory where ESLint can require it.

The  `plugins`  [property value](https://eslint.org/docs/user-guide/configuring#configuring-plugins)  can omit the  `eslint-plugin-`  prefix of the package name.

The  `extends`  property value can consist of:

-   `plugin:`
-   the package name (from which you can omit the prefix, for example,  `react`)
-   `/`
-   the configuration name (for example,  `recommended`)

Example of a configuration file in JSON format:

```
{
    "plugins": [
        "react"
    ],
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended"
    ],
    "rules": {
       "react/no-set-state": "off"
    }
}

```

### Using a configuration file[](https://eslint.org/docs/user-guide/configuring#using-a-configuration-file)

The  `extends`  property value can be an absolute or relative path to a base  [configuration file](https://eslint.org/docs/user-guide/configuring#using-configuration-files). ESLint resolves a relative path to a base configuration file relative to the configuration file that uses it.

Example of a configuration file in JSON format:

```
{
    "extends": [
        "./node_modules/coding-standard/eslintDefaults.js",
        "./node_modules/coding-standard/.eslintrc-es6",
        "./node_modules/coding-standard/.eslintrc-jsx"
    ],
    "rules": {
        "eqeqeq": "warn"
    }
}

```

### Using  `"eslint:all"`[](https://eslint.org/docs/user-guide/configuring#using-eslint-all)

The  `extends`  property value can be  `"eslint:all"`  to enable all core rules in the currently installed version of ESLint. The set of core rules can change at any minor or major version of ESLint.

**Important:**  This configuration is  **not recommended for production use**  because it changes with every minor and major version of ESLint. Use at your own risk.

If you configure ESLint to automatically enable new rules when you upgrade, ESLint can report new problems when there are no changes to source code, therefore any newer minor version of ESLint can behave as if it has breaking changes.

You might enable all core rules as a shortcut to explore rules and options while you decide on the configuration for a project, especially if you rarely override options or disable rules. The default options for rules are not endorsements by ESLint (for example, the default option for the  `quotes`  rule does not mean double quotes are better than single quotes).

If your configuration extends all core rules: after you upgrade to a newer major or minor version of ESLint, review the reported problems before you use the  `--fix`  option on the  [command line](https://eslint.org/docs/user-guide/command-line-interface#fix), so you know if a new fixable rule will make changes to the code.

Example of a configuration file in JavaScript format:

```
module.exports = {
    "extends": "eslint:all",
    "rules": {
        // override default options
        "comma-dangle": ["error", "always"],
        "indent": ["error", 2],
        "no-cond-assign": ["error", "always"],

        // disable now, but enable in the future
        "one-var": "off", // ["error", "never"]

        // disable
        "init-declarations": "off",
        "no-console": "off",
        "no-inline-comments": "off",
    }
}
```

## Configuration Based on Glob Patterns[](https://eslint.org/docs/user-guide/configuring#configuration-based-on-glob-patterns)

**v4.1.0+.**  Sometimes a more fine-controlled configuration is necessary, for example if the configuration for files within the same directory has to be different. Therefore you can provide configurations under the  `overrides`  key that will only apply to files that match specific glob patterns, using the same format you would pass on the command line (e.g.,  `app/**/*.test.js`).

### How it works[](https://eslint.org/docs/user-guide/configuring#how-it-works)

-   The patterns are applied against the file path relative to the directory of the config file. For example, if your config file has the path  `/Users/john/workspace/any-project/.eslintrc.js`  and the file you want to lint has the path  `/Users/john/workspace/any-project/lib/util.js`, then the pattern provided in  `.eslintrc.js`  will be executed against the relative path  `lib/util.js`.
-   Glob pattern overrides have higher precedence than the regular configuration in the same config file. Multiple overrides within the same config are applied in order. That is, the last override block in a config file always has the highest precedence.
-   A glob specific configuration works almost the same as any other ESLint config. Override blocks can contain any configuration options that are valid in a regular config, with the exception of  `root`  and  `ignorePatterns`.
    -   A glob specific configuration can have  `extends`  setting, but the  `root`  property in the extended configs is ignored. The  `ignorePatterns`  property in the extended configs is used only for the files the glob specific configuration matched.
    -   Nested  `overrides`  setting will be applied only if the glob patterns of both of the parent config and the child config matched. This is the same when the extended configs have  `overrides`  setting.
-   Multiple glob patterns can be provided within a single override block. A file must match at least one of the supplied patterns for the configuration to apply.
-   Override blocks can also specify patterns to exclude from matches. If a file matches any of the excluded patterns, the configuration won't apply.

### Relative glob patterns[](https://eslint.org/docs/user-guide/configuring#relative-glob-patterns)

```
project-root
├── app
│   ├── lib
│   │   ├── foo.js
│   │   ├── fooSpec.js
│   ├── components
│   │   ├── bar.js
│   │   ├── barSpec.js
│   ├── .eslintrc.json
├── server
│   ├── server.js
│   ├── serverSpec.js
├── .eslintrc.json

```

The config in  `app/.eslintrc.json`  defines the glob pattern  `**/*Spec.js`. This pattern is relative to the base directory of  `app/.eslintrc.json`. So, this pattern would match  `app/lib/fooSpec.js`  and  `app/components/barSpec.js`  but  **NOT**  `server/serverSpec.js`. If you defined the same pattern in the  `.eslintrc.json`  file within in the  `project-root`  folder, it would match all three of the  `*Spec`  files.

If a config is provided via the  `--config`  CLI option, the glob patterns in the config are relative to the current working directory rather than the base directory of the given config. For example, if  `--config configs/.eslintrc.json`  is present, the glob patterns in the config are relative to  `.`  rather than  `./configs`.

### Example configuration[](https://eslint.org/docs/user-guide/configuring#example-configuration)

In your  `.eslintrc.json`:

```
{
  "rules": {
    "quotes": ["error", "double"]
  },

  "overrides": [
    {
      "files": ["bin/*.js", "lib/*.js"],
      "excludedFiles": "*.test.js",
      "rules": {
        "quotes": ["error", "single"]
      }
    }
  ]
}

```

### Specifying Target Files to Lint[](https://eslint.org/docs/user-guide/configuring#specifying-target-files-to-lint)

If you specified directories with CLI (e.g.,  `eslint lib`), ESLint searches target files in the directory to lint. The target files are  `*.js`  or the files that match any of  `overrides`  entries (but exclude entries that are any of  `files`  end with  `*`).

If you specified the  [`--ext`](https://eslint.org/docs/user-guide/command-line-interface#ext)  command line option along with directories, the target files are only the files that have specified file extensions regardless of  `overrides`  entries.

## Comments in Configuration Files[](https://eslint.org/docs/user-guide/configuring#comments-in-configuration-files)

Both the JSON and YAML configuration file formats support comments (`package.json`  files should not include them). You can use JavaScript-style comments or YAML-style comments in either type of file and ESLint will safely ignore them. This allows your configuration files to be more human-friendly. For example:

```
{
    "env": {
        "browser": true
    },
    "rules": {
        // Override our default settings just for this directory
        "eqeqeq": "warn",
        "strict": "off"
    }
}

```

## Ignoring Files and Directories[](https://eslint.org/docs/user-guide/configuring#ignoring-files-and-directories)

### `ignorePatterns`  in config files[](https://eslint.org/docs/user-guide/configuring#ignorepatterns-in-config-files)

You can tell ESLint to ignore specific files and directories by  `ignorePatterns`  in your config files. Each value of  `ignorePatterns`  is the same pattern as each line of  `.eslintignore`  in the next section.

```
{
    "ignorePatterns": ["temp.js", "**/vendor/*.js"],
    "rules": {
        //...
    }
}

```

-   The  `ignorePatterns`  property affects only the directory that the config file placed.
-   You cannot write  `ignorePatterns`  property under  `overrides`  property.
-   `.eslintignore`  can override  `ignorePatterns`  property of config files.

If a glob pattern starts with  `/`, the pattern is relative to the base directory of the config file. For example,  `/foo.js`  in  `lib/.eslintrc.json`  matches to  `lib/foo.js`  but not  `lib/subdir/foo.js`.

If a config is provided via the  `--config`  CLI option, the ignore patterns that start with  `/`  in the config are relative to the current working directory rather than the base directory of the given config. For example, if  `--config configs/.eslintrc.json`  is present, the ignore patterns in the config are relative to  `.`  rather than  `./configs`.

### `.eslintignore`[](https://eslint.org/docs/user-guide/configuring#eslintignore)

You can tell ESLint to ignore specific files and directories by creating an  `.eslintignore`  file in your project's root directory. The  `.eslintignore`  file is a plain text file where each line is a glob pattern indicating which paths should be omitted from linting. For example, the following will omit all JavaScript files:

```
**/*.js

```

When ESLint is run, it looks in the current working directory to find an  `.eslintignore`  file before determining which files to lint. If this file is found, then those preferences are applied when traversing directories. Only one  `.eslintignore`  file can be used at a time, so  `.eslintignore`  files other than the one in the current working directory will not be used.

Globs are matched using  [node-ignore](https://github.com/kaelzhang/node-ignore), so a number of features are available:

-   Lines beginning with  `#`  are treated as comments and do not affect ignore patterns.
-   Paths are relative to the current working directory. This is also true of paths passed in via the  `--ignore-pattern`  [command](https://eslint.org/docs/user-guide/command-line-interface#--ignore-pattern).
-   Lines preceded by  `!`  are negated patterns that re-include a pattern that was ignored by an earlier pattern.
-   Ignore patterns behave according to the  `.gitignore`  [specification](https://git-scm.com/docs/gitignore).

Of particular note is that like  `.gitignore`  files, all paths used as patterns for both  `.eslintignore`  and  `--ignore-pattern`  must use forward slashes as their path separators.

```
# Valid
/root/src/*.js

# Invalid
\root\src\*.js

```

Please see  `.gitignore`'s specification for further examples of valid syntax.

In addition to any patterns in the  `.eslintignore`  file, ESLint always follows a couple implicit ignore rules even if the  `--no-ignore`  flag is passed. The implicit rules are as follows:

-   `node_modules/`  is ignored.
-   Dotfiles (except for  `.eslintrc.*`) as well as Dotfolders and their contents are ignored.

There are also some exceptions to these rules:

-   If the path to lint is a glob pattern or directory path and contains a Dotfolder, all Dotfiles and Dotfolders will be linted. This includes sub-dotfiles and sub-dotfolders that are buried deeper in the directory structure.
    
    For example,  `eslint .config/`  will lint all Dotfolders and Dotfiles in the  `.config`  directory, including immediate children as well as children that are deeper in the directory structure.
    
-   If the path to lint is a specific file path and the  `--no-ignore`  flag has been passed, ESLint will lint the file regardless of the implicit ignore rules.
    
    For example,  `eslint .config/my-config-file.js --no-ignore`  will cause  `my-config-file.js`  to be linted. It should be noted that the same command without the  `--no-ignore`  line will not lint the  `my-config-file.js`  file.
    
-   Allowlist and denylist rules specified via  `--ignore-pattern`  or  `.eslintignore`  are prioritized above implicit ignore rules.
    
    For example, in this scenario,  `.build/test.js`  is the desired file to allowlist. Because all Dotfolders and their children are ignored by default,  `.build`  must first be allowlisted so that eslint because aware of its children. Then,  `.build/test.js`  must be explicitly allowlisted, while the rest of the content is denylisted. This is done with the following  `.eslintignore`  file:
    
    ```
    # Allowlist 'test.js' in the '.build' folder
    # But do not allow anything else in the '.build' folder to be linted
    !.build
    .build/*
    !.build/test.js
    
    ```
    
    The following  `--ignore-pattern`  is also equivalent:
    
    ```
    eslint --ignore-pattern '!.build' --ignore-pattern '.build/*' --ignore-pattern '!.build/test.js' parent-folder/
    
    ```
    

### Using an Alternate File[](https://eslint.org/docs/user-guide/configuring#using-an-alternate-file)

If you'd prefer to use a different file than the  `.eslintignore`  in the current working directory, you can specify it on the command line using the  `--ignore-path`  option. For example, you can use  `.jshintignore`  file because it has the same format:

```
eslint --ignore-path .jshintignore file.js

```

You can also use your  `.gitignore`  file:

```
eslint --ignore-path .gitignore file.js

```

Any file that follows the standard ignore file format can be used. Keep in mind that specifying  `--ignore-path`  means that any existing  `.eslintignore`  file will not be used. Note that globbing rules in  `.eslintignore`  follow those of  `.gitignore`.

### Using eslintIgnore in package.json[](https://eslint.org/docs/user-guide/configuring#using-eslintignore-in-package-json)

If an  `.eslintignore`  file is not found and an alternate file is not specified, ESLint will look in package.json for an  `eslintIgnore`  key to check for files to ignore.

```
{
  "name": "mypackage",
  "version": "0.0.1",
  "eslintConfig": {
      "env": {
          "browser": true,
          "node": true
      }
  },
  "eslintIgnore": ["hello.js", "world.js"]
}

```

### Ignored File Warnings[](https://eslint.org/docs/user-guide/configuring#ignored-file-warnings)

When you pass directories to ESLint, files and directories are silently ignored. If you pass a specific file to ESLint, then you will see a warning indicating that the file was skipped. For example, suppose you have an  `.eslintignore`  file that looks like this:

```
foo.js

```

And then you run:

```
eslint foo.js

```

You'll see this warning:

```
foo.js
  0:0  warning  File ignored because of a matching ignore pattern. Use "--no-ignore" to override.

✖ 1 problem (0 errors, 1 warning)

```

This message occurs because ESLint is unsure if you wanted to actually lint the file or not. As the message indicates, you can use  `--no-ignore`  to omit using the ignore rules.

Consider another scenario where you may want to run ESLint on a specific Dotfile or Dotfolder, but have forgotten to specifically allow those files in your  `.eslintignore`  file. You would run something like this:

```
eslint .config/foo.js

```

You would see this warning:

```
.config/foo.js
  0:0  warning  File ignored by default.  Use a negated ignore pattern (like "--ignore-pattern '!<relative/path/to/filename>'") to override

✖ 1 problem (0 errors, 1 warning)

```

This messages occurs because, normally, this file would be ignored by ESLint's implicit ignore rules (as mentioned above). A negated ignore rule in your  `.eslintignore`  file would override the implicit rule and reinclude this file for linting. Additionally, in this specific case,  `--no-ignore`  could be used to lint the file as well.

## Personal Configuration File (deprecated)[](https://eslint.org/docs/user-guide/configuring#personal-configuration-file-deprecated)

⚠️  **This feature has been deprecated**. This feature will be removed in the 8.0.0 release. If you want to continue to use personal configuration files, please use the  [`--config`  CLI option](https://eslint.org/docs/user-guide/command-line-interface#-c---config). For more information regarding this decision, please see  [RFC 28](https://github.com/eslint/rfcs/pull/28)  and  [RFC 32](https://github.com/eslint/rfcs/pull/32).

`~/`  refers to  [the home directory of the current user on your preferred operating system](https://nodejs.org/api/os.html#os_os_homedir). The personal configuration file being referred to here is  `~/.eslintrc.*`  file, which is currently handled differently than other configuration files.

### How ESLint Finds Personal Configuration File[](https://eslint.org/docs/user-guide/configuring#how-eslint-finds-personal-configuration-file)

If  `eslint`  could not find any configuration file in the project,  `eslint`  loads  `~/.eslintrc.*`  file.

If  `eslint`  could find configuration files in the project,  `eslint`  ignores  `~/.eslintrc.*`  file even if it's in an ancestor directory of the project directory.

### How Personal Configuration File Behaves[](https://eslint.org/docs/user-guide/configuring#how-personal-configuration-file-behaves)

`~/.eslintrc.*`  files behave similarly to regular configuration files, with some exceptions:

`~/.eslintrc.*`  files load shareable configs and custom parsers from  `~/node_modules/`  – similarly to  `require()`  – in the user's home directory. Please note that it doesn't load global-installed packages.

`~/.eslintrc.*`  files load plugins from  `$CWD/node_modules`  by default in order to identify plugins uniquely. If you want to use plugins with  `~/.eslintrc.*`  files, plugins must be installed locally per project. Alternatively, you can use the  [`--resolve-plugins-relative-to`  CLI option](https://eslint.org/docs/user-guide/command-line-interface#--resolve-plugins-relative-to)  to change the location from which ESLint loads plugins.