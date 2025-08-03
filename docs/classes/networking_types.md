---
title: Networking Types
description: A class reference for Networking Types.
icon: material/table-network
---

# Networking Types

Miscellaneous types and functions used by networking APIs. These are part of the newer networking classes; not to be confused with the [older, now-deprecated Networking class](networking.md).

These Networking Type functions are all unique to GodotSteam since we cannot work with C++ structs directly in GDscript. These will create networking identities to use with [Networking Messages](networking_messages.md), [Networking Sockets](networking_sockets.md), and [Networking Utils](networking_utils.md) classes. Much like how it works in a C++ implementation, the struct must be created (with either [addIdentity](#addidentity) or [addIPAddress](#addipaddress)) then it must be populated with data (Steam ID, IP address, etc.).

For more information on using Networking Types for [Networking Messages](networking_messages.md), look over the _on_network_messages_session_request function_ in the [Networking Tutorial](https://github.com/GodotSteam/GodotSteam-Example-Project/blob/godot3/example/src/networking.gd) to understand the flow of addIdentity, setIdentitySteamID64, & acceptSessionWithUser before you call your Networking Messages functions.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } and [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

!!! warning "This class no longer exists in GodotSteam as of versions 3.25 / 4.8"

{==
## :material-function-variant: Functions
==}

### addIdentity

!!! function "addIdentity( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The name to give this newly created identity.

	Create a new network identity struct and store it for use. When this network identity is used in other functions, you will always use the reference_name to use this struct.

	You will have to set the IP, Steam ID, string, or bytes with other functions below otherwise the identity is invalid.

	!!! returns "Returns: bool"

### addIPAddress

!!! function "addIPAddress( `string` refrence_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The name to give this newly created IP address struct.

	Add a new IP address struct and store it for use. When this networking IP address is used in other functions, you will always use the `reference_name` to use this struct.

	!!! returns "Returns: bool"

### clearIPAddress

!!! function "clearIPAddress( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The IP address to clear.

	IP Address - Set everything to zero; eg. [::]:0.

	!!! returns "Returns: void"

### clearIdentity

!!! function "clearIdentity( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to clear.

	Clear a network identity's data.

	!!! returns "Returns: void"

### getGenericBytes

!!! function "getGenericBytes( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to get set bytes data for.

	Returns null if not generic bytes type.

	!!! returns "Returns: uint8"

### getGenericString

!!! function "getGenericString( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to get set string data for.

	Returns null if not generic string type.

	!!! returns "Returns: string"

### getIdentities

!!! function "getIdentities( )"
	Get a list of all known network identities.

	!!! returns "Returns: array of dictionaries"
		Each contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| reference_name | string |The identity name.
		| steam_id | uint64_t | The Steam ID for this identity.
		| type | int | The type of identity this is.

### getIdentityIPAddr

!!! function "getIdentityIPAddr( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to get the set IP address for.

	Returns null if we are not an IP address.

	!!! returns "Returns: uint32_t"

### getIdentitySteamID

!!! function "getIdentitySteamID( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to get the set Steam ID for.

	Return back a Steam ID32 or 0 if identity is not a Steam ID.

	!!! returns "Returns: uint32_t"

### getIdentitySteamID64

!!! function "getIdentitySteamID64( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to get the set Steam ID64 for. |

	Returns 0 if identity is not a Steam ID.

	!!! returns "Returns: uint64_t"

### getIPAddresses

!!! function "getIPAddresses( )"
	Get a list of all IP address structs and their names.

	!!! returns "Returns: array of dictionaries"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| reference_name | string | The reference name of this IP address.
		| localhost | bool | Whether this is the localhost or not.
		| ip_address | uint32_t | The IP address associated.

### getIPv4

!!! function "getIPv4( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to get the set IPv4 for.

	Returns IP in host byte order (e.g. aa.bb.cc.dd as 0xaabbccdd). Returns 0 if IP is not mapped IPv4.

	!!! returns "Returns: uint32_t"

### getPSNID

!!! function "getPSNID( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to get the set Playstation Network ID for.

	Retrieve this identity's Playstation Network ID.

	!!! returns "Returns: uint64_t"

### getStadiaID

!!! function "getStadiaID( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to get the set Stadia ID for.

	Retrieve this identity's Google Stadia ID.

	!!! returns "Returns: uint64_t"

### getXboxPairwiseID

!!! function "getXboxPairwiseID( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to get the set XBox Pairwise ID for.

	Retrieve this identity's XBox pair ID.

	!!! returns "Returns: string"

### isAddressLocalHost

!!! function "isAddressLocalHost( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to check.

	Return true if this identity is localhost. (Either IPv6 ::1, or IPv4 127.0.0.1).

	!!! returns "Returns: bool"

### isIdentityInvalid

!!! function "isIdentityInvalid( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to check.

	Return true if we are the invalid type. Does not make any other validity checks (e.g. is SteamID actually valid).

	!!! returns "Returns: bool"

### isIdentityLocalHost

!!! function "isIdentityLocalHost( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to check.

	Return true if this identity is localhost.

	!!! returns "Returns: bool"

### isIPv4

!!! function "isIPv4( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to check.

	Return true if IP is mapped IPv4.

	!!! returns "Returns: bool"

### isIPv6AllZeros

!!! function "isIPv6AllZeros( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to check.

	Return true if the IP is ::0. (Doesn't check port.).

	!!! returns "Returns: bool"

### parseIPAddressString

!!! function "parseIPAddressString( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The IP address struct to parse.

	Parse an IP address and optional port. If a port is not present, it is set to 0. (This means that you cannot tell if a zero port was explicitly specified.).

	!!! returns "Returns: string"

### parseIdentityString

!!! function "parseIdentityString( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to parse.

	Parse back a string that was generated using ToString. If we don't understand the string, but it looks "reasonable" (it matches the pattern `type:<type-data>` and doesn't have any funky characters, etc), then we will return true, and the type is set to k_ESteamNetworkingIdentityType_UnknownType (0). False will only be returned if the string looks invalid.

	!!! returns "Returns: string"

### setGenericBytes

!!! function "setGenericBytes( `string` reference_name, `uint8` data )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.
    | data | uint8 | The generic byte data to set.

	Returns false if invalid size.

	!!! returns "Returns: bool"

### setIdentityIPAddr

!!! function "setIdentityIPAddr( `string` reference_name, `string` ip_address_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.
    | ip_address_name | string | The IP address to set.

	Set to specified IP:port.

	!!! returns "Returns: bool"

### setIdentityLocalHost

!!! function "setIdentityLocalHost( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.

	Set to localhost. (We always use IPv6 ::1 for this, not 127.0.0.1).

	!!! returns "Returns: void"

### setIdentitySteamID

!!! function "setIdentitySteamID( `string` reference_name, `uint32_t` steam_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.
    | steam_id | uint32_t | The Steam ID32 to set.

	Set a 32-bit Steam ID.

	!!! returns "Returns: void"

### setIdentitySteamID64

!!! function "setIdentitySteamID64( `string` reference_name, `uint64_t` steam_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.
    | steam_id | uint64_t | The Steam ID64 to set.

	Takes SteamID as raw 64-bit number.
	
	!!! returns "Returns: void"

### setGenericString

!!! function "setGenericString( `string` reference_name, `string` this_string )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.
    | this_string | string | The generic string data to set.

    Returns false if invalid length.

	!!! returns "Returns: bool"

### setPSNID

!!! function "setPSNID( `string` reference_name, `uint64_t` psn_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.
    | psn_id | uint64_t | The Playstation Network ID to set.

	Set the Playstation Network ID for this identity.

	!!! returns "Returns: void"

### setStadiaID

!!! function "setStadiaID( `string` reference_name, `uint64_t` stadia_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.
    | stadia_id | uint64_t | The Stadia ID to set.

	Set the Google Stadia ID for this identity.

	!!! returns "Returns: void"

### setXboxPairwiseID

!!! function "setXboxPairwiseID( `string` reference_name, `string` xbox_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.
    | xbox_id | uint64_t | The XBox Pairwise ID to set.

	Set the Xbox Pairwise ID for this identity.

	!!! returns "Returns: bool"

### toIdentityString

!!! function "toIdentityString( `string` reference_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to get the string for.

	Print to a human-readable string. This is suitable for debug messages or any other time you need to encode the identity as a string. It has a URL-like format `type:<type-data>`. Your buffer should be at least k_cchMaxString (128) bytes big to avoid truncation.

	!!! returns "Returns: void"

### toIPAddressString

!!! function "toIPAddressString( `string` reference_name, `bool` with_port )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The IP address to get the string for.
    | with_port | bool | Whether or not to get the port as well.

	Print to a string, with or without the port. Mapped IPv4 addresses are printed as dotted decimal (12.34.56.78), otherwise this will print the canonical form according to RFC5952. If you include the port, IPv6 will be surrounded by brackets, e.g. [::1:2]:80. Your buffer should be at least k_cchMaxString (128) bytes to avoid truncation.

	!!! returns "Returns: string"

### setIPv4

!!! function "setIPv4( `string` reference_name, `uint32` ip, `uint16` port )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.
    | ip | uint32 | The IPv4 address to set.
    | port | uint16 | The port to set.

    Sets to IPv4 mapped address. IP and port are in host byte order.

	!!! returns "Returns: void"

### setIPv6

!!! function "setIPv6( `string` reference_name, `uint8` ipv6, `uint16` port )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.
    | ipv6 | uint8 | The IPv6 address to set.
    | port | uint16 | The port to set.

    Set IPv6 address. IP is interpreted as bytes, so there are no endian issues. (Same as inaddr_in6.) The IP can be a mapped IPv4 address.

	!!! returns "Returns: void"

### setIPv6LocalHost

!!! function "setIPv6LocalHost( `string` reference_name, `uint16` port )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | reference_name | string | The identity to update.
    | port | uint16 | The port to set.

    Set to the IPv6 localhost address ::1, and the specified port.

	!!! returns "Returns: void"

