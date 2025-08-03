---
title: Game Server
description: A class reference for Game Server.
icon: material/server-network
---

# Game Server

!!! info "Only available in the [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### associateWithClan

!!! function "associateWithClan( `uint64_t` clan_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam ID of the group you want to be associated with.

	Associate this game server with this clan for the purposes of computing player compatibility.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[associate_clan](#associate_clan) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#AssociateWithClan){ .md-button .md-button--store target="_blank" }

### beginAuthSession

!!! function "beginAuthSession( `PackedByteArray` auth_ticket, `uint64_t` steam_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | auth_ticket | PackedByteArray | The auth ticket to validate.
    | steam_id | uint64_t | The entity's Steam ID that sent this ticket.

	Authenticate the ticket from the entity Steam ID to be sure it is valid and isn't reused.

	The ticket is created on the entity with [getAuthSessionTicket](#getauthsessionticket) and then needs to be provided over the network for the other end to validate. This registers for [validate_auth_ticket_response](user.md#validate_auth_ticket_response) callbacks if the entity goes offline or cancels the ticket. [See EAuthSessionResponse for more information](https://partner.steamgames.com/doc/api/steam_api#EAuthSessionResponse){ target="\_blank" }.

	When the multiplayer session terminates you must call [endAuthSession](#endauthsession).

	!!! returns "Returns: [BeginAuthSessionResult enum](main_server.md#beginauthsessionresult)"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#BeginAuthSession){ .md-button .md-button--store target="_blank" }

### cancelAuthTicket

!!! function "cancelAuthTicket( `uint32_t` auth_ticket )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | auth_ticket_handle | uint32_t | The active auth ticket to cancel.

	Cancel auth ticket from [getAuthSessionTicket](#getauthsessionticket); called when no longer playing game with the entity you gave the ticket to.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#CancelAuthTicket){ .md-button .md-button--store target="_blank" }

### clearAllKeyValues

!!! function "clearAllKeyValues( )"
	Call this to clear the whole list of key / values that are sent in rule queries.

	!!! returns "Returns: void"
 
    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#ClearAllKeyValues){ .md-button .md-button--store target="_blank" }

### computeNewPlayerCompatibility

!!! function "computeNewPlayerCompatibility( `uint64_t` steam_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the player that is attempting to join.

	Checks if any of the current players don't want to play with this new player that is attempting to join or vice versa; based on the frenemy system.

	!!! returns "Returns: void"
 
 	!!! trigger "Triggers"
 		[player_compat](#player_compat) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#ComputeNewPlayerCompatibility){ .md-button .md-button--store target="_blank" }

### createUnauthenticatedUserConnection

!!! function "createUnauthenticatedUserConnection( )"
	Creates a fake user (ie, a bot) which will be listed as playing on the server, but skips validation.

	!!! returns "Returns: uint64_t"
		Returns a SteamID for the user to be tracked with, you should call [endAuthSession](#endauthsession) when this user leaves the server just like you would for a real user.

	!!! info "Notes"
		This is part of the old user authentication API and should not be mixed with the new API.

### endAuthSession

!!! function "endAuthSession( `uint64_t` steam_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The entity to end the active auth session with.

	Stop tracking started by [beginAuthSession](#beginauthsession); called when no longer playing game with this entity.

	!!! returns "Returns: void"
 
    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#EndAuthSession){ .md-button .md-button--store target="_blank" }

### getAuthSessionTicket

!!! function "getAuthSessionTicket( )"
	Retrieve a authentication ticket to be sent to the entity who wishes to authenticate you. After calling this you can send the ticket to the entity where they can then call [beginAuthSession](#beginauthsession) to verify this entities integrity.

	When creating a ticket for use by the ISteamUserAuth/AuthenticateUserTicket Web API, the calling application should wait for the [get_auth_session_ticket_response](user.md#get_auth_session_ticket_response) callback generated by the API call before attempting to use the ticket to ensure that the ticket has been communicated to the server.

	If this callback does not come in a timely fashion (10 - 20 seconds), then your client is not connected to Steam, and the [authenticateUserTicket](https://partner.steamgames.com/doc/webapi/ISteamUserAuth#AuthenticateUserTicket){ target="_blank" } call will fail because it can not authenticate the user.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| handle | uint32_t | A handle to the auth ticket.
		| buffer | PackedByteArray | The new auth ticket if the call was successful.
		| size | uint32_t | Returns the length of the actual ticket.

		When you're done interacting with the entity you must call [cancelAuthTicket](#cancelauthticket) on the handle.

	!!! trigger "Triggers"
		 [get_auth_session_ticket_response](user.md#get_auth_session_ticket_response) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#GetAuthSessionTicket){ .md-button .md-button--store target="_blank" }

### getNextOutgoingPacket

!!! function "getNextOutgoingPacket( )"
	Gets a packet that the master server updater needs to send out on UDP when in GameSocketShare mode. GameSocketShare mode can be enabled when calling [serverInit](main_server.md#serverinit).

	This **must** be called repeatedly each frame until it returns 0 when in GameSocketShare mode.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| length | int | The length of the packet that needs to be to sent or 0 if there are no more packets to send this frame.
		| out | PackedByteArray | The packet that needs to be sent.
		| address | uint32_t | Returns the The IP address that this packet needs to be sent to, in host order.
		| port | uint16 | Returns the port that this packet needs to be sent through, in host order.

	!!! info "Notes"
		This should only ever be called **after** calling [handleIncomingPacket](#handleincomingpacket) for any packets that came in that frame.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#GetNextOutgoingPacket){ .md-button .md-button--store target="_blank" }

### getPublicIP

!!! function "getPublicIP( )"
	Gets the public IP of the server according to Steam. This is useful when the server is behind NAT and you want to advertise its IP in a lobby for other clients to directly connect to.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| ipv4 | int | The public IPv4 address, if available.
		| ipv6 | int | The public IPv6 address, if available.
		| type | [IPType enum](main_server.md#iptype) | The type of IP address returned.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#GetPublicIP){ .md-button .md-button--store target="_blank" }

### getSteamID

!!! function "getSteamID( )"
	Gets the Steam ID of the game server.

	!!! returns "Returns: uint64_t"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#GetSteamID){ .md-button .md-button--store target="_blank" }

### handleIncomingPacket

!!! function "handleIncomingPacket( `int` packet, `string` ip, `uint16` port )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | packet | int | The packet handle.
    | ip | string | The IP address that this packet was sent to in host order, i.e 127.0.0.1.
    | port | uint16 | The port that this packet was sent through, in host order.

    Handles a Steam master server packet when in GameSocketShare mode. When in GameSocketShare mode, instead of ISteamGameServer creating its own socket to talk to the master server on, it lets the game use its socket to forward messages back and forth. This prevents us from requiring server ops to open up yet another port in their firewalls.

	This should be called whenever a packet that starts with 0xFFFFFFFF comes in. That means it's for us. The IP and port parameters are used when you've elected to multiplex the game server's UDP socket rather than having the master server updater use its own sockets.

	Source engine games use this to simplify the job of the server admins, so they don't have to open up more ports on their firewalls. Only _after_ calling this, you should call [getNextOutgoingPacket](#getnextoutgoingpacket). GameSocketShare mode can be enabled when calling [serverInit](main_server.md#serverinit) or [serverInitEx](main_server.md#serverinit).

	!!! returns "Returns: dictionary"
		Contains the following key:

		| Key | Type | Notes |
        | --- | ---- | ----- |
        | incoming | bool | Is there an incoming packet for us?
		| data | PackedByteArray | The data from the incoming packet.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#HandleIncomingPacket){ .md-button .md-button--store target="_blank" }

### loggedOn

!!! function "loggedOn( )"
	Checks if the game server is logged on.

	!!! returns "Returns: bool"
		Returns true if the game server is logged on; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#BLoggedOn){ .md-button .md-button--store target="_blank" }

### logOff

!!! function "logOff( )"
	Begin process of logging game server out of Steam.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		* [server_connect_failure](#server_connect_failure) callback
		* [server_connected](#server_connected) callback
		* [server_disconnected](#server_disconnected) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#LogOff){ .md-button .md-button--store target="_blank" }

### logOn

!!! function "logOn( `string` token )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | token | string | -

	Begin process to login to a persistent game server account.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		* [server_connect_failure](#server_connect_failure) callback
		* [server_connected](#server_connected) callback
		* [server_disconnected](#server_disconnected) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#LogOn){ .md-button .md-button--store target="_blank" }

### logOnAnonymous

!!! function "logOnAnonymous( )"
	Login to a generic, anonymous account.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		* [server_connect_failure](#server_connect_failure) callback
		* [server_connected](#server_connected) callback
		* [server_disconnected](#server_disconnected) callback

	!!! info "Notes"
		In previous versions of the SDK, this was automatically called within serverInit, but this is no longer the case.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#LogOnAnonymous){ .md-button .md-button--store target="_blank" }

### requestUserGroupStatus

!!! function "requestUserGroupStatus( `uint64_t` steam_id, `uint64_t` group_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The user to check the group status of.
    | group_id | uint64_t | The group to check.

	Ask if user is in the specified group; results returned by [client_group_status](#client_group_status).

	!!! returns "Returns: bool"
		Returns false if we're not connected to the steam servers and thus cannot ask.

	!!! trigger "Triggers"
		 [client_group_status](#client_group_status) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#RequestUserGroupStatus){ .md-button .md-button--store target="_blank" }

### secure

!!! function "secure( )"
	Checks whether the game server is in "Secure" mode.

	!!! returns "Returns: bool"
		Returns true if the game server secure; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#BSecure){ .md-button .md-button--store target="_blank" }

### setAdvertiseServerActive

!!! function "setAdvertiseServerActive( `bool` active )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | active | bool | Whether to set as active or not.

	Indicate whether you wish to be listed on the master server list and / or respond to server browser / LAN discovery packets.

	The server starts with this value set to false.  You should set all relevant server parameters before enabling advertisement on the server.

	!!! returns "Returns: void"

	!!! info "Notes"
		This function used to be named **EnableHeartbeats**, so if you are wondering where that function went, it's right here.  It does the same thing as before, the old name was just confusing.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#EnableHeartbeats){ .md-button .md-button--store target="_blank" }

### setBotPlayerCount

!!! function "setBotPlayerCount( `int` bots )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | bots | int | The number of bot / AI players currently playing on the server.

	Sets the number of bot / AI players on the game server. The default value is 0.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetBotPlayerCount){ .md-button .md-button--store target="_blank" }

### setDedicatedServer

!!! function "setDedicatedServer( `bool` dedicated )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | dedicated | bool | Is this a dedicated server (true) or a listen server (false)?

	Sets the whether this is a dedicated server or a listen server. The default is listen server.

	!!! returns "Returns: void"

	!!! info "Notes"
		This only be set before calling [logOn](#logon) or [logOnAnonymous](#logonanonymous).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetDedicatedServer){ .md-button .md-button--store target="_blank" }

### setGameData


!!! function "setGameData( `string` data )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | data | string | The new "gamedata" value to set. Must not be an empty string. This can not be longer than [MAX_GAME_SERVER_GAME_DATA (2048)](main_server.md#constants).

	Sets a string defining the "gamedata" for this server, this is optional, but if set it allows users to filter in the matchmaking/server-browser interfaces based on the value.  Maximum of [MAX_GAME_SERVER_GAME_DATA (2048)](main_server.md#constants)

	This is usually formatted as a comma or semicolon separated list. Don't set this unless it actually changes, its only uploaded to the master once; when acknowledged.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetGameData){ .md-button .md-button--store target="_blank" }

### setGameDescription

!!! function "setGameDescription( `string` description )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | description | string | The description of your game. Must not be an empty string. This can not be longer than [MAX_GAME_SERVER_GAME_DESCRIPTION (64)](main_server.md#constants).

	Description of the game.  This is a required field, but it will go away eventually, as the data should be determined from the AppID.

	Setting this to the full name of your game is recommended.

	!!! returns "Returns: void"

	!!! info "Notes"
		This is required for all game servers and can only be set before calling [logOn](#logon) or [logOnAnonymous](#logonanonymous).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetGameDescription){ .md-button .md-button--store target="_blank" }

### setGameTags

!!! function "setGameTags( `string` tags )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | tags | string | The new "gametags" value to set. Must not be NULL or an empty string (""). This can not be longer than [MAX_GAME_SERVER_TAGS (128)](main_server.md#constants).

	Sets a string defining the "gametags" for this server, this is optional, but if set it allows users to filter in the matchmaking/server-browser interfaces based on the value.  Maximum of [MAX_GAME_SERVER_TAGS (128)](main_server.md#constants)

	This is usually formatted as a comma or semicolon separated list. Don't set this unless it actually changes, its only uploaded to the master once; when acknowledged.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetGameTags){ .md-button .md-button--store target="_blank" }

### setKeyValue

!!! function "setKeyValue( `string` key, `string` value )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | key | string | The key being set.
    | value | string | The value set for this key.

	Add / update a rules key / value pair.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetKeyValue){ .md-button .md-button--store target="_blank" }

### setMapName

!!! function "setMapName( `string` map )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | map | string | The new map name to set. Must not be an empty string. This can not be longer than [MAX_GAME_SERVER_MAP_NAME (32)](main_server.md#constants).

	Sets the name of map to report in the server browser.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetMapName){ .md-button .md-button--store target="_blank" }

### setMaxPlayerCount

!!! function "setMaxPlayerCount( `int` players_max )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | players_max | int | The new maximum number of players allowed on this server.

	Max player count that will be reported to server browser and client queries.  This value may be changed at any time.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetMaxPlayerCount){ .md-button .md-button--store target="_blank" }

### setModDir

!!! function "setModDir( `string` mod_directory )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | mod_directory | string | The game directory to set. Must not be an empty string. This can not be longer than [MAX_GAME_SERVER_GAME_DIR (32)](main_server.md#constants).

	Sets the game directory. This should be the same directory game where gets installed into. Just the folder name, not the whole path. e.g. "Spacewar".

	!!! returns "Returns: void"

	!!! info "Notes"
		This is required for all game servers and can only be set before calling [logOn](#logon) or [logOnAnonymous](#logonanonymous).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetModDir){ .md-button .md-button--store target="_blank" }

### setPasswordProtected

!!! function "setPasswordProtected( `bool` password_protected )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | password_protected | bool | Enable (true) or disable (false) password protection.

	Set whether the game server will require a password once when the user tries to join.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetPasswordProtected){ .md-button .md-button--store target="_blank" }

### setProduct

!!! function "setProduct( `string` product )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | product | string | The unique identifier for your game. Must not be an empty string.

	Sets the game product identifier.  This is currently used by the master server for version checking purposes.

	It's a required field, but will eventually will go away, and the app ID will be used for this purpose.  For now, converting the game's app ID to a string for this is recommended.

	!!! returns "Returns: void"

	!!! info "Notes"
		This is required for all game servers and can only be set before calling [logOn](#logon) or [logOnAnonymous](#logonanonymous).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetProduct){ .md-button .md-button--store target="_blank" }

### setRegion

!!! function "setRegion( `string` region )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | region | string | - 

	Region identifier. This is an optional field, the default value is an empty string, meaning the "world" region.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetRegion){ .md-button .md-button--store target="_blank" }

### setServerName

!!! function "setServerName( `string` name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | name | string | The new server name to set. Must not be an empty string. This can not be longer than [MAX_GAME_SERVER_NAME (64)](main_server.md#constants).

	Sets the name of server as it will appear in the server browser.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetServerName){ .md-button .md-button--store target="_blank" }

### setSpectatorPort

!!! function "setSpectatorPort( `int` port )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | port | int | The port for spectators to join.

	Spectator server port to advertise.  The default value is zero, meaning the service is not used.  If your server receives any info requests on the LAN, this is the value that will be placed into the reply for such local queries.

	This is also the value that will be advertised by the master server.  The only exception is if your server is using a [fakeIP](networking_sockets.md).  Then the second fake port number (index 1) assigned to your server will be listed on the master server as the spectator port, if you set this value to any non-zero value.

	This function merely controls the values that are advertised, it's up to you to configure the server to actually listen on this port and handle any spectator traffic.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetSpectatorPort){ .md-button .md-button--store target="_blank" }

### setSpectatorServerName

!!! function "setSpectatorServerName( `string` name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | name | string | The spectator server name to set. Must not be an empty string. This can not be longer than [MAX_GAME_SERVER_MAP_NAME](main_server.md#constants).

	Sets the name of the spectator server. This is only used if spectator port is non-zero.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#SetSpectatorServerName){ .md-button .md-button--store target="_blank" }

### updateUserData

!!! function "updateUserData( `uint64_t` steam_id, `string` player_name, `uint32_t` score )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the user.
    | player_name | string | The name of the user.
    | score | uint32_t | The current score of the user.

	Update the data to be displayed in the server browser and matchmaking interfaces for a user currently connected to the server.

	!!! returns "Returns: bool"
	 	Returns true if successful; otherwise, false if failure; ie, **steam_id** wasn't for an active player.

### userHasLicenseForApp

!!! function "userHasLicenseForApp( `uint64_t` steam_id, `uint32_t` app_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the user that sent the auth ticket.
    | app_id | uint32_t | The DLC app ID to check if the user owns it.

	Checks if the user owns a specific piece of Downloadable Content (DLC). This can only be called after sending the users auth ticket to [beginAuthSession.](#beginauthsession)

	!!! returns "Returns: [UserHasLicenseForAppResult enum](main_server.md#userhaslicenseforappresult)"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#UserHasLicenseForApp){ .md-button .md-button--store target="_blank" }

### wasRestartRequested

!!! function "wasRestartRequested( )"
	Checks if the master server has alerted us that we are out of date and has requested a restart. This reverts back to false after calling this function. Only returns true once per request.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#WasRestartRequested){ .md-button .md-button--store target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### associate_clan

!!! function "associate_clan"
	Sent as a reply to [associateWithClan](#associatewithclan).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [Result enum](main_server.md#result) | Result of the call.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#AssociateWithClanResult_t){ .md-button .md-button--store target="_blank" }

### client_approved

!!! function "client_approved"
	Client has been approved to connect to this game server.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| steam_id | uint64_t | Steam ID of approved player.
		| owner_id | uint64_t | Steam ID of original owner for game license.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#GSClientApprove_t){ .md-button .md-button--store target="_blank" }

### client_denied

!!! function "client_denied"
	Client has been denied to connection to this game server.

	Emits signal in response to function sendUserConnectAndAuthenticate (currently missing from Server).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| steam_id | uint64_t | The Steam ID of the user that attempted to connect.
		| reason | [DenyReason enum](main_server.md#denyreason) | The reason the player was denied.
		| optional_message | string | An optional text message explaining the deny reason. Typically unused except for logging.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#GSClientDeny_t){ .md-button .md-button--store target="_blank" }

### client_group_status

!!! function "client_group_status"
	Sent as a reply to [requestUserGroupStatus](#requestusergroupstatus).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| steam_id | uint64_t | The user whose group status we queried.
		| group_id | uint64_t | The group that we queried.
		| member | bool | Is the user a member of the group (true) or not (false)?
		| officer | bool | Is the user an officer in the group (true) or not (false)? This will never be true if **member** is false.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#GSClientGroupStatus_t){ .md-button .md-button--store target="_blank" }

### client_kick

!!! function "client_kick"
	Request the game server should kick the user.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| steam_id | uint64_t | The Steam ID of the player that should be kicked.
		| reason | [DenyReason enum](main_server.md#denyreason) | The reason the player is being kicked.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#GSClientKick_t){ .md-button .md-button--store target="_blank" }

### gamplay_stats

!!! function "gameplay_stats"
	GameServer gameplay stats info.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main_server.md#result) | The result of the operation.
        | rank | int32_t | The overall rank of the server (0-based).
        | total_connects | uint32_t | Total number of clients who have ever connected to the server.
        | total_minutes_played | uint32_t | Total number of minutes ever played on the server.

### player_compat

!!! function "player_compat"
	Sent as a reply to [computeNewPlayerCompatibility](#computenewplayercompatibility).
	
	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [Result enum](main_server.md#result) | The result of the operation. Possible values: [RESULT_OK](main_server.md#result) on success, [RESULT_FAIL](main_server.md#result) for generic failures, [RESULT_TIMEOUT](main_server.md#result) if the message was sent to the Steam servers but it didn't respond, [RESULT_NO_CONNECTION](main_server.md#result) if your Steam client doesn't have a connection to the back-end.
		| players_dont_like_candidate | int | The number of current players that don't like playing with the specified player.
		| players_candidate_doesnt_like | int | The number of players on the server that the specified player doesn't like playing with.
		| clan_players_dont_like_candidate | int | The number of players in the associated Steam group that don't like playing with the player.
		| steam_id | uint64_t | The Steam ID of the specified player.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#ComputeNewPlayerCompatibilityResult_t){ .md-button .md-button--store target="_blank" }

### policy_response

!!! function "policy_response"
	Received when the game server requests to be displayed as secure (VAC protected).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | secure | uint8 | Secure is true if the game server should display itself as secure to users; otherwise, false.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamGameServer#GSPolicyResponse_t){ .md-button .md-button--store target="_blank" }

### server_connect_failure

!!! function "server_connect_failure"
	Logging the game server onto Steam.

	Emits signal in response to functions [logOff](#logoff), [logOn](#logon), or [logOnAnonymous](#logonanonymous).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main_server.md#result) | The reason why the connection failed.
		| retrying | bool | Is the Steam client still trying to connect to the server?

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#SteamServerConnectFailure_t){ .md-button .md-button--store target="_blank" }

### server_connected

!!! function "server_connected"
	Server has connected to the Steam back-end.

	Emits signal in response to functions [logOff](#logoff), [logOn](#logon), or [logOnAnonymous](#logonanonymous).

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#SteamServersConnected_t){ .md-button .md-button--store target="_blank" }

### server_disconnected

!!! function "server_disconnected"
	Called if the client has lost connection to the Steam servers. Real-time services will be disabled until a matching [server_connected](#server_connected) has been posted.

	Emits signal in response to functions [logOff](#logoff), [logOn](#logon), or [logOnAnonymous](#logonanonymous).

	!!! returns "Returns"
		| Key | Type | Notes |
		| --- | ---- | ----- |
		| result | [Result enum](main_server.md#result) | The reason we were disconnected from Steam.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#SteamServersDisconnected_t){ .md-button .md-button--store target="_blank" }
