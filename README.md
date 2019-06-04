# WalletConnect
============

[WalletConnect](https://walletconnect.org/) Swift SDK, implements 1.0.0 websocket based protocol.

### Demo video

<a href="https://www.youtube.com/watch?v=sFZzzNDLd8Y" ><img src="https://img.youtube.com/vi/sFZzzNDLd8Y/maxresdefault.jpg" width="90%"></a>

## Requirements

- iOS 11
- Xcode 10.2
- Swift 5

## Features

- [x] Connect and disconnect
- [x] Approve / Reject / Kill session
- [x] Approve and reject `eth_sign` / `personal_sign` / `eth_signTypedData`
- [x] Approve and reject `eth_signTransaction` / `eth_sendTransaction`
- [x] Approve and reject `bnb_sign` (binance dex orders)

Todo:

- [ ] Create Session
- [ ] session persistent / recovery
- [ ] push notification (APNS)

## Example

To run the example project, clone the repo, and run `pod install` from the Example directory first.

## Installation

WalletConnect is available through [CocoaPods](https://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'WalletConnect', git: 'https://github.com/WalletConnect/swift-walletconnect-lib', branch: 'master'
```

## Usage

parse session from scanned QR code:

```swift
let string = "wc:..."
guard let session = WCSession.from(string: string) else {
    // invalid session
    return
}
// handle session
```

configure and handle incoming message:

```swift
let interactor = WCInteractor(session: session, meta: clientMeta)
interactor.onSessionRequest = { [weak self] (id, peer) in
    // ask for user consent
}

interactor.onDisconnect = { [weak self] (error) in
    // handle disconnect
}

interactor.onEthSign = { [weak self] (id, payload) in
    // handle eth_sign, personal_sign, eth_signTypedData
}

interactor.onEthTransaction = { [weak self] (id, event, transaction) in
    // handle eth_signTransaction / eth_sendTransaction
}

interactor.onBnbSign = { [weak self] (id, order) in
    // handle bnb_sign
}
```

approve session

```swift
interactor.approveSession(accounts: accounts, chainId: chainId).done {
    print("<== approveSession done")
}.cauterize()
```

approve request

```swift
interactor.approveRequest(id: id, result: result.hexString).done {
    print("<== approveRequest done")
}.cauterize()
```

approve binance dex orders

```swift
interactor?.approveBnbOrder(id: id, signed: signed).done({ confirm in
    print("<== approveBnbOrder", confirm)
}).cauterize()
```

## Author

hewigovens

## License

WalletConnect is available under the MIT license. See the LICENSE file for more info.
