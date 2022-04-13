# Node.js

## Install

:::info

For Node.js, the WalletConnect client additionally requires `better-sqlite3` to manage storage internally.

:::

```bash npm2yarn
npm install --save @walletconnect/client@experimental better-sqlite3
```

## Create Session

1. Initiate your WalletConnect client with the relay server, using [your Project ID](../../advanced/api-reference/project-id.md).

```javascript
import WalletConnectClient from "@walletconnect/client";

const client = await WalletConnectClient.init({
  projectId: "c4f79cc...",
  relayUrl: "wss://relay.walletconnect.com",
  metadata: {
    name: "Test Wallet",
    description: "Test Wallet",
    url: "#",
    icons: ["https://walletconnect.com/walletconnect-logo.png"],
  },
});
```
