# Networking Messages

Networking API intended to make it easy to port non-connection-oriented code to take advantage of P2P connectivity and [Steam Datagram Relay](https://partner.steamgames.com/doc/features/multiplayer/steamdatagramrelay){ target="\_blank" }. These are part of the newer networking classes; not to be confused with the [older, now-deprecated Networking class](networking.md).

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } and [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### acceptSessionWithUser

!!! function "acceptSessionWithUser( ```uint64_t``` remote_steam_id )"
	Call this in response to a [network_messages_session_request](#network_messages_session_request) callback. [network_messages_session_request](#network_messages_session_request) callbacks are posted when a user tries to send you a message, and you haven't tried to talk to them first. If you don't want to talk to them, just ignore the request. If the user continues to send you messages, [network_messages_session_request](#network_messages_session_request) callbacks will continue to be posted periodically.

	Returns false if there is no session with the user pending or otherwise. If there is an existing active session, this function will return true, even if it is not pending.

	Calling [sendMessageToUser](#sendmessagetouser) will implicitly accept any pending session request to that user.

	**Returns:** bool

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#AcceptSessionWithUser){ .md-button .md-button--store target="_blank" }

### closeChannelWithUser

!!! function "closeChannelWithUser( ```uint64_t``` remote_steam_id, ```int``` channel )"
	Call this  when you're done talking to a user on a specific channel. Once all open channels to a user have been closed, the open session to the user will be closed, and any new data from this user will trigger a [network_messages_session_request](#network_messages_session_request) callback.

	**Returns:** bool

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#CloseChannelWithUser){ .md-button .md-button--store target="_blank" }

### closeSessionWithUser

!!! function "closeSessionWithUser( ```uint64_t``` remote_steam_id )"
	Call this when you're done talking to a user to immediately free up resources under-the-hood. If the remote user tries to send data to you again, another [network_messages_session_request](#network_messages_session_request) callback will be posted.

	Note that sessions that go unused for a few minutes are automatically timed out.

	**Returns:** bool.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#CloseSessionWithUser){ .md-button .md-button--store target="_blank" }

### getSessionConnectionInfo

!!! function "getSessionConnectionInfo```uint64_t``` remote_steam_id, ```bool``` get_connection, ```bool``` get_status )"
	Returns information about the latest state of a connection, if any, with the given peer. Primarily intended for debugging purposes, but can also be used to get more detailed failure information.

	Returns the value of connection state or 0 if no connection exists with specified peer. You may pass _false_ for either **get_connection** or **get_status** if you do not need the corresponding details. Note that sessions time out after a while, so if a connection fails, or [sendMessageToUser](#sendmessagetouser) returns 3, you cannot wait indefinitely to obtain the reason for failure.

	**Returns:** dictionary

	Contains the following keys:

	* state (int) with values being:

		* 0 - none
		* 1 - connecting
		* 2 - finding route
		* 3 - connected
		* 4 - closed by peer
		* 5 - problem detected locally
		* -1 - wait
		* -2 - linger
		* -3 - dead

	If **get_connection** is true:

	* identity (uint64_t)
	* user_data (uint64_t)
	* listen_socket (uint32)
	* remote_address (string)
	* remote_pop (uint32)
	* pop_relay (uint32)
	* connection_state (int); same as above states
	* end_reason (int)
	* end_debug (string)
	* debug_description (string)
	* info_flags (int)

	If **get_status** is true:

	* ping (int)
	* local_quality (float)
	* remote_quality (float)
	* packets_out_per_second (float)
	* bytes_out_per_second (float)
	* packets_in_per_second (float)
	* bytes_in_per_second (float)
	* send_rate (int)
	* pending_unreliable (int)
	* pending_reliable (int)
	* send_unacknowledged_reliable (int)
	* queue_time (uint64_t)

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#GetSessionConnectionInfo){ .md-button .md-button--store target="_blank" }

### receiveMessagesOnChannel

!!! function "receiveMessagesOnChannel( ```int``` channel, ```int``` max_messages )"	
	Reads the next message that has been sent from another user via [sendMessageToUser](#sendmessagetouser) on the given channel. Returns number of messages returned into your list. (0 if no message are available on that channel.)

	**Returns:** array

	Contains a list of:

	* messages (dictionary)

	Contains the following keys:

	* payload (string)
	* size (int)
	* connection (uint32)
	* identity (uint64_t)
	* receiver_user_data (uint64_t)
	* time_received (uint64_t)
	* message_number (uint64_t)
	* channel (int)
	* flags (int)
	* sender_user_data (uint64_t)

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#ReceiveMessagesOnChannel){ .md-button .md-button--store target="_blank" }

### sendMessageToUser

!!! function "sendMessageToUser( ```uint64_t``` remote_steam_id, ```PoolByteArray``` data, ```int``` flags, ```int``` channel )"
	Sends a message to the specified host via **identity_reference**. If we don't already have a session with that user, a session is implicitly created. There might be some handshaking that needs to happen before we can actually begin sending message data. If this handshaking fails and we can't get through, an error will be posted via the callback [network_messages_session_failed](#network_messages_session_failed). There is no notification when the operation succeeds. (You should have the peer send a reply for this purpose.)

	Sending a message to a host will also implicitly accept any incoming connection from that host.

	**flags** is a bitmask of k_nSteamNetworkingSend_xxx options.

	**channel** is a routing number you can use to help route message to different systems. You'll have to call [receiveMessagesOnChannel](#receivemessagesonchannel) with the same channel number in order to retrieve the data on the other end.

	Using different channels to talk to the same user will still use the same underlying connection, saving on resources. If you don't need this feature, use 0. Otherwise, small integers are the most efficient.

	It is guaranteed that reliable messages to the same host on the same channel will be be received by the remote host (if they are received at all) exactly once, and in the same order that they were sent.

	No other order guarantees exist! In particular, unreliable messages may be dropped, received out of order with respect to each other and with respect to reliable data, or may be received multiple times. Messages on different channels are *not* guaranteed to be received in the order they were sent.

	A note for those familiar with TCP/IP ports, or converting an existing codebase that opened multiple sockets: You might notice that there is only one channel, and with TCP/IP each endpoint has a port number. You can think of the channel number as the destination port. If you need each message to also include a source port (so the recipient can route the reply), then just put that in your message. That is essentially how UDP works!

	**Returns:** int

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#SendMessageToUser){ .md-button .md-button--store target="_blank" }

{==
## :material-signal: Signals
==}

### network_messages_session_request

!!! function "network_messages_session_request"
	Posted when a remote host is sending us a message, and we do not already have a session with them.

	**Returns:**

	* remote_steam_id (uint64_t)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#SteamNetworkingMessagesSessionRequest_t){ .md-button .md-button--store target="_blank" }

### network_messages_session_failed

!!! function "network_messages_session_failed"
	Posted when we fail to establish a connection, or we detect that communications have been disrupted it an unusual way. There is no notification when a peer proactively closes the session. ("Closed by peer" is not a concept of UDP-style communications, and Networking Messages is primarily intended to make porting UDP code easy.)

	Remember: callbacks are asynchronous. See notes on [sendMessageToUser](#sendmessagetouser), and NETWORKING_SEND_AUTO_RESTART_BROKEN_SESSION in particular.

	Also, if a session times out due to inactivity, no callbacks will be posted. The only way to detect that this is happening is that querying the session state may return none, connecting, and findingroute again.

	**Returns:**

	* reason (int)
	* remote_steam_id (int)
	* connection_state (int)
	* debug_message (string)

	The returned reason integer will map to these enums: [NetworkingConnectionEnd](#networkingconnectionend)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingMessages#SteamNetworkingMessagesSessionFailed_t){ .md-button .md-button--store target="_blank" }

{==
## :material-numeric: Enums
==}

### NetworkingConnectionEnd

Enumerator | Value
---------- | -----
CONNECTION_END_INVALID | 0
CONNECTION_END_APP_MIN | 1000
CONNECTION_END_MAX | 1999
CONNECTION_END_APP_EXCEPTION_MIN | 2000
CONNECTION_END_APP_EXCEPTION_MAX | 2999
CONNECTION_END_LOCAL_MIN | 3000
CONNECTION_END_LOCAL_OFFLINE_MODE | 3001
CONNECTION_END_LOCAL_MANY_RELAY_CONNECTIVITY | 3002
CONNECTION_END_LOCAL_HOSTED_sERVER_PRIMARY_RELAY | 3003
CONNECTION_END_LOCAL_NETWORK_CONFIG | 3004
CONNECTION_END_LOCAL_RIGHTS | 3005
CONNECTION_END_LOCAL_MAX | 3999
CONNECTION_END_REMOVE_MIN | 4000
CONNECTION_END_REMOTE_TIMEOUT | 4001
CONNECTION_END_REMOTE_BAD_CRYPT | 4002
CONNECTION_END_REMOTE_BAD_CERT | 4003
CONNECTION_END_REMOTE_NOT_LOGGED_IN | 4004
CONNECTION_END_REMOTE_NOT_RUNNING_APP | 4005
CONNECTION_END_BAD_PROTOCOL_VERSION | 4006
CONNECTION_END_REMOTE_MAX | 4999
CONNECTION_END_MISC_MIN | 5000
CONNECTION_END_MISC_GENERIC | 5001
CONNECTION_END_MISC_INTERNAL_ERROR | 5002
CONNECTION_END_MISC_TIMEOUT | 5003
CONNECTION_END_MISC_RELAY_CONNECTIVITY | 5004
CONNECTION_END_MISC_STEAM_CONNECTIVITY | 5005
CONNECTION_END_MISC_NO_RELAY_SESSIONS_TO_CLIENT | 5006
CONNECTION_END_MISC_MAX | 5999