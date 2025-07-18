---
title: Networking Utils
description: A class reference for Networking Utilities.
icon: material/plus-network
---

# Networking Utils

Miscellaneous networking utilities for checking the local networking environment and estimating pings. These are part of the newer networking classes; not to be confused with the [older, now-deprecated Networking class](networking.md).

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } and [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### checkPingDataUpToDate

!!! function "checkPingDataUpToDate( ```float``` max_age_in_seconds )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | max_age_in_seconds | float | The maximum age the ping data can be.

	Check if the ping data of sufficient recency is available, and if it's too old, start refreshing it.

	Please only call this function when you really do need to force an immediate refresh of the data. (For example, in response to a specific user input to refresh this information.) Don't call it "just in case", before every connection, etc. That will cause extra traffic to be sent for no benefit. The library will automatically refresh the information as needed.

	**Returns:** bool

	True if sufficiently recent data is already available. False if sufficiently recent data is not available. In this case, ping measurement is initiated, if it is not already active. (You cannot restart a measurement already in progress.)
 
    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#CheckPingDataUpToDate){ .md-button .md-button--doc_classes target="_blank" }

### convertPingLocationToString

!!! function "convertPingLocationToString( ```PoolByteArray``` location )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | location | PoolByteArray | The ping location data to convert. |

    Convert a ping location into a text format suitable for sending over the wire. The format is a compact and human readable. However, it is subject to change so please do not parse it yourself.

	**Returns:** string

	   ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#ConvertPingLocationToString){ .md-button .md-button--doc_classes target="_blank" }

### estimatePingTimeBetweenTwoLocations

!!! function "estimatePingTimeBetweenTwoLocations( ```PoolByteArray``` location1, ```PoolByteArray``` location2 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | location1 | PoolByteArray | The ping location data to check against.
    | location2 | PoolByteArray | The ping location data to check against.

	Estimate the round-trip latency between two arbitrary locations, in milliseconds. This is a conservative estimate, based on routing through the relay network. For most basic relayed connections, this ping time will be pretty accurate, since it will be based on the route likely to be actually used.

	If a direct IP route is used (perhaps via NAT traversal), then the route will be different, and the ping time might be better. Or it might actually be a bit worse! Standard IP routing is frequently suboptimal! But even in this case, the estimate obtained using this method is a reasonable upper bound on the ping time. (Also it has the advantage of returning immediately and not sending any packets.)

	In a few cases we might not able to estimate the route. In this case a negative value is returned. k_nSteamNetworkingPing_Failed means the reason was because of some networking difficulty. (Failure to ping, etc) k_nSteamNetworkingPing_Unknown is returned if we cannot currently answer the question for some other reason.

	**Returns:** int

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#EstimatePingTimeBetweenTwoLocations){ .md-button .md-button--doc_classes target="_blank" }

### estimatePingTimeFromLocalHost

!!! function "estimatePingTimeFromLocalHost( ```PoolByteArray``` location )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | location | PoolByteArray | The ping location data to check against. |

    Same as [estimatePingTimeBetweenTwoLocations](#estimatepingtimebetweentwolocations), but assumes that one location is the local host. This is a bit faster, especially if you need to calculate a bunch of these in a loop to find the fastest one.
	In rare cases this might return a slightly different estimate than combining [getLocalPingLocation](#getlocalpinglocation) with [estimatePingTimeBetweenTwoLocations](#estimatepingtimebetweentwolocations). That's because this function uses a slightly more complete set of information about what route would be taken.

	**Returns:** int

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#EstimatePingTimeFromLocalHost){ .md-button .md-button--doc_classes target="_blank" }

### getConfigValue

!!! function "getConfigValue( ```NetworkingConfigValue``` config_value, ```NetworkingConfigScope``` scope_type, ```uint32_t``` connection_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | config_value | [NetworkingConfigValue enum](#networkingconfigvalue) | Which value to fetch. |
    | scope_type | [NetworkingConfigScope enum](#networkingconfigscope) | Query setting on what type of object. |
    | connection_handle | uint32_t | The connection to query configuration values from. |

    Get a configuration value.

	For values to pass to config_value, [check the SDK's listing.](https://partner.steamgames.com/doc/api/steamnetworkingtypes#ESteamNetworkingConfigValue){ target="_blank" } or [NetworkingConfigValue enum](#networkingconfigvalue).

	For values to pass to scope_type, [check the SDK's listing.](https://partner.steamgames.com/doc/api/steamnetworkingtypes#ESteamNetworkingConfigScope){ target="_blank" } or [NetworkingConfigScope enum](#networkingconfigscope).

	**Returns:** dictionary

	Contains the following keys: 

	* result (int), which may be:
		* -3: buffer too small
		* -2: bad connection handle
		* -1: bad value
		* 1: OK
		* 2: OK, inherited
	* type (int)
	* value (PoolByteArray)
	* buffer (int)

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#GetConfigValue){ .md-button .md-button--doc_classes target="_blank" }

### getConfigValueInfo

!!! function "getConfigValueInfo( ```NetworkingConfigValue``` config_value )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | config_value | [NetworkingConfigValue enum](#networkingconfigvalue) | Which value to fetch. |

    Returns info about a configuration value.

    For values to pass to config_value, [check the SDK's listing.](https://partner.steamgames.com/doc/api/steamnetworkingtypes#ESteamNetworkingConfigValue){ target="\_blank" } or [NetworkingConfigValue enum](#networkingconfigvalue).

	**Returns:** dictionary

	Contains the following keys:

	* type (int)
	* scope (int)
	* next_value (int)

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#GetConfigValueInfo){ .md-button .md-button--doc_classes target="_blank" }

### getDirectPingToPOP

!!! function "getDirectPingToPOP( ```uint32_t``` pop_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | pop_id | uint32_t | Identifier used for a network location point of presence. |

    Get *direct* ping time to the relays at the point of presence.

	**Returns:** int

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#GetDirectPingToPOP){ .md-button .md-button--doc_classes target="_blank" }

### getLocalPingLocation

!!! function "getLocalPingLocation( )"
	Return location info for the current host. Returns the approximate age of the data, in seconds, or -1 if no data is available. You can use this value in [checkPingDataUpToDate](#checkpingdatauptodate).

	It takes a few seconds to initialize access to the relay network. If you call this very soon after calling [initRelayNetworkAccess](#initrelaynetworkaccess), the data may not be available yet.

	This always return the most up-to-date information we have available right now, even if we are in the middle of re-calculating ping times.

	**Returns:** dictionary

	Contains the following keys:

	* location (PoolByteArray)
	* age (float)

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#GetLocalPingLocation){ .md-button .md-button--doc_classes target="_blank" }

### getLocalTimestamp

!!! function "getLocalTimestamp( )"
	A general purpose high resolution local timer with the following properties:

	* Monotonicity is guaranteed.
	* The initial value will be at least 24*3600*30*1e6, i.e. about 30 days worth of microseconds. In this way, the timestamp value of 0 will always be at least "30 days ago". Also, negative numbers will never be returned.
	* Wraparound / overflow is not a practical concern.

	If you are running under the debugger and stop the process, the clock might not advance the full wall clock time that has elapsed between calls. If the process is not blocked from normal operation, the timestamp values will track wall clock time, even if you don't call the function frequently.

	The value is only meaningful for this run of the process. Don't compare it to values obtained on another computer, or other runs of the same process.

	**Returns:** uint64_t

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#GetLocalTimestamp){ .md-button .md-button--doc_classes target="_blank" }

### getPingToDataCenter

!!! function "	getPingToDataCenter( ```uint32``` pop_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | pop_id | uint32_t | Identifier used for a network location point of presence. |

    Fetch ping time of best available relayed route from this host to the specified data center.

	**Returns:** dictionary

	Contains the following keys:

	* pop_relay (uint32)
	* ping (int)

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#GetPingToDataCenter){ .md-button .md-button--doc_classes target="_blank" }

### getPOPCount

!!! function "getPOPCount( )"
	Get number of network points of presence in the config.

	**Returns:** int

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#GetPOPCount){ .md-button .md-button--doc_classes target="_blank" }

### getPOPList

!!! function "getPOPList( )"
	Get list of all POP IDs. Returns the number of entries that were filled into your list.

	**Returns:** array of POP IDs.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#GetPOPList){ .md-button .md-button--doc_classes target="_blank" }

### getRelayNetworkStatus

!!! function "getRelayNetworkStatus( )"
	Fetch current status of the relay network.

	[relay_network_status](#relay_network_status) is also a callback. It will be triggered on both the user and gameserver interfaces any time the status changes, or ping measurement starts or stops.

	**Returns:** int which can be:

	* -102 - cannot try
	* -101 - failed
 	* -100 - previously worked, now there is a problem
	* -10 - retrying
	* 0 - unknown
	* 1 - never tried
	* 2 - waiting
	* 3 - attempting
	* 100 - current

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#GetRelayNetworkStatus){ .md-button .md-button--doc_classes target="_blank" }

### initRelayNetworkAccess

!!! function "initRelayNetworkAccess( )"
	If you know that you are going to be using the relay network (for example, because you anticipate making P2P connections), call this to initialize the relay network. If you do not call this, the initialization will be delayed until the first time you use a feature that requires access to the relay network, which will delay that first access.

	You can also call this to force a retry if the previous attempt has failed. Performing any action that requires access to the relay network will also trigger a retry, and so calling this function is never strictly necessary, but it can be useful to call it a program launch time, if access to the relay network is anticipated. Use [getRelayNetworkStatus](#getrelaynetworkstatus) or listen for [relay_network_status](#relay_network_status) callbacks to know when initialization has completed. Typically initialization completes in a few seconds.

	**Returns:** void

	**Note:** dedicated servers hosted in known data centers do <strong>not</strong> need to call this, since they do not make routing decisions. However, if the dedicated server will be using P2P functionality, it will act as a "client" and this should be called.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#InitRelayNetworkAccess){ .md-button .md-button--doc_classes target="_blank" }

### parsePingLocationString

!!! function "parsePingLocationString( ```string``` location_string )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | location_string | string | The location string to be parsed back into a location object. |

    Parse back location_string string. Returns false if we couldn't understand the string.

	**Returns:** dictionary

	Contains the following keys:

	* success (bool)
	* ping_location (PoolByeArray)

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#ParsePingLocationString){ .md-button .md-button--doc_classes target="_blank" }

### setConnectionConfigValueFloat

!!! function "	setConnectionConfigValueFloat( ```uint32_t``` connection_handle, ```NetworkingConfigValue``` config, ```float``` value )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The connection to set configuration values for. |
    | config | [NetworkingConfigValue enum](#networkingconfigvalue) | Which value to set. |
    | value | float | The actual value to set. |

    Set a configuration value.

	**Returns:** bool

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#SetConfigValue){ .md-button .md-button--doc_classes target="_blank" }

### setConnectionConfigValueInt32

!!! function "setConnectionConfigValueInt32( ```uint32_t``` connection_handle, ```NetworkingConfigValue``` config, ```int32``` value )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The connection to set configuration values for. |
    | config | [NetworkingConfigValue enum](#networkingconfigvalue) | Which value to set. |
    | value | int32 | The actual value to set. |

    Set a configuration value.

	**Returns:** bool

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#SetConfigValue){ .md-button .md-button--doc_classes target="_blank" }

### setConnectionConfigValueString

!!! function "setConnectionConfigValueString( ```uint32_t``` connection_handle, ```NetworkingConfigValue``` config, ```string``` value )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The connection to set configuration values for. |
    | config | [NetworkingConfigValue enum](#networkingconfigvalue) | Which value to set. |
    | value | string | The actual value to set. |

    Set a configuration value.

	**Returns:** bool

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#SetConfigValue){ .md-button .md-button--doc_classes target="_blank" }

### setGlobalConfigValueFloat

!!! function "setGlobalConfigValueFloat( ```NetworkingConfigValue``` config, ```float``` value )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | config | [NetworkingConfigValue enum](#networkingconfigvalue) | Which value to set. |
    | value | float | The actual value to set. |

    Set a configuration value.

	**Returns:** bool

   ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#shortcuts){ .md-button .md-button--doc_classes target="_blank" }

### setGlobalConfigValueInt32

!!! function "setGlobalConfigValueInt32( ```NetworkingConfigValue``` config, ```int32``` value )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | config | [NetworkingConfigValue enum](#networkingconfigvalue) | Which value to set. |
    | value | int32 | The actual value to set. |

    Set a configuration value.

	**Returns:** bool

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#shortcuts){ .md-button .md-button--doc_classes target="_blank" }

### setGlobalConfigValueString

!!! function "setGlobalConfigValueString( ```NetworkingConfigValue``` config, ```string``` value )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | config | [NetworkingConfigValue enum](#networkingconfigvalue) | Which value to set. |
    | value | string | The actual value to set. |

    Set a configuration value.

	**Returns:** bool

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingUtils#shortcuts){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-infinity: Constants
==}

Found in steamnetworkingtypes.h.

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
LISTEN_SOCKET_INVALID | k_HSteamListenSocket_Invalid | 0 | None.
MAX_NETWORKING_ERROR_MESSAGE | k_cchMaxSteamNetworkingErrMsg | 1024 | Max length of diagnostic error message.
MAX_NETWORKING_PING_LOCATION_STRING | k_cchMaxSteamNetworkingPingLocationString | 1024 | Max possible length of a ping location, in string format.  This is an extremely conservative worst case value which leaves room for future syntax enhancements.  Most strings in practice are a lot shorter. If you are storing many of these, you will very likely benefit from using dynamic memory.
NETWORKING_CONFIG_P2P_TRANSPORT_ICE_DEFAULT | k_nSteamNetworkingConfig_P2P_Transport_ICE_Enable_Default | -1 | Special value - use user defaults.
NETWORKING_CONFIG_P2P_TRANSPORT_ICE_DISABLE | k_nSteamNetworkingConfig_P2P_Transport_ICE_Enable_Disable | 0 | Do not do any ICE work at all or share any IP addresses with peer.
NETWORKING_CONFIG_P2P_TRANSPORT_ICE_RELAY | k_nSteamNetworkingConfig_P2P_Transport_ICE_Enable_Relay | 1 | Relayed connection via TURN server.
NETWORKING_CONFIG_P2P_TRANSPORT_ICE_PRIVATE | k_nSteamNetworkingConfig_P2P_Transport_ICE_Enable_Private | 2 | host addresses that appear to be link-local or RFC1918 addresses.
NETWORKING_CONFIG_P2P_TRANSPORT_ICE_PUBLIC | k_nSteamNetworkingConfig_P2P_Transport_ICE_Enable_Public | 4 | STUN reflexive addresses, or host address that isn't a "private" address. 
NETWORKING_CONFIG_P2P_TRANSPORT_ICE_ALL | k_nSteamNetworkingConfig_P2P_Transport_ICE_Enable_All | 0x7fffffff | None.
NETWORKING_CONNECTION_INFO_FLAG_UNAUTHENTICATED | k_nSteamNetworkConnectionInfoFlags_Unauthenticated | 1 | We don't have a certificate for the remote host.
NETWORKING_CONNECTION_INFO_FLAG_UNENCRYPTED | k_nSteamNetworkConnectionInfoFlags_Unencrypted | 2 | Information is being sent out over a wire unencrypted (by this library).
NETWORKING_CONNECTION_INFO_FLAG_LOOPBACK_BUFFERS | k_nSteamNetworkConnectionInfoFlags_LoopbackBuffers | 4 | Internal loopback buffers.  Won't be true for localhost.  (You can check the address to determine that.)  This implies NETWORKING_CONNECTION_INFO_FLAG_FAST.
NETWORKING_CONNECTION_INFO_FLAG_FAST | k_nSteamNetworkConnectionInfoFlags_Fast | 8 | The connection is "fast" and "reliable".  Either internal/localhost (check the address to find out), or the peer is on the same LAN.  (Probably.  It's based on the address and the ping time, this is actually hard to determine unambiguously).
NETWORKING_CONNECTION_INFO_FLAG_RELAYED | k_nSteamNetworkConnectionInfoFlags_Relayed | 16 | The connection is relayed somehow (SDR or TURN).
NETWORKING_CONNECTION_INFO_FLAG_DUALWIFI | k_nSteamNetworkConnectionInfoFlags_DualWifi | 32 | We're taking advantage of dual-wifi multi-path.
NETWORKING_CONNECTION_INVALID | k_HSteamNetConnection_Invalid | 0 | None.
NETWORKING_MAX_CONNECTION_APP_NAME | k_cchSteamNetworkingMaxConnectionAppName | 32 | Max length of the app's part of the description.
NETWORKING_MAX_CONNECTION_CLOSE_REASON | k_cchSteamNetworkingMaxConnectionCloseReason | 128 | Max length, in bytes (including null terminator) of the reason string when a connection is closed.
NETWORKING_MAX_CONNECTION_DESCRIPTION | k_cchSteamNetworkingMaxConnectionDescription | 128 | Max length, in bytes (include null terminator) of debug description of a connection.
NETWORKING_PING_FAILED | k_nSteamNetworkingPing_Failed | -1 | Special values that are returned by some functions that return a ping.
NETWORKING_PING_UNKNOWN | k_nSteamNetworkingPing_Unknown | -2 | Special values that are returned by some functions that return a ping.
NETWORKING_SEND_UNRELIABLE | k_nSteamNetworkingSend_Unreliable | 0 | Send the message unreliably. Can be lost.  Messages *can* be larger than a single MTU (UDP packet), but there is no retransmission, so if any piece of the message is lost, the entire message will be dropped. The sending API does have some knowledge of the underlying connection, so if there is no NAT-traversal accomplished or there is a recognized adjustment happening on the connection, the packet will be batched until the connection is open again. Migration note: This is not exactly the same as [P2P_SEND_UNRELIABLE](networking.md#p2psend)! You probably want NETWORKING_SEND_URELIABLE_NO_NAGLE.
NETWORKING_SEND_NO_NAGLE | k_nSteamNetworkingSend_NoNagle | 1 | Disable Nagle's algorithm. By default, Nagle's algorithm is applied to all outbound messages.  This means that the message will NOT be sent immediately, in case further messages are sent soon after you send this, which can be grouped together.  Any time there is enough buffered data to fill a packet, the packets will be pushed out immediately, but partially-full packets not be sent until the Nagle timer expires.  See [flushMessagesOnConnection](networking_sockets.md#flushmessagesonconnection). Note: Don't just send every message without Nagle because you want packets to get there quicker.  Make sure you understand the problem that Nagle is solving before disabling it. If you are sending small messages, often many at the same time, then it is very likely that it will be more efficient to leave Nagle enabled.  A typical proper use of this flag is when you are sending what you know will be the last message sent for a while (e.g. the last in the server simulation tick to a particular client), and you use this flag to flush all messages.
NETWORKING_SEND_URELIABLE_NO_NAGLE | k_nSteamNetworkingSend_UnreliableNoNagle | 0/1 | Send a message unreliably, bypassing Nagle's algorithm for this message and any messages currently pending on the Nagle timer.  This is equivalent to using NETWORKING_SEND_UNRELIABLE and then immediately flushing the messages using [flushMessagesOnConnection](networking_sockets.md#flushmessagesonconnection).  (But using this flag is more efficient since you only make one API call.
NETWORKING_SEND_NO_DELAY | k_nSteamNetworkingSend_NoDelay | 4 | If the message cannot be sent very soon (because the connection is still doing some initial handshaking, route negotiations, etc), then just drop it.  This is only applicable for unreliable messages.  Using this flag on reliable messages is invalid.
NETWORKING_SEND_UNRELIABLE_NO_DELAY | k_nSteamNetworkingSend_UnreliableNoDelay | 0/4/1 | Send an unreliable message, but if it cannot be sent relatively quickly, just drop it instead of queuing it. This is useful for messages that are not useful if they are excessively delayed, such as voice data. Note: The Nagle algorithm is not used, and if the message is not dropped, any messages waiting on the  Nagle timer are immediately flushed. A message will be dropped under the following circumstances: the connection is not fully connected.  (E.g. the "Connecting" or "FindingRoute" states) there is a sufficiently large number of messages queued up already such that the current message will not be placed on the wire in the next ~200ms or so. If a message is dropped for these reasons, [RESULT_IGNORED](main.md#result) will be returned.
NETWORKING_SEND_RELIABLE | k_nSteamNetworkingSend_Reliable | 8 | Reliable message send. Can send up to [MAX_STEAM_PACKET_SIZE](networking_sockets.md#constants) bytes in a single message. Does fragmentation/re-assembly of messages under the hood, as well as a sliding window for efficient sends of large chunks of data. The Nagle algorithm is used.  See notes on NETWORKING_SEND_UNRELIABLE for more details. See NETWORKING_SEND_RELIABLE_NO_NAGLE, [flushMessagesOnConnection](networking_sockets.md#flushmessagesonconnection). Migration note: This is NOT the same as [P2P_SEND_RELIABLE](networking.md#p2psend), it's more like [P2P_SEND_RELIABLE_WITH_BUFFERING](networking.md#p2psend).
NETWORKING_SEND_RELIABLE_NO_NAGLE | k_nSteamNetworkingSend_ReliableNoNagle | 8/1 | Send a message reliably, but bypass Nagle's algorithm. Migration note: This is equivalent to [P2P_SEND_RELIABLE](networking.md#p2psend)
NETWORKING_SEND_USE_CURRENT_THREAD | k_nSteamNetworkingSend_UseCurrentThread | 16 | By default, message sending is queued, and the work of encryption and talking to the operating system sockets, etc is done on a service thread.  This is usually a a performance win when messages are sent from the "main thread".  However, if this flag is set, and data is ready to be sent immediately (either from this message or earlier queued data), then that work will be done in the current thread, before the current call returns.  If data is not ready to be sent (due to rate limiting or Nagle), then this flag has no effect. This is an advanced flag used to control performance at a very low level.  For most applications running on modern hardware with more than one CPU core, doing the work of sending on a service thread will yield the best performance.  Only use this flag if you have a really good reason and understand what you are doing. Otherwise you will probably just make performance worse.
NETWORKING_SEND_AUTORESTART_BROKEN_SESSION | k_nSteamNetworkingSend_AutoRestartBrokenSession | 32 | When sending a message using ISteamNetworkingMessages, automatically re-establish a broken session, without returning k_EResultNoConnection.  Without this flag, if you attempt to send a message, and the session was proactively closed by the peer, or an error occurred that disrupted communications, then you must close the session using [closeSessionWithUser](networking_messages.md#closesessionwithuser) before attempting to send another message.  (Or you can simply add this flag and retry.)  In this way, the disruption cannot go unnoticed, and a more clear order of events can be ascertained. This is especially important when reliable messages are used, since if the connection is disrupted, some of those messages will not have been delivered, and it is in general not possible to know which.  Although a [network_messages_session_failed](networking_messages.md#network_messages_session_failed) callback will be posted when an error occurs to notify you that a failure has happened, callbacks are asynchronous, so it is not possible to tell exactly when it happened.  And because the primary purpose of Networking Messages is to be like UDP, there is no notification when a peer closes the session. If you are not using any reliable messages (e.g. you are using Networking Messages exactly as a transport replacement for UDP-style datagrams only), you may not need to know when an underlying connection fails, and so you may not need this notification.

{==
## :material-signal: Signals
==}

These callbacks require you to run ```Steam.run_callbacks()``` in your ```_process()``` function to receive them.

### relay_network_status

!!! function "relay_network_status"
	A struct used to describe our readiness to use the relay network.

	**Returns:**

	* available (int)
	* ping_measurement (int)
	* available_config (int)
	* available_relay (int)
	* debug_message (string)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steamnetworkingtypes#SteamRelayNetworkStatus_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-numeric: Enums
==}

### NetworkingAvailability
Negative values indicate a problem.

In general, we will not automatically retry unless you take some action that depends on of requests this resource, such as querying the status, attempting to initiate a connection, receive a connection, etc.  If you do not take any action at all, we do not automatically retry in the background.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
NETWORKING_AVAILABILITY_CANNOT_TRY | k_ESteamNetworkingAvailability_CannotTry | -102 | A dependent resource is missing, so this service is unavailable.  (E.g. we cannot talk to routers because Internet is down or we don't have the network config.)
NETWORKING_AVAILABILITY_FAILED | k_ESteamNetworkingAvailability_Failed | -101 | We have tried for enough time that we would expect to have been successful by now.  We have never been successful.
NETWORKING_AVAILABILITY_PREVIOUSLY | k_ESteamNetworkingAvailability_Previously | -100 | We tried and were successful at one time, but now it looks like we have a problem.
NETWORKING_AVAILABILITY_RETRYING | k_ESteamNetworkingAvailability_Retrying | -10 | We previously failed and are currently retrying.
NETWORKING_AVAILABILITY_UNKNOWN | k_ESteamNetworkingAvailability_Unknown | 0 | Internal dummy/sentinel, or value is not applicable in this context.
NETWORKING_AVAILABILITY_NEVER_TRIED | k_ESteamNetworkingAvailability_NeverTried | 1 | We don't know because we haven't ever checked/tried.
NETWORKING_AVAILABILITY_WAITING | k_ESteamNetworkingAvailability_Waiting | 2 | We're waiting on a dependent resource to be acquired.  (E.g. we cannot obtain a cert until we are logged into Steam.  We cannot measure latency to relays until we have the network config.)
NETWORKING_AVAILABILITY_ATTEMPTING | k_ESteamNetworkingAvailability_Attempting | 3 | We're actively trying now, but are not yet successful.
NETWORKING_AVAILABILITY_CURRENT | k_ESteamNetworkingAvailability_Current | 100 | Resource is online/available.
NETWORKING_AVAILABILITY_FORCE_32BIT | k_ESteamNetworkingAvailability__Force32bit | 0x7fffffff | -

### NetworkingConfigScope

Configuration values can be applied to different types of objects.

Found in steamnetworkingtypes.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
NETWORKING_CONFIG_SCOPE_GLOBAL | k_ESteamNetworkingConfig_Global | 1 | Get/set global option, or defaults.  Even options that apply to more specific scopes have global scope, and you may be able to just change the global defaults.  If you need different settings per connection (for example), then you will need to set those options at the more specific scope.
NETWORKING_CONFIG_SCOPE_SOCKETS_INTERFACE | k_ESteamNetworkingConfig_SocketsInterface | 2 | Some options are specific to a particular interface.  Note that all connection and listen socket settings can also be set at the interface level, and they will apply to objects created through those interfaces.
NETWORKING_CONFIG_SCOPE_LISTEN_SOCKET | k_ESteamNetworkingConfig_ListenSocket | 3 | Options for a listen socket.  Listen socket options can be set at the interface layer, if  you have multiple listen sockets and they all use the same options. You can also set connection options on a listen socket, and they set the defaults for all connections accepted through this listen socket.  (They will be used if you don't set a connection option.)
NETWORKING_CONFIG_SCOPE_CONNECTION | k_ESteamNetworkingConfig_Connection | 4 | Options for a specific connection.
NETWORKING_CONFIG_SCOPE_FORCE_32BIT | k_ESteamNetworkingConfigScope__Force32Bit | 0x7fffffff | -

### NetworkingConfigDataType

Different configuration values have different data types.

Found in steamnetworkingtypes.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
NETWORKING_CONFIG_TYPE_INT32 | k_ESteamNetworkingConfig_Int32 | 1 | -
NETWORKING_CONFIG_TYPE_INT64 | k_ESteamNetworkingConfig_Int64 | 2 | -
NETWORKING_CONFIG_TYPE_FLOAT | k_ESteamNetworkingConfig_Float | 3 | -
NETWORKING_CONFIG_TYPE_STRING | k_ESteamNetworkingConfig_String | 4 | -
NETWORKING_CONFIG_TYPE_FUNCTION_PTR | k_ESteamNetworkingConfig_Ptr | 5 | -
NETWORKING_CONFIG_TYPE_FORCE_32BIT | k_ESteamNetworkingConfigDataType__Force32Bit | 0x7fffffff | -

### NetworkingConfigValue

Found in steamnetworkingtypes.h

Enumerator | Steam Name | Value | Notes
---------- | ---------- | ----- | -----
NETWORKING_CONFIG_INVALID | k_ESteamNetworkingConfig_Invalid | 0 | -

NETWORKING_CONFIG_SEND_BUFFER_SIZE | k_ESteamNetworkingConfig_SendBufferSize | 9 | [connection int32] Upper limit of buffered pending bytes to be sent, if this is reached SendMessage will return RESULT_LIMIT_EXCEEDED. Default is 512k (524288 bytes).
NETWORKING_CONFIG_SEND_RATE_MIN | k_ESteamNetworkingConfig_SendRateMin | 10 | [connection int32] Minimum/maximum send rate clamp, in bytes/sec. At the time of this writing these two options should always be set to the same value, to manually configure a specific send rate.  The default value is 256K.  Eventually we hope to have the library estimate the bandwidth of the channel and set the send rate to that estimated bandwidth, and these values will only set limits on that send rate.
NETWORKING_CONFIG_SEND_RATE_MAX | k_ESteamNetworkingConfig_SendRateMax | 11 | -
NETWORKING_CONFIG_NAGLE_TIME | k_ESteamNetworkingConfig_NagleTime | 12 | [connection int32] Nagle time, in microseconds. When SendMessage is called, if the outgoing message is less than the size of the MTU, it will be queued for a delay equal to the Nagle timer value.  This is to ensure that if the application sends several small messages rapidly, they are coalesced into a single packet. See historical RFC 896. Value is in microseconds. Default is 5000us (5ms).

NETWORKING_CONFIG_FAKE_PACKET_LOSS_SEND | k_ESteamNetworkingConfig_FakePacketLoss_Send | 
NETWORKING_CONFIG_FAKE_PACKET_LOSS_RECV | k_ESteamNetworkingConfig_FakePacketLoss_Recv | 
NETWORKING_CONFIG_FAKE_PACKET_LAG_SEND | k_ESteamNetworkingConfig_FakePacketLag_Send | 
NETWORKING_CONFIG_FAKE_PACKET_LAG_RECV | k_ESteamNetworkingConfig_FakePacketLag_Recv | 
NETWORKING_CONFIG_FAKE_PACKET_JITTER_SEND_AVG | k_ESteamNetworkingConfig_FakePacketJitter_Send_Avg | 
NETWORKING_CONFIG_FAKE_PACKET_JITTER_SEND_MAX | k_ESteamNetworkingConfig_FakePacketJitter_Send_Max | 
NETWORKING_CONFIG_FAKE_PACKET_JITTER_SEND_PCT | k_ESteamNetworkingConfig_FakePacketJitter_Send_Pct | 
NETWORKING_CONFIG_FAKE_PACKET_JITTER_RECV_AVG | k_ESteamNetworkingConfig_FakePacketJitter_Recv_Avg | 
NETWORKING_CONFIG_FAKE_PACKET_JITTER_RECV_MAX | k_ESteamNetworkingConfig_FakePacketJitter_Recv_Max | 
NETWORKING_CONFIG_FAKE_PACKET_JITTER_RECV_PCT | k_ESteamNetworkingConfig_FakePacketJitter_Recv_Pct | 
NETWORKING_CONFIG_FAKE_PACKET_REORDER_SEND | k_ESteamNetworkingConfig_FakePacketReorder_Send | 
NETWORKING_CONFIG_FAKE_PACKET_REORDER_RECV | k_ESteamNetworkingConfig_FakePacketReorder_Recv | 
NETWORKING_CONFIG_FAKE_PACKET_REORDER_TIME | k_ESteamNetworkingConfig_FakePacketReorder_Time | 
NETWORKING_CONFIG_FAKE_PACKET_DUP_SEND | k_ESteamNetworkingConfig_FakePacketDup_Send | 
NETWORKING_CONFIG_FAKE_PACKET_DUP_REVC | k_ESteamNetworkingConfig_FakePacketDup_Recv | 
NETWORKING_CONFIG_FAKE_PACKET_DUP_TIME_MAX | k_ESteamNetworkingConfig_FakePacketDup_TimeMax | 
NETWORKING_CONFIG_PACKET_TRACE_MAX_BYTES | k_ESteamNetworkingConfig_PacketTraceMaxBytes | 
NETWORKING_CONFIG_FAKE_RATE_LIMIT_SEND_RATE | k_ESteamNetworkingConfig_FakeRateLimit_Send_Rate | 
NETWORKING_CONFIG_FAKE_RATE_LIMIT_SEND_BURST | k_ESteamNetworkingConfig_FakeRateLimit_Send_Burst | 
NETWORKING_CONFIG_FAKE_RATE_LIMIT_RECV_RATE | k_ESteamNetworkingConfig_FakeRateLimit_Recv_Rate | 
NETWORKING_CONFIG_FAKE_RATE_LIMIT_RECV_BURST | k_ESteamNetworkingConfig_FakeRateLimit_Recv_Burst | 
NETWORKING_CONFIG_OUT_OF_ORDER_CORRECTION_WINDOW_MICROSECONDS | k_ESteamNetworkingConfig_OutOfOrderCorrectionWindowMicroseconds | 



NETWORKING_CONFIG_TIMEOUT_INITIAL | k_ESteamNetworkingConfig_TimeoutInitial | 24 | [connection int32] Timeout value (in ms) to use when first connecting.
NETWORKING_CONFIG_TIMEOUT_CONNECTED | k_ESteamNetworkingConfig_TimeoutConnected | 25 | [connection int32] Timeout value (in ms) to use after connection is established.

NETWORKING_CONFIG_CONNECTION_USER_DATA | k_ESteamNetworkingConfig_ConnectionUserData | 40 | [connection int64] Get/set userdata as a configuration option. The default value is -1.   You may want to set the user data as a config value, instead of using [setConnectionUserData](networking_sockets.md#setconnectionuserdata) in two specific instances: You wish to set the userdata atomically when creating an outbound connection, so that the userdata is filled in properly for any callbacks that happen.  However, note that this trick only works for connections initiated locally!  For incoming connections, multiple state transitions may happen and callbacks be queued, before you are able to service the first callback!  Be careful! You can set the default userdata for all newly created connections by setting this value at a higher level (e.g. on the listen socket or at the global level.)  Then this default value will be inherited when the connection is created. This is useful in case -1 is a valid userdata value, and you wish to use something else as the default value so you can tell if it has been set or not. However: once a connection is created, the effective value is then bound to the connection. Unlike other connection options, if you change it again at a higher level, the new value will not be inherited by connections. Using the userdata field in callback structs is not advised because of tricky race conditions. Instead, you might try one of these methods: Use a separate map with the connection_handle as the key. Fetch the userdata from the connection in your callback using [getConnectionUserData](#getconnectionuserdata), to ensure you have the current value.

NETWORKING_CONFIG_RECV_BUFFER_SIZE | k_ESteamNetworkingConfig_RecvBufferSize | 47 | [connection int32] Upper limit on total size (in bytes) of received messages that will be buffered waiting to be processed by the application.  If this limit is exceeded, packets will be dropped.  This is to protect us from a malicious peer flooding us with messages faster than we can process them. This must be bigger than NETWORKING_CONFIG_RECV_MAX_MESSAGE_SIZE.
NETWORKING_CONFIG_RECV_BUFFER_MESSAGES | k_ESteamNetworkingConfig_RecvBufferMessages | 48 | [connection int32] Upper limit on the number of received messages that will that will be buffered waiting to be processed by the application.  If this limit is exceeded, packets will be dropped.  This is to protect us from a malicious peer flooding us with messages faster than we can pull them off the wire.
NETWORKING_CONFIG_RECV_MAX_MESSAGE_SIZE | k_ESteamNetworkingConfig_RecvMaxMessageSize | 49 | [connection int32] Maximum message size that we are willing to receive. If a client attempts to send us a message larger than this, the connection will be immediately closed. Default is 512k (524288 bytes). Note that the peer needs to be able to send a message this big.  (See k_cbMaxSteamNetworkingSocketsMessageSizeSend.)
NETWORKING_CONFIG_RECV_MAX_SEGMENTS_PER_PACKET | k_ESteamNetworkingConfig_RecvMaxSegmentsPerPacket | 50 | [connection int32] Max number of message segments that can be received in a single UDP packet.  While decoding a packet, if the number of segments exceeds this, we will abort further packet processing. The default is effectively unlimited. If you know that you very rarely send small packets, you can protect yourself from malicious senders by lowering this number. In particular, if you are NOT using the reliability layer and are only using Networking Sockets for datagram transport, setting this to a very low number may be beneficial. (We recommend a value of 2.)  Make sure your sender disables Nagle!



NETWORKING_CONFIG_IP_ALLOW_WITHOUT_AUTH | k_ESteamNetworkingConfig_IP_AllowWithoutAuth | 23 | [connection int32] Don't automatically fail IP connections that don't have strong auth. On clients, this means we will attempt the connection even if we don't know our identity or can't get a cert. On the server, it means that we won't automatically reject a connection due to a failure to authenticate. (You can examine the incoming connection and decide whether to accept it.)

- 0: Don't attempt or accept unauthorized connections
- 1: Attempt authorization when connecting, and allow unauthorized peers, but emit warnings
- 2: don't attempt authentication, or complain if peer is unauthenticated

This is a dev configuration value, and you should not let users modify it in production.


NETWORKING_CONFIG_IP_LOCAL_HOST_ALLOW_WITHOUT_AUTH | k_ESteamNetworkingConfig_IPLocalHost_AllowWithoutAuth | 52 | [connection int32] The same as NETWORKING_CONFIG_IP_ALLOW_WITHOUT_AUTH, but will only apply for connections to/from localhost addresses. Whichever value is larger (more permissive) will be used.


NETWORKING_CONFIG_MTU_PACKET_SIZE | k_ESteamNetworkingConfig_MTU_PacketSize | 32 | [connection int32] Do not send UDP packets with a payload of larger than N bytes.  If you set this, NETWORKING_CONFIG_MTU_DATA_SIZE is automatically adjusted
NETWORKING_CONFIG_MTU_DATA_SIZE | k_ESteamNetworkingConfig_MTU_DataSize | 33 | [connection int32] (read only) Maximum message size you can send that will not fragment, based on NETWORKING_CONFIG_MTU_PACKET_SIZE.

NETWORKING_CONFIG_UNENCRYPTED | k_ESteamNetworkingConfig_Unencrypted | 34 | [connection int32] Allow unencrypted (and unauthenticated) communication.

- 0: Not allowed (the default)
- 1: Allowed, but prefer encrypted
- 2: Allowed, and preferred
- 3: Required.  (Fail the connection if the peer requires encryption.)

 This is a dev configuration value, since its purpose is to disable encryption. You should not let users modify it in production.  (But note that it requires the peer to also modify their value in order for encryption to be disabled.)
NETWORKING_CONFIG_SYMMETRIC_CONNECT | k_ESteamNetworkingConfig_SymmetricConnect | 
NETWORKING_CONFIG_LOCAL_VIRTUAL_PORT | k_ESteamNetworkingConfig_LocalVirtualPort | 
NETWORKING_CONFIG_DUAL_WIFI_ENABLE | k_ESteamNetworkingConfig_DualWifi_Enable | 
NETWORKING_CONFIG_ENABLE_DIAGNOSTICS_UI | k_ESteamNetworkingConfig_EnableDiagnosticsUI | 
NETWORKING_CONFIG_SEND_TIME_SINCE_PREVIOUS_PACKET | k_ESteamNetworkingConfig_SendTimeSincePreviousPacket | 
NETWORKING_CONFIG_SDR_CLIENT_CONSEC_PING_TIMEOUT_FAIL_INITIAL | k_ESteamNetworkingConfig_SDRClient_ConsecutitivePingTimeoutsFailInitial | 
NETWORKING_CONFIG_SDR_CLIENT_CONSEC_PING_TIMEOUT_FAIL | k_ESteamNetworkingConfig_SDRClient_ConsecutitivePingTimeoutsFail | 
NETWORKING_CONFIG_SDR_CLIENT_MIN_PINGS_BEFORE_PING_ACCURATE | k_ESteamNetworkingConfig_SDRClient_MinPingsBeforePingAccurate | 
NETWORKING_CONFIG_SDR_CLIENT_SINGLE_SOCKET | k_ESteamNetworkingConfig_SDRClient_SingleSocket | 
NETWORKING_CONFIG_SDR_CLIENT_FORCE_RELAY_CLUSTER | k_ESteamNetworkingConfig_SDRClient_ForceRelayCluster | 
NETWORKING_CONFIG_SDR_CLIENT_DEV_TICKET | k_ESteamNetworkingConfig_SDRClient_DevTicket | 
NETWORKING_CONFIG_SDR_CLIENT_FORCE_PROXY_ADDR | k_ESteamNetworkingConfig_SDRClient_ForceProxyAddr | 
NETWORKING_CONFIG_SDR_CLIENT_FAKE_CLUSTER_PING | k_ESteamNetworkingConfig_SDRClient_FakeClusterPing | 
NETWORKING_CONFIG_SDR_CLIENT_LIMIT_PING_PROBES_TO_NEAREST_N | k_ESteamNetworkingConfig_SDRClient_LimitPingProbesToNearestN | 
NETWORKING_CONFIG_LOG_LEVEL_ACK_RTT | k_ESteamNetworkingConfig_LogLevel_AckRTT | 
NETWORKING_CONFIG_LOG_LEVEL_PACKET_DECODE | k_ESteamNetworkingConfig_LogLevel_PacketDecode | 
NETWORKING_CONFIG_LOG_LEVEL_MESSAGE | k_ESteamNetworkingConfig_LogLevel_Message | 
NETWORKING_CONFIG_LOG_LEVEL_PACKET_GAPS | k_ESteamNetworkingConfig_LogLevel_PacketGaps | 
NETWORKING_CONFIG_LOG_LEVEL_P2P_RENDEZVOUS | k_ESteamNetworkingConfig_LogLevel_P2PRendezvous | 
NETWORKING_CONFIG_LOG_LEVEL_SRD_RELAY_PINGS | k_ESteamNetworkingConfig_LogLevel_SDRRelayPings | 
NETWORKING_CONFIG_CALLBACK_CONNECTION_STATUS_CHANGED | k_ESteamNetworkingConfig_Callback_ConnectionStatusChanged | 
NETWORKING_CONFIG_CALLBACK_AUTH_STATUS_CHANGED | k_ESteamNetworkingConfig_Callback_AuthStatusChanged | 
NETWORKING_CONFIG_CALLBACK_RELAY_NETWORK_STATUS_CHANGED | k_ESteamNetworkingConfig_Callback_RelayNetworkStatusChanged | 
NETWORKING_CONFIG_CALLBACK_MESSAGE_SESSION_REQUEST | k_ESteamNetworkingConfig_Callback_MessagesSessionRequest | 
NETWORKING_CONFIG_CALLBACK_MESSAGES_SESSION_FAILED | k_ESteamNetworkingConfig_Callback_MessagesSessionFailed | 
NETWORKING_CONFIG_CALLBACK_CREATE_CONNECTION_SIGNALING | k_ESteamNetworkingConfig_Callback_CreateConnectionSignaling | 
NETWORKING_CONFIG_CALLBACK_FAKE_IP_RESULT | k_ESteamNetworkingConfig_Callback_FakeIPResult | 
NETWORKING_CONFIG_P2P_STUN_SERVER_LIST | k_ESteamNetworkingConfig_P2P_STUN_ServerList | 
NETWORKING_CONFIG_P2P_TRANSPORT_ICE_ENABLE | k_ESteamNetworkingConfig_P2P_Transport_ICE_Enable | 
NETWORKING_CONFIG_P2P_TRANSPORT_ICE_PENALTY | k_ESteamNetworkingConfig_P2P_Transport_ICE_Penalty | 
NETWORKING_CONFIG_P2P_TRANSPORT_SDR_PENALTY | k_ESteamNetworkingConfig_P2P_Transport_SDR_Penalty | 
NETWORKING_CONFIG_P2P_TURN_SERVER_LIST | k_ESteamNetworkingConfig_P2P_TURN_ServerList | 
NETWORKING_CONFIG_P2P_TURN_uSER_LIST | k_ESteamNetworkingConfig_P2P_TURN_UserList | 
NETWORKING_CONFIG_P2P_TURN_PASS_LIST | k_ESteamNetworkingConfig_P2P_TURN_PassList | 
//			NETWORKING_CONFIG_P2P_TRANSPORT_LAN_BEACON_PENALTY | k_ESteamNetworkingConfig_P2P_Transport_LANBeacon_Penalty | 
NETWORKING_CONFIG_P2P_TRANSPORT_ICE_IMPLEMENTATION | k_ESteamNetworkingConfig_P2P_Transport_ICE_Implementation | 
NETWORKING_CONFIG_ECN | k_ESteamNetworkingConfig_ECN | 
NETWORKING_CONFIG_VALUE_FORCE32BIT | k_ESteamNetworkingConfigValue__Force32Bit

### NetworkingConnectionEnd

Enumerate various causes of connection termination.  These are designed to work similar to HTTP error codes: the numeric range gives you a rough classification as to the source of the problem.

Found in steamnetworkingtypes.h

CONNECTION_END_INVALID | k_ESteamNetConnectionEnd_Invalid | 0 | Invalid/sentinel value.
CONNECTION_END_APP_MIN | k_ESteamNetConnectionEnd_App_Min | 1000 | Application codes.  These are the values you will pass to [CloseConnection](#closeconnection). You can use these codes if you want to plumb through application-specific reason codes. If you don't need this facility, feel free to always pass CONNECTION_END_APP_GENERIC. The distinction between "normal" and "exceptional" termination is one you may use if you find useful, but it's not necessary for you to do so. The only place where we distinguish between normal and exceptional is in connection analytics. If a significant proportion of connections terminates in an exceptional manner, this can trigger an alert. 1xxx: Application ended the connection in a "usual" manner. E.g.: user intentionally disconnected from the server, gameplay ended normally, etc.
CONNECTION_END_APP_GENERIC | k_ESteamNetConnectionEnd_App_Generic | k_ESteamNetConnectionEnd_App_Min | Use codes in this range for "normal" disconnection
CONNECTION_END_APP_MAX | k_ESteamNetConnectionEnd_App_Max | 1999 | -
CONNECTION_END_APP_EXCEPTION_MIN | k_ESteamNetConnectionEnd_AppException_Min | 2000 | 2xxx: Application ended the connection in some sort of exceptional or unusual manner that might indicate a bug or configuration issue.
CONNECTION_END_APP_EXCEPTION_GENERIC | k_ESteamNetConnectionEnd_AppException_Generic | k_ESteamNetConnectionEnd_AppException_Min | Use codes in this range for "unusual" disconnection.
CONNECTION_END_APP_EXCEPTION_MAX | k_ESteamNetConnectionEnd_AppException_Max | 2999 | -
CONNECTION_END_LOCAL_MIN | k_ESteamNetConnectionEnd_Local_Min | 3000 | 3xxx: Connection failed or ended because of problem with the local host or their connection to the Internet.
CONNECTION_END_LOCAL_OFFLINE_MODE | k_ESteamNetConnectionEnd_Local_OfflineMode | 3001 | 3xxx: Connection failed or ended because of problem with the local host or their connection to the Internet.
CONNECTION_END_LOCAL_MANY_RELAY_CONNECTIVITY | k_ESteamNetConnectionEnd_Local_ManyRelayConnectivity | 3002 | We're having trouble contacting many (perhaps all) relays. Since it's unlikely that they all went offline at once, the best explanation is that we have a problem on our end.  Note that we don't bother distinguishing between "many" and "all", because in practice, it takes time to detect a connection problem, and by the time the connection has timed out, we might not have been able to actively probe all of the relay clusters, even if we were able to contact them at one time.  So this code just means that:
 * We don't have any recent successful communication with any relay.
 * We have evidence of recent failures to communicate with multiple relays.
CONNECTION_END_LOCAL_HOSTED_SERVER_PRIMARY_RELAY | k_ESteamNetConnectionEnd_Local_HostedServerPrimaryRelay | 3003 | A hosted server is having trouble talking to the relay that the client was using, so the problem is most likely on our end.
CONNECTION_END_LOCAL_NETWORK_CONFIG | k_ESteamNetConnectionEnd_Local_NetworkConfig | 3004 | We're not able to get the SDR network config.  This is *almost* always a local issue, since the network config comes from the CDN, which is pretty darn reliable.
CONNECTION_END_LOCAL_RIGHTS | k_ESteamNetConnectionEnd_Local_Rights | 3005 | Steam rejected our request because we don't have rights to do this.
CONNECTION_END_NO_PUBLIC_ADDRESS | k_ESteamNetConnectionEnd_Local_P2P_ICE_NoPublicAddresses | 3006 | 
CONNECTION_END_LOCAL_MAX | k_ESteamNetConnectionEnd_Local_Max | 3999 | -
CONNECTION_END_REMOVE_MIN | k_ESteamNetConnectionEnd_Remote_Min | 4000 | 4xxx: Connection failed or ended, and it appears that the cause does NOT have to do with the local host or their connection to the Internet.  It could be caused by the remote host, or it could be somewhere in between.
CONNECTION_END_REMOTE_TIMEOUT | k_ESteamNetConnectionEnd_Remote_Timeout | 4001 | The connection was lost, and as far as we can tell our connection to relevant services (relays) has not been disrupted.  This doesn't mean that the problem is "their fault", it just means that it doesn't appear that we are having network issues on our end.
CONNECTION_END_REMOTE_BAD_CRYPT | k_ESteamNetConnectionEnd_Remote_BadCrypt | 4002 | Something was invalid with the cert or crypt handshake info you gave me, I don't understand or like your key types, etc.
CONNECTION_END_REMOTE_BAD_CERT | k_ESteamNetConnectionEnd_Remote_BadCert | 4003 | You presented me with a cert that was I was able to parse and *technically* we could use encrypted communication. But there was a problem that prevents me from checking your identity or ensuring that somebody int he middle can't observe our communication. E.g.: - the CA key was missing (and I don't accept unsigned certs). The CA key isn't one that I trust. The cert doesn't was appropriately restricted by app, user, time, data center, etc. The cert wasn't issued to you.
CONNECTION_END_BAD_PROTOCOL_VERSION | k_ESteamNetConnectionEnd_Remote_BadProtocolVersion | 4006 | These will never be returned: 4004 or 4005. Something wrong with the protocol version you are using. (Probably the code you are running is too old.)
CONNECTION_END_REMOTE_P2P_ICE_NO_PUBLIC_ADDRESSES | k_ESteamNetConnectionEnd_Remote_P2P_ICE_NoPublicAddresses | 4007 | NAT punch failed failed because we never received any public addresses from the remote host.  (But we did receive some signals form them.) If relay fallback is available (it always is on Steam), then this is only used internally and will not be returned as a high level failure.
CONNECTION_END_REMOTE_MAX | k_ESteamNetConnectionEnd_Remote_Max | 4999 | -
CONNECTION_END_MISC_MIN | k_ESteamNetConnectionEnd_Misc_Min | 5000 | 5xxx: Connection failed for some other reason.
CONNECTION_END_MISC_GENERIC | k_ESteamNetConnectionEnd_Misc_Generic | 5001 | A failure that isn't necessarily the result of a software bug, but that should happen rarely enough that it isn't worth specifically writing UI or making a localized message for. The debug string should contain further details.
CONNECTION_END_MISC_INTERNAL_ERROR | k_ESteamNetConnectionEnd_Misc_InternalError | 5002 | Generic failure that is most likely a software bug.
CONNECTION_END_MISC_TIMEOUT | k_ESteamNetConnectionEnd_Misc_Timeout | 5003 | The connection to the remote host timed out, but we don't know if the problem is on our end, in the middle, or on their end.
CONNECTION_END_MISC_STEAM_CONNECTIVITY | k_ESteamNetConnectionEnd_Misc_SteamConnectivity | 5005 | There's some trouble talking to Steam.
CONNECTION_END_MISC_NO_RELAY_SESSIONS_TO_CLIENT | k_ESteamNetConnectionEnd_Misc_NoRelaySessionsToClient | 5006 | A server in a dedicated hosting situation has no relay sessions active with which to talk back to a client.  (It's the client's job to open and maintain those sessions.)
CONNECTION_END_MISC_P2P_RENDEZVOUS | k_ESteamNetConnectionEnd_Misc_P2P_Rendezvous | 5008 | P2P rendezvous failed in a way that we don't have more specific information.
CONNECTION_END_MISC_P2P_NAT_FIREWALL | k_ESteamNetConnectionEnd_Misc_P2P_NAT_Firewall | 5009 | NAT punch failed, probably due to NAT/firewall configuration. If relay fallback is available (it always is on Steam), then this is only used internally and will not be returned as a high level failure.
CONNECTION_END_MISC_PEER_SENT_NO_CONNECTION | k_ESteamNetConnectionEnd_Misc_PeerSentNoConnection | 5010 | Our peer replied that it has no record of the connection. This should not happen ordinarily, but can happen in a few exception cases: This is an old connection, and the peer has already cleaned up and forgotten about it.  (Perhaps it timed out and they closed it and were not able to communicate this to us.) A bug or internal protocol error has caused us to try to talk to the peer about the connection before we received confirmation that the peer has accepted the connection. The peer thinks that we have closed the connection for some reason (perhaps a bug), and believes that is it is acknowledging our closure.
CONNECTION_END_MISC_MAX | k_ESteamNetConnectionEnd_Misc_Max | 5999 | - 
CONNECTION_END_FORCE32BIT | k_ESteamNetConnectionEnd__Force32Bit | 0x7fffffff | -

### NetworkingConnectionState

High level connection status.

Found in steamnetworkingtypes.h

CONNECTION_STATE_NONE | k_ESteamNetworkingConnectionState_None | 0 | Dummy value used to indicate an error condition in the API. Specified connection doesn't exist or has already been closed.
CONNECTION_STATE_CONNECTING | k_ESteamNetworkingConnectionState_Connecting | 1 | We are trying to establish whether peers can talk to each other, whether they WANT to talk to each other, perform basic auth, and exchange crypt keys. For connections on the "client" side (initiated locally): We're in the process of trying to establish a connection. Depending on the connection type, we might not know who they are. Note that it is not possible to tell if we are waiting on the network to complete handshake packets, or for the application layer to accept the connection. For connections on the "server" side (accepted through listen socket): We have completed some basic handshake and the client has presented some proof of identity.  The connection is ready to be accepted using [acceptConnection](#acceptconnection). In either case, any unreliable packets sent now are almost certain to be dropped.  Attempts to receive packets are guaranteed to fail. You may send messages if the send mode allows for them to be queued. but if you close the connection before the connection is actually established, any queued messages will be discarded immediately. (We will not attempt to flush the queue and confirm delivery to the remote host, which ordinarily happens when a connection is closed.)
CONNECTION_STATE_FINDING_ROUTE | k_ESteamNetworkingConnectionState_FindingRoute | 2 | Some connection types use a back channel or trusted 3rd party for earliest communication.  If the server accepts the connection, then these connections switch into the rendezvous state.  During this state, we still have not yet established an end-to-end route (through the relay network), and so if you send any messages unreliable, they are going to be discarded.
CONNECTION_STATE_CONNECTED | k_ESteamNetworkingConnectionState_Connected | 3 | We've received communications from our peer (and we know who they are) and are all good.  If you close the connection now, we will make our best effort to flush out any reliable sent data that has not been acknowledged by the peer.  (But note that this happens from within the application process, so unlike a TCP connection, you are not totally handing it off to the operating system to deal with it.)
CONNECTION_STATE_CLOSED_BY_PEER | k_ESteamNetworkingConnectionState_ClosedByPeer | 4 | Connection has been closed by our peer, but not closed locally. The connection still exists from an API perspective.  You must close the handle to free up resources.  If there are any messages in the inbound queue, you may retrieve them.  Otherwise, nothing may be done with the connection except to close it. This stats is similar to CLOSE_WAIT in the TCP state machine.
CONNECTION_STATE_PROBLEM_DETECTED_LOCALLY | k_ESteamNetworkingConnectionState_ProblemDetectedLocally | 5 | A disruption in the connection has been detected locally.  (E.g. timeout, local internet connection disrupted, etc.) The connection still exists from an API perspective.  You must close the handle to free up resources. Attempts to send further messages will fail.  Any remaining received messages in the queue are available.
CONNECTION_STATE_FIN_WAIT | k_ESteamNetworkingConnectionState_FinWait | -1 | We've disconnected on our side, and from an API perspective the connection is closed. No more data may be sent or received.  All reliable data has been flushed, or else we've given up and discarded it. We do not yet know for sure that the peer knows the connection has been closed, however, so we're just hanging around so that if we do get a packet from them, we can send them the appropriate packets so that they can know why the connection was closed (and not have to rely on a timeout, which makes it appear as if something is wrong).
CONNECTION_STATE_LINGER | k_ESteamNetworkingConnectionState_Linger | -2 | We've disconnected on our side, and from an API perspective the connection is closed. No more data may be sent or received.  From a network perspective, however, on the wire, we have not yet given any indication to the peer that the connection is closed. We are in the process of flushing out the last bit of reliable data.  Once that is done, we will inform the peer that the connection has been closed, and transition to the FinWait state. Note that no indication is given to the remote host that we have closed the connection, until the data has been flushed.  If the remote host attempts to send us data, we will do whatever is necessary to keep the connection alive until it can be closed properly. But in fact the data will be discarded, since there is no way for the application to read it back.  Typically this is not a problem, as application protocols that utilize the lingering functionality are designed for the remote host to wait for the response before sending any more data.
CONNECTION_STATE_DEAD | k_ESteamNetworkingConnectionState_Dead | -3 | Connection is completely inactive and ready to be destroyed.
CONNECTION_STATE_FORCE_32BIT | k_ESteamNetworkingConnectionState__Force32Bit | 0x7fffffff | -

### NetworkingFakeIPType

"Fake IPs" are assigned to hosts, to make it easier to interface with older code that assumed all hosts will have an IPv4 address. No enum values are given in the SDK.

Found in steamnetworkingtypes.h

FAKE_IP_TYPE_INVALID | k_ESteamNetworkingFakeIPType_Invalid | - | Error, argument was not even an IP address, etc.
FAKE_IP_TYPE_NOT_FAKE | k_ESteamNetworkingFakeIPType_NotFake | - | Argument was a valid IP, but was not from the reserved "fake" range.
FAKE_IP_TYPE_GLOBAL_IPV4 | k_ESteamNetworkingFakeIPType_GlobalIPv4 | - | Globally unique (for a given app) IPv4 address.  Address space managed by Steam.
FAKE_IP_TYPE_LOCAL_IPV4 | k_ESteamNetworkingFakeIPType_LocalIPv4 | - | Locally unique IPv4 address.  Address space managed by the local process.  For internal use only; should not be shared!
FAKE_IP_TYPE_FORCE32BIT | k_ESteamNetworkingFakeIPType__Force32Bit | 0x7fffffff | -

### NetworkingGetConfigValueResult 

Found in steamnetworkingtypes.h

NETWORKING_GET_CONFIG_VALUE_BAD_VALUE | k_ESteamNetworkingGetConfigValue_BadValue | 
NETWORKING_GET_CONFIG_VALUE_BAD_SCOPE_OBJ | k_ESteamNetworkingGetConfigValue_BadScopeObj | 
NETWORKING_GET_CONFIG_VALUE_BUFFER_TOO_SMALL | k_ESteamNetworkingGetConfigValue_BufferTooSmall | 
NETWORKING_GET_CONFIG_VALUE_OK | k_ESteamNetworkingGetConfigValue_OK | 
NETWORKING_GET_CONFIG_VALUE_OK_INHERITED | k_ESteamNetworkingGetConfigValue_OKInherited | 
NETWORKING_GET_CONFIG_VALUE_FORCE_32BIT | k_ESteamNetworkingGetConfigValueResult__Force32Bit

### NetworkingIdentityType

Describing network hosts. Different methods of describing the identity of a network host.

Found in steamnetworkingtypes.h

IDENTITY_TYPE_INVALID | k_ESteamNetworkingIdentityType_Invalid | 0 | Dummy/empty/invalid. Please note that if we parse a string that we don't recognize but that appears reasonable, we will NOT use this type.  Instead we'll use IDENTITY_TYPE_UNKNOWN_TYPE.
IDENTITY_TYPE_IP_ADDRESS | k_ESteamNetworkingIdentityType_IPAddress | 1 | Use their IP address (and port) as their "identity". These types of identities are always unauthenticated. They are useful for porting plain sockets code, and other situations where you don't care about authentication.  In this case, the local identity will be "localhost", and the remote address will be their network address. We use the same type for either IPv4 or IPv6, and the address is always store as IPv6.  We use IPv4 mapped addresses to handle IPv4.
IDENTITY_TYPE_GENERIC_STRING | k_ESteamNetworkingIdentityType_GenericString | 2 | Generic string/binary blobs.  It's up to your app to interpret this. This library can tell you if the remote host presented a certificate signed by somebody you have chosen to trust, with this identity on it. It's up to you to ultimately decide what this identity means.
IDENTITY_TYPE_GENERIC_BYTES | k_ESteamNetworkingIdentityType_GenericBytes | 3 | -
IDENTITY_TYPE_UNKNOWN_TYPE | k_ESteamNetworkingIdentityType_UnknownType | 4 | This identity type is used when we parse a string that looks like is a valid identity, just of a kind that we don't recognize.  In this case, we  can often still communicate with the peer!  Allowing such identities for types we do not recognize useful is very useful for forward compatibility.
IDENTITY_TYPE_STEAMID | k_ESteamNetworkingIdentityType_SteamID | 16 | 64-bit CSteamID.
IDENTITY_TYPE_XBOX_PAIRWISE | k_ESteamNetworkingIdentityType_XboxPairwiseID | 17 | Publisher-specific user identity, as string.
IDENTITY_TYPE_SONY_PSN | k_ESteamNetworkingIdentityType_SonyPSN | 18 | 64-bit ID.
IDENTITY_TYPE_FORCE_32BIT | k_ESteamNetworkingIdentityType__Force32bit | 0x7fffffff | Make sure this enum is stored in an int.

### NetworkingSocketsDebugOutputType

Found in steamnetworkingtypes.h

NETWORKING_SOCKET_DEBUG_OUTPUT_TYPE_NONE | k_ESteamNetworkingSocketsDebugOutputType_None | 
NETWORKING_SOCKET_DEBUG_OUTPUT_TYPE_BUG | k_ESteamNetworkingSocketsDebugOutputType_Bug | 
NETWORKING_SOCKET_DEBUG_OUTPUT_TYPE_ERROR | k_ESteamNetworkingSocketsDebugOutputType_Error | 
NETWORKING_SOCKET_DEBUG_OUTPUT_TYPE_IMPORTANT | k_ESteamNetworkingSocketsDebugOutputType_Important | 
NETWORKING_SOCKET_DEBUG_OUTPUT_TYPE_WARNING | k_ESteamNetworkingSocketsDebugOutputType_Warning | 
NETWORKING_SOCKET_DEBUG_OUTPUT_TYPE_MSG | k_ESteamNetworkingSocketsDebugOutputType_Msg | 
NETWORKING_SOCKET_DEBUG_OUTPUT_TYPE_VERBOSE | k_ESteamNetworkingSocketsDebugOutputType_Verbose | 
NETWORKING_SOCKET_DEBUG_OUTPUT_TYPE_DEBUG | k_ESteamNetworkingSocketsDebugOutputType_Debug | 
NETWORKING_SOCKET_DEBUG_OUTPUT_TYPE_EVERYTHING | k_ESteamNetworkingSocketsDebugOutputType_Everything | 
NETWORKING_SOCKET_DEBUG_OUTPUT_TYPE_FORCE_32BIT | k_ESteamNetworkingSocketsDebugOutputType__Force32Bit | 