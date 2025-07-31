---
title: MultiplayerPeer SteamPacketPeer
description: A class reference for MultiplayerPeer SteamPacketPeer.
icon: material/package-variant
---

# MultiplayerPeer SteamPacketPeer

!!! info "Only available in the [GodotSteam MultiplayerPeer repo](https://github.com/GodotSteam/MultiplayerPeer){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### disconnect_peer

!!! function "disconnect_peer( `bool` force )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | force | bool | Whether or not to force the disconnection.

    Disconnects from this peer.  Calls [closeConnections](networking_sockets.md#closeconnections) under-the-hood.

    If **force** is false, it will set the state to STATE_LINGER.

	!!! returns "Returns: void"

### get_peer_id

!!! function "get_peer_id( )"
	Gets the internally-stored peer ID.

	!!! returns "Returns: uint32_t"

### get_steam_id

!!! function "get_steam_id( )"
	Gets the internally-stored Steam ID.

	!!! returns "Returns: uint64_t"

{==
## :material-numeric: Enums
==}

### PeerState
High level connection status. Found in steamnetworkingtypes.h as ESteamNetworkingConnectionState.

Enumerator | SDK Name | Value | Details
---------- | -------- | ----- | -------
STATE_NONE | k_ESteamNetworkingConnectionState_None | 0 | Dummy value used to indicate an error condition in the API. Specified connection doesn't exist or has already been closed.
STATE_CONNECTING | k_ESteamNetworkingConnectionState_Connecting | 1 | We are trying to establish whether peers can talk to each other, whether they WANT to talk to each other, perform basic auth, and exchange crypt keys. For connections on the "client" side (initiated locally): We're in the process of trying to establish a connection. Depending on the connection type, we might not know who they are. Note that it is not possible to tell if we are waiting on the network to complete handshake packets, or for the application layer to accept the connection. For connections on the "server" side (accepted through listen socket): We have completed some basic handshake and the client has presented some proof of identity.  The connection is ready to be accepted using [acceptConnection](#acceptconnection). In either case, any unreliable packets sent now are almost certain to be dropped.  Attempts to receive packets are guaranteed to fail. You may send messages if the send mode allows for them to be queued. but if you close the connection before the connection is actually established, any queued messages will be discarded immediately. (We will not attempt to flush the queue and confirm delivery to the remote host, which ordinarily happens when a connection is closed.)
STATE_FINDING_ROUTE | k_ESteamNetworkingConnectionState_FindingRoute | 2 | Some connection types use a back channel or trusted 3rd party for earliest communication.  If the server accepts the connection, then these connections switch into the rendezvous state.  During this state, we still have not yet established an end-to-end route (through the relay network), and so if you send any messages unreliable, they are going to be discarded.
STATE_CONNECTED | k_ESteamNetworkingConnectionState_Connected | 3 | We've received communications from our peer (and we know who they are) and are all good.  If you close the connection now, we will make our best effort to flush out any reliable sent data that has not been acknowledged by the peer.  (But note that this happens from within the application process, so unlike a TCP connection, you are not totally handing it off to the operating system to deal with it.)
STATE_CLOSED_BY_PEER | k_ESteamNetworkingConnectionState_ClosedByPeer | 4 | Connection has been closed by our peer, but not closed locally. The connection still exists from an API perspective.  You must close the handle to free up resources.  If there are any messages in the inbound queue, you may retrieve them.  Otherwise, nothing may be done with the connection except to close it. This stats is similar to CLOSE_WAIT in the TCP state machine.
STATE_PROBLEM_DETECTED_LOCALLY | k_ESteamNetworkingConnectionState_ProblemDetectedLocally | 5 | A disruption in the connection has been detected locally.  (E.g. timeout, local internet connection disrupted, etc.) The connection still exists from an API perspective.  You must close the handle to free up resources. Attempts to send further messages will fail.  Any remaining received messages in the queue are available.
STATE_FIN_WAIT | k_ESteamNetworkingConnectionState_FinWait | -1 | We've disconnected on our side, and from an API perspective the connection is closed. No more data may be sent or received.  All reliable data has been flushed, or else we've given up and discarded it. We do not yet know for sure that the peer knows the connection has been closed, however, so we're just hanging around so that if we do get a packet from them, we can send them the appropriate packets so that they can know why the connection was closed (and not have to rely on a timeout, which makes it appear as if something is wrong).
STATE_LINGER | k_ESteamNetworkingConnectionState_Linger | -2 | We've disconnected on our side, and from an API perspective the connection is closed. No more data may be sent or received.  From a network perspective, however, on the wire, we have not yet given any indication to the peer that the connection is closed. We are in the process of flushing out the last bit of reliable data.  Once that is done, we will inform the peer that the connection has been closed, and transition to the FinWait state. Note that no indication is given to the remote host that we have closed the connection, until the data has been flushed.  If the remote host attempts to send us data, we will do whatever is necessary to keep the connection alive until it can be closed properly. But in fact the data will be discarded, since there is no way for the application to read it back.  Typically this is not a problem, as application protocols that utilize the lingering functionality are designed for the remote host to wait for the response before sending any more data.
STATE_DEAD | k_ESteamNetworkingConnectionState_Dead | -3 | Connection is completely inactive and ready to be destroyed.