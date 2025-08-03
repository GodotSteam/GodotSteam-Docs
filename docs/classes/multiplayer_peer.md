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

### add_peer

!!! function "add_peer( `uint64_t` steam_id, `int` virtual_port = 0 )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the player to add as a peer.
    | virtual_port | int | Expected to be 0; only helpful if you want to have multiple connections between two users. Defaults to 0.

    Creates a connection with a new peer.  Runs [connectP2P](networking_sockets.md#connectp2p) under the hood.

    !!! returns "Returns: [Error enum](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#enum-globalscope-error){ target="\_blank" }"
		Returns OK upon success; otherwise, ERR_CANT_CREATE if the underlying P2P connection fails.

### connect_to_lobby

!!! function "connect_to_lobby( `uint64_t` lobby_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | lobby_id | uint64_t | The Steam lobby ID to connect to.

    Checks you are in the lobby by calling [getLobbyOwner](matchmaking.md#getlobbyowner) or prints an error [ERR_CANT_CREATE](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#enum-globalscope-error).

    It then calls [create_client](#create_client) for the host or prints an [Error enum](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#enum-globalscope-error){ target="\_blank" } if there is no host found.

    Finally it connects to the rest of the members of the lobby, calling [add_peer](#add_peer) for each.

	!!! returns "Returns: [Error enum](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#enum-globalscope-error){ target="\_blank" }"
		Returns OK upon success; otherwise, [ERR_ALREADY_IN_USE](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#enum-globalscope-error) if **connection_status** is not [CONNECTION_DISCONNECTED](https://docs.godotengine.org/en/stable/classes/class_multiplayerpeer.html#enum-multiplayerpeer-connectionstatus){ target="\_blank" }.

### create_client

!!! function "create_client( `uint64_t` steam_id, `int` virtual_port = 0 )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the host we're connecting to.
    | virtual_port | int | Expected to be 0; only helpful if you want to have multiple connections between two users. Defaults to 0.

    On success, this sets **server** to false and **unique_id** to a newly generated unique ID.

    It then creates a listen socket with [createListenSocketP2P](networking_sockets.md#createlistensocketp2p) and a poll group with [createPollGroup](networking_sockets.md#createpollgroup) under-the-hood.

    It then calls [add_peer](#add_peer) with the host **steam_id** and **virtual_port**.

    Finally it sets **set_refuse_new_connections** to false and **connection_status** to CONNECTION_CONNECTING.

	!!! returns "Returns: [Error enum](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#enum-globalscope-error){ target="\_blank" }"
		Returns OK upon success; otherwise, ERR_ALREADY_IN_USE if **connection_status** is not CONNECTION_DISCONNECTED.

### create_host

!!! function "create_host( `int` virtual_port = 0 )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | virtual_port | int | Expected to be 0; only helpful if you want to have multiple connections between two users. Defaults to 0.

	On success, this sets **server** to true and **unique_id** to 1.

    It then creates a listen socket with [createListenSocketP2P](networking_sockets.md#createlistensocketp2p) and a poll group with [createPollGroup](networking_sockets.md#createpollgroup) under-the-hood.

    Finally it sets **set_refuse_new_connections** to false and **connection_status** to CONNECTION_CONNECTED.

	!!! returns "Returns: [Error enum](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#enum-globalscope-error){ target="\_blank" }"
		Returns OK upon success; otherwise, ERR_ALREADY_IN_USE if **connection_status** is not CONNECTION_DISCONNECTED.

### get_debug_level

!!! function "get_debug_level( )"
	Returns the internal **debug_level** property value.

	!!! returns "Returns: DebugLevel enum"

### get_no_delay

!!! function "get_no_delay( )"
	Returns the internal **no_delay** property value.

	!!! returns "Returns: bool"

### get_no_nagle

!!! function "get_no_nagle( )"
	Returns the internal **no_nagle** property value.

	!!! returns "Returns: bool"

### get_peer

!!! function "get_peer( `int` peer_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | peer_id | int | The ID of the packet peer to retrieve.

	!!! returns "Returns: SteamPacketPeer"
		Returns a [SteamPacketPeer](steam_packet_peer.md) RefCounted object on success; otherwise, null.

### get_peer_id_for_steam_id

!!! function "get_peer_id_for_steam_id( `uint64_t` steam_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID to get the peer ID from.

    Returns the peer ID for the given Steam ID; if it exists.

    !!! returns "Returns: int"
    	Returns the peer ID on success or the **unique_id** if passing the local user's Steam ID; otherwise, 0.

### get_steam_id_for_peer_id

!!! function "get_steam_id_for_peer_id( `int` peer_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | peer_id | int | The peer ID to get the Steam ID from.

    Returns the Steam ID for the given peer ID; if it exists.

    !!! returns "Returns: uint64_t"
    	Returns the Steam ID on success or the local user's Steam ID if passing the **unique_id**; otherwise, 0.

### host_with_lobby

!!! function "host_with_lobby( `uint64_t` lobby_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | lobby_id | uint64_t | The lobby ID we are hosting.

    You must have created a lobby with [createLobby](matchmaking.md#createlobby) prior to calling this.  If you use a **lobby_id** you are not the owner of, this will error with ERR_CANT_CREATE.

    Sets **tracked_lobby** to this lobby on success.

    It then calls [create_host](#create_host) and [add_peer](#add_peer) for all lobby members.

	!!! returns "Returns: [Error enum](https://docs.godotengine.org/en/stable/classes/class_@globalscope.html#enum-globalscope-error){ target="\_blank" }"
		Returns OK upon success; otherwise, ERR_ALREADY_IN_USE if **connection_status** is not CONNECTION_DISCONNECTED.

### set_debug_level

!!! function "set_debug_level( `DebugLevel` debug_level )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | debug_level | [DebugLevel enum](#debuglevel) | The debug level to set.

    Sets the **debug_level** property internally.

    !!! returns "Returns: void"

### set_no_delay

!!! function "set_no_delay( `bool` no_delay )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | no_delay | bool | Whether or not to use [NETWORKING_SEND_NO_DELAY](networking_utils.md#constants) flag.

    Sets the **no_delay** property internally.

	!!! returns "Returns: void"

### set_no_nagle

!!! function "set_no_nagle( `bool` no_nagle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | no_nagle | bool | Whether or not to use [NETWORKING_SEND_NO_NAGLE](networking_utils.md#constants) flag.

    Sets the **no_nagle** property internally.

	!!! returns "Returns: void"

{==
## :material-variable: Properties
==}

Name | Variant | Set | Get
---- | ------- | --- | ---
debug_level | int | [set_debug_level](#set_debug_level) | [get_debug_level](#get_debug_level)
no_delay | bool | [set_no_delay](#set_no_delay) | [get_no_delay](#get_no_delay)
no_nagle | bool | [set_no_nagle](#set_no_nagle) | [get_no_nagle](#get_no_nagle)

{==
## :material-numeric: Enums
==}

### DebugLevel

Enumerator | SDK Name | Value | Details
---------- | -------- | ----- | -------
DEBUG_LEVEL_NONE | - | 0 | -
DEBUG_LEVEL_PEER | - | 1 | -
DEBUG_LEVEL_STEAM | - | 2 | -