---
title: "Listen VPN connection in Android"
date: 2023-10-20
tags: ["Android"]
draft: false
image: "/common/android.png"
---

![android](/common/android.png)

# The reason

I found that my VPN app has a bug that when disconnecting the VPN from settings (Network & internet -> VPN), my VpnService wouldn't be killed.

Finally, I got the cause, it's because the `VpnService.onRevoke()` was not called when the VPN was killed from settings.

So I was thinking, is there any way to listen VPN connection in Android?

## Here is what I got:

### Listen VPN connection using `ConnectivityManager`

Close the VPN connection when `onLost` was called.

```
// In VpnService

// Callback
private val networkCallback = object : NetworkCallback() {
    override fun onAvailable(network: Network) {
        Log.i("Network onAvailable: ")
    }

    override fun onLost(network: Network) {
        Log.i("Network onLost: ")
        // Do something to stop VPN 
    }
}

// Start listen VPN connection
override fun onCreate() {
    val request = NetworkRequest.Builder()
        .addTransportType(TRANSPORT_VPN)
        .removeCapability(NET_CAPABILITY_NOT_VPN)
        .removeCapability(NET_CAPABILITY_INTERNET)
        .build()

    getSystemService(ConnectivityManager::class.java)
        .registerNetworkCallback(request, networkCallback)
}

// Stop listen VPN connection
override fun onDestroy() {
    getSystemService(ConnectivityManager::class.java)
        .unregisterNetworkCallback(networkCallback)
}
```

### Call `onRevoke` when `onTransact` was triggered in `Binder`

Also, I can use `onTransact` to close the VPN connection when users disconnect the VPN from settings.

I'm using a customized `Binder` for communicating to my `VpnService`, it will trigger the `onTransact` when the VPN was killed from settings.

```
override fun onTransact(code: Int, data: Parcel, reply: Parcel?, flags: Int): Boolean {
    if (code == IBinder.LAST_CALL_TRANSACTION) {
        onRevoke()
        return true
    }
    return super.onTransact(code, data, reply, flags)
}    
```

### References

- [Register a callback so we will be notified when our VPN comes up.](https://android.googlesource.com/platform/cts/+/4a305d5%5E%21/)
- [ConnectivityManager.NetworkCallback calls missing when a VPN is connected/disconnected](https://stackoverflow.com/a/65122254)
- [How to add additional handlings to Vpn disconnection in Android?](https://stackoverflow.com/a/15731435)
