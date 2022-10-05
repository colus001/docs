---
weight: 10
title: Get Started with xpla.js
---

# Get Started with xpla.js

This is an in-depth guide on how to use the `xpla.js` SDK.

In this tutorial, you'll learn how to:

1. [Set up your project](#1-set-up-your-project)
2. [Set up a XPLA Chain LCD (light client daemon)](#2-initialize-the-lcd)
3. [Create and connect a wallet](#3-create-a-cube-testnet-wallet)
4. [Query a contract](#4-query-a-contract-and-set-up-the-transaction)
5. [Create, sign, and broadcast a transaction](#5-broadcast-the-transaction)

By the end of this guide, you'll be able to execute a token swap from your application using xpla.js.

## Prerequisites

- [npm and node.js](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
- XPLA extension vault

## 1. Set-up Your Project

1. Create a new directory for your project:

   ```sh
   mkdir my-xpla-js-project

   ```

2. Enter your new project directory:

   ```sh
   cd <my-xpla-js-project>
   ```

3. Next, initialize npm, install the `xpla.js` package, and create an `index.js` file to house the code:

   ```sh
   npm init -y
   npm install @xpla/xpla.js
   touch index.js
   ```

4. Open the `package.json` file in a code editor and add `"type": "module",`.

   ```json
   {
     // ...
     "type": "module"
     // ...
   }
   ```

## 2. Initialize the LCD

XPLA Chain’s LCD or Light Client Daemon allows users to connect to the blockchain, make queries, create wallets, and submit transactions. It's the main workhorse behind `xpla.js`.

1. Install a fetch library to make HTTP requests and dynamically pull recommended gas prices. You can use the one wallet referenced below or choose your favorite.

   ```sh
   npm install --save isomorphic-fetch
   ```

2. Open your `index.js` file in a code editor and input the following to initialize the LCD:

   ```ts
   import fetch from "isomorphic-fetch";
   import { Coins, LCDClient } from "@xpla/xpla.js";
   const gasPrices = await fetch(
     "https://cube-fcd.xpla.dev/v1/txs/gas_prices", { redirect: 'follow' }
   );
   const gasPricesJson = await gasPrices.json();
   const gasPricesCoins = new Coins(gasPricesJson);
   const lcd = new LCDClient({
     URL: "https://cube-lcd.xpla.dev", // Use "https://dimension-lcd.xpla.dev" for prod
     chainID: "cube_47-5", // Use "dimension_37-1" for production
     gasPrices: gasPricesCoins,
     gasAdjustment: "1.5", // Increase gas price slightly so transactions go through smoothly.
     gas: 10000000,
   });
   ```

   {{< hint info >}}
   **Note**  
   The previous code block shows how to connect to the cube testnet. To connect to the dimension_37-1 mainnet for production, use “`https://dimension-lcd.xpla.dev`”.
   You will also need to change the `chainID` from `"cube_47-5"` to `"dimension_37-1"`.
   {{< /hint >}}

## 3. Create a Cube Testnet Wallet

1. You'll need a wallet to sign and submit transactions. [Create a new wallet]({{< ref "/docs/learn/xpla-vault/download/extension-vault" >}}) using the XPLA extension vault. Be sure to save your mnemonic key!

2. After creating your wallet, you’ll need to set it to use the testnet. Click the gear icon in the extension and change the network from `mainnet` to `testnet`.

3. Add the following code to your `index.js` file and input your mnemonic key:

   ```ts
   import { MnemonicKey } from "@xpla/xpla.js";
   const mk = new MnemonicKey({
     mnemonic: " //Input your 24-word mnemonic key here//",
   });
   const wallet = lcd.wallet(mk);
   ```

   {{< hint warning >}}
   **Warning**  
   Although this tutorial has you input your mnemonic directly, this practice should be avoided in production.
   For security reasons, it's better to store your mnemonic key data in your environment by using `process.env.SECRET_MNEMONIC` or `process.env.SECRET_PRIV_KEY`. This practice is more secure than a hard-coded string.
   {{< /hint >}}

4. Request testnet funds for your wallet by navigating to the [XPLA faucet](https://faucet.xpla.io) and inputting your wallet address. You'll need these funds to perform swaps and pay for gas fees. Once the funds are in your wallet, you’re ready to move on to the next step.

## 4. Query a Contract and Set-up the Transaction

Before you can perform a transaction, you can check the token information

1. Add the following code to your `index.js` file. Make sure the contract address is correct.

   ```ts
   const contract = "<TOKEN_CONTRACT_ADDRESS>"; // A token contract address on cube.
   const info = await lcd.wasm.contractQuery(contract, { token_info: {} }); // Query
   ```

2. Next, generate a message to broadcast to the network:

   ```ts
   import { MsgExecuteContract } from "@xpla/xpla.js";
   const txMsg = new MsgExecuteContract(
     wallet.key.accAddress,
     contract,
     {
       transfer: {
         amount: "10",
         recipient: "<RECIPIENT_ADDRESS>"
       }
     }
   );
   ```

## 5. Broadcast the Transaction

1. Add the following code to `index.js` to create, sign, and broadcast the transaction. It's important to specify `axpla` as the fee denomination because XPLA is the only denomination the faucet sends.

   ```ts
   const tx = await wallet.createAndSignTx({
     msgs: [txMsg],
     feeDenoms: ["axpla"],
   });
   const result = await lcd.tx.broadcast(tx);
   console.log(result);
   ```

2. Run the code in your terminal:

   ```sh
   node index.js
   ```

If successful, you'll see a log of the successful transaction in your wallet and that's it!

Try executing a transaction with other contracts as well for practice. Be sure to use the correct testnet or mainnet contract address.

## More Examples

View the [Common examples]({{< ref "common-examples" >}}) section for more information on using xpla.js.
