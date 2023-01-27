# FAQ

## How do I add native wallet to web3modal?

Depending on user's device, Web3Modal will show a selection of native or desktop browsers that can be deep / universally linked into. Users can also scan qr code no matter which device they are on.

In order for your wallet to show up in Web3Modal, you need to register it at our [Cloud Dashboard](https://cloud.walletconnect.com/). Make sure to specify correct data and pay extra attention to deeplink and universal link section.

When users click on your wallet within Web3Modal, they will be redirected to your deeplink / universal link with appended `wc` URI query. You are responsible for handling this redirect and processing such query.

Example deep link (preferred for desktop wallets)

```
mywallet://wc?uri=<URI_ENCODED_WC_PAIRING_URL>
```

Example universal link (preferred for mobile wallets)

```
https://mywallet.com/wc?uri=<URI_ENCODED_WC_PAIRING_URL>
```

## How do I add injected / extension wallet to web3modal?

coming soon to [Cloud Dashboard](https://cloud.walletconnect.com/)

## What is the difference between standalone and other packages?

Standalone is a very lightweight package, that doesn't include wagmi as dependency, thus it also doesn't handle injected / extension wallets, wagmi connectors or ethereum providers. This is pure WalletConnect experience that you can use if you already handle everything above yourself.

## How can I get ethersjs provider?

Use wagmi's `useProvider` or `getProvider` actions to get ethersjs provider.

## How can I get web3js provider?

You will first need to get ethersjs provider from wagmi and then convert it to web3js compatible one. Perhaps you'll find package like [ethers-to-web3](https://www.npmjs.com/package/ethers-to-web3) useful.

## Do you support cdn script installation?

Not at the moment, but while we are working on one you can easily compile the library using tools like [Vite](https://vitejs.dev/) or [Webpack](https://webpack.js.org/).