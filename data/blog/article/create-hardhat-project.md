---
title: 创建 Hardhat 项目
date: '2023-06-09'
tags: ['solidity', 'blockchain', 'ethereum', 'hardhat']
draft: false
summary: Create New Hardhat(TS) + Solidity Project.
---

# 创建 Hardhat 项目

## 一、创建并初始化 hardhat 项目

1. 创建文件夹并初始化

```
mkdir dev-project
cd ./dev-project
yarn init -y
```

2. 安装 **hardhat**

```js
yarn add hardhat
```

3. 初始化 **hardhat** 项目

```js
yarn hardhat
```

运行后会出现以下代码：

```js
888    888                      888 888               888
888    888                      888 888               888
888    888                      888 888               888
8888888888  8888b.  888d888 .d88888 88888b.   8888b.  888888
888    888     "88b 888P"  d88" 888 888 "88b     "88b 888
888    888 .d888888 888    888  888 888  888 .d888888 888
888    888 888  888 888    Y88b 888 888  888 888  888 Y88b.
888    888 "Y888888 888     "Y88888 888  888 "Y888888  "Y888

👷 Welcome to Hardhat v2.15.0 👷‍

? What do you want to do? …
❯ Create a JavaScript project
  Create a TypeScript project
  Create an empty hardhat.config.js
  Quit
```

这里选择 **Create a TypeScript project**；

接着需要选择 **Hardhat 目录**，直接回车则是 **当前** 目录；

```js
Hardhat project root: ›
```

这里是添加 **.gitignore 文件** ，回车确认；

```js
Do you want to add a .gitignore? (Y/n) › y
```

接下来是安装所需依赖，继续回车；

```
Do you want to install this sample project's dependencies with yarn .........
```

出现以下文字则是安装完成；

```
✨ Project created ✨
```

4. 文件目录说明

**contracts**：存放合约文件；

**scripts**：存放部署脚本文件；

**test**：存放测试脚本文件；

**hardhat.config.ts**：hardhat 配置文件；

**tsconfig.json**：TypeScript 配置文件；

**typechain-types**：存放合约 TypeScript 类型文件；

**artifacts**：存放合约的 ABI 文件和二进制代码文件；

**cache**：存放 Hardhat 的缓存文件；

## 二、安装工具插件和配置项目

安装工具及 Hardhat 插件：

1. [**hardhat-deploy**](https://www.npmjs.com/package/hardhat-deploy)
2. [**hardhat-gas-reporter**](https://www.npmjs.com/package/hardhat-gas-reporter)
3. [**solidity-coverage**](https://www.npmjs.com/package/solidity-coverage)
4. [**dotenv**](https://www.npmjs.com/package/dotenv)
5. [**prettier**](https://www.npmjs.com/package/prettier)
6. [**prettier-plugin-solidity**](https://www.npmjs.com/package/prettier-plugin-solidity)
7. [**solhint**](https://www.npmjs.com/package/solhint)
8. [**solhint-plugin-prettier**](https://www.npmjs.com/package/solhint-plugin-prettier)

安装依赖：

```
yarn add hardhat-deploy hardhat-gas-reporter solidity-coverage prettier prettier-plugin-solidity solhint solhint-plugin-prettier dotenv @openzeppelin/contracts
```

添加文件 **.prettierrc.json**；

```json
{
  "overrides": [
    {
      "files": "*.sol",
      "options": {
        "printWidth": 120,
        "tabWidth": 4,
        "useTabs": false,
        "singleQuote": false,
        "bracketSpacing": false
      }
    }
  ]
}
```

编辑 **.gitignore** 或者 添加文件 **.prettierignore**；

```
node_modules
.env
coverage
coverage.json
typechain
typechain-types

# Hardhat files
cache
artifacts
# MacOS file
.DS_Store
```

添加一个 **.solhint.json** 配置文件

```json
{
  "extends": "solhint:default",
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error"
  }
}
```

**hardhat.config.ts** 引入及配置；

```typescript
import { HardhatUserConfig } from 'hardhat/config'
import '@nomicfoundation/hardhat-toolbox'
import 'hardhat-deploy'
import 'hardhat-gas-reporter'
import 'solidity-coverage'
import 'dotenv/config'

const config: HardhatUserConfig = {
  solidity: { version: '0.8.19' },
  namedAccounts: {
    deployer: 0,
    tokenOwner: 1,
  },
  networks: {
    hardhat: {
      chainId: 1337,
    },
    testnet: {
      url: process.env.RPC_URI_TEST || '',
      accounts: [process.env.PRIVATE_KEY_TEST || ''],
    },
    mainnet: {
      url: process.env.RPC_URI_MAIN || '',
      accounts: [process.env.PRIVATE_KEY_MAIN || ''],
    },
  },
  etherscan: {
    // Your API key for Etherscan
    // Obtain one at https://bscscan.com/
    apiKey: process.env.API_KEY || '',
  },
  gasReporter: {
    enabled: process.env.REPORT_GAS ? true : false,
    currency: 'USD',
  },
}

export default config
```

添加 **.env** 和 **.env.example** 文件 (根据需求补全以下配置，.env.example 不补全)；

```
API_KEY=
REPORT_GAS=
RPC_URI_MAIN=
PRIVATE_KEY_MAIN=
RPC_URI_TEST=
PRIVATE_KEY_TEST=
```

> ⚠️ 注：安装 hardhat-deploy 插件后当前部署文件和测试文件可能会报错，后续开发需引入插件修改代码，参考 hardhat-deploy 例子：https://github.com/wighawag/template-ethereum-contracts；
