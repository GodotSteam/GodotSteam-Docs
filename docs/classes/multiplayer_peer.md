---
title: MultiplayerPeer
description: A class reference for MultiplayerPeer.
icon: material/account-multiple
---

# MultiplayerPeer

!!! info "Only available in the [GodotSteam MultiplayerPeer repo](https://github.com/GodotSteam/MultiplayerPeer){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### collect_debug_data

!!! function "collect_debug_data( )"
	Returns a list of details about the current lobby.

	**Returns:** dictionary

	* lobby_id (int)
	* lobby_owner (int)
	* lobby_state (int)
	* no_delay (bool)
	* no_nagle (bool)
	* steam_id (int)
	* target_peer (int)
	* unique_id (int)

### connect_lobby

!!! function "connect_lobby( `int` lobby_id )"
	Connect to the Steam lobby by given lobby ID. Signal [code]lobby_joined[/code] will be emitted after attempting to connect.

	**Returns:** result (int)


### create_lobby

!!! function "create_lobby( `int` lobby_type, `int` max_players )"
	Create a new matchmaking lobby.

	Triggers all three callbacks: [lobby_created](matchmaking.md#lobby_created), [lobby_joined](matchmaking.md#lobby_joined), and [lobby_data_update](matchmaking.md#lobby_data_update).

	**Returns:** void

	If the results returned via the [lobby_created](#lobby_created) call result indicate success then the lobby is joined and ready to use at this point.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#CreateLobby){ .md-button .md-button--store target="_blank" }

### get_all_lobby_data

!!! function "get_all_lobby_data( )"
	Get all know data about the current lobby.
	
	**Returns:** dictionary of key-value pairs.

### get_lobby_data

!!! function "get_lobby_data( `string` key )"
	Gets the metadata associated with the specified key from the specified lobby.

	**Returns:** string

	**Note:** This can only get metadata from lobbies that the client knows about, either after receiving a list of lobbies from lobby_match_list, retrieving the data with requestLobbyData, or after joining a lobby.

### get_lobby_id

!!! function "get_lobby_id( )"
	Returns the current lobby's ID.

	**Returns:** int

### get_peer_id_from_steam64

!!! function "get_peer_id_from_steam64( `int` steam_id )"
	Converts Steam user ID into peer ID of MultiplayerAPI's current [code]multiplayer_peer[/code].

	**Returns:** int

### get_peer_info

!!! function "get_peer_info( `int` peer_id )"
	Get a list of data about a given peer.

	**Returns:** dictionary

	* peer_id (int)
	* steam_id (int)
	* pending_packet_count (int)
	* connection_status (string)
	* packets_out_per_sec (float)
	* bytes_out_per_sec (float)
	* packets_in_per_sec (float)
	* bytes_in_per_sec (float)
	* connection_quality_local (float)
	* connection_quality_remote (float)
	* send_rate_bytes_per_second (int)
	* pending_unreliable (string)
	* pending_reliable (string)
	* sent_unacked_reliable (string)
	* queue_time (int)
	* ping (string)

### get_peer_map

!!! function "get_peer_map( )"
	Get the list of all connected peers.

	**Returns:** dictionary of key-value pairs

	* { peer id (int) : steam id (int) }

### get_state

!!! function "get_state( )"
	Get the state of the lobby the local player is currently in.

	**Returns:** int


### get_steam64_from_peer_id

!!! function "get_steam64_from_peer_id( `int` peer_id )"
	Converts peer ID of MultiplayerAPI's current **multiplayer_peer** into Steam user ID.

	**Returns:** int

### send_direct_message

!!! function "send_direct_message( `PackedByteArray` message )"
	Send a direct message as a packet.

	**Returns:** bool

### set_lobby_data

!!! function "set_lobby_data( `string` key, `string` value )"
	Sets a key/value pair in the lobby metadata. This can be used to set the the lobby name, current map, game mode, etc. This can only be set by the owner of the lobby. Each user in the lobby will be receive notification of the lobby data change via a [lobby_data_update](matchmaking.md#lobby_data_update) callback. This will only send the data if it has changed. There is a slight delay before sending the data so you can call this repeatedly to set all the data you need to and it will automatically be batched up and sent after the last sequential call. True if the data has been set successfully. False if key was invalid or value is too long.

	**Returns:** bool

### set_lobby_joinable

!!! function "set_lobby_joinable( `bool` joinable )"
	Sets whether or not a lobby is joinable by other players. This always defaults to **true** for a new lobby. If joining is disabled, then no players can join, even if they are a friend or have been invited. Lobbies with joining disabled will not be returned from a lobby search.

	**Returns:** void

{==
## :material-file-document-check-outline: Properties
==}

### as_relay
	This peer is acting as a relay.

	**Type:** bool (defaults to false)
	**Setter:** set_as_relay
	**Getter:88 get_as_relay

###  no_delay
	Has a delay.

	**Type:** bool (defaults to false)
	**Setter:** set_no_delay
	**Getter:** get_no_delay"

### no_nagle
	Is using Nagle or not.

	**Type:** bool (defaults to false)
	**Setter:** set_no_nagle
	**Getter:** get_no_nagle

{==
## :material-signal: Signals
==}

### debug_data

	Sends out dumps of internal debug data from SteamMultiplayerPeer.

	**Returns:** dictionary

	* msg (string)
	* value

### lobby_chat_update

	A lobby chat room state has changed, this is usually sent when a user has joined or left the lobby.

	**Returns:**

	* lobby_id (int)
	* changed_id (int)
	* making_change_id (int)
	* chat_state (int)

### lobby_created

	Gets emitted after creating a lobby and a game server. Creator automatically joins the lobby. Provided that result is RESULT_OK, callback for this signal would be a good moment to assign the peer to MultiplayerAPI by calling `multiplayer.set_multiplayer_peer()`.

	**Returns:**

	* result (int)
	* lobby_id (int)

### lobby_data_update

	The lobby metadata has changed.

	**Returns:**

	* success (int)
	* lobby_id (int)
	* member_id (int)

### lobby_joined

	Received upon attempting to enter a lobby. Lobby metadata is available to use immediately after receiving this. Provided that result is CHAT_ROOM_ENTER_RESPONSE_SUCCESS, callback for this signal would be a good moment to assign the peer to MultiplayerAPI by calling `multiplayer.set_multiplayer_peer()`.

	**Returns:**

	* lobby (int)
	* permissions (int)
	* locked (bool)
	* response (int)

### lobby_message

	A chat (text or binary) message for this lobby has been received.

	**Returns:**

	* lobby_id (int)
	* user (int)
	* message (String)
	* chat_type (int)

### network_session_failed

	Gets emitted when SteamMultiplayerPeer fails to establish a message session. Can be used to exit the lobby or retry the connection from client's side. Returned Steam ID can be empty, depending on the current state of connection. For more information on reason of failure check **reason** in [NetworkingConnectionEnd](networking_sockets.md#networkingconnectionend) and **connection_state** in [NetworkingConnectionState](networking_sockets.md#networkingconnectionstate) or check output of [debug_data](#debug_data) signal.

	**Returns:**

	* steam_id (int)
	* reason (int)
	* connection_state (bool)

{==
## :material-numeric: Enums
==}

### LobbyType
---|---|---
LOBBY_TYPE_PRIVATE | 0 | Lobby invisible to all other Steam users. The only way to join the lobby is from an invite.
LOBBY_TYPE_FRIENDS_ONLY | 1 | Joinable by friends and invitees, but does not show up in the lobby list provided by Steam in [lobby_match_list](matchmaking.md#lobby_match_list) callback.
LOBBY_TYPE_PUBLIC | 2 | Returned by search and visible to friends.
LOBBY_TYPE_INVISIBLE | 3 | Returned by search, but not visible to other friends.
LOBBY_TYPE_PRIVATE_UNIQUE | 4 | Private, unique and does not delete when empty - only one of these may exist per unique keypair set can only create from the Web API.

### LobbyState
LOBBY_STATE_NOT_CONNECTED | 0 | The peer is not yet connected to the lobby.
LOBBY_STATE_HOST_PENDING | 1 | The peer is creating a lobby to host.
LOBBY_STATE_HOSTING | 2 | The peer is hosting a lobby.
LOBBY_STATE_CLIENT_PENDING | 3 | The peer is joining a lobby.
LOBBY_STATE_CLIENT | 4 | The peer has joined a lobby as a client.