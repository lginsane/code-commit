# code-commit

代码 commit 前检验处理 yorkie/husky + lint-staged

## eslint + commitlint + lint-staged

- 安装

```$
yarn add eslint @commitlint/cli @commitlint/config-conventional lint-staged -S
```

- 初始化配置

```$
# 第一步初始化 eslint
./node_modules/.bin/eslint --init

# 第三步初始化 commitlint
echo "module.exports = { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
```

- 具体配置

1. `package.json` 添加 `lint-staged` 配置

```json
{
    // ...
    "lint-staged": {
        "*.js": "eslint --fix"
    }
}
```

## husky

- 下载

```$
yarn add husky -S
```

- 初始化 husky

```.husky
./node_modules/.bin/husky install
```

- `.husky` 文件下新增 commit-msg

```.husky/commit-msg
# fileName: .husky/commit-msg

#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx --no-install commitlint --edit 
```

- `.husky` 文件下新增 pre-commit

```.husky/pre-commit
# fileName: .husky/pre-commit

#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx lint-staged
```

### 问题

1. husky 不执行。 警告信息：`git config advice.ignoredHook false`

  [解决办法, 如下](https://github.com/typicode/husky/issues/1113)

```$
chmod +x .husky/pre-commit
# and
chmod +x .husky/commit-msg
```

## yorkie

- 下载

```$
yarn add yorkie -S
```

- `package.json`配置

```json
{
    // ...
    "gitHooks": {
        "commit-msg": "commitlint --edit $1",
        "pre-commit": "lint-staged"
    }
}
```
