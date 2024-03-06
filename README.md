# XMTP Privy Quickstart

![xmtp](https://github.com/xmtp/xmtp-quickstart-reactjs/assets/1447073/3f2979ec-4d13-4c3d-bf20-deab3b2ffaa1)

## Installation

```bash
yarn install
yarn dev
```

## Concepts

Head to our docs to understand XMTP's concepts

- [Get started](https://xmtp.org/docs/build/get-started/overview?sdk=react)
- [Authentication](https://xmtp.org/docs/build/authentication?sdk=react)
- [Conversations](https://xmtp.org/docs/build/conversations?sdk=react)
- [Messages](https://xmtp.org/docs/build/messages/?sdk=react)
- [Streams](https://xmtp.org/docs/build/streams/?sdk=react)

#### Troubleshooting

If you get into issues with `Buffer` and `polyfills` check out the fix below:

- [Check out Buffer issue](https://github.com/xmtp/xmtp-js/issues/487)

## Privy

### Setup

First, you need to import the necessary libraries and components. In your index.js file, import the `PrivyProvider` from @privy-io/react-auth and wrap your main component with it.

```jsx
import { PrivyProvider } from "@privy-io/react-auth";
```

```jsx
<PrivyProvider
appId={process.env.REACT_APP_PRIVY_APP_ID}
onSuccess={(user) => console.log(User ${user.id} logged in!)}
>
<InboxPage />
</PrivyProvider>
```

### User authentication

In your main component, use the `usePrivy` hook to get the user's authentication status and other details.

```jsx
const { ready, authenticated, user, login, logout } = usePrivy();
```

### Wallet integration

Use the `useWallets` hook to get the user's wallets. Then, find the embedded wallet and set it as the signer.

```jsx
useEffect(() => {
  const getSigner = async () => {
    const embeddedWallet =
      wallets.find((wallet) => wallet.walletClientType === "privy") ||
      wallets[0];
    if (embeddedWallet) {
      const provider = await embeddedWallet.getEthersProvider();
      setSigner(provider.getSigner());
    }
  };

  if (wallets.length > 0) {
    getSigner();
  }
```

### XMTP Integration

In your `Home` component, use the `useClient` hook from `@xmtp/react-sdk` to get the XMTP client.

```jsx
const { client, error, isLoading, initialize } = useClient();
```

That's it! You've now created an XMTP app with Privy.
