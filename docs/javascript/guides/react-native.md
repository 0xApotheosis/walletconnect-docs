# React Native

## Install

:::info

For React Native, the WalletConnect client additionally requires `@react-native-async-storage/async-storage`
to manage storage internally.

:::

```bash npm2yarn
npm install --save @walletconnect/client@experimental @react-native-async-storage/async-storage
```

## Create Session

1. Initiate your WalletConnect client with the relay server, using [your Project ID](../../advanced/api-reference/project-id.md).

```javascript
import WalletConnectClient from "@walletconnect/client";

const client = await WalletConnectClient.init({
  controller: true,
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
