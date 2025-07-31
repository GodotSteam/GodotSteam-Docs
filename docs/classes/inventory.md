---
title: Inventory
description: A class reference for Inventory.
icon: material/invoice-list
---

# Inventory

Steam Inventory query and manipulation API. See [Steam Inventory Service](https://partner.steamgames.com/doc/features/inventory){ target="\_blank" } for more information.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } and [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### addPromoItem

!!! function "addPromoItem( `uint32_t` item )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | item | uint32_t | The ItemDef to grant the player.

    Grant a specific one-time promotional item to the current user.

	This can be safely called from the client because the items it can grant can be locked down via policies in the itemdefs. One of the primary scenarios for this call is to grant an item to users who also own a specific other game. This can be useful if your game has custom UI for showing a specific promo item to the user otherwise if you want to grant multiple promotional items then use [addPromoItems](#addpromoitems) or [grantPromoItems](#grantpromoitems).

	Any items that can be granted _must_ have a "promo" attribute in their itemdef. That promo item list a set of app IDs that the user must own to be granted this given item. This version will grant all items that have promo attributes specified for them in the configured item definitions. This allows adding additional promotional items without having to update the game client. For example the following will allow the item to be granted if the user owns either TF2 or SpaceWar.

	!!! returns "Returns: int32"
		This function always returns a valid inventory handle when called by a regular user and always returns 0 when called from SteamGameServer.

		On success, the inventory result will include items which were granted, if any. If no items were granted because the user isn't eligible for any promotions, this is still considered a success.

		It will overwrite any previously stored inventory handle.

	!!! info "Notes"
		You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#AddPromoItem){ .md-button .md-button--store target="_blank" }

### addPromoItems

!!! function "addPromoItems( `PackedInt64Array` items )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | items | PackedInt64Array | The list of items to grant the user.

    Grant a specific one-time promotional item to the current user.

	This can be safely called from the client because the items it can grant can be locked down via policies in the itemdefs. One of the primary scenarios for this call is to grant an item to users who also own a specific other game. If you want to grant a single promotional item then use [addPromoItems](#addpromoitems). If you want to grant all possible promo items then use [grantPromoItems](#grantpromoitems).

	Any items that can be granted _must_ have a "promo" attribute in their itemdef. That promo item list a set of app ID's that the user must own to be granted this given item. This version will grant all items that have promo attributes specified for them in the configured item definitions. This allows adding additional promotional items without having to update the game client. For example the following will allow the item to be granted if the user owns either TF2 or SpaceWar.

	!!! returns "Returns: int32"
		This function always returns an inventory handle when called by a regular user and always returns 0 when called from SteamGameServer.

		On success, the inventory result will include items which were granted, if any. If no items were granted because the user isn't eligible for any promotions, this is still considered a success.

		It will overwrite any previously stored inventory handle.

	!!! info "Notes"
		You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#AddPromoItems){ .md-button .md-button--store target="_blank" }

### checkResultSteamID

!!! function "checkResultSteamID( `uint64_t` steam_id_expected, `int32` this_inventory_handle = 0 )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id_expected | uint64_t | The Steam ID to verify.
    | this_inventory_handle | int32 | The inventory result handle to check the Steam ID on. Defaults to 0.

	Checks whether an inventory result handle belongs to the specified Steam ID.
	This is important when using [deserializeResult](#deserializeresult), to verify that a remote player is not pretending to have a different user's inventory.

	!!! returns "Returns: bool"
		Returns true if the result belongs to the target steam ID; otherwise, false.

	!!! info "Notes"
		If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally-stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#CheckResultSteamID){ .md-button .md-button--store target="_blank" }

### consumeItem

!!! function "consumeItem( `uint64_t` item_consume, `uint32_t` quantity )"	
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | item_consume | uint64_t | The item instance id to consume.
    | quantity | uint32_t | The number of items in that stack to consume.

    Consumes items from a user's inventory. If the quantity of the given item goes to zero, it is permanently removed.

	Once an item is removed it cannot be recovered. This is not for the faint of heart; if your game implements item removal at all, a high-friction UI confirmation process is highly recommended.

	!!! returns "Returns: int32"
		This function always returns an inventory handle when called by a regular user and always returns 0 when called from SteamGameServer.

		It will overwrite any previously stored inventory handle.

	!!! info "Notes"
		You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#ConsumeItem){ .md-button .md-button--store target="_blank" }

### deserializeResult

!!! function "deserializeResult( `PackedByteArray` buffer )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | buffer | PackedByteArray | The buffer to deserialize.

    Deserializes a result set and verifies the signature bytes.

	This call has a potential soft-failure mode where the handle status is set to 27. [getResultItems](#getresultitems) will still succeed in this mode. The "expired" result could indicate that the data may be out of date - not just due to timed expiration (one hour), but also because one of the items in the result set may have been traded or consumed since the result set was generated.

	You could compare the timestamp from [getResultTimestamp](#getresulttimestamp) to [getServerRealTime](utils.md#getserverrealtime) to determine how old the data is. You could simply ignore the "expired" result code and continue as normal, or you could request the player with expired data to send an updated result set.

	You should call [checkResultSteamID](#checkresultsteamid) on the result handle when it completes to verify that a remote player is not pretending to have a different user's inventory.

	!!! returns "Returns: int32"
		The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

		It will deliver error codes via [getResultStatus](#getresultstatus).

	!!! info "Notes"
		You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#DeserializeResult){ .md-button .md-button--store target="_blank" }

### destroyResult

!!! function "destroyResult( `int32` this_inventory_handle = 0 )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | this_inventory_handle | int32 | The inventory result handle to destroy. Defaults to 0.

    Destroys a result handle and frees all associated memory.

	!!! returns "Returns: void"

	!!! info "Notes"
		If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally-stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#DestroyResult){ .md-button .md-button--store target="_blank" }

### exchangeItems

!!! function "exchangeItems( `PackedInt64Array` output_items, `PackedInt32Array` output_quantity, `PackedInt64Array` input_items, `PackedInt32Array` input_quantity )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | output_items | PackedInt64Array | The list of items that will be created by this call. Currently can only be 1 item.
    | output_quantity | PackedInt32Array | The quantity of each item in **output_items** to create. Currently can only be 1 item and it must be set to 1.
    | input_items | PackedInt64Array | The list of items that will be destroyed by this call.
    | input_quantity | PackedInt32Array | The quantity of each item in **input_items** to destroy.

    Grant one item in exchange for a set of other items.

	This can be used to implement crafting recipes or transmutations, or items which unpack themselves into other items (e.g., a chest).

	The caller of this API passes in the requested item and an array of existing items and quantities to exchange for it. The API currently takes an array of items to generate but at this time the size of that array must be 1 and the quantity of the new item must be 1.

	Any items that can be granted **must** have an exchange attribute in their itemdef. The exchange attribute specifies a set of recipes that are valid exchanges for this item. Exchange recipes are evaluated atomically by the Inventory Service; if the supplied components do not match the recipe, or do not contain sufficient quantity, the exchange will fail.

	Will allow the item to be exchanged for either one #101 and one #102, five #103s or three #104s and three #105s. See the [Steam Inventory Schema](https://partner.steamgames.com/doc/features/inventory/schema){ .md-button .md-button--store target="_blank" } documentation for more details.

	!!! returns "Returns: int32"
		This function returns an inventory handle indicating success; otherwise, it returns 0 when called from SteamGameServer or when **output_quantity** is not set to 1. Exchanges that do not match a recipe, or do not provide the required amounts, will fail.

		The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

	!!! info "Notes"
		You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#ExchangeItems){ .md-button .md-button--store target="_blank" }

### generateItems

!!! function "generateItems( `PackedInt64Array` items, `PackedInt32Array` quantity )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | items | PackedInt64Array | The list of items to grant the user.
    | quantity | PackedInt32Array | The quantity of each item in **items** to grant.

	Grants specific items to the current user, for developers only. This API is only intended for prototyping; it is only usable by Steam accounts that belong to the publisher group for your game.

	You can pass in an array of items, identified by their item defintion int's and optionally a second array of corresponding quantities for each item. The length of these arrays **must** match!

	!!! returns "Returns: int32"
		This function always returns a valid inventory handle when called by a regular user and always returns 0 when called from SteamGameServer.

		It will overwrite any previously stored inventory handle.

	!!! info "Notes"
		You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GenerateItems){ .md-button .md-button--store target="_blank" }

### getAllItems

!!! function "getAllItems( )"
	Start retrieving all items in the current users inventory.

	Calls to this function are subject to rate limits and may return cached results if called too frequently. It is suggested that you call this function only when you are about to display the user's full inventory, or if you expect that the inventory may have changed.

	!!! returns "Returns: int32"
		This function always returns a valid inventory handle when called by a regular user and always returns 0 when called from SteamGameServer.

		It will overwrite any previously stored inventory handle.

	!!! info "Notes"
		You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetAllItems){ .md-button .md-button--store target="_blank" }
		

### getItemDefinitionProperty

!!! function "getItemDefinitionProperty( `uint32_t` definition, `string` name )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | definition | uint32_t | The item definition to get the properties for.
    | name | string | The property name to get the value for. If you pass in NULL then pchValueBuffer will contain a comma-separated list of all the available names.

    Gets a string property from the specified item definition or a property value for a specific item definition.

	Note that some properties (for example, "name") may be localized and will depend on the current Steam language settings; see [getCurrentGameLanguage](apps.md#getcurrentgamelanguage). Property names are always ASCII alphanumeric and underscores.

	Pass an empty string for **name** to get a comma-separated list of available property names.

	Call [loadItemDefinitions](#loaditemdefinitions) first, to ensure that items are ready to be used before calling [getItemDefinitionProperty](#getitemdefinitionproperty).

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
        | property | string | The associated value or comma-separated list of names.
    	| success | bool | This returns true upon success; otherwise, false indicating that the item definitions have not been loaded from the server, no item definitions exist for the current application, or the property name was not found in the item definition.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetItemDefinitionProperty){ .md-button .md-button--store target="_blank" }

### getItemsByID

!!! function "getItemsByID( `PackedInt64Array` id_array )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | id_array | PackedInt64Array | A list of the item instance ids to update the state of.

	Gets the state of a subset of the current user's inventory, 32specified by an array of item instance IDs.

	The results from this call can be serialized using [serializeResult](#serializeresult) and passed to other players to "prove" that the current user owns specific items, without exposing the user's entire inventory.

	For example, you could call this with the IDs of the user's currently equipped items and serialize this to a buffer, and then transmit this buffer to other players upon joining a game.

	!!! returns "Returns: int32"
		This function always returns a valid inventory handle when called by a regular user and always returns 0 when called from SteamGameServer.

		It will overwrite any previously stored inventory handle.

	!!! info "Notes"
		You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetItemsByID){ .md-button .md-button--store target="_blank" }

### getItemPrice

!!! function "getItemPrice( `uint32_t` definition )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | definition | uint32_t | The item definition id to retrieve the price for.

	After a successful call to [requestPrices](#requestprices), you can call this method to get the pricing for a specific item definition.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| price | uint64_t | The current price rendered in the user's local currency. May be lower than base price due to sales or such.
		| base_price | uint64_t | The base price rendered in the user's local currency.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetItemPrice){ .md-button .md-button--store target="_blank" }

### getItemsWithPrices

!!! function "getItemsWithPrices( )"
	After a successful call to [requestPrices](#requestprices), you can call this method to get all the pricing, in the user's local currency, for applicable item definitions.

	!!! returns "Returns: array"
		Contains a list of:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| price_group | dictionary | Data about prices.

		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| item | int32 | The item definition ID.
		| price | uint64_t | The current price rendered in the user's local currency. May be lower than the base price due to sales or such.
		| base_price | uint64_t | The base price rendered in the user's local currency.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetItemsWithPrices){ .md-button .md-button--store target="_blank" }

### getResultItemProperty

!!! function "getResultItemProperty( `uint32_t` index, `string` name, `int32` this_inventory_handle = 0 )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | index | uint32_t | The index of the returned array in [getResultItems](#getresultitems).
    | name | string | The property name to get the value for. If you pass an empty string then this will return a comma-separated list of all the available names.
    | this_inventory_handle | int32 | The result handle containing the item to get the properties of. Defaults to 0.

    Gets the dynamic properties from an item in an inventory result set.

	Property names are always composed of ASCII letters, numbers, and/or underscores.

	Passing an empty string for **name** will return a comma-separated list of available property names.

	If the results do not fit in the given buffer, partial results may be copied.

	!!! returns "Returns: string"
		Returns some string value upon success; otherwise, an empty string indicating that the inventory result handle was invalid or the provided index does not contain an item.

	!!! info "Notes"
		If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally-stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetResultItemProperty){ .md-button .md-button--store target="_blank" }

### getResultItems

!!! function "getResultItems( `int32` this_inventory_handle = 0 )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | this_inventory_handle | int32 | The inventory result handle to get the items for. Defaults to 0.

    Get the items associated with an inventory result handle.

	!!! returns "Returns: array"
		Containing a list of:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| item_info | dictionary | Data about the item.

		Each **item_info** dictionary contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| item_id | int | The globally unique item instance handle.
		| item_definition | int | The item definition number for this item.
		| flags | int | This is a bitmasked collection of [SteamItemFlags](#steamitemflags).
		| quantity | int | The current quantity of the item.

	!!! info "Notes"
		If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally-stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetResultItems){ .md-button .md-button--store target="_blank" }

### getResultStatus

!!! function "getResultStatus( `int32` this_inventory_handle = 0 )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | this_inventory_handle | int32 | The inventory result handle to get the status for. Defaults to 0.

	Find out the status of an asynchronous inventory result handle.
	This is a polling equivalent to registering a callback function for [inventory_result_ready](#inventory_result_ready).

	!!! returns "Returns: [Result enum](main.md#result)"
		Possible values include:

		* RESULT_PENDING - Still in progress.
		* RESULT_OK - Done; the request has completed successfully and the result is ready.
		* RESULT_EXPIRED - Done; result ready but maybe out of date. See [deserializeResult](#deserializeresult).
		* RESULT_INVALID_PARAM - Eerror; invalid API call parameters.
		* RESULT_SERVICE_UNAVAILABLE - Error; service temporarily down, you may retry later.
		* RESULT_LIMIT_EXCEEDED - Error; operation would exceed per-user inventory limits.
		* RESULT_FAIL - Error; generic error.

	!!! info "Notes"
		If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally-stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetResultStatus){ .md-button .md-button--store target="_blank" }

### getResultTimestamp

!!! function "getResultTimestamp( `int32` this_inventory_handle = 0 )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | this_inventory_handle | int32 | The inventory result handle to get the timestamp for. Defaults to 0.

	Gets the server time at which the result was generated.

	You can compare this value against [getServerRealTime](utils.md#getserverrealtime) to determine the age of the result.

	!!! returns "Returns: uint32_t"
		The timestamp is provided as Unix epoch time (Time since Jan 1st, 1970).

	!!! info "Notes"
		If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally-stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetResultTimestamp){ .md-button .md-button--store target="_blank" }

### grantPromoItems

!!! function "grantPromoItems( )"
	Grant all potential one-time promotional items to the current user.

	This can be safely called from the client because the items it can grant can be locked down via policies in the itemdefs. One of the primary scenarios for this call is to grant an item to users who also own a specific other game. If you want to grant specific promotional items rather than all of them see: [addPromoItem](#addpromoitem) and [addPromoItems](#addpromoitems).

	Any items that can be granted **must** have a "promo" attribute in their itemdef. That promo item list a set of app ID's that the user must own to be granted this given item. This version will grant all items that have promo attributes specified for them in the configured item definitions. This allows adding additional promotional items without having to update the game client. 

	For example the following will allow the item to be granted if the user owns either TF2 or SpaceWar.

	!!! returns "Returns: int32"
		This function always returns a valid inventory result when called by a regular user and always returns 0 when called from SteamGameServer.

		On success, the inventory result will include items which were granted, if any. If no items were granted because the user isn't eligible for any promotions, this is still considered a success.

		It will overwrite any previously stored inventory handle.

	!!! info "Notes"
		You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GrantPromoItems){ .md-button .md-button--store target="_blank" }

### loadItemDefinitions

!!! function "loadItemDefinitions( )"
	Triggers an asynchronous load and refresh of item definitions.

	Item definitions are a mapping of "definition IDs" (integers between 1 and 999999999) to a set of string properties. Some of these properties are required to display items on the Steam community web site. Other properties can be defined by applications. There is no reason to call this function if your game hardcoded the numeric definition IDs (e.g. purple face mask = 20, blue weapon mod = 55) and does not allow for adding new item types without a client patch.

	!!! returns "Returns: bool"
		This call will always return true.

	!!! trigger "Triggers"
		* [inventory_definition_update](#inventory_definition_update) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#LoadItemDefinitions){ .md-button .md-button--store target="_blank" }

### removeProperty

!!! function "removeProperty( `uint64_t` item_id, `string` name, `int32` this_inventory_update_handle = 0 )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | item_id | uint64_t | ID of the item being modified.
    | name | string | The dynamic property being removed.
    | this_inventory_update_handle | int32 | The update handle corresponding to the transaction request, returned from [startUpdateProperties](#startupdateproperties). Defaults to 0.

	Removes a [dynamic property](https://partner.steamgames.com/doc/features/inventory/dynamicproperties){ target="\_blank" } for the given item.

	!!! returns "Returns: bool"

	!!! info "Notes"
		If the argument **this_inventory_update_handle** is omitted, GodotSteam will use the internally-stored ID. This is different from **this_inventory_handle** and is stored separately.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#RemoveProperty){ .md-button .md-button--store target="_blank" }

### requestEligiblePromoItemDefinitionsIDs

!!! function "requestEligiblePromoItemDefinitionsIDs( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the user to request the eligible promo items for.

    Request the list of "eligible" promo items that can be manually granted to the given user.

	These are promo items of type "manual" that won't be granted automatically. An example usage of this is an item that becomes available every week.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		* [inventory_eligible_promo_item](#inventory_eligible_promo_item) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#RequestEligiblePromoItemDefinitionsIDs){ .md-button .md-button--store target="_blank" }

### requestPrices

!!! function "requestPrices( )"
	Request prices for all item definitions that can be purchased in the user's local currency.

	A [inventory_request_prices_result](#inventory_request_prices_result) call result will be returned with the user's local currency code. After that, you can call [getItemsWithPrices](#getitemswithprices) to get prices for all the known item definitions, or [getItemPrice](#getitemprice) for a specific item definition.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		* [inventory_request_prices_result](#inventory_request_prices_result) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#RequestPrices){ .md-button .md-button--store target="_blank" }

### serializeResult

!!! function "serializeResult( `int32` this_inventory_handle = 0 )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | this_inventory_handle | int32 | The inventory result handle to serialize. Defaults to 0.

	Serialized result sets contain a short signature which can't be forged or replayed across different game sessions.

	A result set can be serialized on the local client, transmitted to other players via your game networking, and deserialized by the remote players. This is a secure way of preventing hackers from lying about posessing rare/high-value items. Serializes a result set with signature bytes to an output buffer. The size of a serialized result depends on the number items which are being serialized. When securely transmitting items to other players, it is recommended to use [getItemsByID](#getitemsbyid) first to create a minimal result set.

	Results have a built-in timestamp which will be considered "expired" after an hour has elapsed. See [deserializeResult](#deserializeresult) for expiration handling.

	!!! returns "Returns: PackedByteArray"
		Upon success the returned array will be filled; otherwise, it will be empty under the following conditions:

		* The function was not called by a regular user. This call is not supported on GameServers.
		* **this_inventory_handle** is invalid or the inventory result handle is not ready.

	!!! info "Notes"
		If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally-stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SerializeResult){ .md-button .md-button--store target="_blank" }

### setPropertyBool

!!! function "setPropertyBool( `uint64_t` item_id, `string` name, `bool` value, `int32` this_inventory_update_handle )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | item_id | uint64_t | ID of the item being modified.
    | name | string | The dynamic property being added or updated.
    | value | bool | The boolean value being set.
    | this_inventory_update_handle | int32 | The update handle corresponding to the transaction request, returned from [startUpdateProperties](#startupdateproperties). Defaults to 0.

	Sets a [dynamic property](https://partner.steamgames.com/doc/features/inventory/dynamicproperties){ target="\_blank" } for the given item. Supported value types are boolean.

	!!! returns "Returns: bool"

	!!! info "Notes"
		If the argument **this_inventory_update_handle** is omitted, GodotSteam will use the internally-stored ID. This is different from **this_inventory_handle** and is stored separately.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SetProperty){ .md-button .md-button--store target="_blank" }

### setPropertyFloat

!!! function "setPropertyFloat( `uint64_t` item_id, `string` name, `float` value, `int32` this_inventory_update_handle )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | item_id | uint64_t | ID of the item being modified.
    | name | string | The dynamic property being added or updated.
    | value | float | The floating point number value being set.
    | this_inventory_update_handle | int32 | The update handle corresponding to the transaction request, returned from [startUpdateProperties](#startupdateproperties). Defaults to 0.

	Sets a [dynamic property](https://partner.steamgames.com/doc/features/inventory/dynamicproperties){ target="\_blank" } for the given item. Supported value types are 32 bit floats.

	!!! returns "Returns: bool"

	!!! info "Notes"
		If the argument **this_inventory_update_handle** is omitted, GodotSteam will use the internally-stored ID. Also note this different from **this_inventory_handle** and is stored separately.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SetProperty){ .md-button .md-button--store target="_blank" }

### setPropertyInt

!!! function "setPropertyInt( `uint64_t` item_id, `string` name, `uint64_t` value, `int32` this_inventory_update_handle )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | item_id | uint64_t | ID of the item being modified.
    | name | string | The dynamic property being added or updated.
    | value | uint64_t | The 64 bit integer value being set.
    | this_inventory_update_handle | int32 | The update handle corresponding to the transaction request, returned from [startUpdateProperties](#startupdateproperties). Defaults to 0.

	Sets a [dynamic property](https://partner.steamgames.com/doc/features/inventory/dynamicproperties){ target="\_blank" } for the given item. Supported value types are 64 bit integers.

	!!! returns "Returns: bool"

	!!! info "Notes"
		If the argument **this_inventory_update_handle** is omitted, GodotSteam will use the internally-stored ID. This is different from **this_inventory_handle** and is stored separately.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SetProperty){ .md-button .md-button--store target="_blank" }

### setPropertyString

!!! function "setPropertyString( `uint64_t` item_id, `string` name, `string` value, `int32` this_inventory_update_handle = 0 )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | item_id | uint64_t | ID of the item being modified.
    | name | string | The dynamic property being added or updated.
    | value | string | The string value being set.
    | this_inventory_update_handle | int32 | The update handle corresponding to the transaction request, returned from [startUpdateProperties](#startupdateproperties). Defaults to 0.

	Sets a [dynamic property](https://partner.steamgames.com/doc/features/inventory/dynamicproperties){ target="\_blank" } for the given item. Supported value types are strings.

	!!! returns "Returns: bool"

	!!! info "Notes"
		If the argument **this_inventory_update_handle** is omitted, GodotSteam will use the internally-stored ID. This is different from **this_inventory_handle** and is stored separately.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SetProperty){ .md-button .md-button--store target="_blank" }

### startPurchase

!!! function "startPurchase( `PackedInt64Array` items, `PackedInt32Array` quantity )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | items | PackedInt64Array | The array of item definition ids that the user wants to purchase.
    | quantity | PackedInt32Array | The array of quantities of each item definition that the user wants to purchase.

    Starts the purchase process for the user, given a "shopping cart" of item definitions that the user would like to buy. The user will be prompted in the Steam Overlay to complete the purchase in their local currency, funding their Steam Wallet if necessary, etc.

	If the purchase process was started successfully, then **order_id** and **transaction_id** will be valid in the [inventory_start_purchase_result](#inventory_start_purchase_result) call result.

	If the user authorizes the transaction and completes the purchase, then the callback [inventory_result_ready](#inventory_result_ready) will be triggered and you can then retrieve what new items the user has acquired.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		* [inventory_start_purchase_result](#inventory_start_purchase_result) callback

	!!! info "Notes"
		You must call [destroyResult](#destroyresult) on the inventory result for when you are done with it.

		Testing while in development, testing [startPurchase](#startpurchase) before your app is released, all transactions made by members of the development / publishing team will be made through the sandbox micro-transaction API internally. This means you will not be charged for the purchases made before the app is released if you are part of the Steamworks publisher.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#StartPurchase){ .md-button .md-button--store target="_blank" }

### startUpdateProperties

!!! function "startUpdateProperties( )"
	Starts a transaction request to update dynamic properties on items for the current user. This call is rate-limited by user, so property modifications should be batched as much as possible (e.g. at the end of a map or game session).

	After calling [setPropertyInt](#setpropertyint), [setPropertyfloat](#setpropertyfloat),[setPropertyBool](#setpropertybool), [setPropertyString](#setpropertystring), or [removeProperty](#removeproperty) for all the items that you want to modify, you will need to call [submitUpdateProperties](#submitupdateproperties) to send the request to the Steam servers. A [inventory_result_ready](#inventory_result_ready) callback will be fired with the results of the operation.

	!!! returns "Returns: uint64_t"
		It will overwrite any previously stored inventory update handle.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#StartUpdateProperties){ .md-button .md-button--store target="_blank" }

### submitUpdateProperties

!!! function "submitUpdateProperties( uint64_t this_inventory_update_handle = 0 )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | this_inventory_update_handle | uint64_t | The update handle corresponding to the transaction request, returned from [startUpdateProperties](#startupdateproperties).

	Submits the transaction request to modify dynamic properties on items for the current user. See [startUpdateProperties](#startupdateproperties).

	!!! returns "Returns: int32"

		It will overwrite any previously stored inventory handle.

	!!! info "Notes"
		You must call [destroyResult](#destroyresult) on the provided inventory result for when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SubmitUpdateProperties){ .md-button .md-button--store target="_blank" }

### transferItemQuantity

!!! function "transferItemQuantity( `uint64_t` item_id, `uint32_t` quantity, `uint64_t` item_destination, `bool` split )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | item_id | uint64_t | The source item to transfer.
    | quantity | uint32_t | The quantity of the item that will be transfered from **item_id** to **item_destination**.
    | item_destination | uint64_t | The destination item.
    | split | bool | Whether or not to split the stack.

	Transfer items between stacks within a user's inventory.

	This can be used to stack, split, and moving items. The source and destination items must have the same itemdef id. To move items onto a destination stack specify the source, the quantity to move, and the destination item id.

	To split an existing stack, pass true into **split**. A new item stack will be generated with the requested quantity.

	!!! returns "Returns: int32"
		This function always returns a valid inventory handle when called by a regular user and always returns 0 when called from SteamGameServer.

		It will overwrite any previously stored inventory handle.

	!!! info "Notes"
		Tradability / marketability restrictions are copied along with transferred items. The destination stack receives the latest tradability / marketability date of any item in its composition.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#TransferItemQuantity){ .md-button .md-button--store target="_blank" }

### triggerItemDrop

!!! function "triggerItemDrop( `uint32_t` definition )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | definition | uint32_t | This must refer to an itemdefid of the type "playtimegenerator". See the [inventory schema for more details](https://partner.steamgames.com/doc/features/inventory/schema){ target="\_blank" }.

    Trigger an item drop if the user has played a long enough period of time.

	This period can be customized in two places:

	* At the application level within Inventory Service: Playtime Item Grants. This will automatically apply to all "playtimegenerator" items that do not specify any overrides.
	* In an individual "playtimegenerator" item definition. The settings would take precedence over any application-level settings.

	Only item definitions which are marked as "playtime item generators" can be spawned.

	Typically this function should be called at the end of a game or level or match or any point of significance in the game in which an item drop could occur. The granularity of the playtime generator settings is in minutes, so calling it more frequently than minutes is not useful and will be rate limited in the Steam client. The Steam servers will perform playtime accounting to prevent too-frequent drops. The servers will also manage adding the item to the players inventory.

	!!! returns "Returns: int32"
		This function always returns a valid inventory handle when called by a regular user and always returns 0 when called from SteamGameServer.

		It will overwrite any previously stored inventory handle.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#TriggerItemDrop){ .md-button .md-button--store target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to run `Steam.run_callbacks()` in your `_process()` function to receive them.

### inventory_definition_update

!!! function "inventory_definition_update"
	This callback is triggered whenever item definitions have been updated, which could be in response to [loadItemDefinitions](#loaditemdefinitions) or any time new item definitions are available (eg, from the dynamic addition of new item types while players are still in-game).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | definitions | array | A list of item definintions that were updated.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryDefinitionUpdate_t){ .md-button .md-button--store target="_blank" }

### inventory_eligible_promo_item

!!! function "inventory_eligible_promo_item"
	Returned when you have requested the list of "eligible" promo items that can be manually granted to the given user. These are promo items of type "manual" that won't be granted automatically.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [Result enum](main.md#result) | RESULT_OK upon success, any other value indicates failure.
		| cached | bool | Indicates that the data was retrieved from the cache and not the server. This happens if the user is not logged on or can not otherwise connect to the Steam servers.
		| definitions | array | The list of eligible item definition IDs.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryEligiblePromoItemDefIDs_t){ .md-button .md-button--store target="_blank" }

### inventory_full_update

!!! function "inventory_full_update"
	Triggered when [getAllItems](#getallitems) successfully returns a result which is newer / fresher than the last known result. (It will not trigger if the inventory hasn't changed, or if results from two overlapping calls are reversed in flight and the earlier result is already known to be stale/out-of-date.) The regular [inventory_result_ready](#inventory_result_ready) callback will still be triggered immediately afterwards; this is an additional notification for your convenience.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| inventory_handle | int32 | A new inventory result handle.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryFullUpdate_t){ .md-button .md-button--store target="_blank" }

### inventory_request_prices_result

!!! function "inventory_request_prices_result"
	Returned after [requestPrices](#requestprices) is called.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [Result enum](main.md#result) | RESULT_OK upon success, any other value indicates failure.
		| currency | string | The string representing the [user's local currency code](https://partner.steamgames.com/doc/store/pricing/currencies){ target="\_blank" }.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryRequestPricesResult_t){ .md-button .md-button--store target="_blank" }

### inventory_result_ready

!!! function "inventory_result_ready"
	This is fired whenever an inventory result transitions from [RESULT_PENDING](main.md#result) to any other completed state, see [getResultStatus](#getresultstatus) for the complete list of states. There will always be exactly one callback per handle.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [Result enum](main.md#result) | The new status of the handle. This is equivalent to calling [getResultStatus](#getresultstatus).
		| inventory_handle | int32 | The inventory result which is now ready.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryResultReady_t){ .md-button .md-button--store target="_blank" }

### inventory_start_purchase_result

!!! function "inventory_start_purchase_result"
	Returned after [startPurchase](#startpurchase) is called.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [Result enum](main.md#result) | RESULT_OK upon success, any other value indicates failure.
		| order_id | uint64_t | The auto-generated order id for the initiated purchase.
		| transaction_id | uint64_t | The auto-generated transaction id for the initiated purchase.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryStartPurchaseResult_t){ .md-button .md-button--store target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
INVENTORY_RESULT_INVALID | k_SteamInventoryResultInvalid | -1 | An invalid Steam inventory result handle.
INVENTORY_UPDATE_HANDLE_INVALID | k_SteamInventoryUpdateHandleInvalid | 0xffffffffffffffffull | -
ITEM_INSTANCE_ID_INVALID | k_SteamItemInstanceIDInvalid | (SteamItemInstanceID_t)~0 | An invalid item instance id. This is usually returned when an operation has failed. It's recommended that you initialize all new item instances with this value.

{==
## :material-numeric: Enums
==}

### SteamItemFlags

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
STEAM_ITEM_NO_TRADE | k_ESteamItemNoTrade | (1<<0) | This item is account-locked and cannot be traded or given away. This is an item status flag which is permanently attached to specific item instances.
STEAM_ITEM_REMOVED | k_ESteamItemRemoved | (1<<8) | The item has been destroyed, traded away, expired, or otherwise invalidated. This is an action confirmation flag which is only set one time, as part of a result set.
STEAM_ITEM_CONSUMED | k_ESteamItemConsumed | (1<<9) | The item quantity has been decreased by 1 via [consumeItem](#consumeitem) API. This is an action confirmation flag which is only set one time, as part of a result set.