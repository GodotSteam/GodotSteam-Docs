---
title: Networking Messages
description: A class reference for Networking Messages.
icon: material/message
---

# Networking Messages

Networking API intended to make it easy to port non-connection-oriented code to take advantage of P2P connectivity and [Steam Datagram Relay](https://partner.steamgames.com/doc/features/multiplayer/steamdatagramrelay){ target="\_blank" }. These are part of the newer networking classes; not to be confused with the [older, now-deprecated Networking class](networking.md).

[Networking Sockets](networking_sockets.md) is connection-oriented (like TCP), meaning you need to listen and connect, and then you send messages using a connection handle. Networking Messages, on the other hand, is more like UDP, in that you can just send messages to arbitrary peers at any time. The underlying connections are established implicitly.

Under the hood Networking Messages works on top of the [Networking Sockets](networking_sockets.md) code, so you get the same routing and messaging efficiency. The difference is mainly in your responsibility to explicitly establish a connection and the type of feedback you get about the state of the connection. Both interfaces can do "P2P" communications, both support both unreliable and reliable messages, fragmentation and reassembly, and both can be used to take advantage of [Steam Datagram Relay](https://partner.steamgames.com/doc/features/multiplayer/steamdatagramrelay){ target="\_blank" } to talk to dedicated servers.

The primary purpose of this interface is to be "like UDP", so that UDP-based code can be ported easily to take advantage of relayed connections. If you find yourself needing more low level information or control, or to be able to better handle failure, then you probably need to use [Networking Sockets](networking_sockets.md) directly. Also, note that if your main goal is to obtain a connection between two peers without concerning yourself with assigning roles of "client" and "server", you may find the symmetric connection mode of [Networking Sockets](networking_sockets.md) useful. See [NETWORKING_CONFIG_SYMMETRIC_CONNECT](networking_utils.md#networkingconfigvalue).

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } and [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### acceptSessionWithUser

!!! function "acceptSessionWithUser( ```uint64_t``` remote_steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | remote_steam_id | uint64_t | The Steam ID of the user to accept a Messages session with.

	Call this in response to a [network_messages_session_request](#network_messages_session_request) callback. [network_messages_session_request](#network_messages_session_request) callbacks are posted when a user tries to send you a message, and you haven't tried to talk to them first. If you don't want to talk to them, just ignore the request. If the user continues to send you messages, [network_messages_session_request](#network_messages_session_request) callbacks will continue to be posted periodically.

	Calling [sendMessageToUser](#sendmessagetouser) will implicitly accept any pending session request to that user.

	!!! returns "Returns: bool"
		Returns true if there is an existing active session, even if it is not pending; otherwise, false if there is no session with the user pending or otherwise.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#AcceptSessionWithUser){ .md-button .md-button--store target="_blank" }

### closeChannelWithUser

!!! function "closeChannelWithUser( ```uint64_t``` remote_steam_id, ```int``` channel )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | remote_steam_id | uint64_t | The Steam ID of the user to close the channel with.
    | channel | int | The channel to close.

	Call this  when you're done talking to a user on a specific channel. Once all open channels to a user have been closed, the open session to the user will be closed, and any new data from this user will trigger a [network_messages_session_request](#network_messages_session_request) callback.

	!!! returns "Returns: bool"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#CloseChannelWithUser){ .md-button .md-button--store target="_blank" }

### closeSessionWithUser

!!! function "closeSessionWithUser( ```uint64_t``` remote_steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | remote_staem_id | uint64_t | The Steam ID of the user to close the session with.

	Call this when you're done talking to a user to immediately free up resources under-the-hood. If the remote user tries to send data to you again, another [network_messages_session_request](#network_messages_session_request) callback will be posted.

	Sessions that go unused for a few minutes are automatically timed out.

	!!! returns "Returns: bool"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#CloseSessionWithUser){ .md-button .md-button--store target="_blank" }

### getSessionConnectionInfo

!!! function "getSessionConnectionInfo```uint64_t``` remote_steam_id, ```bool``` get_connection, ```bool``` get_status )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | remote_steam_id | uint64_t | The Steam ID of the user to get connection info for.
    | get_connection | bool | Get the connection information.
    | get_status | bool | Get the connection status.

	Returns information about the latest state of a connection, if any, with the given peer. Primarily intended for debugging purposes but can also be used to get more detailed failure information.

	You may pass false for either **get_connection** or **get_status** if you do not need the corresponding details.

	Sessions time out after a while, so if a connection fails or [sendMessageToUser](#sendmessagetouser) returns 3, you cannot wait indefinitely to obtain the reason for failure.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| connection_state | [NetworkingConnectionState enum](networking_utils.md#networkingconnectionstate) | The value of connection state or 0 if no connection exists with specified peer.

		If **get_connection** is true, also contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| identity | uint64_t | Who is on the other end?  Depending on the connection type and phase of the connection, we might not know.
		| user_data | uint64_t | Arbitrary user data set by the local application code.
		| listen_socket | uint32_t | Handle to listen socket this was connected on or [LISTEN_SOCKET_INVALID](networking_utils.md#constants) if we initiated the connection.
		| remote_address | string | Remote address.  Might be all 0's if we don't know it or if this is N/A; eg. basically everything except direct UDP connection.
		| remote_pop | uint32_t | What data center is the remote host in? 0 if we don't know.
		| pop_relay | uint32_t | What relay are we using to communicate with the remote host? 0 if not applicable.
		| connection_state | int | High level state of the connection.
		| end_reason | [NetworkingConnectionEnd enum](networking_utils.md#networkingconnectionend) | Basic cause of the connection termination or problem.
		| end_debug | string | Human-readable but non-localized explanation for connection termination or problem.  This is intended for debugging / diagnostic purposes only, not to display to users.  It might have some details specific to the issue.
		| debug_description | string | Debug description.  This includes the internal connection ID, connection type (and peer information), and any name given to the connection by the app.  This string is used in various internal logging messages.
		| info_flags | int | Misc flags.  Bitmask of [NETWORKING_CONNECTION_INFO_FLAG_](networking_utils.md#constants).

		If **get_status** is true, also contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
        | state | [NetworkingConnectionState enum](networking_utils.md#networkingconnectionstate) | High level state of the connection.
		| ping | int | Current ping in milliseconds.
		| local_quality | float | Connection quality measured locally, 0 to 1. Percentage of packets delivered end-to-end in order).
		| remote_quality | float | Packet delivery success rate as observed from remote host.
		| packets_out_per_second | float | Current data rates from recent history.
		| bytes_out_per_second | float | Current data rates from recent history.
		| packets_in_per_second | float | Current data rates from recent history.
		| bytes_in_per_second | float | Current data rates from recent history.
		| send_rate | int | Estimate rate that we believe that we can send data to our peer. This could be significantly higher than **bytes_out_per_second**, meaning the capacity of the channel is higher than you are sending data and that's OK.
		| pending_unreliable | int | Number of bytes pending to be sent.
		| pending_reliable | int | Number of bytes pending to be sent.
		| send_unacknowledged_reliable | int | Number of bytes of reliable data that has been placed the wire but for which we have not yet received an acknowledgment and thus we may have to re-transmit.
		| queue_time | uint64_t | If you queued a message right now, approximately how long would that message wait in the queue before we actually started putting its data on the wire in a packet?

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#GetSessionConnectionInfo){ .md-button .md-button--store target="_blank" }

### receiveMessagesOnChannel

!!! function "receiveMessagesOnChannel( ```int``` channel, ```int``` max_messages )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | channel | int | The channel to receive messages on.
    | max_messages | int | The maximum number of messages to receive.

	Reads the next message that has been sent from another user via [sendMessageToUser](#sendmessagetouser) on the given channel.

	!!! returns "Returns: array of dictionaries"
		Each contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| payload | PackedByteArray | The message payload.
		| size | int | Size of the payload.
		| connection | uint32_t | What connection did this come from?
		| identity | uint64_t | The Steam ID of the user who sent the message.
		| receiver_user_data | uint64_t | The user data associated with the connection.
		| time_received | uint64_t | Local timestamp when the message was received.
		| message_number | uint64_t | Message number assigned by teh sender.
		| channel | int | The channel number the message was received on.
		| flags | int | Bitmask of [NETWORKING_SEND_](main.md#constants) flags; only the [NETWORKING_SEND_RELIABLE](main.md#constants) bit is valid.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#ReceiveMessagesOnChannel){ .md-button .md-button--store target="_blank" }

### sendMessageToUser

!!! function "sendMessageToUser( ```uint64_t``` remote_steam_id, ```PackedByteArray``` data, ```int``` flags, ```int``` channel )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | remote_steam_id | uint64_t | The Steam ID of the user you want to send the message to.
    | data | PackedByteArray | The packet data to send.
    | flags | int | Bitmask of [NETWORKING_SEND_](main.md#constants) options. 
    | channel | int | Routing number you can use to help route message to different systems. You'll have to call [receiveMessagesOnChannel](#receivemessagesonchannel) with the same channel number in order to retrieve the data on the other end.

	Sends a message to the specified host via **identity_reference**. If we don't already have a session with that user, a session is implicitly created. There might be some handshaking that needs to happen before we can actually begin sending message data. If this handshaking fails and we can't get through, an error will be posted via the callback [network_messages_session_failed](#network_messages_session_failed). There is no notification when the operation succeeds. You should have the peer send a reply for this purpose.

	Sending a message to a host will also implicitly accept any incoming connection from that host.

	Using different channels to talk to the same user will still use the same underlying connection, saving on resources. If you don't need this feature, use 0. Otherwise, small integers are the most efficient.

	It is guaranteed that reliable messages to the same host on the same channel will be be received by the remote host, if they are received at all, exactly once and in the same order that they were sent.

	No other order guarantees exist. In particular, unreliable messages may be dropped, received out of order with respect to each other and with respect to reliable data, or may be received multiple times. Messages on different channels are *not* guaranteed to be received in the order they were sent.

	A note for those familiar with TCP/IP ports, or converting an existing codebase that opened multiple sockets: You might notice that there is only one channel, and with TCP/IP each endpoint has a port number. You can think of the channel number as the destination port. If you need each message to also include a source port, so the recipient can route the reply, then just put that in your message. That is essentially how UDP works.

	!!! returns "Returns: int"
		Possible results may be:

		* [RESULT_OK](main.md#result) on success.
		* [RESULT_NO_CONNECTION](main.md#result) if the session has failed or was closed by the peer and [NETWORKING_SEND_AUTORESTART_BROKEN_SESSION](main.md#constants) was not specified. You can use [getSessionConnectionInfo](#getsessionconnectioninfo) to get the details.) In order to acknowledge the broken session and start a new one, you must call [closeSessionWithUser](#closesessionwithuser) or you may repeat the call with NETWORKING_SEND_AUTORESTART_BROKEN_SESSION](main.md#constants). See NETWORKING_SEND_AUTORESTART_BROKEN_SESSION](main.md#constants) for more details.
		* [RESULT_NO_CONNECTION](main.md#result) will be returned if the session has failed or was closed by the peer and NETWORKING_SEND_AUTORESTART_BROKEN_SESSION](main.md#constants) is not used. You can use [getSessionConnectionInfo](#getsessionconnectioninfo) to get the details. In order to acknowledge the broken session and start a new one, you must call [closeSessionWithUser](#closesessionwithuser).
		* See [sendMessageToConnection](networking_sockets.md#sendmessagetoconnection) for more possible return values.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#SendMessageToUser){ .md-button .md-button--store target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### network_messages_session_request

!!! function "network_messages_session_request"
	Posted when a remote host is sending us a message, and we do not already have a session with them.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| remote_steam_id | uint64_t | User who wants to talk to us.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#SteamNetworkingMessagesSessionRequest_t){ .md-button .md-button--store target="_blank" }

### network_messages_session_failed

!!! function "network_messages_session_failed"
	Posted when we fail to establish a connection or we detect that communications have been disrupted it an unusual way. There is no notification when a peer proactively closes the session. "Closed by peer" is not a concept of UDP-style communications and Networking Messages is primarily intended to make porting UDP code easy.

	Remember, callbacks are asynchronous. See notes on [sendMessageToUser](#sendmessagetouser) and [NETWORKING_SEND_AUTORESTART_BROKEN_SESSION constant](networking_utils.md#constants) in particular.

	Also, if a session times out due to inactivity, no callbacks will be posted. The only way to detect that this is happening is that querying the session state may return none, connecting, and findingroute again.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| reason | [NetworkingConnectionEnd enum](networking_utils.md#networkingconnectionend) | Basic cause of the connection termination or problem.
		| remote_steam_id | int | Who is on the other end?  Depending on the connection type and phase of the connection, we might not know. 
		| connection_state | [NetworkingConnectionState enum](networking_utils.md#networkingconnectionstate) | High level state of the connection. 
		| debug_message | string | Human-readable but non-localized explanation for connection termination or problem.  This is intended for debugging / diagnostic purposes only, not to display to users.  It might have some details specific to the issue.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#SteamNetworkingMessagesSessionFailed_t){ .md-button .md-button--store target="_blank" }