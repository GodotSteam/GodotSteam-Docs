---
title: Parties
description: A class reference for Steam Parties.
icon: material/party-popper
---

# Parties

This API can be used to selectively advertise your multiplayer game session in a Steam chat room group. Tell Steam the number of player spots that are available for your party and give a join-game string. It will then show a beacon in the selected group and allow that many users to “follow” the beacon to your party. Adjust the number of open slots if other players join through alternate matchmaking methods.

For example, you can use Steam Parties in conjunction with a private lobby. Create a private lobby, and then use [createBeacon](#createbeacon) to create a party "beacon" for the number of players desired. The game connect string should indicate the ID of the private lobby.

The beacon will appear in Steam in the specified location (e.g. a Chat Room Group) and also via the in-game ISteamParties API as described below. Steam creates "reservation" slots for the number of desired players. Whenever a user follows the beacon, Steam will hold a reservation slot for them and launch the game using the given connect string.

The game session that created the beacon will be notified of this reservation, so the game can display the appropriate "User <username> is joining your party" or some other indicator. Once the user joins successfully, the game session should call [onReservationCompleted](#onreservationcompleted) to tell Steam that the user successfully joined (otherwise, Steam will eventually timeout their reservation and re-open the slot).

When all of the beacon slots are occupied, either by reservations for users still launching the game or completed slots for users in the party, Steam will hide and disable the beacon.

To cancel the beacon, for instance when the party is full and the game begins, call [destroyBeacon](#destroybeacon).

The client side of this operation, seeing and following beacons, can also be managed by your game. Using [getNumActiveBeacons](#getnumactivebeacons) and [getBeaconDetails](#getbeacondetails), your game can get a list of beacons from other users that are currently active in locations relevant to the current user. If the user desires, call [joinParty](#joinparty) to "follow" one of those beacons.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### cancelReservation

!!! function "cancelReservation( `uint64_t` beacon_id, `uint64_t` steam_id )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | beacon_id | uint64_t | Beacon ID for the beacon created by your process. |
    | steam_id | uint64_t | Steam ID of the user this reservation was for. |

    To cancel a reservation (due to timeout or user input), call this. Steam will open a new reservation slot.

	**Note:** The user may already be in-flight to your game, so it's possible they will still connect and try to join your party.

	!!! returns "Returns: void"

### changeNumOpenSlots

!!! function "changeNumOpenSlots( `uint64_t` beacon_id, `uint32_t` open_slots )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | beacon_id | uint64_t | Beacon ID for the beacon created by your process. |
    | open_slots | uint32_t | The new number of open slots in your party. |

    If a user joins your party through other matchmaking (perhaps a direct Steam friend, or your own matchmaking system), your game should reduce the number of open slots that Steam is managing through the party beacon.

    For example, if you created a beacon with five slots, and Steam sent you two [reservation_notification](#reservation_notification) callbacks, and then a third user joined directly, you would want to call [changeNumOpenSlots](#changenumopenslots) with a value of 2 for open_slots. That value represents the total number of new users that you would like Steam to send to your party.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[change_num_open_slots](#change_num_open_slots) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#ChangeNumOpenSlots){ .md-button .md-button--doc_classes target="_blank" }

### createBeacon

!!! function "createBeacon( `uint32_t` open_slots, `uint64_t` location_id, `PartyBeaconLocationType` type, `string` connect_string, `string` metadata )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | open_slots | uint32_t | 	Number of reservation slots to create for the beacon. Normally, this is the size of your desired party minus one (for the current user). |
    | location_id | uint64_t | Opaque identifier of this location. This is usually returned by [getAvailableBeaconLocations](#getavailablebeaconlocations). |
    | location_type | [PartyBeaconLocationType enum](#steampartybeaconlocationtype) | The type of beacon location; this is pretty much always chat group as there are currently no other options but "invalid". |
    | connect_string | string | Connect string that will be given to the game on launch for a user that follows the beacon. |
    | metadata | string | Additional game metadata that can be set on the beacon, and is exposed via [getBeaconDetails](#getbeacondetails). |

    Create a beacon. You can only create one beacon at a time. Steam will display the beacon in the specified location, and let up to **open_slots** users "follow" the beacon to your party.

	If users join your party through other matchmaking, adjust the number of remaining open slots using [changeNumOpenSlots](#changenumopenslots).

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[create_beacon](#create_beacon) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#CreateBeacon){ .md-button .md-button--doc_classes target="_blank" }

### destroyBeacon

!!! function "destroyBeacon( `uint6_t` beacon_id )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | beacon_id | uint64_t | Beacon ID to be destroyed. |

    Call this method to destroy the Steam party beacon. This will immediately cause Steam to stop showing the beacon in the target location. Note that any users currently in-flight may still arrive at your party expecting to join.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#DestroyBeacon){ .md-button .md-button--doc_classes target="_blank" }

### getAvailableBeaconLocations

!!! function "getAvailableBeaconLocations( `uint32_t` max_locations )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | max_locations | uint32_t | The maximum number of entries to put into the above list. Should be less than or equal to the result from [getNumAvailableBeaconLocations](#getnumavailablebeaconlocations); higher values will be capped at the returned result. |

    Get the list of locations in which you can post a party beacon.

	!!! returns "Returns: array of dictionaries"
		Each containing the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| type | [BeaconLocationType enum](#steampartybeaconlocationtype) | The beacon location type.
		| location_id | uint64_t | The ID for this location.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#GetAvailableBeaconLocations){ .md-button .md-button--doc_classes target="_blank" }

### getBeaconByIndex

!!! function "getBeaconByIndex( `uint32_t` index )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | index | uint32_t | Index of Beacon. |

    Use with [getNumActiveBeacons](#getnumactivebeacons) to iterate the active beacons visible to the current user. Argument [index is a zero-based index, so iterate over the range [0, getNumActiveBeacons() - 1].

	!!! returns "Returns: uint64_t"
		Returns a **beacon_id** that can be used with [getBeaconDetails](#getbeacondetails) to get information about the beacons suitable for display to the user.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#GetBeaconByIndex){ .md-button .md-button--doc_classes target="_blank" }

### getBeaconDetails

!!! function "getBeaconDetails( `uint64_t` beacon_id )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | beacon_id | uint64_t | Beacon ID to query. |

    Get details about the specified beacon. You can use the Friends API to get further details about owner_id, and [getBeaconLocationData](#getbeaconlocationdata) to get further details about location_id.

    The metadata contents are specific to your game, and will be whatever was set (if anything) by the game process that created the beacon.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| beacon_id | uint64_t | Beacon ID to query.
		| owner_id | uint64_t | Creator of the beacon.
		| type | [PartyBeaconLocationType enum](#steampartybeaconlocationtype) | Location the beacon has been posted.
		| location_id | uint64_t | Opaque identifier of this location.
		| metadata | string | Any additional metadata the game has set on this beacon; eg. game mode.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#GetBeaconDetails){ .md-button .md-button--doc_classes target="_blank" }

### getBeaconLocationData

!!! function "getBeaconLocationData( `uint64_t` location_id, `PartyBeaconLocationType` location_type, `PartyBeaconLocationData` location_data )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | location_id | uint64_t | Opaque identifier of this location. This is usually returned by [getAvailableBeaconLocations](#getavailablebeaconlocations). |
    | location_type | [PartyBeaconLocationType enum](#steampartybeaconlocationtype) | The type of beacon location; this is pretty much always chat group as there are currently no other options but "invalid". |
    | location_data | [PartyBeaconLocationData enum](#steampartybeaconlocationdata) | Type of location data you wish to get. |

	Query general metadata for the given beacon location. For instance the Name, or the URL for an icon if the location type supports icons (for example, the icon for a Steam Chat Room Group).
	
	!!! returns "Returns: string"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#GetBeaconLocationData){ .md-button .md-button--doc_classes target="_blank" }

### getNumActiveBeacons

!!! function "getNumActiveBeacons( )"
	Get the number of active party beacons created by other users for your game, that are visible to the current user.

	!!! returns "Returns: uint32_t"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#GetNumActiveBeacons){ .md-button .md-button--doc_classes target="_blank" }

### getNumAvailableBeaconLocations

!!! function "getNumAvailableBeaconLocations( )"
	Get the number of locations in which you are able to post a party beacon.  Use this to size your result list for a call to [getAvailableBeaconLocations](#getavailablebeaconlocations).

	!!! returns "Returns: uint32_t"
		If the returned integer is -1, then the function failed to work correctly.

### joinParty

!!! function "joinParty( `uint64_t` beacon_id )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | beacon_id | uint64_t | Beacon ID for the party you wish to join. |

    When the user indicates they wish to join the party advertised by a given beacon, call this method. On success, Steam will reserve a slot for this user in the party and return the necessary "join game" string to use to complete the connection.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[join_party](#join_party) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#JoinParty){ .md-button .md-button--doc_classes target="_blank" }

### onReservationCompleted

!!! function "onReservationCompleted( `uint64_t` beacon_id, `uint64_t` steam_id )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | beacon_id | uint64_t | Beacon ID for the beacon created by your process. |
    | steam_id | uint64_t | SteamID of the user joining your party. |

    When a user follows your beacon, Steam will reserve one of the open party slots for them, and send your game a [reservation_notification](#reservation_notification) callback. When that user joins your party, call [onReservationCompleted](#onreservationcompleted) to notify Steam that the user has joined successfully.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[reservation_notification](#reservation_notification) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#OnReservationCompleted){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### active_beacons_updated

!!! function "active_beacons_updated"
	Notification that the list of active beacons visible to the current user has changed.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#ActiveBeaconsUpdated_t){ .md-button .md-button--doc_classes target="_blank" }

### available_beacon_locations_updated

!!! function "available_beacon_locations_updated"
	Notification that the list of available locations for posting a beacon has been updated. 

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#AvailableBeaconLocationsUpdated_t){ .md-button .md-button--doc_classes target="_blank" }

### change_num_open_slots

!!! function "change_num_open_slots"
	Call result for [changeNumOpenSlots](#changenumopenslots).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| result | [Result enum](main.md#result) | The result of the attempt to change the number of open slots

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#ChangeNumOpenSlotsCallback_t){ .md-button .md-button--doc_classes target="_blank" }

### create_beacon

!!! function "create_beacon"
	This callback is used as a call response for [createBeacon](#createbeacon). If successful, your beacon has been posted in the desired location and you may start receiving [reservation_notification](#reservation_notification) callbacks for users following the beacon.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| result | [Result enum](main.md#result) | The result of the attempt to create a beacon.
		| beacon_id | uint64_t | Beacon ID of the newly created beacon.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#CreateBeaconCallback_t){ .md-button .md-button--doc_classes target="_blank" }

### join_party

!!! function "join_party"
	This callback is used as a call response for [joinParty](#joinparty). On success, you will have reserved a slot in the beacon-owner's party, and should use connect_string to connect to their game and complete the process.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| result | [Result enum](main.md#result) | The result of the attempt to join the party.
		| beacon_id | uint64_t | Beacon ID used in the attempt.
		| steam_id | uint64_t | Creator of the beacon used in the attempt.
		| connect_string | string | If successful, a "join game" string for your game to use to complete the process of joining the desired party.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#JoinPartyCallback_t){ .md-button .md-button--doc_classes target="_blank" }

### reservation_notification

!!! function "reservation_notification"
	After creating a beacon, when a user "follows" that beacon Steam will send you this callback to know that you should be prepared for the user to join your game. When they do join, be sure to call [onReservationCompleted](#onreservationcompleted) to let Steam know.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| beacon_id | uint64_t | Beacon ID of your beacon.
		| steam_id | uint64_t | Steam ID of the user following your beacon.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteamparties#ReservationNotificationCallback_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-numeric: Enums
==}

### SteamPartyBeaconLocationType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
STEAM_PARTY_BEACON_LOCATION_TYPE_INVALID | k_ESteamPartyBeaconLocationType_Invalid | 0 | -
STEAM_PARTY_BEACON_LOCATION_TYPE_CHAT_GROUP | k_ESteamPartyBeaconLocationType_ChatGroup | 1 | -
STEAM_PARTY_BEACON_LOCATION_TYPE_MAX | k_ESteamPartyBeaconLocationType_Max | - | -

### SteamPartyBeaconLocationData

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
STEAM_PARTY_BEACON_LOCATION_DATA_INVALID | k_ESteamPartyBeaconLocationDataInvalid | 0 | -
STEAM_PARTY_BEACON_LOCATION_DATA_NAME | k_ESteamPartyBeaconLocationDataName | 1 | -
STEAM_PARTY_BEACON_LOCATION_DATA_URL_SMALL | k_ESteamPartyBeaconLocationDataIconURLSmall | 2 | -
STEAM_PARTY_BEACON_LOCATION_DATA_URL_MEDIUM | k_ESteamPartyBeaconLocationDataIconURLMedium | 3 | -
STEAM_PARTY_BEACON_LOCATION_DATA_URL_LARGE | k_ESteamPartyBeaconLocationDataIconURLLarge | 4 | -
