# .idea/rc-producer.yml

NodeJS Run Configuration Producer for IntelliJ Platform (IntelliJ IDEA, WebStorm — 2017.1+).

This repository contains docs and sample configurations for various tools (e.g. Jest, Ava). 

Want to share your config? Send a [pull request](https://github.com/develar/ij-rc-producer/pulls).

## Usage

Create file `.idea/rc-producer.yml` in your project root (where `.idea` is a IntelliJ Platform project configuration directory).

Sample files for: [Jest](https://github.com/develar/ij-rc-producer/blob/master/jest/rc-producer.yml)

In most cases you just need to change `files` according to your needs.

<img width="442" alt="screen shot 2016-12-06 at 11 20 40" src="https://cloud.githubusercontent.com/assets/350686/20921813/195b0168-bba6-11e6-9076-8de8bd1dd0e9.png">

[Option object](https://github.com/develar/ij-rc-producer/blob/master/rc-producer.yml) or list of option objects are expected on root level.

Example of option object list:
```yaml
-
  files: ["test/src/**/*", "!**/helpers/**/*",]
-
  files: ["bar",]
```

## Common Options
YAML allows you to avoid duplication — [Merging Options](http://atechie.net/2009/07/merging-hashes-in-yaml-conf-files/). 

You can override:

```yaml
- &defaults
  files: ["test/src/**/*", "!**/helpers/**/*"]
  script: "node_modules/.bin/jest"
  scriptArgs: ["-i", "--env", "jest-environment-node-debug", &filePattern '/${fileNameWithoutExt}\.\w+$']
  rcName: "${fileNameWithoutExt}"
  beforeRun: typescript

-
  <<: *defaults
  lineRegExp: '^\s*(?:test|it)(?:\.\w+)?\("([^"'']+)'
  scriptArgs: ["-i", "--env", "jest-environment-node-debug", "-t", "${0regExp}", *filePattern]
  rcName: "${fileNameWithoutExt}.${0}"
```


Or move list of option objects to property `rc`:

```yaml
defaults: &defaults
  files: ["test/src/**/*", "!**/helpers/**/*"]
  script: "node_modules/.bin/jest"
  beforeRun: typescript

rc:
  -
    <<: *defaults
    lineRegExp: '^\s*(?:test|it)(?:\.\w+)?\("([^"'']+)'
    scriptArgs: ["-i", "-t", "${0regExp}", "${fileNameWithoutExt}"]
    rcName: "${fileNameWithoutExt}.${0}"

  -
    <<: *defaults
    scriptArgs: ["-i", "${fileNameWithoutExt}"]
    rcName: "${fileNameWithoutExt}"
```
