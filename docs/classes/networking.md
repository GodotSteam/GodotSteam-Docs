---
title: Networking
description: A class reference for Networking.
icon: material/network
---

# Networking

Networking functions for making connections and sending data between clients, traversing NATs when possible. See the [Steam Networking](https://partner.steamgames.com/doc/features/multiplayer/networking){ target="\_blank" } overview for more information.

!!! warning "This API Is Deprecated"
	It may be removed in a future Steamworks SDK release but should continue to work. Valve suggest using [Networking Sockets](networking_sockets.md) or [Networking Messages](networking_messages.md) instead. 

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } and [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### acceptP2PSessionWithUser

!!! function "acceptP2PSessionWithUser( `uint64_t` steam_id_remote )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id_remote | uint64_t | The Steam ID of the user that sent the initial packet to us.

	This allows the game to specify accept an incoming packet. This needs to be called before a real connection is established to a remote host; the game will get a chance to say whether or not the remote user is allowed to talk to them.

	When a remote user that you haven't sent a packet to recently tries to first send you a packet, your game will receive a callback [p2p_session_request](#p2p_session_request). This callback contains the Steam ID of the user who wants to send you a packet. In response to this callback, you'll want to see if it's someone you want to talk to (for example, if they're in a lobby with you) and, if so, accept the connection. Otherwise, if you don't want to talk to the user, just ignore the request.

	If the user continues to send you packets, another [p2p_session_request](#p2p_session_request) will be posted periodically. If you've called [sendP2PPacket](#sendp2ppacket) on the other user, this implicitly accepts the session request.

	!!! returns "Returns: bool"
		Returns true upon success; otherwise, false only if **steam_id_remote** is invalid.

	!!! info "Notes"
		This call should only be made in response to a [p2p_session_request](#p2p_session_request) callback.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#AcceptP2PSessionWithUser){ .md-button .md-button--store target="_blank" }

### allowP2PPacketRelay

!!! function "allowP2PPacketRelay( `bool` allow )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | allow | bool | Whether or not to allow the Steam server relay.

    Allow or disallow P2P connections to fall back to being relayed through the Steam servers if a direct connection or NAT-traversal cannot be established.

	This only applies to connections created after setting this value, or to existing connections that need to automatically reconnect after this value is set.

	P2P packet relay is allowed by default.

	!!! returns "Returns: bool"
		This function always returns true.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#AllowP2PPacketRelay){ .md-button .md-button--store target="_blank" }

### closeP2PChannelWithUser

!!! function "closeP2PChannelWithUser( `uint64_t` steam_id_remote, `int` channel )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id_remote | uint64_t | The Steam ID of the user to close the connection with.
    | channel | int | The channel to close.

	Closes a P2P channel when you're done talking to a user on the specific channel.

	Once all channels to a user have been closed, the open session to the user will be closed and new data from this user will trigger a new [p2p_session_request](#p2p_session_request) callback.

	!!! returns "Returns: bool"
		Returns true if the channel was successfully closed; otherwise, false if there was no active session or channel with the user.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#CloseP2PChannelWithUser){ .md-button .md-button--store target="_blank" }

### closeP2PSessionWithUser

!!! function "closeP2PSessionWithUser( `uint64_t` steam_id_remote )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id_remote | uint64_t | The Steam ID of the user to close the connection with.

	This should be called when you're done communicating with a user, as this will free up all of the resources allocated for the connection under-the-hood.

	If the remote user tries to send data to you again, a new [p2p_session_request](#p2p_session_request) callback will be posted.

	!!! returns "Returns: bool"
		Returns true if the session was successfully closed; otherwise, false if no connection was open with **steam_id_remote**.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#CloseP2PSessionWithUser){ .md-button .md-button--store target="_blank" }
 
### getP2PSessionState

!!! function "getP2PSessionState( `uint64_t` steam_id_remote )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id_remote | uint64_t | The user to get the active session state information of.

	Fills out a dictionary with details about the connection like whether or not there is an active connection; number of bytes queued on the connection; the last error code, if any; whether or not a relay server is being used; and the IP and Port of the remote user, if known.

	This should only needed for debugging purposes.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| connection_active | bool | Do we have an active open connection with the user (true) or not (false)?
		| connecting | bool | Are we currently trying to establish a connection with the user (true) or not (false)?
		| session_error | [P2PSessionError enum](#p2psessionerror) | Last error recorded on the socket.
		| using_relay | bool | Is this connection going through a Steam relay server (true) or not (false)?
		| bytes_queued_for_send | int32 | The number of bytes queued up to be sent to the user.
		| packets_queued_for_send | int32 | The number of packets queued up to be sent to the user.
		| remote_ip | uint32 | The IP of remote host if set. Could be a Steam relay server. This only exists for compatibility with older authentication API's.
		| remote_port | uint16 | The port of remote host if set. Could be a Steam relay server. This only exists for compatibility with older authentication API's.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#GetP2PSessionState){ .md-button .md-button--store target="_blank" }

### getAvailableP2PPacketSize

!!! function "getAvailableP2PPacketSize( `int` channel = 0 )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | channel | int | The channel to check if a packet is available in. Defaults to 0.

	Calls [isP2PPacketAvailable](https://partner.steamgames.com/doc/api/ISteamNetworking#IsP2PPacketAvailable){ target="\_blank" } under the hood, returns the size of the available packet or zero if there is no such packet.

	!!! returns "Returns: uint32_t"

### readP2PPacket

!!! function "readP2PPacket( `uint32_t` packet, `int` channel )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | packet_size | uint32_t | The size of the packet from [getAvailableP2PPacketSize](#getavailablep2ppacketsize).
    | channel | int | The channel the packet was sent over.

    Reads in a packet that has been sent from another user via [sendP2PPacket](#sendp2ppacket).

	This call is not blocking and will return an empty dictionary if no data is available.

	Before calling this you should have called [getAvailableP2PPacketSize](#getavailablep2ppacketsize).

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| data | PackedByteArray | The packet data.
		| steam_id_remote | uint64_t | The Steam ID of the user that sent this packet.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#ReadP2PPacket){ .md-button .md-button--store target="_blank" }

### sendP2PPacket

!!! function "sendP2PPacket( `uint64_t` steam_id_remote, `PackedByteArray` data, `P2PSend` send_type, `int` channel )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id_remote | uint64_t | The target user to send the packet to.
    | data | PackedByteArray | The raw byte array for the packet data to send. The maximum size of this packet is defined by **send_type**.
    | send_type | [P2PSend enum](#p2psend) | Specifies how you want the data to be transmitted, such as reliably, unreliable, buffered, etc.
    | channel | int | The channel which acts as a virtual port to send this packet on and allows you help route message to different systems. You'll have to call [readP2PPacket](#readp2ppacket) on the other end with the same channel number in order to retrieve the data on the other end. Using different channels to talk to the same user will still use the same underlying P2P connection, saving on resources. Use 0 for the primary channel or if you do not use this feature.

    Sends a P2P packet to the specified user.  UDP-like, unreliable, and a max packet size of 1200 bytes. The first packet sent may be delayed as the NAT-traversal code runs. If we can't get through to the user, an error will be posted via the callback [p2p_session_connect_fail](#p2p_session_connect_fail).

	This is a session-less API which automatically establishes NAT-traversing or Steam relay server connections.

	See [P2PSend](https://partner.steamgames.com/doc/api/ISteamNetworking#EP2PSend){ target="_blank" } for descriptions of the different ways of sending packets.

	The type of data you send is arbitrary, you can use an off the shelf system like [Protocol Buffers](https://developers.google.com/protocol-buffers/){ target="_blank" } or [Cap'n Proto](https://capnproto.org/){ target="_blank" } to encode your packets in an efficient way, or you can create your own messaging system.

	!!! returns "Returns: bool"
		Returns true if the packet was successfully sent; otherwise, false if:

		* The packet is too large for the send type.
		* The target Steam ID is not valid.
		* There are too many bytes queued up to be sent.

	!!! trigger "Triggers"
		[p2p_session_request](#p2p_session_request) callback

	!!! info "Notes"
		Returning true does not mean successfully received, if we can't get through to the user after a timeout of 20 seconds, then an error will be posted via the [p2p_session_connect_fail](#p2p_session_connect_fail) callback.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#SendP2PPacket){ .md-button .md-button--store target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### p2p_session_connect_fail

!!! function "p2p_session_connect_fail"
	Called when a user sends a packet and it fails.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | steam_id_remote | uint64_t | User we were trying to send the packets to.
		| session_error | uint8_t | Indicates the reason why we're having trouble. Actually a [P2PSessionError](#p2psessionerror).

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#P2PSessionConnectFail_t){ .md-button .md-button--store target="_blank" }

### p2p_session_request

!!! function "p2p_session_request"
	Called when a user sends a packet.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | steam_id_remote | uint64_t | The user who wants to start a P2P session with us.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#P2PSessionRequest_t){ .md-button .md-button--store target="_blank" }

{==
## :material-numeric: Enums
==}

### P2PSend

Enumerator | SDK Name | Value | Details
---------- | -------- | ----- | -------
P2P_SEND_UNRELIABLE | k_EP2PSendUnreliable | 0 | Basic UDP send. Packets can't be bigger than 1200 bytes (your typical MTU size). Can be lost, or arrive out of order (rare). The sending API does have some knowledge of the underlying connection, so if there is no NAT-traversal accomplished or there is a recognized adjustment happening on the connection, the packet will be batched until the connection is open again.
P2P_SEND_UNRELIABLE_NO_DELAY | k_EP2PSendUnreliableNoDelay | 1 | As above, but if the underlying P2P connection isn't yet established the packet will just be thrown away. Using this on the first packet sent to a remote host almost guarantees the packet will be dropped. This is only really useful for kinds of data that should never buffer up, i.e. voice payload packets
P2P_SEND_RELIABLE | k_EP2PSendReliable | 2 | Reliable message send. Can send up to 1MB of data in a single message. Does fragmentation/re-assembly of messages under the hood, as well as a sliding window for efficient sends of large chunks of data.
P2P_SEND_RELIABLE_WITH_BUFFERING | k_EP2PSendReliableWithBuffering | 3 | As above, but applies the Nagle algorithm to the send - sends will accumulate until the current MTU size (typically ~1200 bytes, but can change) or ~200ms has passed (Nagle algorithm). This is useful if you want to send a set of smaller messages but have the coalesced into a single packet. Since the reliable stream is all ordered, you can do several small message sends with P2P_SEND_RELIABLE_WITH_BUFFERING and then do a normal P2P_SEND_RELIABLE to force all the buffered data to be sent.

### P2PSessionError

Enumerator | SDK Name | Value | Details
---------- | -------- | ----- | -------
P2P_SESSION_ERROR_NONE | k_EP2PSessionErrorNone | 0 | There was no error.
P2P_SESSION_ERROR_NOT_RUNNING_APP | k_EP2PSessionErrorNotRunningApp_DELETED | 1 | The target user is not running the same game.
P2P_SESSION_ERROR_NO_RIGHTS_TO_APP | k_EP2PSessionErrorNoRightsToApp | 2 | The local user doesn't own the app that is running.
P2P_SESSION_ERROR_DESTINATION_NOT_LOGGED_ON | k_EP2PSessionErrorDestinationNotLoggedIn_DELETED | 3 | Target user isn't connected to Steam.
P2P_SESSION_ERROR_TIMEOUT | k_EP2PSessionErrorTimeout | 4 | The connection timed out because the target user didn't respond, perhaps they aren't calling [acceptP2PSessionWithUser](#acceptp2psessionwithuser). Corporate firewalls can also block this (NAT traversal is not firewall traversal), make sure that UDP ports 3478, 4379, and 4380 are open in an outbound direction.
P2P_SESSION_ERROR_MAX | k_EP2PSessionErrorMax | 5 | Unused.

### SNetSocketConnectionType

Enumerator | SDK Name | Value | Details
---------- | -------- | ----- | -------
NET_SOCKET_CONNECTION_TYPE_NOT_CONNECTED | k_ESNetSocketConnectionTypeNotConnected | 0 | -
NET_SOCKET_CONNECTION_TYPE_UDP | k_ESNetSocketConnectionTypeUDP | 1 | -
NET_SOCKET_CONNECTION_TYPE_UDP_RELAY | k_ESNetSocketConnectionTypeUDPRelay | 2 | -

### SNetSocketState

Enumerator | SDK Name | Value | Details
---------- | -------- | ----- | -------
NET_SOCKET_STATE_INVALID | k_ESNetSocketStateInvalid | 0 | -
NET_SOCKET_STATE_CONNECTED | k_ESNetSocketStateConnected | 1 | Communication is valid.
NET_SOCKET_STATE_INITIATED | k_ESNetSocketStateInitiated | 10 | States while establishing a connection; the connection state machine has started.
NET_SOCKET_STATE_LOCAL_CANDIDATE_FOUND | k_ESNetSocketStateLocalCandidatesFound | 11 | P2P connections; we've found our local IP info.
NET_SOCKET_STATE_RECEIVED_REMOTE_CANDIDATES | k_ESNetSocketStateReceivedRemoteCandidates | 12 | We've received information from the remote machine, via the Steam back-end, about their IP info.
NET_SOCKET_STATE_CHALLENGE_HANDSHAKE | k_ESNetSocketStateChallengeHandshake | 15 | Direct connections; we've received a challenge packet from the server.
NET_SOCKET_STATE_DISCONNECTING | k_ESNetSocketStateDisconnecting | 21 | Failure states; the API shut it down and we're in the process of telling the other end.
NET_SOCKET_STATE_LOCAL_DISCONNECT | k_ESNetSocketStateLocalDisconnect | 22 | The API shut it down and we've completed shutdown.
NET_SOCKET_STATE_TIMEOUT_DURING_CONNECT | k_ESNetSocketStateTimeoutDuringConnect | 23 | We timed out while trying to creating the connection.
NET_SOCKET_STATE_REMOTE_END_DISCONNECTED | k_ESNetSocketStateRemoteEndDisconnected | 24 | The remote end has disconnected from us.
NET_SOCKET_STATE_BROKEN | k_ESNetSocketStateConnectionBroken | 25 | Connection has been broken; either the other end has disappeared or our local network connection has broke.
