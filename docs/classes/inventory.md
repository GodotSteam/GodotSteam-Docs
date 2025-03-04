---
title: Inventory
description: A class reference for Inventory.
icon: material/invoice-list
---

# Inventory

Steam Inventory query and manipulation API. See [Steam Inventory Service](https://partner.steamgames.com/doc/features/inventory){ target="\_blank" } for more information.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } and [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### addPromoItem

!!! function "addPromoItem( ```uint32``` item )"
	Grant a specific one-time promotional item to the current user.

	This can be safely called from the client because the items it can grant can be locked down via policies in the itemdefs. One of the primary scenarios for this call is to grant an item to users who also own a specific other game. This can be useful if your game has custom UI for showing a specific promo item to the user otherwise if you want to grant multiple promotional items then use [addPromoItems](#addpromoitems) or [grantPromoItems](#grantpromoitems).

	Any items that can be granted _must_ have a "promo" attribute in their itemdef. That promo item list a set of app IDs that the user must own to be granted this given item. This version will grant all items that have promo attributes specified for them in the configured item definitions. This allows adding additional promotional items without having to update the game client. For example the following will allow the item to be granted if the user owns either TF2 or SpaceWar.

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#AddPromoItem){ .md-button .md-button--store target="_blank" }

### addPromoItems

!!! function "addPromoItems( ```PoolIntArray``` items )"
	Grant a specific one-time promotional item to the current user.

	This can be safely called from the client because the items it can grant can be locked down via policies in the itemdefs. One of the primary scenarios for this call is to grant an item to users who also own a specific other game. If you want to grant a single promotional item then use [addPromoItems](#addpromoitems). If you want to grant all possible promo items then use [grantPromoItems](#grantpromoitems).

	Any items that can be granted _must_ have a "promo" attribute in their itemdef. That promo item list a set of app ID's that the user must own to be granted this given item. This version will grant all items that have promo attributes specified for them in the configured item definitions. This allows adding additional promotional items without having to update the game client. For example the following will allow the item to be granted if the user owns either TF2 or SpaceWar.

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#AddPromoItems){ .md-button .md-button--store target="_blank" }

### checkResultSteamID

!!! function "checkResultSteamID( ```uint64_t``` steam_id_expected, ```int32``` this_inventory_handle = 0 )"
	Checks whether an inventory result handle belongs to the specified Steam ID.
	This is important when using [deserializeResult](#deserializeresult), to verify that a remote player is not pretending to have a different user's inventory.

	**Returns:** bool

	**Note:** If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#CheckResultSteamID){ .md-button .md-button--store target="_blank" }

### consumeItem

!!! function "consumeItem( ```uint64_t``` item_consume, ```uint32``` quantity )"	
	Consumes items from a user's inventory. If the quantity of the given item goes to zero, it is permanently removed.

	Once an item is removed it cannot be recovered. This is not for the faint of heart - if your game implements item removal at all, a high-friction UI confirmation process is highly recommended.

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

	**Note:** You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#ConsumeItem){ .md-button .md-button--store target="_blank" }

### deserializeResult

!!! function "deserializeResult( ```PoolIntArray``` buffer )"
	Deserializes a result set and verifies the signature bytes.

	This call has a potential soft-failure mode where the handle status is set to 27. [getResultItems](#getresultitems) will still succeed in this mode. The "expired" result could indicate that the data may be out of date - not just due to timed expiration (one hour), but also because one of the items in the result set may have been traded or consumed since the result set was generated. You could compare the timestamp from [getResultTimestamp](#getresulttimestamp) to [getServerRealTime](utils.md#getserverrealtime) to determine how old the data is. You could simply ignore the "expired" result code and continue as normal, or you could request the player with expired data to send an updated result set.

	You should call [checkResultSteamID](#checkresultsteamid) on the result handle when it completes to verify that a remote player is not pretending to have a different user's inventory.

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#DeserializeResult){ .md-button .md-button--store target="_blank" }

### destroyResult

!!! function "destroyResult( ```int32``` this_inventory_handle = 0 )"
	Destroys a result handle and frees all associated memory.

	**Returns:** void

	**Note:** If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#DestroyResult){ .md-button .md-button--store target="_blank" }

### exchangeItems

!!! function "exchangeItems( ```PoolIntArray``` output_items, ```PoolIntArray``` output_quantity, ```PoolIntArray``` input_items, ```PoolIntArray``` input_quantity )"
	Grant one item in exchange for a set of other items.

	This can be used to implement crafting recipes or transmutations, or items which unpack themselves into other items (e.g., a chest).

	The caller of this API passes in the requested item and an array of existing items and quantities to exchange for it. The API currently takes an array of items to generate but at this time the size of that array must be 1 and the quantity of the new item must be 1.

	Any items that can be granted **must** have an exchange attribute in their itemdef. The exchange attribute specifies a set of recipes that are valid exchanges for this item. Exchange recipes are evaluated atomically by the Inventory Service; if the supplied components do not match the recipe, or do not contain sufficient quantity, the exchange will fail.

	Will allow the item to be exchanged for either one #101 and one #102, five #103s or three #104s and three #105s. See the [Steam Inventory Schema](https://partner.steamgames.com/doc/features/inventory/schema){ .md-button .md-button--store target="_blank" } documentation for more details.

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

	**Note:** You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#ExchangeItems){ .md-button .md-button--store target="_blank" }

### generateItems

!!! function "generateItems( ```PoolIntArray``` items, ```PoolIntArray``` quantity )"
	Grants specific items to the current user, for developers only.

	This API is only intended for prototyping - it is only usable by Steam accounts that belong to the publisher group for your game.

	You can pass in an array of items, identified by their item defintion int's and optionally a second array of corresponding quantities for each item. The length of these arrays _must</strong> match!
	

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

	**Note:** You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GenerateItems){ .md-button .md-button--store target="_blank" }

### getAllItems

!!! function "getAllItems()"
	Start retrieving all items in the current users inventory.

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

	**Note:** Calls to this function are subject to rate limits and may return cached results if called too frequently. It is suggested that you call this function only when you are about to display the user's full inventory, or if you expect that the inventory may have changed.

	**Note:** You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetAllItems){ .md-button .md-button--store target="_blank" }
		

### getItemDefinitionProperty

!!! function "getItemDefinitionProperty( ```uint32``` definition, ```string``` name )"
	Gets a string property from the specified item definition. Gets a property value for a specific item definition.

	Note that some properties (for example, "name") may be localized and will depend on the current Steam language settings (see [getCurrentGameLanguage</strong>). Property names are always ASCII alphanumeric and underscores.

	Pass in NULL for **name** to get a comma-separated list of available property names.

	**Note:** Call [loadItemDefinitions](#loaditemdefinitions) first, to ensure that items are ready to be used before calling [getItemDefinitionProperty](#getitemdefinitionproperty).

	**Returns:** Dictionary.

	Contains the following keys:

    * property (string)
    * success (bool)

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetItemDefinitionProperty){ .md-button .md-button--store target="_blank" }

### getItemsByID

!!! function "getItemsByID( ```PoolIntArray``` id_array )"
	Gets the state of a subset of the current user's inventory.

	The subset is specified by an array of item instance IDs.

	The results from this call can be serialized using [serializeResult</strong> and passed to other players to "prove" that the current user owns specific items, without exposing the user's entire inventory. For example, you could call this with the IDs of the user's currently equipped items and serialize this to a buffer, and then transmit this buffer to other players upon joining a game.

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

	**Note:** You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetItemsByID){ .md-button .md-button--store target="_blank" }

### getItemPrice

!!! function "getItemPrice( ```uint32``` definition )"
	After a successful call to [requestPrices](#requestprices), you can call this method to get the pricing for a specific item definition.

	**Returns:** dictionary

	Contains the following keys:

	* price (uint64_t)
	* base_price (uint64_t)

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetItemPrice){ .md-button .md-button--store target="_blank" }

### getItemsWithPrices

!!! function "getItemsWithPrices()"
	After a successful call to [requestPrices](#requestprices), you can call this method to get all the pricing for applicable item definitions.

	**Returns:** array

	Contains a list of:
	
	* price_group (dictionary)

	Contains the following keys:

	* item (int32)
	* price (uint64_t)
	* base_prices (uint64_t)

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetItemsWithPrices){ .md-button .md-button--store target="_blank" }

### getResultItemProperty

!!! function "getResultItemProperty( ```uint32``` index, ```string``` name, ```int32``` this_inventory_handle )"
	Gets the dynamic properties from an item in an inventory result set.

	Property names are always composed of ASCII letters, numbers, and/or underscores.

	If the results do not fit in the given buffer, partial results may be copied.

	**Returns:** string.

	**Note:** If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetResultItemProperty){ .md-button .md-button--store target="_blank" }

### getResultItems

!!! function "getResultItems( ```int32``` this_inventory_handle )"
	Get the items associated with an inventory result handle.

	**Returns:** array

	Containing a list of:
	
	* item_info (dictionary)

	Contains the following keys:

	* item_id (int)
	* item_definition (int)
	* flags (int)
	* quantity (int)

	**Note:** If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetResultItems){ .md-button .md-button--store target="_blank" }

### getResultStatus

!!! function "getResultStatus( ```int32``` this_inventory_handle )"
	Find out the status of an asynchronous inventory result handle.
	This is a polling equivalent to registering a callback function for [inventory_result_ready](#inventory_result_ready).

	**Note:** If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally stored ID.

	**Returns:** int / Result enum.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetResultStatus){ .md-button .md-button--store target="_blank" }

### getResultTimestamp

!!! function "getResultTimestamp( ```int32``` this_inventory_handle )"
	Gets the server time at which the result was generated.

	You can compare this value against [getServerRealTime](utils.md#getserverrealtime) to determine the age of the result.

	**Returns:** uint32

	**Note:** If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally stored ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GetResultTimestamp){ .md-button .md-button--store target="_blank" }

### grantPromoItems

!!! function "grantPromoItems()"
	Grant all potential one-time promotional items to the current user.

	This can be safely called from the client because the items it can grant can be locked down via policies in the itemdefs. One of the primary scenarios for this call is to grant an item to users who also own a specific other game. If you want to grant specific promotional items rather than all of them see: [addPromoItem](#addpromoitem) and [addPromoItems](#addpromoitems).

	Any items that can be granted _must_ have a "promo" attribute in their itemdef. That promo item list a set of app ID's that the user must own to be granted this given item. This version will grant all items that have promo attributes specified for them in the configured item definitions. This allows adding additional promotional items without having to update the game client. For example the following will allow the item to be granted if the user owns either TF2 or SpaceWar.

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

	**Note:** You must call [destroyResult](#destroyresult) on the provided inventory result when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#GrantPromoItems){ .md-button .md-button--store target="_blank" }

### loadItemDefinitions

!!! function "loadItemDefinitions()"
	Triggers an asynchronous load and refresh of item definitions.

	Item definitions are a mapping of "definition IDs" (integers between 1 and 999999999) to a set of string properties. Some of these properties are required to display items on the Steam community web site. Other properties can be defined by applications. There is no reason to call this function if your game hardcoded the numeric definition IDs (e.g. purple face mask = 20, blue weapon mod = 55) and does not allow for adding new item types without a client patch.

	Triggers a [inventory_definition_update](#inventory_definition_update) callback.

	**Returns:** bool

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#LoadItemDefinitions){ .md-button .md-button--store target="_blank" }

### requestEligiblePromoItemDefinitionsIDs

!!! function "requestEligiblePromoItemDefinitionsIDs( ```uint64_t``` steam_id )"
	Request the list of "eligible" promo items that can be manually granted to the given user.

	These are promo items of type "manual" that won't be granted automatically. An example usage of this is an item that becomes available every week.

	Triggers a [inventory_eligible_promo_item](#inventory_eligible_promo_item) callback.

	**Returns:** void

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#RequestEligiblePromoItemDefinitionsIDs){ .md-button .md-button--store target="_blank" }

### requestPrices

!!! function "requestPrices()"
	Request prices for all item definitions that can be purchased in the user's local currency. A [inventory_request_prices_result](#inventory_request_prices_result) call result will be returned with the user's local currency code. After that, you can call [getItemsWithPrices](#getitemswithprices) to get prices for all the known item definitions, or [getItemPrice](#getitemprice) for a specific item definition.

	Triggers a [inventory_request_prices_result](#inventory_request_prices_result) callback.

	**Returns:** void

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#RequestPrices){ .md-button .md-button--store target="_blank" }

### serializeResult

!!! function "serializeResult( ```int32``` this_inventory_handle )"
	Serialized result sets contain a short signature which can't be forged or replayed across different game sessions.

	A result set can be serialized on the local client, transmitted to other players via your game networking, and deserialized by the remote players. This is a secure way of preventing hackers from lying about posessing rare/high-value items. Serializes a result set with signature bytes to an output buffer. The size of a serialized result depends on the number items which are being serialized. When securely transmitting items to other players, it is recommended to use [getItemsByID](#getitemsbyid) first to create a minimal result set.

	**Returns:** PackedByteArray (4.x) / PoolByteArray (3.x)

	**Note:** If the argument **this_inventory_handle** is omitted, GodotSteam will use the internally stored ID.

	Results have a built-in timestamp which will be considered "expired" after an hour has elapsed. See [deserializeResult](#deserializeresult) for expiration handling.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SerializeResult){ .md-button .md-button--store target="_blank" }

### startPurchase

!!! function "startPurchase( ```PoolIntArray``` items, ```PoolIntArray``` quantity )"
	Starts the purchase process for the user, given a "shopping cart" of item definitions that the user would like to buy. The user will be prompted in the Steam Overlay to complete the purchase in their local currency, funding their Steam Wallet if necessary, etc.

	If the purchase process was started successfully, then **order_id** and **transaction_id** will be valid in the [inventory_start_purchase_result](#inventory_start_purchase_result) call result.

	If the user authorizes the transaction and completes the purchase, then the callback [inventory_result_ready](#inventory_result_ready) will be triggered and you can then retrieve what new items the user has acquired.

	Triggers a [inventory_start_purchase_result](#inventory_start_purchase_result) callback.

	**Returns:** void 

	**Note:** You must call [destroyResult](#destroyresult) on the inventory result for when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#StartPurchase){ .md-button .md-button--store target="_blank" }

### transferItemQuantity

!!! function "transferItemQuantity( ```uint64_t``` item_id, ```uint32``` quantity, ```uint64_t``` item_destination, ```bool``` split )"
	Transfer items between stacks within a user's inventory.

	This can be used to stack, split, and moving items. The source and destination items must have the same itemdef id. To move items onto a destination stack specify the source, the quantity to move, and the destination item id. To split an existing stack, pass -1 into **item_destination**. A new item stack will be generated with the requested quantity.

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

	**Note:** Tradability/marketability restrictions are copied along with transferred items. The destination stack receives the latest tradability/marketability date of any item in its composition.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#TransferItemQuantity){ .md-button .md-button--store target="_blank" }

### triggerItemDrop

!!! function "triggerItemDrop( ```uint32``` definition )"
	Trigger an item drop if the user has played a long enough period of time.

	This period can be customized in two places:

	* At the application level within Inventory Service: Playtime Item Grants. This will automatically apply to all "playtimegenerator" items that do not specify any overrides.
	* In an individual "playtimegenerator" item definition. The settings would take precedence over any application-level settings.

	Only item definitions which are marked as "playtime item generators" can be spawned.
	Typically this function should be called at the end of a game or level or match or any point of significance in the game in which an item drop could occur. The granularity of the playtime generator settings is in minutes, so calling it more frequently than minutes is not useful and will be rate limited in the Steam client. The Steam servers will perform playtime accounting to prevent too-frequent drops. The servers will also manage adding the item to the players inventory.

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#TriggerItemDrop){ .md-button .md-button--store target="_blank" }

### startUpdateProperties

!!! function "startUpdateProperties()"
	Starts a transaction request to update dynamic properties on items for the current user. This call is rate-limited by user, so property modifications should be batched as much as possible (e.g. at the end of a map or game session). After calling [setPropertyInt](#setpropertyint), [setPropertyfloat](#setpropertyfloat),[setPropertyBool](#setpropertybool), [setPropertyString](#setpropertystring), or [removeProperty](#removeproperty) for all the items that you want to modify, you will need to call [submitUpdateProperties](#submitupdateproperties) to send the request to the Steam servers. A [inventory_result_ready](#inventory_result_ready) callback will be fired with the results of the operation.

	**Returns:** void

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#StartUpdateProperties){ .md-button .md-button--store target="_blank" }

### submitUpdateProperties

!!! function "submitUpdateProperties()"
	Submits the transaction request to modify dynamic properties on items for the current user. See [startUpdateProperties](#startupdateproperties).

	**Returns:** int32

	The inventory handle, which is also stored internally. It will overwrite any previously stored inventory handle.

	**Note:** You must call [destroyResult](#destroyresult) on the provided inventory result for when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SubmitUpdateProperties){ .md-button .md-button--store target="_blank" }

### removeProperty

!!! function "removeProperty( ```uint64_t``` item_id, ```string``` name, ```int32``` this_inventory_update_handle )"
	Removes a [dynamic property](https://partner.steamgames.com/doc/features/inventory/dynamicproperties){ target="\_blank" } for the given item.

	**Returns:** bool

	**Note:** If the argument **this_inventory_update_handle** is omitted, GodotSteam will use the internally stored ID. This is different from **this_inventory_handle** and is stored separately.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#RemoveProperty){ .md-button .md-button--store target="_blank" }

### setPropertyString

!!! function "setPropertyString( ```uint64_t``` item_id, ```string``` name, ```string``` value, ```int32``` this_inventory_update_handle )"
	Sets a [dynamic property](https://partner.steamgames.com/doc/features/inventory/dynamicproperties){ target="\_blank" } for the given item. Supported value types are strings.

	**Returns:** bool

	**Note:** If the argument **this_inventory_update_handle** is omitted, GodotSteam will use the internally stored ID. This is different from **this_inventory_handle** and is stored separately.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SetProperty){ .md-button .md-button--store target="_blank" }

### setPropertyBool

!!! function "setPropertyBool( ```uint64_t``` item_id, ```string``` name, ```bool``` value, ```int32``` this_inventory_update_handle )"
	Sets a [dynamic property](https://partner.steamgames.com/doc/features/inventory/dynamicproperties){ target="\_blank" } for the given item. Supported value types are boolean.

	**Returns:** bool

	**Note:** If the argument **this_inventory_update_handle** is omitted, GodotSteam will use the internally stored ID. This is different from **this_inventory_handle** and is stored separately.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SetProperty){ .md-button .md-button--store target="_blank" }

### setPropertyInt

!!! function "setPropertyInt( ```uint64_t``` item_id, ```string``` name, ```uint64_t``` value, ```int32``` this_inventory_update_handle )"
	Sets a [dynamic property](https://partner.steamgames.com/doc/features/inventory/dynamicproperties){ target="\_blank" } for the given item. Supported value types are 64 bit integers.

	**Returns:** bool

	**Note:** If the argument **this_inventory_update_handle** is omitted, GodotSteam will use the internally stored ID. This is different from **this_inventory_handle** and is stored separately.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SetProperty){ .md-button .md-button--store target="_blank" }

### setPropertyFloat

!!! function "setPropertyFloat( ```uint64_t``` item_id, ```string``` name, ```float``` value, ```int32``` this_inventory_update_handle )"
	Sets a [dynamic property](https://partner.steamgames.com/doc/features/inventory/dynamicproperties){ target="\_blank" } for the given item. Supported value types are 32 bit floats.

	**Returns:** bool

	**Note:** If the argument **this_inventory_update_handle** is omitted, GodotSteam will use the internally stored ID. Also note this different from **this_inventory_handle** and is stored separately.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SetProperty){ .md-button .md-button--store target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to run ```Steam.run_callbacks()``` in your ```_process()``` function to receive them.

### inventory_definition_update

!!! function "inventory_definition_update"
	This callback is triggered whenever item definitions have been updated, which could be in response to [loadItemDefinitions](#loaditemdefinitions) or any time new item definitions are available (eg, from the dynamic addition of new item types while players are still in-game).

	**Returns:**

	* definitions (array)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryDefinitionUpdate_t){ .md-button .md-button--store target="_blank" }

### inventory_eligible_promo_item

!!! function "inventory_eligible_promo_item"
	Returned when you have requested the list of "eligible" promo items that can be manually granted to the given user. These are promo items of type "manual" that won't be granted automatically.

	**Returns:**

	* result (int)
	* cached (bool)
	* definitions (array)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryEligiblePromoItemDefIDs_t){ .md-button .md-button--store target="_blank" }

### inventory_full_update

!!! function "inventory_full_update"
	Triggered when [getAllItems](#getallitems) successfully returns a result which is newer / fresher than the last known result. (It will not trigger if the inventory hasn't changed, or if results from two overlapping calls are reversed in flight and the earlier result is already known to be stale/out-of-date.) The regular [inventory_result_ready](#inventory_result_ready) callback will still be triggered immediately afterwards; this is an additional notification for your convenience.

	**Returns:**

	* inventory_handle (int32)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryFullUpdate_t){ .md-button .md-button--store target="_blank" }

### inventory_request_prices_result

!!! function "inventory_request_prices_result"
	Returned after [requestPrices](#requestprices) is called.

	**Returns:**

	* result (int)
	* currency (string)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryRequestPricesResult_t){ .md-button .md-button--store target="_blank" }

### inventory_result_ready

!!! function "inventory_result_ready"
	This is fired whenever an inventory result transitions from k_EResultPending to any other completed state, see [getResultStatus](#getresultstatus) for the complete list of states. There will always be exactly one callback per handle.

	**Returns:**

	* result (int)
	* inventory_handle (int32)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryResultReady_t){ .md-button .md-button--store target="_blank" }

### inventory_start_purchase_result

!!! function "inventory_start_purchase_result"
	Returned after [startPurchase](#startpurchase) is called.

	**Returns:**

	* result (string)
	* order_id (uint64_t)
	* transaction_id (uint64_t)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamInventory#SteamInventoryStartPurchaseResult_t){ .md-button .md-button--store target="_blank" }

{==
## :material-numeric: Enums
==}

### SteamItemFlags

Enumerator | Value
---------- | -----
STEAM_ITEM_NO_TRADE | (1<<0)
STEAM_ITEM_REMOVED | (1<<8)
STEAM_ITEM_CONSUMED | (1<<9)
