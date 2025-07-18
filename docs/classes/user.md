---
title: User
description: A class reference for User.
icon: fontawesome/regular/user
---

# User

Functions for accessing and manipulating Steam user information. This is also where the APIs for [Steam Voice](https://partner.steamgames.com/doc/features/voice){ target="\_blank" } are exposed.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### advertiseGame

!!! function "advertiseGame( ```string``` server_ip = "", ```uint16``` port = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | server_ip | string | The IP of the game server in host order, i.e 127.0.0.1 == 0x7f000001. Defaults to empty string. |
    | port | uint16 | The connection port of the game server, in host order. Defaults to 0. |

	Set the rich presence data for an unsecured game server that the user is playing on. This allows friends to be able to view the game info and join your game.

	When you are using Steam authentication system this call is never required, the auth system automatically sets the appropriate rich presence.

	If either or both defaults are left, calling this function will clear the game advertisement.

	!!! return "Returns: void"

	!!! info "Notes"
		This is a legacy function.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#AdvertiseGame){ .md-button .md-button--doc_classes target="_blank" }

### beginAuthSession

!!! function "beginAuthSession( ```PoolByteArray / PackedByteArray``` auth_ticket, ```int``` ticket_size, ```uint64_t``` steam_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | auth_ticket | PackedByteArray | The auth ticket to validate. |
    | ticket_size | int | The size in bytes of the auth_ticket. This must be the 'size' key form the dictionary provided by the call that created this ticket. |
    | steam_id | uint64_t | The entity's Steam ID that sent this ticket.

    Authenticate the ticket from the entity's Steam ID to be sure it is valid and isn't reused. The ticket is first created by the entity with [getAuthSessionTicket](#getauthsessionticket) and then needs to be provided over the network for the other end to validate.

	When the multiplayer session terminates you must call [endAuthSession](#endauthsession).

	Beginning an auth session also registers for these callbacks if the entity goes offline or cancels the ticket.

	!!! returns "Returns: int / [BeginAuthSessionResult enum](main.md#beginauthsessionresult)"

	!!! trigger "Triggers"
		[validate_auth_ticket_response](#validate_auth_ticket_response) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#BeginAuthSession){ .md-button .md-button--doc_classes target="_blank" }

### cancelAuthTicket

!!! function "cancelAuthTicket( ```uint32_t``` auth_ticket )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | auth_ticket | uint32_t | The active auth ticket to cancel. |

	Cancels an auth ticket received from [getAuthSessionTicket](#getauthsessionticket). This should be called when no longer playing with the specified entity.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[validate_auth_ticket_response](#validate_auth_ticket_response) callback for any other server or client who called [beginAuthSession](#beginauthsession) using this ticket to let them know it is no longer valid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#CancelAuthTicket){ .md-button .md-button--doc_classes target="_blank" }

### decompressVoice

!!! function "decompressVoice( ```PoolByteArray / PackedByteArray``` voice, ```uint32_t``` sample_rate, ```uint32_t``` buffer_size_override = 20480 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | voice_data | PackedByteArray | 
    | sampe_rate | uint32_t | The sample rate that will be returned. This can be from 11025 to 48000, you should either use the rate that works best for your audio playback system, which likely takes the users audio hardware into account, or you can use [getVoiceOptimalSampleRate](#getvoiceoptimalsamplerate) to get the native sample rate of the Steam voice decoder. |
    | buffer_size_override | uint32_t | Starter size of the buffer. Defaults to 20480. |

    Decodes the compressed voice data returned by [getVoice](#getvoice).

	The output data is raw single-channel 16-bit PCM audio. The decoder supports any sample rate from 11025 to 48000. See [getVoiceOptimalSampleRate](#getvoiceoptimalsamplerate) for more information.

	If the output buffer is not large enough, then the buffer will be set to the required buffer size, and [VOICE_RESULT_BUFFER_TOO_SMALL](main.md#voiceresult) is returned. It is recommended that you start with a 20KiB buffer and then reallocate as necessary.

	See [Steam Voice](https://partner.steamgames.com/doc/features/voice){ target="_blank" } for more information.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [VoiceResult enum](main.md#voiceresult) | The result of the call.
		| size | uint32_t | The size of the buffer passed into **uncompressed**.
		| uncompressed | PoolByteArray / PackedByteArray | The buffer where the raw audio data will be returned. This can then be passed to your audio subsystems for playback.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#DecompressVoice){ .md-button .md-button--doc_classes target="_blank" }

### endAuthSession

!!! function "endAuthSession( ```uint64_t``` steam_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the entity to end the active auth session with. |

    Ends an auth session that was started with [beginAuthSession](#beginauthsession). This should be called when no longer playing with the specified entity.

	!!! returns "Returns: void"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#EndAuthSession){ .md-button .md-button--doc_classes target="_blank" }

### getAuthSessionTicket

!!! function "getAuthSessionTicket( ```uint64_t``` remote_steam_id = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | remote_steam_id | uint64_t | The Steam ID of the remote system that will authenticate the ticket. Defaults to 0. |

    Retrieve an authentication ticket to be sent to the entity who wishes to authenticate you.

	After calling this you can send the ticket to the entity where they can then call [beginAuthSession](#beginauthsession) to verify this entity's integrity.

	**remote_steam_id** is an optional input parameter to use the public IP address via identity system or Steam ID of the entity you are connecting to. Steam will only allow the ticket to be used by an entity with that identity.

	**Note:** This API can not be used to create a ticket for use by the [AuthenticateUserTicket Web API](https://partner.steamgames.com/doc/webapi/ISteamUserAuth){ target="_blank" }. Use [getAuthTicketForWebApi](#getauthticketforwebapi) for that instead.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| id | uint32_t | A handle to the auth ticket. When you're done interacting with the entity you must call [cancelAuthTicket](#cancelauthticket) on the handle.
		| buffer | PoolByteArray / PackedByteArray | 
		| size | uint32_t | The length of the ticket; unnecessary really.

	!!! trigger "Triggers"
		[get_auth_session_ticket_response](#get_auth_session_ticket_response) callback

	!!! info "Notes"
		As of Steamworks SDK 1.57, you may pass a Steam ID class. However, this is optional and defaults to NULL.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetAuthSessionTicket){ .md-button .md-button--doc_classes target="_blank" }

### getAuthTicketForWebApi

!!! function "getAuthTicketForWebApi( ```string``` service_identity = "" )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | service_identity | string | The identity of the remote service that will authenticate the ticket. The service should provide a string identifier; pass an empty string if none was provided. Defaults to empty string. |

    Request a ticket which will be used for the [AuthenticateUserTicket Web API](https://partner.steamgames.com/doc/webapi/ISteamUserAuth){ target="\_blank" }.

	!!! returns "Returns: auth_ticket_handle (uint32_t)"

	!!! trigger "Triggers"
		[get_ticket_for_web_api](#get_ticket_for_web_api) callback which will include the actual ticket.

	!!! info "Notes"
		The **service_identity** is **not** a network identity used by or created with GodotSteam's internal identity system. It is an optional input parameter to identify the service the ticket will be sent to.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetAuthTicketForWebApi){ .md-button .md-button--doc_classes target="_blank" }

### getAvailableVoice

!!! function "getAvailableVoice( )"
	Checks to see if there is captured audio data available from [getVoice](#getvoice), and gets the size of the data.

	Most applications will only use compressed data and should ignore the other parameters, which exist primarily for backwards compatibility. See [getVoice](#getvoice) for further explanation of "uncompressed" data.

	See [Steam Voice](https://partner.steamgames.com/doc/features/voice){ target="_blank" } for more information.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [VoiceResult enum](main.md#voiceresult) | The result of the call.
		| buffer | uint32_t | The size of the voice data.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetAvailableVoice){ .md-button .md-button--doc_classes target="_blank" }
    [ :material-tag-remove: Removed GodotSteam 4.16](../changelog/godot4.md/#version-416){ .md-button .md-button--changes target="_blank" }

### getDurationControl

!!! function "getDurationControl( )"
	Retrieves anti indulgence / duration control for current user / game combination.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[duration_control](#duration_control) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetDurationControl){ .md-button .md-button--doc_classes target="_blank" }

### getEncryptedAppTicket

!!! function "getEncryptedAppTicket( )"
	Retrieve an encrypted ticket.

	This should be called after requesting an encrypted app ticket with [requestEncryptedAppTicket](#requestencryptedappticket) and receiving the [encrypted_app_ticket_response](#encrypted_app_ticket_response) call result.

	You should then pass this encrypted ticket to your secure servers to be decrypted using your secret key.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| buffer | PoolByteArray / PackedByteArray | The encrypted app ticket is copied into this buffer.
		| size | uint32_t | The size of the ticket buffer; unnecessary.

		!!! info "Notes"
			If you call this without calling [requestEncryptedAppTicket](#requestencryptedappticket), the call may succeed but you will likely get a stale ticket.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetEncryptedAppTicket){ .md-button .md-button--doc_classes target="_blank" }

### getGameBadgeLevel

!!! function "getGameBadgeLevel( ```int``` series, ```bool``` foil )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | series | int | If you only have one set of cards, the series will be 1. |
    | foil | bool | Check if they have received the foil badge. |

    Gets the level of the users Steam badge for your game.

	The user can have two different badges for a series; the regular badge (max level 5) and the foil badge (max level 1).

	!!! returns "Returns: int"
		The level of the badge; 0 if they do not have it.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetGameBadgeLevel){ .md-button .md-button--doc_classes target="_blank" }

### getMarketEligibility

!!! function "getMarketEligibility( )"
	Get whether or not the current user can trade on Steam Market, if there is a device cooldown in place, and the number of days the user is required to have had Steam Guard on for.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[market_eligibility_response](#market_eligibility_response) call result

	---
	[ :material-tag-plus: Added GodotSteam 4.16](../changelog/godot4.md/#version-416){ .md-button .md-button--changes target="_blank" }

### getPlayerSteamLevel

!!! function "getPlayerSteamLevel( )"
	Gets the Steam level of the user, as shown on their Steam community profile.

	!!! returns "Returns: int"
		The current level of the user.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetPlayerSteamLevel){ .md-button .md-button--doc_classes target="_blank" }

### getSteamID

!!! function "getSteamID( )"
	Gets the Steam ID (ID64) of the account currently logged into the Steam client. This is commonly called the 'current user', or 'local user'.

	A Steam ID is a unique identifier for a Steam accounts, Steam groups, Lobbies and Chat rooms, and used to differentiate users in all parts of the Steamworks API.

	!!! returns "Returns: uint64_t"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetSteamID){ .md-button .md-button--doc_classes target="_blank" }

### getVoice

!!! function "getVoice( ```uint32_t``` buffer_size_override = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | buffer_size_override | uint32_t | The base buffer size, if it this is 0 then GodotSteam will internally call [getAvailableVoice](#getavailablevoice) to resize the buffer. Defaults to 0. |

    Read captured audio data from the microphone buffer.

	The compressed data can be transmitted by your application and decoded back into raw audio data using [decompressVoice](#decompressvoice) on the other side. The compressed data provided is in an arbitrary format and is not meant to be played directly.

	This should be called once per frame, and at worst no more than four times a second to keep the microphone input delay as low as possible. Calling this any less may result in gaps in the returned stream.

	It is recommended that you pass in an 8 kilobytes or larger destination buffer for compressed audio. Static buffers are recommended for performance reasons. However, if you would like to allocate precisely the right amount of space for a buffer before each call you may use [getAvailableVoice](#getavailablevoice) to find out how much data is available to be read.

	See [Steam Voice](https://partner.steamgames.com/doc/features/voice){ target="_blank" } for more information.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [VoiceResult enum](main.md#voiceresult) | The result of the call.
		| written | uint32_t | The size of the buffer.
		| buffer | PackedByteArray | The buffer of the audio data.

	!!! info "Notes"
		"Uncompressed" audio is a deprecated feature and should not be used by most applications. It is raw single-channel 16-bit PCM wave data which may have been run through preprocessing filters and/or had silence removed, so the uncompressed audio could have a shorter duration than you expect. There may be no data at all during long periods of silence.

		Also, fetching uncompressed audio will cause [getVoice](#getvoice) to discard any leftover compressed audio, so you must fetch both types at once.

		Finally, [getAvailableVoice](#getavailablevoice) is not precisely accurate when the uncompressed size is requested. So if you really need to use uncompressed audio, you should call [getVoice](#getvoice) frequently with two very large (20KiB+) output buffers instead of trying to allocate perfectly-sized buffers. But most applications should ignore all of these details and simply leave the "uncompressed" parameters as NULL/0.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetVoice){ .md-button .md-button--doc_classes target="_blank" }	

### getVoiceOptimalSampleRate

!!! function "getVoiceOptimalSampleRate( )"
	Gets the native sample rate of the Steam voice decoder.

	Using this sample rate for [decompressVoice](#decompressvoice) will perform the least CPU processing. However, the final audio quality will depend on how well the audio device (and/or your application's audio output SDK) deals with lower sample rates. You may find that you get the best audio output quality when you ignore this function and use the native sample rate of your audio output device, which is usually 48000 or 44100.

	See [Steam Voice](https://partner.steamgames.com/doc/features/voice){ target="_blank" } for more information.

	!!! returns "Returns: int"
		The native sample rate of the Steam voice decoder.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetVoiceOptimalSampleRate){ .md-button .md-button--doc_classes target="_blank" }

### initiateGameConnection

!!! function "initiateGameConnection( ```uint64_t``` server_id, ```uint32_t``` server_ip, ```uint16``` server_port, ```bool``` secure )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | server_id | uint64_t | The Steam ID of the game server, received from the game server by the client. |
    | server_ip | uint32_t | The IP address of the game server in host order. |
    | server_port | uint16 | The connection port of the game server, in host order. |
    | secure | bool | Whether or not the client thinks that the game server is reporting itself as secure (i.e. VAC is running.) |

	This starts the state machine for authenticating the game client with the game server.

	It is the client portion of a three-way handshake between the client, the game server, and the steam servers.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| auth_blob | PoolByteArray / PackedByteArray | The server's authentication token.
		| server_id | uint64_t | The Steam ID of the game server, received from the game server by the client.
		| server_ip | uint32_t | The IP address of the game server in host order.
		| server_port | uint16 | The connection port of the game server, in host order.

	!!! info "Notes"
		When you're done with the connection you must call [terminateGameConnection](#terminategameconnection).

		This is part of the old user authentication API and should not be mixed with the new API. Please migrate to [beginAuthSession](#beginauthsession) and related functions.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#InitiateGameConnection){ .md-button .md-button--doc_classes target="_blank" }

### isBehindNAT

!!! function "isBehindNAT( )"
	Checks if the current user looks like they are behind a NAT device.

	This is only valid if the user is connected to the Steam servers and may not catch all forms of NAT.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#BIsBehindNAT){ .md-button .md-button--doc_classes target="_blank" }

### isPhoneIdentifying

!!! function "isPhoneIdentifying( )"
	Checks whether the user's phone number is used to uniquely identify them.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#BIsPhoneIdentifying){ .md-button .md-button--doc_classes target="_blank" }

### isPhoneRequiringVerification

!!! function "isPhoneRequiringVerification( )"
	Checks whether the current user's phone number is awaiting (re)verification.

	!!! returns "Returns: bool"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#BIsPhoneRequiringVerification){ .md-button .md-button--doc_classes target="_blank" }

### isPhoneVerified

!!! function "isPhoneVerified( )"
	Checks whether the current user has verified their phone number.

	See the [Steam Guard Mobile Authenticator](https://support.steampowered.com/kb_article.php?ref=8625-wrah-9030){ target="_blank" } page on the customer facing Steam Support site for more information.
	
	!!! returns "Returns: bool"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#BIsPhoneVerified){ .md-button .md-button--doc_classes target="_blank" }

### isTwoFactorEnabled

!!! function "isTwoFactorEnabled( )"
	Checks whether the current user has Steam Guard two factor authentication enabled on their account.

	See the [Steam Guard Mobile Authenticator](https://support.steampowered.com/kb_article.php?ref=8625-wrah-9030){ target="_blank" } page on the customer facing Steam Support site for more information.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#BIsTwoFactorEnabled){ .md-button .md-button--doc_classes target="_blank" }

### loggedOn

!!! function "loggedOn( )"
	Checks if the current user's Steam client is connected to the Steam servers.

	If it's not then no real-time services provided by the Steamworks API will be enabled. The Steam client will automatically be trying to recreate the connection as often as possible. When the connection is restored a [steam_server_connected](#steam_server_connected) callback will be posted.

	You usually don't need to check for this yourself. All of the API calls that rely on this will check internally. Forcefully disabling stuff when the player loses access is usually not a very good experience for the player and you could be preventing them from accessing APIs that do not need a live connection to Steam.

	!!! returns "Returns: bool"
		Returns true if the Steam client currently has a live connection to the Steam servers; otherwise, false if there is no active connection due to either a networking issue on the local machine or the Steam server is down / busy.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#BLoggedOn){ .md-button .md-button--doc_classes target="_blank" }

### requestEncryptedAppTicket

!!! function "requestEncryptedAppTicket( ```string``` secret )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | secret | string | The data which will be encrypted into the ticket. |

	Requests an application ticket encrypted with the secret "encrypted app ticket key".

	The encryption key can be obtained from the [Encrypted App Ticket Key](https://partner.steamgames.com/apps/sdkauth/){ target="_blank" } page on the App Admin for your app.

	There can only be one [encrypted_app_ticket_response](#encrypted_app_ticket_response) pending, and this call is subject to a 60 second rate limit.

	After receiving the response you should call [getEncryptedAppTicket](#getencryptedappticket) to get the ticket data, and then you need to send it to a secure server to be decrypted with the [SteamEncryptedAppTicket](https://partner.steamgames.com/doc/api/SteamEncryptedAppTicket){ target="_blank" } functions.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[encrypted_app_ticket_response](#encrypted_app_ticket_response) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#RequestEncryptedAppTicket){ .md-button .md-button--doc_classes target="_blank" }

### requestStoreAuthURL

!!! function "requestStoreAuthURL( ```string``` redirect_url )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | redirect_url | string | The URL the store check-out page redirects to. |

	Requests a URL which authenticates an in-game browser for store check-out, and then redirects to the specified URL.

	As long as the in-game browser accepts and handles session cookies, Steam microtransaction checkout pages will automatically recognize the user instead of presenting a login page.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[store_auth_url_response](#store_auth_url_response) call result

	!!! info "Notes"
		The URL has a very short lifetime to prevent history-snooping attacks, so you should only call this API when you are about to launch the browser, or else immediately navigate to the result URL using a hidden browser window.

		The resulting authorization cookie has an expiration time of one day, so it would be a good idea to request and visit a new auth URL every 12 hours.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#RequestStoreAuthURL){ .md-button .md-button--doc_classes target="_blank" }

### setDurationControlOnlineState

!!! function "setDurationControlOnlineState( ```DurationControlOnlineState``` new_state )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | new_state | [DurationControlOnlineState enum](main.md#durationcontrolonlinestate) | The new state for the duration controls. |

    Allows the game to specify the offline / online gameplay state for Steam China duration control.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#BSetDurationControlOnlineState){ .md-button .md-button--doc_classes target="_blank" }

### startVoiceRecording

!!! function "startVoiceRecording( )"
	Starts voice recording.

	Once started, use [getAvailableVoice](#getavailablevoice) and [getVoice](#getvoice) to get the data, and then call [stopVoiceRecording](#stopvoicerecording) when the user has released their push-to-talk hotkey or the game session has completed.

	See [Steam Voice](https://partner.steamgames.com/doc/features/voice){ target="_blank" } for more information.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#StartVoiceRecording){ .md-button .md-button--doc_classes target="_blank" }

### stopVoiceRecording

!!! function "stopVoiceRecording( )"
	Stops voice recording.

	Because people often release push-to-talk keys early, the system will keep recording for a little bit after this function is called. As such, [getVoice](#getvoice) should continue to be called until it returns 2, only then will voice recording be stopped.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#StopVoiceRecording){ .md-button .md-button--doc_classes target="_blank" }

### terminateGameConnection

!!! function "terminateGameConnection( ```string``` server_ip, ```uint16``` server_port )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | server_ip | string | The IP address of the server to disconnect from.
    | server_port | uint16 | The port of the server to disconnect from.

    Notify the game server that we are disconnecting.

	This needs to occur when the game client leaves the specified game server, needs to match with the [initiateGameConnection](#initiategameconnection) call.

	!!! returns "Returns: void"

	!!! info "Notes"
		This is part of the old user authentication API and should not be mixed with the new API. Please migrate to [beginAuthSession](#beginauthsession) and related functions.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#TerminateGameConnection){ .md-button .md-button--doc_classes target="_blank" }

### userHasLicenseForApp

!!! function "userHasLicenseForApp( ```uint64_t``` steam_id, ```uint32_t``` app_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the user that sent the auth ticket. |
    | app_id | uint32_t | The DLC App ID to check if the user owns it. |

    Checks if the user owns a specific piece of Downloadable Content (DLC).

	This can only be called after sending the users auth ticket to [beginAuthSession](#beginauthsession).

	!!! returns "Returns: int / [UserHasLicenseForAppResult enum](main.md#userhaslicenseforappresult)"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#UserHasLicenseForApp){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### client_game_server_deny

!!! function "client_game_server_deny"
	Sent by the Steam server to the client telling it to disconnect from the specified game server, which it may be in the process of or already connected to. The game client should immediately disconnect upon receiving this message. This can usually occur if the user doesn't have rights to play on the game server.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| app_id | uint32_t | The app ID this call is for.
		| server_ip | string | The IP of the game server that is telling us to disconnect, in host order.
		| server_port | uint16 | The port of the game server that is telling us to disconnect, in host order.
		| secure | uint16 | Is the game server VAC secure (true) or not (false)?
		| reason | uint32_t | No notes about what this means.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#ClientGameServerDeny_t){ .md-button .md-button--doc_classes target="_blank" }

### duration_control

!!! function "duration_control"
	Sent for games with enabled anti indulgence / duration control, for enabled users. Lets the game know whether persistent rewards or XP should be granted at normal rate, half rate, or zero rate.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | int / [Result enum](main.md#result) | The result of the call; always [RESULT_OK](main.md#result) for asynchronous timer-based notifications.
		| duration | dictionary | Details about the duration control.

		The **duration** dictionary contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| app_id | uint32_t | The app ID generating the playtime.
		| applicable | bool | Is duration control applicable to user and game combination?
		| seconds_last_5hrs | int32 | Playtime in trailing 5 hour window plus current session, in seconds.
		| progress | int | Description of whether the game should exit.
		| notification | int | Notification to show, if any; always [DURATION_CONTROL_NOTIFICATION_NONE](main.md#durationcontrol) for API calls.
		| notification_verbal | string | The human-readable string version of **notification**.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#DurationControl_t){ .md-button .md-button--doc_classes target="_blank" }

### encrypted_app_ticket_response

!!! function "encrypted_app_ticket_response"
	Called when an encrypted application ticket has been received.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | int / [Result enum](main.md#result) | Was the call successful?  May be [RESULT_DUPLICATE_REQUEST](main.md#result) if there is already a pending call or [RESULT_LIMIT_EXCEEDED](main.md#result) as this call is subject to a 60-second rate limit.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#EncryptedAppTicketResponse_t){ .md-button .md-button--doc_classes target="_blank" }

### game_web_callback

!!! function "game_web_callback"
	Sent to your game in response to a ```steam://gamewebcallback/``` command from a user clicking a link in the Steam overlay browser. You can use this to add support for external site signups where you want to pop back into the browser after some web page signup sequence, and optionally get back some detail about that.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | url | string | The complete url that the user clicked on.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GameWebCallback_t){ .md-button .md-button--doc_classes target="_blank" }

### get_auth_session_ticket_response

!!! function "get_auth_session_ticket_response"
	Result when creating an auth session ticket with [getAuthSessionTicket](#getauthsessionticket).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| auth_ticket | uint32_t | The handle to the ticket that was created.
		| result | int / [Result enum](main.md#result) | The result of the operation.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetAuthSessionTicketResponse_t){ .md-button .md-button--doc_classes target="_blank" }

### get_ticket_for_web_api

!!! function "get_ticket_for_web_api"
	Result when creating a Web API ticket with [getAuthTicketForWebApi](#getauthticketforwebapi).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| auth_ticket | uint32_t | The handle to the ticket that was created.
		| result | int / [Result enum](main.md#result) | The result of the operation.
		| ticket_size | int | The length of the ticket that was created.
		| ticket_buffer | uint8 | The ticket that was created.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#GetTicketForWebApiResponse_t){ .md-button .md-button--doc_classes target="_blank" }	

### ipc_failure

!!! function "ipc_failure"
	Called when the callback system for this client is in an error state (and has flushed pending callbacks). When getting this message the client should disconnect from Steam, reset any stored Steam state and reconnect. This usually occurs in the rare event the Steam client has some kind of fatal error.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | type | uint8 | Will either be [FAILURE_FLUSHED_CALLBACK_QUEUE](#failuretype) or [FAILURE_PIPE_FAIL](#failuretype).

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#IPCFailure_t){ .md-button .md-button--doc_classes target="_blank" }

### licenses_updated

!!! function "licenses_updated"
	Called whenever the users licenses (owned packages) changes.

	!!! returns "Returns"
		 Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#LicensesUpdated_t){ .md-button .md-button--doc_classes target="_blank" }

### market_eligibility_response

!!! function "market_eligibility_response"
	Called in response to the [getMarketEligibility](#getmarketeligibility) function.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | is_allowed | bool | Whther or not the user is allowed to use Market features.
		| disallow_reason | int / [MarketNotAllowedReasonFlags enum](main.md#marketnotallowedreasonflags) | The reason why the user is disallowed, if disallowed.
		| allowed_at_time | int | The Unix time for when the user is allowed to use Market features.
		| steam_guard_required_days | int | The number of days any user is required to have had Steam Guard before they can use the market.
		| new_device_cooldown | int | The number of days after initial device authorization a user must wait before using the market on that device.

	---
	[ :material-tag-plus: Added GodotSteam 4.16](../changelog/godot4.md/#version-416){ .md-button .md-button--changes target="_blank" }

### microtransaction_auth_response

!!! function "microtransaction_auth_response"
	Called when a user has responded to a microtransaction authorization request.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | app_id | uint32_t | The app ID for this microtransaction.
		| order_id | uint64_t | The order ID provided for the microtransaction.
		| authorized | bool | Did the user authorize the transaction (1) or not (0)?

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#MicroTxnAuthorizationResponse_t){ .md-button .md-button--doc_classes target="_blank" }

### steam_server_connect_failed

!!! function "steam_server_connect_failed"
	Called when a connection attempt has failed. This will occur periodically if the Steam client is not connected, and has failed when retrying to establish a connection.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | int /  [Result enum](main.md#result) | The reason why the connection failed.
		| retrying | bool | Is the Steam client still trying to connect to the server?

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#SteamServerConnectFailure_t){ .md-button .md-button--doc_classes target="_blank" }

### steam_server_connected

!!! function "steam_server_connected"
	Called when a connections to the Steam back-end has been established. This means the Steam client now has a working connection to the Steam servers. Usually this will have occurred before the game has launched, and should only be seen if the user has dropped connection due to a networking issue or a Steam server update.

	!!! returns "Returns"
		 Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#SteamServersConnected_t){ .md-button .md-button--doc_classes target="_blank" }

### steam_server_disconnected

!!! function "steam_server_disconnected"
	Called if the client has lost connection to the Steam servers. Real-time services will be disabled until a matching [steam_server_connected](#steam_server_connected) has been posted.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | int / [Result enum](main.md#result) | The reason we were disconnected from Steam.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#SteamServersDisconnected_t){ .md-button .md-button--doc_classes target="_blank" }

### store_auth_url_response

!!! function "store_auth_url_response"
	Response when we have recieved the authentication URL after a call to [requestStoreAuthURL](#requeststoreauthurl).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | url | string | The authentication URL.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#StoreAuthURLResponse_t){ .md-button .md-button--doc_classes target="_blank" }

### validate_auth_ticket_response

!!! function "validate_auth_ticket_response"
	Called when an auth ticket has been validated.

	Emits signal in response to function [beginAuthSession](#beginauthsession). Also received when a user you have an active auth session with cancels their auth ticket so that you can respond appropriately by calling [endAuthSession](#endauthsession), etc.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | auth_id | uint64_t | The Steam ID of the entity that provided the auth ticket.
		| response | uint32_t / [AuthSessionResponse enum](main.md#authsessionresponse) | The result of the validation.
		| owner_id | uint64_t | The Steam ID that owns the game, this will be different from **auth_id** if the game is being accessed via Steam Family Sharing. 

		See [EAuthSessionResponse](https://partner.steamgames.com/doc/api/steam_api#EAuthSessionResponse){ target="_blank" } for more details about possible responses.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUser#ValidateAuthTicketResponse_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-numeric: Enums
==}

### FailureType

The failure reason for the client's callback system.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
FAILURE_FLUSHED_CALLBACK_QUEUE | k_EFailureFlushedCallbackQueue | 0 | -
FAILURE_PIPE_FAIL | k_EFailurePipeFail | 1 | -