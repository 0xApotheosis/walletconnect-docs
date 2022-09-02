# Dapp Usage


### **Initialize Sign Client**

```kotlin
val appMetaData = Sign.Model.AppMetaData(
    name = "Wallet Name",
    description = "Wallet Description",
    url = "Wallet Url",
    icons = listOfIconUrlStrings
)

val connectionType = Sign.ConnectionType.AUTOMATIC or Sign.ConnectionType.MANUAL

val init = Sign.Params.Init(
    application = application,
    relayServerUrl = /*websocket server with scheme, authority, and projectId as query parameter*/
    appMetaData = appMetaData,
    connectionType = connectionType
)

// or

val init = Sign.Params.Init(
    application = application,
    useTls = /*true or false*/,
    hostName = /*websocket server with scheme and authority*/,
    projectId = /*projectId*/,
    appMetaData = appMetaData,
    connectionType = connectionType
)

SignClient.initalize(init)
```

The wallet client will always be responsible for exposing accounts (CAPI10 compatible) to a Dapp and therefore is also in charge of signing.
To initialize the Sign client, create a `Sign.Params.Init` object in the Android Application class. The Init object will need the
application class, the Project ID, and the apps's AppMetaData. The `Sign.Params.Init` object will then be passed to the `SignClient`
initialize function. `Sign.Params.Init` also allows for custom URLs by passing URL string into the `hostName` property.

We allow developers to choose between the `Sign.ConnectionType.MANUAL` and `Sign.ConnectionType.AUTOMATIC`connection type. The default
one(`Sign.ConnectionType.AUTOMATIC`) disconnects wss connection when app enters background and reconnects when app is brought back to the
foreground. The `Sign.ConnectionType.MANUAL` allows developers to control when to open WebSocket connection and when to close it.
Accordingly, `SignClient.WebSocket.open()` and `SignClient.WebSocket.close()`.

Above, there are two example on how to create the initalizing parameters.



## **Dapp**

### **SignClient.DappDelegate**

```kotlin
val dappDelegate = object : SignClient.DappDelegate {
    override fun onSessionApproved(approvedSession: Sign.Model.ApprovedSession) {
        // Triggered when Dapp receives the session approval from wallet
    }

    override fun onSessionRejected(rejectedSession: Sign.Model.RejectedSession) {
        // Triggered when Dapp receives the session rejection from wallet
    }

    override fun onSessionUpdate(updatedSession: Sign.Model.UpdatedSession) {
        // Triggered when Dapp receives the session update from wallet
    }

    override fun onSessionExtend(session: Sign.Model.Session) {
        // Triggered when Dapp receives the session extend from wallet
    }

    override fun onSessionEvent(sessionEvent: Sign.Model.SessionEvent) {
        // Triggered when the peer emits events that match the list of events agreed upon session settlement
    }

    override fun onSessionDelete(deletedSession: Sign.Model.DeletedSession) {
        // Triggered when Dapp receives the session delete from wallet
    }

    override fun onSessionRequestResponse(response: Sign.Model.SessionRequestResponse) {
        // Triggered when Dapp receives the session request response from wallet
    }

    override fun onConnectionStateChange(state: Sign.Model.ConnectionState) {
        //Triggered whenever the connection state is changed
    }
}
SignClient.setWalletDelegate(dappDelegate)
```

The SignClient needs a `SignClient.DappDelegate` passed to it for it to be able to expose asynchronously updates sent from the Wallet.



### **Connect**

```kotlin
val namespace: String = /*Namespace identifier, see for reference: https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md#syntax*/
val chains: List<String> = /*List of chains that wallet will be requested for*/
val methods: List<String> = /*List of methods that wallet will be requested for*/
val events: List<String> = /*List of events that wallet will be requested for*/
val namespaces: Map<String, Sign.Model.Namespaces.Proposal> = mapOf(namespace, Sign.Model.Namespaces.Proposal(accounts, methods, events))
val pairingTopic: String? =  /* Optional parameter, use it when the pairing between peers is already established*/
val connectParams = Sign.Params.Connect(namespaces, pairingTopic)

fun SignClient.connect(connectParams, { proposedSequence -> /*callback that returns the WalletConnect.Model.ProposedSequence*/ }, { error -> /*callback for error while sending session proposal*/ })
```

The `SignClient.connect` asynchronously exposes the pairing URI that is shared with wallet out of bound, as qr code or mobile linking. The
Sign.Model.ProposedSequence returns either a Pairing or Session flag depending on if there is already an established pairing between peers.
To establish a session between peers, pass the existing pairing's topic to the connect method. The SDK will send the SessionProposal under
the hood for the given topic and expect session approval or rejection in onSessionApproved and onSessionRejected in DappDelegate
accordingly.



### **Get List of Settled Sessions**

```kotlin
SignClient.getListOfSettledSessions()
```

To get a list of the most current settled sessions, call `SignClient.getListOfSettledSessions()` which will return a list of type `Session`.



### **Get List of Settled Pairings**

```kotlin
SignClient.getListOfSettledPairings()
```

To get a list of the most current settled pairings, call `SignClient.getListOfSettledPairings()` which will return a list of type `Pairing`.



### **Get list of pending session requests for a topic**

```kotlin
SignClient.getPendingRequests(topic: String)
```

To get a list of pending session requests for a topic, call `SignClient.getPendingRequests()` and pass a topic which will return
a `PendingRequest` object containing requestId, method, chainIs and params for pending request.



## Project ID

For the Project ID look at [Project ID](https://www.walletconnect.com).
