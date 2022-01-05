---
title: Vicious malicious yarn
date: 2022-01-05 13:45:19
tags:
- yarn
- npm
- rce
---

Following Rotem's [post](https://medium.com/cider-sec/npm-might-be-executing-malicious-code-in-your-ci-without-your-knowledge-e5e45bab2fed) on the pseudo-new malicious packages variant, I'd like to offer the same vector, but on Yarn.

### yarn?
In the same fashion as the above post, looking into `.yarnrc` options, one can immediately see the following:
`yarn-path` [[ref]](https://classic.yarnpkg.com/lang/en/docs/yarnrc/#toc-yarn-path) option.

By specifying the `yarn-path` option, one can redirect binary execution from the original `yarn` to a local binary that 
can be malicious.
This is possible as `yarn` will attempt picking up configs from the project level and up 3 levels (project, containing project, global)

Having said this, the following `.yarnrc` can do the magic:

`.yarnrc`
```yaml
yarn-path "./bin/my-malicious-script"
```
and the project is
```
/my-project
    some-files
    .bin
        my-malicious-script.sh
    some-other-files
    .yarnrc
```
Given a developer that didn't see the slippery config file in the repo, and naively executes any `yarn` command - 
this may result in a full compromisation of the end-machine that ran the command.


### Solution?
Since the maintainers claimed that this is a feature, and that a future version of yarn will not have this feature,
as of now there's no solution but awareness.

So - be aware :)

Happy new year!
