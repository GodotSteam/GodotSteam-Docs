---
title: Networking Sockets
description: A class reference for Networking Sockets.
icon: material/power-socket
---

# Networking Sockets

These are part of the newer networking classes; not to be confused with the [older, now-deprecated Networking class](networking.md).

Networking API similar to Berkeley sockets, but for games:

- It's a connection-oriented API (like TCP, not UDP). When sending and receiving messages, the peer is identified using a connection handle.
- But unlike TCP, it's message-oriented, not stream-oriented. (The boundaries between the messages are maintained by the API.)
- Both reliable and unreliable messages are supported.
- Large messages are split into multiple packets, small messages are combined into fewer packets.
- A robust ACK / reassembly / retransmission strategy.
- Strong encryption and authentication. When a player connects, you can be sure that if a certain Steam ID is authenticated, that someone who has access to that person's account has authorized the connection. Eavesdropping / tampering requires hacking into the VAC-secured process. Impersonation requires access to the target's computer.
- Supports relayed connections through the Valve network. This prevents IP addresses from being revealed, protecting your players and gameservers from attack.
- Also supports standard connectivity over plain UDP using IPv4 or IPv6.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } and [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### acceptConnection

!!! function "acceptConnection( `uint32_t` connection_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The incoming connection handle.

	Accept an incoming connection that has been received on a listen socket.

	When a connection attempt is received (perhaps after a few basic handshake packets have been exchanged to prevent trivial spoofing), a connection interface object is created in the [CONNECTION_STATE_CONNECTING](#networkingconnectionstate) state and a [network_connection_status_changed](#network_connection_status_changed) is posted. At this point, your application **must** either accept or close the connection. (It may not ignore it.) Accepting the connection will transition it either into the connected state, or the finding route state, depending on the connection type.

	You should take action within a second or two, because accepting the connection is what actually sends the reply notifying the client that they are connected. If you delay taking action, from the client's perspective it is the same as the network being unresponsive, and the client may timeout the connection attempt. In other words, the client cannot distinguish between a delay caused by network problems and a delay caused by the application.

	This means that if your application goes for more than a few seconds without processing callbacks (for example, while loading a map), then there is a chance that a client may attempt to connect in that interval and fail due to timeout.

	If the application does not respond to the connection attempt in a timely manner, and we stop receiving communication from the client, the connection attempt will be timed out locally, transitioning the connection to the [CONNECTION_STATE_PROBLEM_DETECTED_LOCALLY](#networkingconnectionstate) state. The client may also close the connection before it is accepted, and a transition to the [CONNECTION_STATE_CLOSED_BY_PEER](#networkingconnectionstate) is also possible depending the exact sequence of events.

	!!! returns "Returns: Result enum"

### beginAsyncRequestFakeIP

!!! function "beginAsyncRequestFakeIP( `int` num_ports )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | num_ports | int | The number of ports to reserve. |

	Begin asynchronous process of allocating a fake IPv4 address that other peers can use to contact us via P2P. IP addresses returned by this function are globally unique for a given appid.

	**num_ports** is the numbers of ports you wish to reserve. This is useful for the same reason that listening on multiple UDP ports is useful for different types of traffic. Because these allocations come from a global namespace, there is a relatively strict limit on the maximum number of ports you may request. (At the time of this writing, the limit is 4.) The port assignments are *not* guaranteed to have any particular order or relationship! Do *not* assume they are contiguous, even though that may often occur in practice.

	Returns false if a request was already in progress, true if a new request was started. A SteamNetworkingFakeIPResult_t will be posted when the request completes.

	For gameservers, you *must* call this after initializing the SDK but before beginning login. Steam needs to know in advance that FakeIP will be used. Everywhere your public IP would normally appear (such as the server browser) will be replaced by the FakeIP, and the fake port at index 0. The request is actually queued until the logon completes, so you must not wait until the allocation completes before logging in. Except for trivial failures that can be detected locally (e.g. invalid parameter), a [fake_ip_result](#fake_ip_result) callback (whether success or failure) will not be posted until after we have logged in. Furthermore, it is assumed that FakeIP allocation is essential for your application to function, and so failure will not be reported until *several* retries have been attempted. This process may last several minutes. It is *highly* recommended to treat failure as fatal.

	To communicate using a connection-oriented (TCP-style) API:

	- Server creates a listen socket using [createListenSocketP2PFakeIP](#createlistensocketp2pfakeip)
    - Client connects using [connectByIPAddress](#connectbyipaddress), passing in the FakeIP address.
    - The connection will behave mostly like a P2P connection. The identities that appear in **connection_info** will be the FakeIP identity until we know the real identity. Then it will be the real identity. If the **remote_address** key is valid, it will be a real IPv4 address of a NAT-punched connection. Otherwise, it will not be valid.

	To communicate using an ad-hoc sendto/recv from (UDP-style) API, use [createFakeUDPPort](#createfakeudpport).

	!!! returns "Returns: bool

	_False_ if a request was already in progress, _true_ if a new request was started.

	**Triggers:** [fake_ip_result](#fake_ip_result) callback

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#AcceptConnection){ .md-button .md-button--doc_classes target="_blank" }

### closeConnection

!!! function "closeConnection( `uint32_t` connection_handle, `int` reason, `string` debug_message, `bool` linger )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to close.
    | reason | int | An application defined code.
    | debug_message | string | Optional human-readable diagnostic string.
    | linger | bool | Whether or not to put the socket into a linger state.

    Disconnects from the remote host and invalidates the connection handle. Any unread data on the connection is discarded.

	**reason** is an application defined code that will be received on the other end and recorded (when possible) in backend analytics. The value should come from a restricted range. ([See ESteamNetConnectionEnd.](https://partner.steamgames.com/doc/api/steamnetworkingtypes#ESteamNetConnectionEnd){ target="_blank" }) If you don't need to communicate any information to the remote host, and do not want analytics to be able to distinguish "normal" connection terminations from "exceptional" ones, you may pass zero, in which case the generic value of **CONNECTION_END_APP_MIN** will be used.

	**debug_message** is an optional human-readable diagnostic string that will be received by the remote host and recorded (when possible) in backend analytics.

	If you wish to put the socket into a "linger" state, where an attempt is made to  flush any remaining sent data, set **linger** to true.  Otherwise reliable data is not flushed.

	If the connection has already ended and you are just freeing up the connection interface, the reason code, debug string, and linger flag are ignored.

	!!! returns "Returns: bool"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#CloseConnection){ .md-button .md-button--doc_classes target="_blank" }

### closeListenSocket

!!! function "closeListenSocket( `uint32_t` socket )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | socket | uint32_t | The listen socket to close. |

    Destroy a listen socket. All the connections that were accepted on the listen socket are closed ungracefully.

	!!! returns "Returns: bool"

### configureConnectionLanes

!!! function "configureConnectionLanes( `uint32_t` connection, `uint32_t` lanes, `Array` priorities, `Array` weights )"
	Configure multiple outbound messages streams ("lanes") on a connection, and control head-of-line blocking between them. Messages within a given lane are always sent in the order they are queued, but messages from different lanes may be sent out of order. Each lane has its own message number sequence. The first message sent on each lane will be assigned the number 1.

	Each lane has a "priority". Lower priority lanes will only be processed when all higher-priority lanes are empty. The magnitudes of the priority values are not relevant, only their sort order. Higher numeric values take priority over lower numeric values.

	Each lane also is assigned a weight, which controls the approximate proportion of the bandwidth that will be consumed by the lane, relative to other lanes of the same priority. (This is assuming the lane stays busy. An idle lane does not build up "credits" to be be spent once a message is queued.) This value is only meaningful as a proportion, relative to other lanes with the same priority. For lanes with different priorities, the strict priority order will prevail, and their weights relative to each other are not relevant. Thus, if a lane has a unique priority value, the weight value for that lane is not relevant.

	Example: 3 lanes, with priorities [ 0, 10, 10 ] and weights [ (NA), 20, 5 ].  Messages sent on the first will always be sent first, before messages in the other two lanes.  Its weight value is irrelevant, since there are no other lanes with priority = 0.  The other two lanes will share bandwidth, with the second and third lanes sharing bandwidth using a ratio of approximately 4:1. The weights [ NA, 4, 1 ] would be equivalent.

	!!! returns "Returns: Result enum"
		Returns [RESULT_OK](main.md#result) on success; otherwise, one of these possible failures:

		* [RESULT_NO_CONNECTION](main.md#result) - Bad **connection**.
		* [RESULT_INVALID_PARAM](main.md#result) - Invalid number of lanes, bad weights, or you tried to reduce the number of lanes.
		* [RESULT_INVALID_STATE](main.md#result) - Connection is already dead, etc.

	!!! info "Notes"
		* At the time of this writing, some code has performance cost that is linear in the number of lanes, so keep the number of lanes to an absolute minimum.  3 or so is fine; great than 8 is a lot.  The max number of lanes on Steam is 255, which is a very large number and not recommended!
		* Lane priority values may be any int.  Their absolute value is not relevant, only the order matters.
		* Weights must be positive, and due to implementation details, they are restricted to 16-bit values.  The absolute magnitudes don't matter, just the proportions.
		* Messages sent on a lane index other than 0 have a small overhead on the wire, so for maximum wire efficiency, lane 0 should be the "most common" lane, regardless of priorities or weights.
		* A connection has a single lane by default.  Calling this function with lanes = 1 is legal but pointless, since the priority and weight values are irrelevant in that case.
		* You may reconfigure connection lanes at any time, however reducing the number of lanes is not allowed.
		* Reconfiguring lanes might restart any bandwidth sharing balancing.  Usually you will call this function once, near the start of the connection, perhaps after exchanging a few messages.
		* Priorities and weights determine the order that messages are sent on the wire. There are **no guarantees** on the order that messages are received.  Due to packet loss, out-of-order delivery, and subtle details of packet serialization, messages might still be received slightly out-of-order.  The **only** strong guarantee is that **reliable** messages on the **same lane** will be delivered in the order they are sent.
		* Each host configures the lanes for the packets they send; the lanes for the flow in one direction are completely unrelated to the lanes in the opposite direction.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#CloseListenSocket){ .md-button .md-button--doc_classes target="_blank" }

### connectByIPAddress

!!! function "connectByIPAddress( `string` ip_address_with_port, `dictionary` config_options )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | ip_address_with_port | string | The IP address, with the port, to connect to.
    | config_options | dictionary | A collection of optional, initial config options.

    Creates a connection and begins talking to a "server" over UDP at the given IPv4 or IPv6 address.  The remote host must be listening with a matching call to [createListenSocketIP](#createlistensocketip) on the specified port.

	A [network_connection_status_changed](#network_connection_status_changed) callback will be triggered when we start connecting, and then another one on either timeout or successful connection.

	If the server does not have any identity configured, then their network address will be the only identity in use.  Or, the network host may provide a platform-specific identity with or without a valid certificate to authenticate that identity.  These details will be contained in the [network_connection_status_changed](#network_connection_status_changed).  It's up to your application to decide whether to allow the connection.

	By default, all connections will get basic encryption sufficient to prevent casual eavesdropping.  But note that without certificates (or a shared secret distributed through some other out-of-band mechanism), you don't have any way of knowing who is actually on the other end, and thus are vulnerable to man-in-the-middle attacks.

	If you need to set any initial config options, pass them here.  Pass your **config_options** as dictionary containing the following key/value pairs:

	* [NetworkingConfigValue](#networkingconfigvalue) (int / enum)
	* config value (int or string)

	Example:
	```gdscript
	{
		NETWORKING_CONFIG_FAKE_PACKET_LAG_SEND : 4,
		NETWORKING_CONFIG_SEND_BUFFER_SIZE: 5000,
		NETWORKING_CONFIG_RECV_BUFFER_SIZE: 3000 
	}
	```

	Alternately you can pass an empty dictionary.

	!!! returns "Returns: uint32_t"
		Returns a networking connection handle on success; otherwise, [NETWORKING_CONNECTION_INVALID](main.md#constants).

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#ConnectByIPAddress){ .md-button .md-button--doc_classes target="_blank" }

### connectP2P

!!! function "connectP2P( `uint64_t` remote_steam_id, `int` virtual_port, `dictionary` config_options )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | remote_steam_id | uint64_t | The Steam ID of the user to connect to. |
    | virtual_port | int | The port to connect through. |
    | config_options | dictionary | A collection of optional, initial config options. |

    Begin connecting to a server that is identified using a platform-specific identifier. This uses the default rendezvous service, which depends on the platform and library configuration. (E.g. on Steam, it goes through the steam backend.) The traffic is relayed over the Steam Datagram Relay network.

	If you use this, you probably want to call [initRelayNetworkAccess](networking_utils.md#initrelaynetworkaccess) when your app initializes. If you need to set any initial config options, pass them here.

	[See SteamNetworkingConfigValue_t](https://partner.steamgames.com/doc/api/steamnetworkingtypes#SteamNetworkingConfigValue_t){ target="_blank" } for more about why this is preferable to setting the options "immediately" after creation.

	Pass your **config_options** as dictionary containing the following key/value pairs:

	* [NetworkingConfigValue](#networkingconfigvalue) (int / enum)
	* config value (int or string)

	Example:
	```gdscript
	{
		NETWORKING_CONFIG_FAKE_PACKET_LAG_SEND : 4,
		NETWORKING_CONFIG_SEND_BUFFER_SIZE: 5000,
		NETWORKING_CONFIG_RECV_BUFFER_SIZE: 3000 
	}
	```

	Alternately you can pass an empty dictionary.

	!!! returns "Returns: uint32_t"
		Returns a networking connection handle on success; otherwise, [NETWORKING_CONNECTION_INVALID](networking_utils.md#constants).

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#ConnectP2P){ .md-button .md-button--doc_classes target="_blank" }

### connectToHostedDedicatedServer

!!! function "connectToHostedDedicatedServer( `uint64_t` remote_steam_id, `int` virtual_port, `dictionary` config_options )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | remote_steam_id | uint64_t | The Steam ID of the user to connect to. |
    | virtual_port | int | The port to connect through. |
    | config_options | dictionary | A collection of optional, initial config options. |

    Client call to connect to a server hosted in a Valve data center, on the specified virtual port. You must have placed a ticket for this server into the cache, or else this connect attempt will fail!

	Pass your **config_options** as dictionary containing the following key/value pairs:

	* [NetworkingConfigValue](#networkingconfigvalue) (int / enum)
	* config value (int or string)

	Example:
	```gdscript
	{
		NETWORKING_CONFIG_FAKE_PACKET_LAG_SEND : 4,
		NETWORKING_CONFIG_SEND_BUFFER_SIZE: 5000,
		NETWORKING_CONFIG_RECV_BUFFER_SIZE: 3000 
	}
	```

	Alternately you can pass an empty dictionary.

	!!! returns "Returns: uint32_t"
		Returns a network connection handle.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#ConnectToHostedDedicatedServer){ .md-button .md-button--doc_classes target="_blank" }

### createFakeUDPPort

!!! function "createFakeUDPPort( `int` fake_server_port)"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | fake_server_port | int | The index of the port allocated by [beginAsyncRequestFakeIP](#beginasyncrequestfakeip). |

    Get an interface that can be used like a UDP port to send/receive datagrams to a FakeIP address. This is intended to make it easy to port existing UDP-based code to take advantage of SDR.

	**fake_server_port** refers to the *index* of the port allocated using [beginAsyncRequestFakeIP](#beginasyncrequestfakeip) and is used to create "server" ports. You may call this before the allocation has completed. However, any attempts to send packets will fail until the allocation has succeeded. When the peer receives packets sent from this interface, the from address of the packet will be the globally-unique FakeIP. If you call this function multiple times and pass the same (nonnegative) fake port index, the same object will be returned, and this object is not reference counted.

	To create a "client" port (e.g. the equivalent of an ephemeral UDP port) pass -1. In this case, a distinct object will be returned for each call. When the peer receives packets sent from this interface, the peer will assign a FakeIP from its own locally-controlled namespace.

	!!! returns "Returns: void

### createHostedDedicatedServerListenSocket

!!! function "createHostedDedicatedServerListenSocket( `int` virtual_port, `dictionary` config_options )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | virtual_port | int | The port to connect through. |
    | config_options | dictionary | A collection of optional, initial config options. |

    Create a listen socket on the specified virtual port. The physical UDP port to use will be determined by the SDR_LISTEN_PORT environment variable. If a UDP port is not configured, this call will fail.

	This function should be used when you are using the ticket generator library to issue your own tickets. Clients connecting to the server on this virtual port will need a ticket, and they must connect using [connectToHostedDedicatedServer](#connecttohosteddedicatedserver).

	Pass your **config_options** as dictionary containing the following key/value pairs:

	* [NetworkingConfigValue](#networkingconfigvalue) (int / enum)
	* config value (int or string)

	Example:
	```gdscript
		NETWORKING_CONFIG_FAKE_PACKET_LAG_SEND : 4,
		NETWORKING_CONFIG_SEND_BUFFER_SIZE: 5000,
		NETWORKING_CONFIG_RECV_BUFFER_SIZE: 3000 
	}
	```

	Alternately you can pass an empty dictionary.

	!!! returns "Returns: uint32_t 

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#CreateHostedDedicatedServerListenSocket){ .md-button .md-button--doc_classes target="_blank" }

### createListenSocketIP

!!! function "createListenSocketIP( `string` ip_address, `dictionary` config_options )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | ip_address | string | The IP address of the listen socket to create.
    | config_options | dictionary | A collection of optional, initial config options.

    Creates a "server" socket that listens for clients to connect to by calling [connectByIPAddress](#connectbyipaddress), over ordinary UDP (IPv4 or IPv6)

    You must select a specific local port to listen on and set it the port field of the local address.

    Usually you will set the IP portion of the address to zero. This means that you will not bind to any particular local interface; ie. the same as INADDR_ANY in plain socket code.  Furthermore, if possible the socket will be bound in "dual stack" mode, which means that it can accept both IPv4 and IPv6 client connections.

    If you really do wish to bind a particular interface, then set the local address to the appropriate IPv4 or IPv6 IP.

	If you need to set any initial config options, pass them here.  Pass your **config_options** as dictionary containing the following key/value pairs:

	* [NetworkingConfigValue](#networkingconfigvalue) (int / enum)
	* config value (int or string)

	Example:
	```gdscript
	{
		NETWORKING_CONFIG_FAKE_PACKET_LAG_SEND : 4,
		NETWORKING_CONFIG_SEND_BUFFER_SIZE: 5000,
		NETWORKING_CONFIG_RECV_BUFFER_SIZE: 3000 
	}
	```

	Alternately you can pass an empty dictionary.

	When a client attempts to connect, a [network_connection_status_changed](#network_connection_status_changed) will be posted.  The connection will be in the connecting state.

	!!! returns "Returns: uint32_t"
		Returns a listen socket handle upon success; otherwise, [LISTEN_SOCKET_INVALID](networking_utils.md#constants).

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#CreateListenSocketIP){ .md-button .md-button--doc_classes target="_blank" }

### createListenSocketP2P

!!! function "createListenSocketP2P( `int` virtual_port, `dictionary` config_options )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | virtual_port | int | The port to connect through.
    | config_options | dictionary | A collection of optional, initial config options.

    Like [createListenSocketIP](#createlistensocketip), but clients will connect using [connectP2P](#connectp2p). The connection will be relayed through the Valve network.

	**virtual_port** specifies how clients can connect to this socket using [connectP2P](#connectp2p). It's very common for applications to only have one listening socket; in that case, use zero. If you need to open multiple listen sockets and have clients be able to connect to one or the other, then virtual_port should be a small integer (<1000) unique to each listen socket you create.

	If you use this, you probably want to call [initRelayNetworkAccess](networking_utils.md#initrelaynetworkaccess) when your app initializes.

	If you are listening on a dedicated servers in known data center, then you can listen using this function instead of [createHostedDedicatedServerListenSocket](#createhosteddedicatedserverlistensocket), to allow clients to connect without a ticket.  Any user that owns the app and is signed into Steam will be able to attempt to connect to your server.  Also, a connection attempt may require the client to be connected to Steam, which is one more moving part that may fail.  When tickets are used, then once a ticket is obtained, a client can connect to your server even if they got disconnected from Steam or Steam is offline.

	Pass your **config_options** as dictionary containing the following key/value pairs:

	* [NetworkingConfigValue](#networkingconfigvalue) (int / enum)
	* config value (int or string)

	Example:
	```gdscript
	{
		NETWORKING_CONFIG_FAKE_PACKET_LAG_SEND : 4,
		NETWORKING_CONFIG_SEND_BUFFER_SIZE: 5000,
		NETWORKING_CONFIG_RECV_BUFFER_SIZE: 3000 
	}
	```

	Alternately you can pass an empty dictionary.

	!!! returns "Returns: uint32_t"
		Returns a listen socket handle on success; otherwise, [LISTEN_SOCKET_INVALID](networking_utils.md#constants). 

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#CreateListenSocketP2P){ .md-button .md-button--doc_classes target="_blank" }

### createListenSocketP2PFakeIP

!!! function "createListenSocketP2PFakeIP( `int` fake_port, `dictionary` config_options )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | fake_port | int | The index of the fake port requested by [beginAsyncRequestFakeIP](#beginasyncrequestfakeip). |
    | config_options | dictionary | A collection of optional, initial config options. |

    Create a listen socket that will listen for P2P connections sent to our FakeIP. A peer can initiate connections to this listen socket by calling [connectByIPAddress](#connectbyipaddress).

	**fake_port** refers to the *index* of the fake port requested, not the actual port number. For example, pass 0 to refer to the first port in the reservation. You must call this only after calling [beginAsyncRequestFakeIP](#beginasyncrequestfakeip). However, you do not need to wait for the request to complete before creating the listen socket.

	Pass your **config_options** as dictionary containing the following key/value pairs:

	* [NetworkingConfigValue](#networkingconfigvalue) (int / enum)
	* config value (int or string)

	Example:
	```gdscript
	{
		NETWORKING_CONFIG_FAKE_PACKET_LAG_SEND : 4,
		NETWORKING_CONFIG_SEND_BUFFER_SIZE: 5000,
		NETWORKING_CONFIG_RECV_BUFFER_SIZE: 3000 
	}
	```

	Alternately you can pass an empty dictionary.

	!!! returns "Returns: uint32_t 

### createPollGroup

!!! function "createPollGroup( )"
	Create a new poll group.

	You should destroy the poll group when you are done using [destroyPollGroup](#destroypollgroup).

	!!! returns "Returns: uint32_t"
		Returns a poll group handle.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#CreatePollGroup){ .md-button .md-button--doc_classes target="_blank" }

### createSocketPair

!!! function "createSocketPair( `bool` loopback, `uint64_t` remote_steam_id1, `uint64_t` remote_steam_id2 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | loopback | bool | Whether or not to send packets through the local network loopback device on ephemeral ports.
    | remote_steam_id1 | uint64_t | The Steam ID of the entity to pair.
    | remote_steam_id2 | uint64_t | The Steam ID of the entity to pair.

    Create a pair of connections that are talking to each other, e.g. a loopback connection. This is very useful for testing, or so that your client/server code can work the same even when you are running a local "server".

	The two connections will immediately be placed into the connected state, and no callbacks will be posted immediately. After this, if you close either connection, the other connection will receive a callback, exactly as if they were communicating over the network. You must close *both* sides in order to fully clean up the resources!

	By default, internal buffers are used, completely bypassing the network, the chopping up of messages into packets, encryption, copying the payload, etc. This means that loopback packets, by default, will not simulate lag or loss. Passing true for **loopback** will cause the socket pair to send packets through the local network loopback device (127.0.0.1) on ephemeral ports. Fake lag and loss are supported in this case, and CPU time is expended to encrypt and decrypt.

	If you use real network loopback, this might be translated to the actual bound loopback port. Otherwise, the port will be zero.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| success | bool | Was the pairing successful?
		| connection1 | int | Connection identity handle.
		| connection2 | int | Connection identity handle.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#CreateSocketPair){ .md-button .md-button--doc_classes target="_blank" }

### destroyPollGroup

!!! function "destroyPollGroup( `uint32_t` poll_group )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | poll_group | uint32_t | The poll group to destroy.

    Destroy a poll group created with [createPollGroup](#createpollgroup).

	If there are any connections in the poll group, they are removed from the group, and left in a state where they are not part of any poll group. Returns false if passed an invalid poll group handle.

	!!! returns "Returns: bool"
		Returns false if passed an invalid poll group handle.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#DestroyPollGroup){ .md-button .md-button--doc_classes target="_blank" }

### flushMessagesOnConnection

!!! function "flushMessagesOnConnection( `uint32_t` connection_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to flush messages for.

	Flush any messages waiting on the Nagle timer and send them at the next transmission opportunity; often that means right now.

	If Nagle is enabled (it's on by default) then when calling [sendMessageToConnection](#sendmessagetoconnection) the message will be buffered, up to the Nagle time before being sent, to merge small messages into the same packet.  [See NETWORKING_CONFIG_NAGLE_TIME](networking_utils.md#networkingconfigvalue).

	!!! returns "Returns: Result enum"

	Possible failures:

	* [RESULT_INVALID_PARAM](main.md#result) - Invalid connection handle.
	* [RESULT_INVALID_STATE](main.md#result) - Connection is in an invalid state.
	* [RESULT_NO_CONNECTION](main.md#result) - Connection has ended.
	* [RESULT_IGNORED](main.md#result) - We weren't yet connected, so this operation has no effect.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#FlushMessagesOnConnection){ .md-button .md-button--doc_classes target="_blank" }

### getAuthenticationStatus

!!! function "getAuthenticationStatus( )"
	Query our readiness to participate in authenticated communications. A [network_authentication_status](#network_authentication_status) callback is posted any time this status changes, but you can use this function to query it at any time.

	We pass NULL internally to only get the high-level status.

	!!! returns "Returns: NetworkingAvailability enum"

### getCertificateRequest

!!! function "getCertificateRequest( )"
	Certificate provision by the application. On Steam, we normally handle all this automatically and you will not need to use these advanced functions.

	Get blob that describes a certificate request. You can send this to your game coordinator. Pass this blob to your game coordinator and call SteamDatagram_CreateCert.

	!!! returns "Returns: dictionary

	Contains the following keys:

	* certificate (int)
	* cert_size (int)
	* error_message (string)

### getConnectionInfo

!!! function "getConnectionInfo( `uint32_t` connection_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to get information for.

	Returns basic information about the high-level state of the connection.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| identity | uint64_t | Who is on the other end?  Depending on the connection type and phase of the connection, we might not know.
		| user_data | uint64_t | Arbitrary user data set by the local application code.
		| listen_socket | uint32_t | Handle to listen socket this was connected on, or [LISTEN_SOCKET_INVALID](networking_utils.md#constants) if we initiated the connection.
		| remote_address | string | Remote address.  Might be all 0's if we don't know it or if this is N/A.
		| remote_pop | uint32_t | What data center is the remote host in?  0 if we don't know.
		| pop_relay | uint32_t | What relay are we using to communicate with the remote host?  0 if not applicable.
		| connection_state | [NetworkingConnectionState enum](networking_utils.md#networkingconnectionstate) | High level state of the connection.
		| end_reason | [NetworkingConnectionEnd enum](networking_utils.md#networkingconnectionend) | Basic cause of the connection termination or problem.
		| end_debug | string | Human-readable, but non-localized explanation for connection termination or problem.  This is intended for debugging / diagnostic purposes only, not to display to users.  It might have some details specific to the issue.
		| debug_description | string | Debug description.  This includes the internal connection ID, connection type (and peer information), and any name given to the connection by the app.  This string is used in various internal logging messages. Note that the connection ID **usually** matches the network connection handle, but in certain cases with symmetric connections it might not.
		| info_flags | int | Misc flags.  Bitmask of [NETWORKING_CONNECTION_INFO_FLAG_](networking_utils.md#constants).

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#GetConnectionInfo){ .md-button .md-button--doc_classes target="_blank" }

### getConnectionName

!!! function "getConnectionName( `int` connection_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to flush messages for. |

	Fetch connection name into your buffer.

	!!! returns "Returns: string"
		Returns empty if the **connection_handle** is invalid or no name is set.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#GetConnectionName){ .md-button .md-button--doc_classes target="_blank" }

### getConnectionRealTimeStatus

!!! function "GetConnectionRealTimeStatus( `uint32_t` connection_handle, `int` lanes, `bool` get_status )" 
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to flush messages for.
    | lanes | int | The number of lanes to get connection information on.
    | get_status | bool | Whether or not to get the connection status data. Defaults to true.

	Returns a small set of information about the real-time state of the connection and the queue status of each lane.

	**get_status** may be false if the information is not desired; eg. you are only interested in the lane information.

	On entry, **lanes** specifies the length of the pLanes array. This may be 0 if you do not wish to receive any lane data. It's OK for this to be smaller than the total number of configured lanes.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| connection_status | dictionary | Only returned if **get_status** is true.
		| lanes_status | array | Data about each lane's status.

		**connection_status** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| state | [NetworkingConnectionState enum](networking_utils.md#networkingconnectionstate) | High level state of the connection.
		| ping | int | Current ping (ms).
		| local_quality | float | Connection quality measured locally, 0 to 1. Percentage of packets delivered end-to-end in order.
		| remote_quality | float | Packet delivery success rate as observed from remote host.
		| packets_out_per_second | float | Current data rates from recent history.
		| bytes_out_per_second | float | Current data rates from recent history.
		| packets_in_per_second | float | Current data rates from recent history.
		| bytes_in_per_second | float | Current data rates from recent history.
		| send_rate | int | Estimate rate that we believe that we can send data to our peer. Note that this could be significantly higher than **bytes_out_per_second**, meaning the capacity of the channel is higher than you are sending data.
		| pending_unreliable | int | Number of bytes pending to be sent.  This is data that you have recently requested to be sent but has not yet actually been put on the wire.
		| pending_reliable | int | This reliable number also includes data that was previously placed on the wire, but has now been scheduled for re-transmission.  Thus, it's possible to observe this increasing between two checks, even if no calls were made to send reliable data between the checks.  Data that is awaiting the Nagle delay will appear in these numbers.
		| send_unacknowledged_reliable | int | Number of bytes of reliable data that has been placed the wire, but for which we have not yet received an acknowledgment, and thus we may have to re-transmit.
		| queue_time | uint64_t | If you queued a message right now, approximately how long would that message wait in the queue before we actually started putting its data on the wire in a packet?

		**lanes_status** array contains dictionaries which each contain the following keys:
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| pending_unreliable | int | Counters for this particular lane.
		| pending_reliable | int | Counters for this particular lane.
		| sent_unacknowledged_reliatble | int | Counters for this particular lane.
		| queue_time | uint64_t | Lane-specific queue time.  This value takes into consideration lane priorities and weights, and how much data is queued in each lane, and attempts to predict how any data currently queued will be sent out.

	!!! info "Notes"
		**queue_time** - In general, data that is sent by the application is limited by the bandwidth of the channel.  If you send data faster than this, it must be queued and put on the wire at a metered rate.  Even sending a small amount of data; eg. a few MTU, say ~3k) will require some of the data to be delayed a bit.

		Ignoring multiple lanes, the estimated delay will be approximately equal to ( **pending_unreliable** + **pending_reliable** ) / **send_rate** plus or minus one MTU.  It depends on how much time has elapsed since the last packet was put on the wire.  For example, the queue might have *just* been emptied, and the last packet placed on the wire, and we are exactly up against the send rate limit.  In that case we might need to wait for one packet's worth of time to elapse before we can send again.  On the other extreme, the queue might have data in it waiting for Nagle.  This will always be less than one packet because as soon as we have a complete packet we would send it.  In that case, we might be ready to send data now, and this value will be 0.

		This value is only valid if multiple lanes are not used.  If multiple lanes are in use, then the queue time will be different for each lane, and you must use the value in SteamNetConnectionRealTimeLaneStatus_t.

		Nagle delay is ignored for the purposes of this calculation.

### getConnectionUserData

!!! function "getConnectionUserData( `uint32_t` connection_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to get user data for.

    Fetch connection user data.

	!!! returns "Returns: uint64_t"
		Returns -1 if handle is invalid or if you haven't set any user data on the connection.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#GetConnectionUserData){ .md-button .md-button--doc_classes target="_blank" }

### getDetailedConnectionStatus

!!! function "getDetailedConnectionStatus( `uint32_t` connection_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to get detailed connection data for.

    Returns very detailed connection stats in diagnostic text format. Useful for dumping to a log, etc. The format of this information is subject to change.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| success | int | -1 failure for bad connection handle or 0 for OK, your buffer was filled in.
		| buffer | string | The verbose connection stats.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#GetDetailedConnectionStatus){ .md-button .md-button--doc_classes target="_blank" }

### getFakeIP

!!! function "getFakeIP( `int` first_port )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | first_port | int | Must always be 0. |

    Return info about the FakeIP and port(s) that we have been assigned, if any.

	**first_port** is currently reserved and must be zero. Make sure and check **result**.

	!!! returns "Returns: dictionary

	Contains the following keys:

	* result (int)
	* identity_type (int)
	* ip (string)
	* ports (uint16)

### getGameCoordinatorServerLogin

!!! function "getGameCoordinatorServerLogin( )"
	Generate an authentication blob that can be used to securely login with your backend, using SteamDatagram_ParseHostedServerLogin. (See steamdatagram_gamecoordinator.h)

	**Currently not enabled.**

	!!! returns "Returns: int

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#GetGameCoordinatorServerLogin){ .md-button .md-button--doc_classes target="_blank" }	

### getHostedDedicatedServerAddress

!!! function "getHostedDedicatedServerAddress( )"
	Return info about the hosted server. This contains the PoPID of the server, and opaque routing information that can be used by the relays to send traffic to your server.

	**Currently not enabled.**

	!!! returns "Returns: int

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#GetHostedDedicatedServerAddress){ .md-button .md-button--doc_classes target="_blank" }

### getHostedDedicatedServerPOPId

!!! function "getHostedDedicatedServerPOPId( )"
	Returns 0 if SDR_LISTEN_PORT is not set. Otherwise, returns the data center the server is running in. This will be k_SteamDatagramPOPID_dev in non-production envirionment.

	!!! returns "Returns: uint32_t

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#GetHostedDedicatedServerPOPID){ .md-button .md-button--doc_classes target="_blank" }

### getHostedDedicatedServerPort

!!! function "getHostedDedicatedServerPort( )"
	Returns the value of the SDR_LISTEN_PORT environment variable. This is the UDP server your server will be listening on. This will configured automatically for you in production environments.

	!!! returns "Returns: uint16"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#GetHostedDedicatedServerPort){ .md-button .md-button--doc_classes target="_blank" }

### getListenSocketAddress

!!! function "getListenSocketAddress( `uint32_t` socket, `bool` with_port )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | socket | uint32_t | The listen socket to get the IP address from.
    | with_port | bool | Whether or not to return the port as well.

	Returns local IP and port that a listen socket created using [CreateListenSocketIP](#createlistensocketip) is bound to. The `with_port` argument defaults to true.

	!!! returns "Returns: string"

	!!! info "Notes"
		This is not how you find out your public IP that clients can connect to.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#GetListenSocketAddress){ .md-button .md-button--doc_classes target="_blank" }

### getIdentity

!!! function "getIdentity( )"
	Get the identity assigned to this interface.

	E.g. on Steam, this is the user's Steam ID, or for the gameserver interface, the Steam ID assigned to the gameserver. Returns false and sets the result to an invalid identity if we don't know our identity yet. (E.g. GameServer has not logged in. On Steam, the user will know their SteamID even if they are not signed into Steam.)

	!!! returns "Returns: string"

	**Notes:** Was removed in GodotSteam 3.25 / 4.8.

### getRemoteFakeIPForConnection

!!! function "getRemoteFakeIPForConnection( `uint32_t` connection_handle )" 
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to get detailed connection data for. |

    If the connection was initiated using the "FakeIP" system, then we we can get an IP address for the remote host. If the remote host had a global FakeIP at the time the connection was established, this function will return that global IP.

	Otherwise, a FakeIP that is unique locally will be allocated from the local FakeIP address space, and that will be returned.

	This should also add the returning struct to your **ip_addresses** vector as **fake_ip_address**.

	!!! returns "Returns: dictionary

	Contains the following keys:

	* result (int)
	* ip_address (string)
	* port (uint16)
	* ip_type (int)

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#GetIdentity){ .md-button .md-button--doc_classes target="_blank" }		

### initAuthentication

!!! function "initAuthentication( )"
	Indicate our desire to be ready participate in authenticated communications.  If we are currently not ready, then steps will be taken to obtain the necessary certificates.  This includes a certificate for us, as well as any CA certificates needed to authenticate peers.

	You can call this at program init time if you know that you are going to be making authenticated connections, so that we will be ready immediately when those connections are attempted.  Note that essentially all connections require authentication, with the exception of ordinary UDP connections with authentication disabled using [NETWORKING_CONFIG_IP_ALLOW_WITHOUT_AUTH](networking_utils.md#networkingconfigvalue).  If you don't call this function, we will wait until a feature is utilized that that necessitates these resources.

	You can also call this function to force a retry, if failure has occurred.  Once we make an attempt and fail, we will not automatically retry.  In this respect, the behavior of the system after trying and failing is the same as before the first attempt: attempting authenticated communication or calling this function will call the system to attempt to acquire the necessary resources.

	You can use [getAuthenticationStatus](#getauthenticationstatus) or listen for [network_authentication_status](#network_authentication_status) to monitor the status.

	!!! returns "Returns: NetworkingAvailability enum"
		Returns the current value that would be returned from [getAuthenticationStatus](#getauthenticationstatus).

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#InitAuthentication){ .md-button .md-button--doc_classes target="_blank" }

### receiveMessagesOnConnection

!!! function "receiveMessagesOnConnection( `uint32_t` connection_handle, `int` max_messages )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to get messages for.
    | max_messages | int | The maximum amount of messages to retrieve.

	Fetch the next available message(s) from the connection, if any.  Returns the number of messages returned into your array, up to **max_messages**.  If the connection handle is invalid, -1 is returned.  If no data is available, 0 is returned.

	The order of the messages returned in the array is relevant.  Reliable messages will be received in the order they were sent and with the same sizes; see [sendMessageToConnection](#sendmessagetoconnection) for on this subtle difference from a stream socket.

	Unreliable messages may be dropped or delivered out of order with respect to each other or with respect to reliable messages.  The same unreliable message may be received multiple times.

	!!! returns "Returns: array of dictionaries"
		Each contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| payload | string | Message payload.
		| size | int | Size of the payload.
		| connection | int | What connection did this come from?
		| identity | uint64_t | Who sent this to us?
		| receiver_user_data | uint64_t | This is the user data associated with the connection.
		| time_received | uint64_t | Local timestamp when the message was received.
		| message_number | uint64_t | Message number assigned by the sender. Note that if multiple lanes are used, each lane has its own message numbers, which are assigned sequentially, so messages from different lanes will share the same numbers.
		| flags | int | Bitmask of k_nSteamNetworkingSend_xxx flags, only the [NETWORKING_SEND_RELIABLE](networking_utils.md#constants) bit is valid.

		**receiver_user_data** is *usually* the same as calling [getConnection](#getconnection) and then fetching the user data associated with that connection, but for the following subtle differences:

		* This user data will match the connection's user data at the time is captured at the time the message is returned by the API.  If you subsequently change the userdata on the connection, this won't be updated.
		* This is an inline call, so it's *much* faster.
		* You might have closed the connection, so fetching the user data would not be possible.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#ReceiveMessagesOnConnection){ .md-button .md-button--doc_classes target="_blank" }

### receiveMessagesOnPollGroup

!!! function "receiveMessagesOnPollGroup( `uint32_t` poll_group, `int` max_messages )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | poll_group | uint32_t | The poll group to get messages for. |
    | max_messages | int | The maximum amount of messages to retrieve. |

	Same as [receiveMessagesOnConnection](#receivemessagesonconnection), but will return the next messages available on any connection in the poll group. Examine **connection** to know which connection. **user_data** might also be useful.

	Delivery order of messages among different connections will usually match the order that the last packet was received which completed the message.  But this is not a strong guarantee, especially for packets received right as a connection is being assigned to poll group.

	Delivery order of messages on the same connection is well defined and the same guarantees are present as mentioned in ReceiveMessagesOnConnection.  But the messages are not grouped by connection, so they will not necessarily appear consecutively in the list; they may be interleaved with messages for other connections.

	!!! returns "Returns: array of dictionaries"
		Each contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| payload | string | Message payload.
		| size | int | Size of the payload.
		| connection | int | What connection did this come from?
		| identity | uint64_t | Who sent this to us?
		| receiver_user_data | uint64_t | This is the user data associated with the connection.
		| time_received | uint64_t | Local timestamp when the message was received.
		| message_number | uint64_t | Message number assigned by the sender. Note that if multiple lanes are used, each lane has its own message numbers, which are assigned sequentially, so messages from different lanes will share the same numbers.
		| flags | int | Bitmask of k_nSteamNetworkingSend_xxx flags, only the [NETWORKING_SEND_RELIABLE](networking_utils.md#constants) bit is valid.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#ReceiveMessagesOnPollGroup){ .md-button .md-button--doc_classes target="_blank" }

### resetIdentity

!!! function "resetIdentity( `uint64_t` remote_steam_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | remote_steam_id | uint64_t | The Steam ID to reset the identity. |

	Reset the identity associated with this instance. Any open connections are closed. Any previous certificates, etc are discarded.

	**Note:** This function is not actually supported on Steam!  It is included for use on other platforms where the active user can sign out and a new user can sign in.

	!!! returns "Returns: void

### runNetworkingCallbacks

!!! function "runNetworkingCallbacks( )"
	Invoke all callback functions queued for this interface. You don't need to call this if you are using Steam's callback dispatch mechanism [run_callbacks](main.md#run_callbacks).

	!!! returns "Returns: void

### sendMessages

!!! function "sendMessages( `Array` messages, `uint32_t` connection_handle, `int` flags )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | messages | Array | An array of message data to send.
    | connection_handle | uint32_t | The network connection to send messages to.
    | flags | int | Bitmask of Networking Send flags.

	Send one or more messages without copying the message payload. This is the most efficient way to send messages.

	The library will take ownership of the message structures.  They may be modified or become invalid at any time, so you must not read them after passing them to this function.

	!!! returns "Returns: array""

	The array contains the **message number** that was assigned to the message if sending was successful.  If sending failed, then a negative [Result value](main.md#result) is placed into the array.  For example, the array will hold
	RESULT_INVALID_STATE / -k_EResultInvalidState if the connection was in an invalid state.

	Possible failure codes include:

	* [RESULT_INVALID_PARAM](main.md#result) / k_EResultInvalidParam
		- invalid connection handle, or the individual message is too big; more than MAX_STEAM_PACKET_SIZE / k_cbMaxSteamNetworkingSocketsMessageSizeSend or 512 * 1024.
	* [RESULT_INVALID_STATE](main.md#result) / k_EResultInvalidState
		- Connection is in an invalid state
	* [RESULT_NO_CONNECTION](main.md#result) / k_EResultNoConnection
		- Connection has ended
	* [RESULT_IGNORED](main.md#result) / k_EResultIgnored
		- You used k_nSteamNetworkingSend_NoDelay, and the message was dropped because we were not ready to send it.
	* [RESULT_LIMIT_EXCEEDED](main.md#result) / k_EResultLimitExceeded

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#SendMessages){ .md-button .md-button--doc_classes target="_blank" }

### sendMessageToConnection

!!! function "sendMessageToConnection( `uint32_t` connection_handle, `PackedByteArray` data, `int` flags )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to send messages to.
    | data | PackedByteArray | The message to send.
    | flags | int | Bitmask of Networking Send flags.

    Send a message to the remote host on the specified connection.

    **flags** determines the delivery guarantees that will be provided, when data should be buffered, etc; like [NETWORKING_SEND_UNRELIABLE](networking_utils.md#constants)

	Note that the semantics we use for messages are not precisely the same as the semantics of a standard "stream" socket. (SOCK_STREAM) For an ordinary stream socket, the boundaries between chunks are not considered relevant, and the sizes of the chunks of data written will not necessarily match up to the sizes of the chunks that are returned by the reads on the other end.  The remote host might read a partial chunk or chunks might be coalesced.  For the message semantics used here, however, the sizes **will** match.  Each send call will match a successful read call on the remote host one-for-one.

	If you are porting existing stream-oriented code to the semantics of reliable messages, your code should work the same, since reliable message semantics are more strict than stream semantics.  The only caveat is related to performance: there is per-message overhead to retain the message sizes, and so if your code sends many small chunks of data, performance will suffer. Any code based on stream sockets that does not write excessively small chunks will work without any changes. 

	!!! returns "Returns: dictionary"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [Result enum](main.md#result) | The message result.
		| message_number | uint64_t | The message number assigned to the message, if sending was successful.

		Result could be:

		* [RESULT_INVALID_PARAM](main.md#result) - invalid connection handle, or the individual message is too big; more than [MAX_STEAM_PACKET_SIZE](#constants).
		* [RESULT_INVALID_STATE](main.md#result) - connection is in an invalid state
		* [RESULT_NO_CONNECTION](main.md#result) - connection has ended
		* [RESULT_IGNORED](main.md#result) - You used [NETWORKING_SEND_NO_DELAY](networking_utils.md#constants) and the message was dropped because we were not ready to send it.
		* [RESULT_LIMIT_EXCEEDED](main.md#result) - There was already too much data queued to be sent; see [NETWORKING_CONFIG_SEND_BUFFER_SIZE](networking_utils.md#networkingconfigvalue).

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#SendMessageToConnection){ .md-button .md-button--doc_classes target="_blank" }

### setConnectionName

!!! function "setConnectionName( `uint32_t` peer, `string` name )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to name.
    | name | string | The name to apply to this network connection.

    Set a name for the connection; used mostly for debugging.

	!!! returns "Returns: void"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#SetConnectionName){ .md-button .md-button--doc_classes target="_blank" }

### setConnectionPollGroup

!!! function "setConnectionPollGroup( `uint32_t` connection_handle, `uint32_t` poll_group )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to assign to the poll group.
    | poll_group | uint32_t | The poll group to add the connection to.

    Assign a connection to a poll group. Note that a connection may only belong to a single poll group. Adding a connection to a poll group implicitly removes it from any other poll group it is in.

	You can pass [NET_POLL_GROUP_INVALID](#constants) to remove a connection from its current poll group without adding it to a new poll group.

	If there are received messages currently pending on the connection, an attempt is made to add them to the queue of messages for the poll group in approximately the order that would have applied if the connection was already part of the poll group at the time that the messages were received.

	!!! returns "Returns: bool"
		Returns false if the connection handle is invalid or if the poll group handle is invalid and not [NET_POLL_GROUP_INVALID](#constants).

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#SetConnectionPollGroup){ .md-button .md-button--doc_classes target="_blank" }

### setConnectionUserData

!!! function "setConnectionUserData( `uint32_t` connection_handle, `int64_t` user_data )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | connection_handle | uint32_t | The network connection to assign to the poll group.
    | user_data | int64_t | -

    Set connection user data.  The data is returned in the following places:

	* You can query it using [getConnectionUserData](#getconnectionuserdata).
	* The SteamNetworkingmessage_t structure.
	* The SteamNetConnectionInfo_t structure, which is a member of [network_connection_status_changed](#network_connection_status_changed) but see warning below.

	Do you need to set this atomically when the connection is created? [See 'Connection User Data'](networking_utils.md#connection_user_data).

	**Warning**: Be **very careful** when using the value provided in callbacks structs.  Callbacks are queued, and the value that you will receive in your callback is the userdata that was effective at the time the callback was queued.  There are subtle race conditions that can happen if you don't understand this!

	If any incoming messages for this connection are queued, the user data field is updated, so that when when you receive messages (eg. with [receiveMessagesOnConnection](#receivemessagesonconnection)), they will always have the very latest user data.  So the tricky race conditions that can happen with callbacks do not apply to retrieving messages.

	!!! returns "Returns: bool"
		Returns true on success; otherwise, false if the handle is invalid.

{==
## :material-signal: Signals
==}

These callbacks require you to run `Steam.run_callbacks()` in your `_process()` function to receive them.

### fake_ip_result

!!! function "fake_ip_result"
	A struct used to describe a "fake IP" we have been assigned to use as an identifier. This callback is posted when [beginAsyncRequestFakeIP](#beginasyncrequestfakeip) completes.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [Result enum](main.md#result) | Status / result of the allocation request.
		| identity | uint64_t | Local identity of the Networking Sockets object that made this request and is assigned the IP.  This is needed in the callback in the case where there are multiple Networking Sockets objects; eg. one for the user and another for the local gameserver.
		| fake_ip | string | Fake IPv4 IP address that we have been assigned.  This IP address is not exclusively ours!  Steam tries to avoid sharing IP addresses, but this may not always be possible.  The IP address may be currently in use by another host, but with different port(s). The exact same IP:port address may have been used previously.  Steam tries to avoid reusing ports until they have not been in use for some time, but this may not always be possible.
		| port_list | array | Port number(s) assigned to us.  Only the first entries will contain nonzero values.  Entries corresponding to ports beyond what was allocated for you will be zero. At the time of this writing, the maximum number of ports you may request is 4.

		Possible failuue values are:

		* [RESULT_BUSY](main.md#result) - You called GetFakeIP but the request has not completed.
		* [RESULT_INVALID_PARAM](main.md#result) - You called [getFakeIP](#getfakeip) with an invalid port index.
		* [RESULT_LIMIT_EXCEEDED](main.md#result) - You asked for too many ports or made an additional request after one had already succeeded.
		* [RESULT_NO_MATCH](main.md#result) - [getFakeIP](#getfakeip) was called but no request has been made.

		With the exception of [RESULT_BUSY](main.md#result) if you are polling, it is highly recommended to treat all failures as fatal.

### network_authentication_status

!!! function "network_authentication_status"
	This callback is posted whenever the state of our readiness changes.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| available | [NetworkingAvailability enum](networking_utils.md#networkingavailability) | The authentication status.
		| debug_message | string | Non-localized English language status.  For diagnostic/debugging purposes only.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#SteamNetAuthenticationStatus_t){ .md-button .md-button--doc_classes target="_blank" }

### network_connection_status_changed

!!! function "network_connection_status_changed"
	This callback is posted whenever a connection is created, destroyed, or changes state. The m_info field will contain a complete description of the connection at the time the change occurred and the callback was posted. In particular, **connection_state** will have the new connection state.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | connect_handle | uint64_t | The connection handle.
		| connection | dictionary | Data about the connection status.
		| old_state | int | The previous connection state.

		**connection** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| identity | uint64_t | Who is on the other end?  Depending on the connection type and phase of the connection, we might not know.
		| user_data | uint64_t | Arbitrary user data set by the local application code.
		| listen_socket | uint32_t | Handle to listen socket this was connected on, or [LISTEN_SOCKET_INVALID](networking_utils.md#constants) if we initiated the connection
		| remote_address | string | Remote address.  Might be all 0's if we don't know it or if this is N/A.
		| remote_pop | uint32_t | What data center is the remote host in?  0 if we don't know.
		| pop_relay | uint32_t | What relay are we using to communicate with the remote host?  0 if not applicable.
		| connection_state | [NetworkingConnectionState enum](networking_utils.md#networkingconnectionstate) | High level state of the connection
		| end_reason | [NetworkingConnectionEnd enum](networking_utils.md#networkingconnectionend) | Basic cause of the connection termination or problem. See ESteamNetConnectionEnd for the values used.
		| end_debug | string | Human-readable, but non-localized explanation for connection termination or problem.  This is intended for debugging / diagnostic purposes only, not to display to users.  It might have some details specific to the issue.
		| debug_description | string | Debug description.  This includes the connection handle, connection type (and peer information), and the app name.  This string is used in various internal logging messages.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamNetworkingSockets#SteamNetConnectionStatusChangedCallback_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
DATAGRAM_POP_ID_DEV | k_SteamDatagramPOPID_dev | ( (uint32)'d' << 16U ) | ( (uint32)'e' << 8U ) | (uint32)'v' | The POPID "dev" is used in non-production environments for testing.
MAX_STEAM_PACKET_SIZE | k_cbMaxSteamNetworkingSocketsMessageSizeSend | 512 * 1024 | Max size of a single message that we can send. We might be willing to receive larger messages and our peer might, too.
NET_POLL_GROUP_INVALID | k_HSteamNetPollGroup_Invalid | 0 | Handle used to identify a poll group; used to query many connections at once efficiently.
NETWORKING_SOCKETS_FAKE_UDP_PORT_RECOMMENDED_MTU | k_cbSteamNetworkingSocketsFakeUDPPortRecommendedMTU | 1200 | It is HIGHLY recommended to limit messages sent via Fake UDP port to this value.  The purpose of a Fake UDP port is to make porting ordinary ad-hoc UDP code easier.  Although the real MTU might be higher than this, this particular conservative value is chosen so that fragmentation won't be occurring and hiding performance problems from you.
NETWORKING_SOCKETS_FAKE_UDP_PORT_MAX_MESSAGE_SIZE | k_cbSteamNetworkingSocketsFakeUDPPortMaxMessageSize | 4096 | Messages larger than this size are not allowed and cannot be sent via Fake UDP port.