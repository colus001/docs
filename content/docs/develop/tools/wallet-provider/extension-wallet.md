---
weight: 30
title: Extension Wallet
---

# Extension Wallet

The API for the XPLA extension vault gets updated periodically. If you are developing a dApp, please check regularly for updates as breaking changes may be introduced frequently.

## What Is the XPLA Extension Vault?

The XPLA extension vault is a web-wallet extension for Chrome that enables webpages to create requests for signing and broadcasting transactions. The webpage can detect the presence of XPLA extension vault and generate a prompt whereby the user can confirm the transaction to be signed.

## Wallet Provider

Wallet Provider is a library that simplifies the development of React dApps that use XPLA extension vault or XPLA mobile vault.

Use one of the following templates to get quickly get started using Wallet Provider:

### Create React App

```sh
npx copy-github-directory https://github.com/xpladev/wallet-provider/tree/main/templates/create-react-app your-app-name
cd your-app-name
yarn install
yarn start
```

[Learn more](https://github.com/xpladev/wallet-provider/tree/main/templates/create-react-app)

### Next.js

```sh
npx copy-github-directory https://github.com/xpladev/wallet-provider/tree/main/templates/next your-app-name
cd your-app-name
yarn install
yarn run dev
```

[Learn more](https://github.com/xpladev/wallet-provider/tree/main/templates/next)

### Experimental Templates

- [Wallet Provider + Vite.js](https://github.com/xpladev/wallet-provider/tree/main/templates/vite)
- [Wallet Controller](https://github.com/xpladev/wallet-provider/tree/main/templates/wallet-controller)

{{< hint info >}}
**Tip**  
The Wallet Controller template is an example of how WalletController behaves underneath the React API. If you are unable to use React, start by using the Wallet Controller template.
{{< /hint >}}

## Usage

Visit the Wallet Provider GitHub for more details on using the APIs provided:

[https://github.com/xpladev/wallet-provider](https://github.com/xpladev/wallet-provider#basic-usage)
