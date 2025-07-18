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

!!! function "acceptP2PSessionWithUser( ```uint64_t``` steam_id_remote )"
	This allows the game to specify accept an incoming packet. This needs to be called before a real connection is established to a remote host, the game will get a chance to say whether or not the remote user is allowed to talk to them.

	When a remote user that you haven't sent a packet to recently, tries to first send you a packet, your game will receive a callback [p2p_session_request](#p2p_session_request). This callback contains the Steam ID of the user who wants to send you a packet. In response to this callback, you'll want to see if it's someone you want to talk to (for example, if they're in a lobby with you), and if so, accept the connection; otherwise if you don't want to talk to the user, just ignore the request. If the user continues to send you packets, another [p2p_session_request](#p2p_session_request) will be posted periodically. If you've called <strong>sendP2PPacket</strong> on the other user, this implicitly accepts the session request.

	**Returns:** bool

	- **true** upon success; **false** only if `steam_id_remote` is invalid.

	**Note:** This call should only be made in response to a [p2p_session_request](#p2p_session_request) callback.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#AcceptP2PSessionWithUser){ .md-button .md-button--store target="_blank" }

### allowP2PPacketRelay

!!! function "allowP2PPacketRelay( ```bool``` allow )"
	Allow or disallow P2P connections to fall back to being relayed through the Steam servers if a direct connection or NAT-traversal cannot be established.

	This only applies to connections created after setting this value, or to existing connections that need to automatically reconnect after this value is set.

	P2P packet relay is allowed by default.

	**Returns:** bool

	- This function always returns **true**.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#AllowP2PPacketRelay){ .md-button .md-button--store target="_blank" }

### closeP2PChannelWithUser

!!! function "closeP2PChannelWithUser( ```uint64_t``` steam_id_remote, ```int``` channel )"
	Closes a P2P channel when you're done talking to a user on the specific channel.

	Once all channels to a user have been closed, the open session to the user will be closed and new data from this user will trigger a new [p2p_session_request](#p2p_session_request) callback.

	**Returns:** bool

	- **true** if the channel was successfully closed; otherwise, **false** if there was no active session or channel with the user.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#CloseP2PChannelWithUser){ .md-button .md-button--store target="_blank" }

### closeP2PSessionWithUser

!!! function "closeP2PSessionWithUser( ```uint64_t``` steam_id_remote )"
	This should be called when you're done communicating with a user, as this will free up all of the resources allocated for the connection under-the-hood.

	If the remote user tries to send data to you again, a new [p2p_session_request](#p2p_session_request) callback will be posted.

	**Returns:** bool

	- **true** if the session was successfully closed; otherwise, **false** if no connection was open with `steam_id_remote`.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#CloseP2PSessionWithUser){ .md-button .md-button--store target="_blank" }
 
### getP2PSessionState

!!! function "getP2PSessionState( ```uint64_t``` steam_id_remote )"
	Fills out a dictionary with details about the connection like whether or not there is an active connection; number of bytes queued on the connection; the last error code, if any; whether or not a relay server is being used; and the IP and Port of the remote user, if known.

	This should only needed for debugging purposes.

	**Returns:** dictionary

	Contains the following keys:

	* connection_active (bool)
	* connecting (bool)
	* session_error (uint8)
	* using_relay (bool)
	* bytes_queued_for_send (int32)
	* packets_queued_for_send (int32)
	* remote_ip (uint32)
	* remote_port (uint16)

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#GetP2PSessionState){ .md-button .md-button--store target="_blank" }

### getAvailableP2PPacketSize

!!! function "getAvailableP2PPacketSize( ```int``` channel )"
	Calls [isP2PPacketAvailable](https://partner.steamgames.com/doc/api/ISteamNetworking#IsP2PPacketAvailable){ target="\_blank" } under the hood, returns the size of the available packet or zero if there is no such packet.

	**Returns:** uint32_t

### readP2PPacket

!!! function "readP2PPacket( ```uint32_t``` packet, ```int``` channel )"
	Reads in a packet that has been sent from another user via SendP2PPacket.

	This call is not blocking, and will return false if no data is available.

	Before calling this you should have called [getAvailableP2PPacketSize](#getavailablep2ppacketsize).

	**Returns:** dictionary

	* data (PoolByteArray)
	* steam_id_remote (uint64_t)

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#ReadP2PPacket){ .md-button .md-button--store target="_blank" }

### sendP2PPacket

!!! function "sendP2PPacket( ```uint64_t``` steam_id_remote, ```array``` data, ```int``` send_type, ```int``` channel )"
	Sends a P2P packet to the specified user.

	This is a session-less API which automatically establishes NAT-traversing or Steam relay server connections.

	See [EP2PSend](https://partner.steamgames.com/doc/api/ISteamNetworking#EP2PSend){ target="_blank" } for descriptions of the different ways of sending packets. The integers passed in GodotSteam are:

	* 0 - send ureliable
	* 1 - send unreliable, no delay
	* 2 - send reliable
	* 3 - send reliable with buffering

	The type of data you send is arbitrary, you can use an off the shelf system like [Protocol Buffers](https://developers.google.com/protocol-buffers/){ target="_blank" } or [Cap'n Proto](https://capnproto.org/){ target="_blank" } to encode your packets in an efficient way, or you can create your own messaging system.

	Sends a P2P packet to the specified user.

	Triggers a [p2p_session_request](#p2p_session_request) callback.

	**Returns:** bool

	_True_ if the packet was successfully sent. Note that this does not mean successfully received, if we can't get through to the user after a timeout of 20 seconds, then an error will be posted via the [p2p_session_connect_fail](#p2p_session_connect_fail) callback.

	**Note:** The first packet send may be delayed as the NAT-traversal code runs.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#SendP2PPacket){ .md-button .md-button--store target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to run ```Steam.run_callbacks()``` in your ```_process()``` function to receive them.

### p2p_session_connect_fail

!!! function "p2p_session_connect_fail"
	Called when a user sends a packet and it fails.

	**Returns:**

	* steam_id_remote (uint64_t)
	* session_error (uint8_t)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#P2PSessionConnectFail_t){ .md-button .md-button--store target="_blank" }

### p2p_session_request

!!! function "p2p_session_request"
	Called when a user sends a packet.
	
	**Returns:**

	* steam_id_remote (uint64_t)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworking#P2PSessionRequest_t){ .md-button .md-button--store target="_blank" }

{==
## :material-numeric: Enums
==}

### P2PSend

Enumerator | Value
---------- | -----
P2P_SEND_UNRELIABLE | 0
P2P_SEND_UNRELIABLE_NO_DELAY | 1
P2P_SEND_RELIABLE | 2
P2P_SEND_RELIABLE_WITH_BUFFERING | 3

### P2PSessionError

Enumerator | Value
---------- | -----
P2P_SESSION_ERROR_NONE | 0
P2P_SESSION_ERROR_NOT_RUNNING_APP | 1
P2P_SESSION_ERROR_NO_RIGHTS_TO_APP | 2
P2P_SESSION_ERROR_DESTINATION_NOT_LOGGED_ON | 3
P2P_SESSION_ERROR_TIMEOUT | 4
P2P_SESSION_ERROR_MAX | 5

### SNetSocketConnectionType

Enumerator | Value
---------- | -----
NET_SOCKET_CONNECTION_TYPE_NOT_CONNECTED | 0
NET_SOCKET_CONNECTION_TYPE_UDP | 1
NET_SOCKET_CONNECTION_TYPE_UDP_RELAY | 2

### SNetSocketState

Enumerator | Value
---------- | -----
NET_SOCKET_STATE_INVALID | 0
NET_SOCKET_STATE_CONNECTED | 1
NET_SOCKET_STATE_INITIATED | 10
NET_SOCKET_STATE_LOCAL_CANDIDATE_FOUND | 11
NET_SOCKET_STATE_RECEIVED_REMOTE_CANDIDATES | 12
NET_SOCKET_STATE_CHALLENGE_HANDSHAKE | 15
NET_SOCKET_STATE_DISCONNECTING | 21
NET_SOCKET_STATE_LOCAL_DISCONNECT | 22
NET_SOCKET_STATE_TIMEOUT_DURING_CONNECT | 23
NET_SOCKET_STATE_REMOTE_END_DISCONNECTED | 24
NET_SOCKET_STATE_BROKEN | 25
