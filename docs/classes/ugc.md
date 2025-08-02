---
title: UGC
description: A class reference for UGC / Steam Workshop.
icon: material/toolbox
---

# UGC

Functions to create, consume, and interact with the [Steam Workshop](https://partner.steamgames.com/doc/features/workshop){ target="\_blank" }.

!!! info "Available in both the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } and [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### addAppDependency

!!! function "addAppDependency( `uint64_t` published_file_id, `uint32_t` app_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item to add a dependency to. |
    | app_id | uint32_t | The required app / dlc for this file. |

    Adds a dependency between the given item and the appid. This list of dependencies can be retrieved by calling [getAppDependencies](#getappdependencies). This is a soft-dependency that is displayed on the web. It is up to the application to determine whether the item can actually be used or not.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[add_app_dependency_result](#add_app_dependency_result) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddAppDependency){ .md-button .md-button--doc_classes target="_blank" }

### addContentDescriptor

!!! function "addContentDescriptor( `uint64_t` update_handle, `int` descriptor_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize.
    | descriptor_id | [UGCContentDescriptorID enum](#ugccontentdescriptorid) | The required app / dlc for this file.

    Sets the given **descriptor_id** on the item.

    !!! returns "Returns: bool"

### addDependency

!!! function "addDependency( `uint64_t` published_file_id, `uint64_t` child_published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item to add a dependency to. |
    | child_published_file_id | uint64_t | The item dependency to add to the parent. |

    Adds a workshop item as a dependency to the specified item. If the published_file_id item is of type [WORKSHOP_FILE_TYPE_COLLECTION](remote_storage.md#workshopfiletype) (2), than the child_published_file_id is simply added to that collection. Otherwise, the dependency is a soft one that is displayed on the web and can be retrieved via the ISteamUGC API using a combination of the **children** key returned from [getQueryUGCResult](#getqueryugcresult) and [getQueryUGCChildren](#getqueryugcchildren).

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[add_ugc_dependency_result](#add_ugc_dependency_result) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddDependency){ .md-button .md-button--doc_classes target="_blank" }

### addExcludedTag

!!! function "addExcludedTag( `uint64_t` query_handle, `string` tag_name )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | tag_name | string | The tag to exclude from a pending UGC query. |

    Adds a excluded tag to a pending UGC Query. This will only return UGC without the specified tag.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid, if the UGC query handle is from [createQueryUGCDetailsRequest](#createqueryugcdetailsrequest) or **tag_name** was blank.

	!!! info "Notes"
        This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddExcludedTag){ .md-button .md-button--doc_classes target="_blank" }

### addItemKeyValueTag

!!! function "addItemKeyValueTag( `uint64_t` update_handle, `string` key, `string` value )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | key | string | The key to set on the item. |
    | value | string | A value to map to the key. |

	Adds a key-value tag pair to an item. Keys can map to multiple different values (1-to-many relationship).

	Key names are restricted to alpha-numeric characters and the '_' character. Both keys and values cannot exceed 255 characters in length. Key-value tags are searchable by exact match only.

	!!! returns "Returns: bool"
        Return true upon success. False if the UGC update handle is invalid, if "key" or "value" are invalid because they are blank or have exceeded the maximum length, or if you are trying to add more than 100 key-value tags in a single update.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddItemKeyValueTag){ .md-button .md-button--doc_classes target="_blank" }

### addItemPreviewFile

!!! function "addItemPreviewFile( `uint64_t` update_handle, `string` preview_file, `int` type )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | preview_file | string | Absolute path to the local image. |
    | preview_type | [ItemPreviewType enum](#itempreviewtype) | The type of this preview file. |

    Adds an additional preview file for the item.

	Then the format of the image should be one that both the web and the application (if necessary) can render, and must be under 1MB. Suggested formats include JPG, PNG and GIF.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC update handle is invalid.

	!!! info "Notes"
        Using 1 or 2 in type are not currently supported with this API. For YouTube videos you should use [addItemPreviewVideo](#additempreviewvideo).

        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddItemPreviewFile){ .md-button .md-button--doc_classes target="_blank" }

### addItemPreviewVideo

!!! function "addItemPreviewVideo( `update_handle` query_handle, `string` video_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | video_id | string | The YouTube video ID to add. (e.g. "jHgZh4GV9G0") |

    Adds an additional video preview from YouTube for the item.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC update handle is invalid.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddItemPreviewVideo){ .md-button .md-button--doc_classes target="_blank" }

### addItemToFavorites

!!! function "addItemToFavorites( `uint32_t` app_id, `uint64_t` published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID that this item belongs to. |
    | published_file_id | uint64_t | The workshop item to add to the user's favorites list. |

    Adds a workshop item to the users favorites list.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[user_favorite_items_list_changed](#user_favorite_items_list_changed) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddItemToFavorites){ .md-button .md-button--doc_classes target="_blank" }

### addRequiredKeyValueTag

!!! function "addRequiredKeyValueTag( `uint64_t` query_handle, `string` key, `string` value )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | key | string | The key-value key that must be attached to the UGC to receive it. |
    | value | string | The key-value value associated with key that must be attached to the UGC to receive it. |

    Adds a required key-value tag to a pending UGC Query. This will only return workshop items that have a key and a value.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid or if **key** or **value** are empty.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddRequiredKeyValueTag){ .md-button .md-button--doc_classes target="_blank" }

### addRequiredTag

!!! function "addRequiredTag( `uint64_t` query_handle, `string` tag_name )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | tag_name | string | The tag must be attached to the UGC to receive it. |

	Adds a required tag to a pending UGC Query. This will only return UGC with the specified tag.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid, if the UGC query handle is from [createQueryUGCDetailsRequest](#createqueryugcdetailsrequest), or **tag_name** was empty.

    !!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddRequiredTag){ .md-button .md-button--doc_classes target="_blank" }

### addRequiredTagGroup

!!! function "addRequiredTagGroup( `uint64_t` query_handle, `array` tag_array )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | tag_array | array | A set of tags where at least one of the tags must attach to the UGC. |

    Adds the requirement that the returned items from the pending UGC Query have at least one of the tags in the given set (logical "or"). For each tag group that is added, at least one tag from each group is required to be on the matching items.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid, if the UGC query handle is from [createQueryUGCDetailsRequest](#createqueryugcdetailsrequest), or **tag_array** was empty.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddRequiredTagGroup){ .md-button .md-button--doc_classes target="_blank" }

### createItem

!!! function "createItem( `uint32_t` app_id, `int` file_type )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID that will be using this item. |
    | file_type | [WorkshopFileType enum](remote_storage.md#workshopfiletype) | The type of UGC to create. |

	Creates a new workshop item with no content attached yet.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[item_created](#item_created) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#CreateItem){ .md-button .md-button--doc_classes target="_blank" }

### createQueryAllUGCRequest

!!! function "createQueryAllUGCRequest( `UGCQuery` query_type, `UGCMatchingUGCType` matching_type, `uint32_t` creator_id, `uint32_t` consumer_id, `int` page )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_type | [UGCQuery enum](#ugcquery) | Used to specify the sorting and filtering for this call. |
    | matching_type | [UGCMatchingUGCType enum](#ugcmatchingugctype) | Used to specify the type of UGC queried for. |
    | creator_id | uint32_t | This should contain the app ID of the app where the item was created. This may be different than **consumer_id** if your item creation tool is a separate app ID. |
    | consumer_id | uint32_t | This should contain the app ID for the current game or application. **Do not** pass the app ID of the workshop item creation tool if that is a separate app ID. |
    | page | int | The page number of the results to receive. This should start at 1 on the first call. |

    Query for all matching UGC. You can use this to list all of the available UGC for your app.

	This will return up to 50 results. You can make subsequent calls to this function, increasing the page each time to get the next set of results.

	To query for the UGC associated with a single user you can use [createQueryUserUGCRequest](#createqueryuserugcrequest) instead.

	!!! returns "Returns: uint64_t"
        Returns a new UGC query handle upon success and [UGC_QUERY_HANDLE_INVALID](#constants) in the following situations:

        * Either **creator_id** or **consumer_id** is not set to the currently running app.
        * **page** was less than 1.
        * An internal error occurred.

        This handle can be used to further customize the query before sending it out with [sendQueryUGCRequest](#sendqueryugcrequest).

	!!! info "Notes"
        Either consumer_id or creator_id must have a valid app ID.

        You must release the handle returned by this function by calling [releaseQueryUGCRequest](#releasequeryugcrequest) when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#CreateQueryAllUGCRequest){ .md-button .md-button--doc_classes target="_blank" }

### createQueryUGCDetailsRequest

!!! function "createQueryUGCDetailsRequest( `array` published_file_id_array )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id_array | array | 

	Query for the details of specific workshop items.

	This will return up to 50 results.

	To query all the UGC for your app you can use [createQueryAllUGCRequest](#createqueryallugcrequest) instead.

	!!! returns "Returns: uint64_t"
        Returns a new UGC query handle upon success and [UGC_QUERY_HANDLE_INVALID](#constants) in the following situations:

        * **published_file_id_array** has no elements.
        * An internal error occurred.

        This handle can be used to further customize the query before sending it out with [sendQueryUGCRequest](#sendqueryugcrequest).

	!!! info "Notes"
        You must release the handle returned by this function by calling [releaseQueryUGCRequest](#releasequeryugcrequest) when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#CreateQueryUGCDetailsRequest){ .md-button .md-button--doc_classes target="_blank" }
		

### createQueryUserUGCRequest

!!! function "createQueryUserUGCRequest( `uint64_t` steam_id, `int` list_type, `int` matching_ugc_type, `int` sort_order, `uint32_t` creator_id, `uint32_t` consumer_id, `int` page )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID to query the UGC for. |
    | list_type | [UserUGCList enum](#userugclist) | Used to specify the type of list to get. If the currently logged in user is different than the user specified in **steam_id**, then some options are not allowed. |
    | matching_ugc_type | [UGCMatchingUGCType enum](#ugcmatchingugctype) | Used to specify the type of UGC queried for. |
    | sort_order | [UserUGCListSortOrder enum](#userugclistsortorder) | Used to specify the order that the list will be sorted in. |
    | creator_id | uint32_t | This should contain the app ID of the app where the item was created. This may be different than **consumer_id** if your item creation tool is a separate app ID. |
    | consumer_id | uint32_t | This should contain the app ID for the current game or application. **Do not** pass the app ID of the workshop item creation tool if that is a separate app ID. |
    | page | int | The page number of the results to receive. This should start at 1 on the first call. |

    Query UGC associated with a user. You can use this to list the UGC the user is subscribed to amongst other things.

	This will return up to 50 results.

	To query all the UGC for your app you can use [createQueryAllUGCRequest](#createqueryallugcrequest) instead.

	!!! returns "Returns: uint64_t"
        Returns a new UGC query handle upon success and [UGC_QUERY_HANDLE_INVALID](#constants) in the following situations:

        * Either **creator_id** or **consumer_id** is not set to the currently running app.
        * **page** was less than 1.
        * The given **list_type** is not supported by users other than the one requesting the details.
        * An internal error occurred.

        This handle can be used to further customize the query before sending it out with [sendQueryUGCRequest](#sendqueryugcrequest).

	!!! info "Notes"
        Either consumer_id or creator_id must have a valid app ID.

        You must release the handle returned by this function by calling [releaseQueryUGCRequest](#releasequeryugcrequest) when you are done with it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#CreateQueryUserUGCRequest){ .md-button .md-button--doc_classes target="_blank" }
		

### deleteItem

!!! function "deleteItem( `uint64_t` published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The item to delete. |

    Deletes the item without prompting the user.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[item_deleted](#item_deleted) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#DeleteItem){ .md-button .md-button--doc_classes target="_blank" }

### downloadItem

!!! function "downloadItem( `uint64_t` published_file_id, `bool` high_priority )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The item to download. |
    | high_priority | bool | Start the download in high priority mode, pausing any existing in-progress Steam downloads and immediately begin downloading this workshop item. |

    Download new or update already installed item.

	If the return value is true then register and wait for the callback [item_downloaded](#item_downloaded) before calling [getItemInstallInfo](#getiteminstallinfo) or accessing the workshop item on disk.

	If the user is not subscribed to the item (e.g. a Game Server using anonymous login), the workshop item will be downloaded and cached temporarily.

	If the workshop item has an item state of 8, then this function can be called to initiate the update. Do not access the workshop item on disk until the callback [item_downloaded](#item_downloaded) is called.

	!!! returns "Returns: bool"
        Returns true if the download was successfully started; otherwise, false if **published_file_id** is invalid or the user is not logged on.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#DownloadItem){ .md-button .md-button--doc_classes target="_blank" }

### getAppDependencies

!!! function "getAppDependencies( `uint64_t` published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item to get app dependencies for. |

	Get the app dependencies associated with the given published_file_id. These are "soft" dependencies that are shown on the web. It is up to the application to determine whether an item can be used or not.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[get_app_dependencies_result](#get_app_dependencies_result) callback

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetAppDependencies){ .md-button .md-button--doc_classes target="_blank" }

### getItemDownloadInfo

!!! function "getItemDownloadInfo( `uint64_t` published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item to get the download info for. |

	Get info about a pending download of a workshop item that has 8 set.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| ret | bool | Returns true if the download information was available; otherwise, false.
    	| downloaded | uint64_t | Returns the current bytes downloaded.
    	| total | uint64_t | Returns the total bytes. This is only valid after the download has started.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetItemDownloadInfo){ .md-button .md-button--doc_classes target="_blank" }

### getItemInstallInfo

!!! function "getItemInstallInfo( `uint64_t` published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item to get the install info for. |

	Gets info about currently installed content on the disc for workshop items that have [ITEM_STATE_INSTALLED](#itemstate) (4) set.

	Calling this sets the "used" flag on the workshop item for the current player and adds it to their [USER_UGC_LIST_USED_OR_PLAYED](#userugclist) (7) list.

	If [ITEM_STATE_LEGACY_ITEM](#itemstate) (2) is set then folder contains the path to the legacy file itself, not a folder.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| ret | bool | Returns true if the workshop item is already installed. False if the **folder_size** is 0, the workshop item has no content, or the  workshop item is not installed.
    	| size | int | Returns the size of the workshop item in bytes.
    	| folder | string | Returns the absolute path to the folder containing the content by copying it.
    	| folder_size | uint32_t | The size of **folder** in bytes.
    	| timestamp | uint32_t | Returns the time when the workshop item was last updated.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetItemInstallInfo){ .md-button .md-button--doc_classes target="_blank" }

### getItemState

!!! function "getItemState( `uint64_t` published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item to get the state for. |

	Gets the current state of a workshop item on this client.

	!!! returns "Returns: uint32_t"
        Returns the item state. Should be used with the [ItemState](#itemstate) flags to determine the state of the workshop item.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetItemState){ .md-button .md-button--doc_classes target="_blank" }

### getItemUpdateProgress

!!! function "getItemUpdateProgress( `uint64_t` update_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The update handle to get the progress for.

	Gets the progress of an item update.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| status | [ItemUpdateStatus enum](#itemupdatestatus) | The current status.
    	| processed | uint64_t | Returns the current number of bytes uploaded.
    	| total | uint64_t | Returns the total number of bytes that will be uploaded.

    !!! info "Notes"
    	You may notice that data comes back in **processed** and **total** in status 2; unsure if this is a bug or intended.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetItemUpdateProgress){ .md-button .md-button--doc_classes target="_blank" }

### getNumSubscribedItems

!!! function "getNumSubscribedItems( `bool` include_locally_disabled = false )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | include_locally_disabled | bool | Whether to include locally disabled items in the return value or not. Defaults to false. |

    Gets the total number of items the current user is subscribed to for the game or application.

	!!! returns "Returns: uint32_t"
        Returns 0 if called from a game server.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetNumSubscribedItems){ .md-button .md-button--doc_classes target="_blank" }

### getNumSupportedGameVersions

!!! function "getNumSupportedGameVersions( `uint64_t` query_handle, `uint32_t` index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item from the query. |

    Get the number of supported game versions for this UGC content.

	!!! returns "Returns: uint32_t"
        The number of versions available for this item.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed call result](#ugc_query_completed).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetNumSupportedGameVersions){ .md-button .md-button--doc_classes target="_blank" }

### getQueryUGCAdditionalPreview

!!! function "getQueryUGCAdditionalPreview( `uint64_t` query_handle, `uint32_t` index, `uint32_t` preview_index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |
    | preview_index | uint32_t | The index of the additional preview to get the details of. |

    Retrieve the details of an additional preview associated with an individual workshop item after receiving a querying UGC call result.

	You should call this in a loop to get the details of all the workshop items returned.

	Before calling this you should call [getQueryUGCNumAdditionalPreviews](#getqueryugcnumadditionalpreviews) to get number of additional previews.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| success | bool | Return true upon success, indicates that **url_or_video** and **type** have been filled out. Otherwise, false if the UGC query handle is invalid, the index is out of bounds, or **preview** is out of bounds.
    	| handle | uint64_t | The UGC query handle to get the results from.
    	| index | uint32_t | The index of the item to get the details of.
    	| preview | uint32_t | The index of the additional preview to get the details of.
    	| url_or_video | string | Returns a URL or Video ID by copying it into this string.
    	| filename | string | Returns the original file name.
    	| type | [ItemPreviewType](#itempreviewtype) | The type of preview that was returned.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCAdditionalPreview){ .md-button .md-button--doc_classes target="_blank" }

### getQueryUGCChildren

!!! function "getQueryUGCChildren( `uint64_t` query_handle, `uint32_t` index, `int32_t` child_count )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |
    | child_count | uint32_t | Should be **num_children** from returned dictionary of [getQueryUGCResult](#getqueryugcresult). |

    Retrieve the IDs of any child items of an individual workshop item after receiving a querying UGC call result. These items can either be a part of a collection or some other dependency (see [addDependency](#adddependency)).

	You should create published_file_id with num_children provided in returned dictionary after getting the UGC details with [getQueryUGCResult](#getqueryugcresult).

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| success | bool | Returns true upon success, indicates that **children** has been filled out. Otherwise, false if the UGC query handle is invalid or the index is out of bounds.
    	| handle | uint64_t | The UGC query handle to get the results from.
    	| index | uint32_t | The index of the item to get the details of.
    	| children | Array | The UGC children.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCChildren){ .md-button .md-button--doc_classes target="_blank" }


### getQueryUGCContentDescriptors

!!! function "getQueryUGCContentDescriptors( `uint64_t` query_handle, `uint32_t` index, `uint32_t` max_entries = 5 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |
    | max_entries | uint32_t | Size of the returned array. Defaults to 5. |

    Get an UGC item's content descriptors for mature content.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| result | uint32_t | Returns the number of [UGC content descriptor ID enums](#ugccontentdescriptorid) set on the item.
    	| handle | uint64_t | The UGC query handle.
    	| index | uint32_t | The index of the item to get the details of.
    	| descriptors | array | An array of [UGC content descriptor ID enums](#ugccontentdescriptorid).

    	Descriptors array will contain a list of integers that correspond to the following UGC enums for descriptors:

    	* Nudity or sexual content - 1
    	* Frequent violence or gore - 2
    	* Adult only sexual content - 3
    	* Gratuitous sexual content - 4
    	* Any mature content - 5

	!!! info "Notes"
        Valve does not have any documentation covering this function at this time.

### getQueryUGCKeyValueTag

!!! function "getQueryUGCKeyValueTag( `uint64_t` query_handle, `uint32_t` index, `uint32_t` key_value_tag_index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |
    | key_value_tag_index | uint32_t | The index of the tag to get the details of. |

    Retrieve the details of a key-value tag associated with an individual workshop item after receiving a querying UGC call result.

	You should call this in a loop to get the details of all the workshop items returned.

	Before calling this you should call [getQueryUGCNumKeyValueTags](#getqueryugcnumkeyvaluetags) to get number of tags.

	!!! returns "Returns: dictionary"
        Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| success | bool | Returns true upon success, indicates that **key** and **value** have been filled out. Otherwise, false if the UGC query handle is invalid, the index is out of bounds, or **key_value_tag_index** is out of bounds.
    	| handle | uint64_t | The UGC query handle.
    	| index | uint32_t | The index for the next iteration of this call.
    	| tag | uint32_t | The index of the tag for the next iteration of this call.
    	| key | string | The key name for this UGC item.
    	| value | string | The value for this UGC item.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCKeyValueTag){ .md-button .md-button--doc_classes target="_blank" }

### getQueryUGCMetadata

!!! function "getQueryUGCMetadata( `uint64_t` query_handle, `uint32_t` index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |

    Retrieve the developer set metadata of an individual workshop item after receiving a querying UGC call result.
	You should call this in a loop to get the details of all the workshop items returned.

	!!! returns "Returns: string"
        May be empty if the UGC query handle is invalid or the index is out of bounds.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCMetadata){ .md-button .md-button--doc_classes target="_blank" }

### getQueryUGCNumAdditionalPreviews

!!! function "getQueryUGCNumAdditionalPreviews( `uint64_t` query_handle, `uint32_t` index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |

    Retrieve the number of additional previews of an individual workshop item after receiving a querying UGC call result.

	You can then call [getQueryUGCAdditionalPreview](#getqueryugcadditionalpreview) to get the details of each additional preview.

	!!! returns "Returns: uint32_t"
        The number of additional previews associated with the specified workshop item. Returns 0 if the UGC query handle is invalid or the index is out of bounds.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCNumAdditionalPreviews){ .md-button .md-button--doc_classes target="_blank" }		

### getQueryUGCNumKeyValueTags

!!! function "getQueryUGCNumKeyValueTags( `uint64_t` query_handle, `uint32_t` index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |

    Retrieve the number of key-value tags of an individual workshop item after receiving a querying UGC call result.

	You can then call [getQueryUGCKeyValueTag](#getqueryugckeyvaluetag) to get the details of each tag.

	!!! returns "Returns: uint32_t"
        The number of key-value tags associated with the specified workshop item. Returns 0 if the UGC query handle is invalid or the index is out of bounds.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCNumKeyValueTags){ .md-button .md-button--doc_classes target="_blank" }

### getQueryUGCNumTags

!!! function "getQueryUGCNumTags( `uint64_t` query_handle, `uint32_t` index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |

    Retrieve the number of tags for an individual workshop item after receiving a querying UGC call result.

	You can then call [getQueryUGCTag](#getqueryugctag) to get the tag name or [getQueryUGCTagDisplayName](#getqueryugctagdisplayname) to get the localized tag string (if any).

	!!! returns "Returns: uint32_t"
        The number of key-value tags associated with the specified workshop item. Returns 0 if the UGC query handle is invalid or the index is out of bounds.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCNumTags){ .md-button .md-button--doc_classes target="_blank" }

### getQueryUGCPreviewURL

!!! function "getQueryUGCPreviewURL( `uint64_t` query_handle, `uint32_t` index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |

    Retrieve the URL to the preview image of an individual workshop item after receiving a querying UGC call result.

	You can use this URL to download and display the preview image instead of having to download it using the handle_preview_file key in the return dictionary from [getQueryUGCResult](#getqueryugcresult).

	!!! returns "Returns: string"
        May be empty if the UGC query handle is invalid or the index is out of bounds.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCPreviewURL){ .md-button .md-button--doc_classes target="_blank" }

### getQueryUGCResult

!!! function "getQueryUGCResult( `uint64_t` query_handle, `uint32_t` index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize.
    | index | uint32_t | The index of the item to get the details of.

    Retrieve the details of an individual workshop item after receiving a querying UGC call result.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| result | [Result enum](main.md#result) | The result of the operation.
    	| file_id | uint64_t | The globally unique item handle to this piece of UGC.
    	| file_type | uint64_t | The type of the item.
    	| creator_app_id | uint32_t | App ID of the app that created this item.
    	| consumer_app_id | uint32_t | App ID of the app that will consume this item.
    	| title | string | The title of the item.
    	| description | string | The description of the item.
    	| steam_id_owner | uint64_t | Steam ID of the user who created this content.
    	| time_created | uint32_t | Time when the published item was created, provided in Unix epoch format (time since Jan 1st, 1970).
    	| time_updated | uint32_t | Time when the published item was last updated, provided in Unix epoch format (time since Jan 1st, 1970).
    	| time_added_to_user_list | uint32_t | Time when the user added the published item to their list (not always applicable), provided in Unix epoch format (time since Jan 1st, 1970).
    	| visibility | int | The visibility of the item.
    	| banned | bool | Whether the item was banned.
    	| accepted_for_use | bool | Whether the developer of this app has specifically flagged this item as accepted in the Workshop.
    	| tags_truncated | bool | Whether the list of tags was too long to be returned in the provided buffer, and were therefore truncated.
    	| tags | string | Comma separated list of all tags associated with this item.
    	| handle_file | uint64_t | The handle of the primary file.
    	| handle_preview_file | uint64_t | The handle of the preview file.
    	| file_name | string | The cloud filename of the primary file.
    	| file_size | int32 | The file size of the primary file.
    	| preview_file_size | int32 | The file size of the preview file.
    	| url | string | The URL associated with this item. (For a video or a website.)
    	| votes_up | uint32_t | Number of votes up.
    	| votes_down | uint32_t | Number of votes down.
    	| score | float | The bayesian average for up votes / total votes, between 0 and 1.
    	| num_children | uint32_t | The number of items in the collection if **file_type** is [WORKSHOP_FILE_TYPE_COLLECTION](remote_storage.md#workshopfiletype), or the number of items this specific item has a dependency on (see [addDependency](#adddependency)).
    	| total_files_size | uint64_t | Total size of all the files.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCResult){ .md-button .md-button--doc_classes target="_blank" }

### getQueryUGCStatistic

!!! function "getQueryUGCStatistic( `uint64_t` query_handle, `uint32_t` index, `ItemStatistic` stat_type )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |
    | stat_type | [ItemStatistic enum](#itemstatistic) | The statistic to retrieve. |

    Retrieve various statistics of an individual workshop item after receiving a querying UGC call result.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| success | bool | Returns true upon success, indicates that **value** has been filled out. Otherwise, false if the UGC query handle is invalid, the index is out of bounds, or **stat_type** was invalid.
    	| handle | uint64_t | The UGC query handle.
    	| index | uint32_t | The index of the item for the next iteration of the call.
    	| type | int | The statistic retrieved.
    	| value | uint64_t | Returns the value associated with the specified statistic.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCStatistic){ .md-button .md-button--doc_classes target="_blank" }

### getQueryUGCTag

!!! function "getQueryUGCTag( `uint64_t` query_handle, `uint32_t` index, `uint32_t` tag_index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |
    | tag_index | uint32_t | The index of the tag. |

    Retrieve the "nth" tag associated with an individual workshop item after receiving a querying UGC call result.

	Before calling this you should call [getQueryUGCNumTags](#getqueryugcnumtags) to get number of tags.

	!!! returns "Returns: string"

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCTag){ .md-button .md-button--doc_classes target="_blank" }

### getQueryUGCTagDisplayName

!!! function "getQueryUGCTagDisplayName( `uint64_t` query_handle, `uint32_t` index, `uint32_t` tag_index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |
    | tag_index | uint32_t | The index of the tag. |

    Retrieve the "nth" display string (usually localized) for a tag, which is associated with an individual workshop item after receiving a querying UGC call result.

	Before calling this you should call [getQueryUGCNumTags](#getqueryugcnumtags) to get number of tags.

	!!! returns "Returns: string"

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed](#ugc_query_completed) call result.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetQueryUGCTagDisplayName){ .md-button .md-button--doc_classes target="_blank" }

### getSubscribedItems

!!! function "getSubscribedItems( `bool` include_locally_disabled = false )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | include_locally_disabled | bool | Whether to include locally disabled items in the return value or not. Defaults to false. |

    Gets a list of all of the items the current user is subscribed to for the current game.

	!!! returns "Returns: array"
        A list of published file IDs.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetSubscribedItems){ .md-button .md-button--doc_classes target="_blank" }

### getSupportedGameVersionData

!!! function "getSupportedGameVersionData( `uint64_t` query_handle, `uint32_t` index, `uint32_t` version_index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | index | uint32_t | The index of the item to get the details of. |
    | version_index | uint32_t | The index of the item version. |

    Use this function to retrieve what Steam (beta) branches this item version is valid for. If the minimum branch is an empty string, then it is valid for all versions up to the maximum branch. If the maximum branch is an empty string, then the this item version is valid for every branch published after the minimum branch. If both strings are empty, then this item version is valid for all Steam branches. The version that is downloaded by the Steam client is dictated by what versions are valid for the item and what Steam (beta) branch the user has opted into.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| min | string | The minimum Steam (beta) branch this version of the item supports.
    	| max | string | The maximum Steam (beta) branch version this version of the item supports.

	!!! info "Notes"
        This must only be called with the handle obtained from a successful [ugc_query_completed call result](#ugc_query_completed).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetSupportedGameVersionData){ .md-button .md-button--doc_classes target="_blank" }

### getWorkshopEULAStatus

!!! function "getWorkshopEULAStatus( )"
	Asynchronously retrieves data about whether the user accepted the Workshop EULA for the current app.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetWorkshopEULAStatus){ .md-button .md-button--doc_classes target="_blank" }

### getUserContentDescriptorPreferences

!!! function "getUserContentDescriptorPreferences( `uint32_t` max_entries = 5 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | max_entries | uint32_t | The maximum number of content descriptor preferences sent back. Defaults to 5. |

    Return the user's community content descriptor preferences.

	!!! returns "Returns: array"
        A list of [UGC content descriptor ID enums](#ugccontentdescriptorid).

### getUserItemVote

!!! function "getUserItemVote( `uint64_t` published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item to get the user vote for. |

    Gets the users vote status on a workshop item.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[get_item_vote_result](#get_item_vote_result) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetUserItemVote){ .md-button .md-button--doc_classes target="_blank" }

### initWorkshopForGameServer

!!! function "initWorkshopForGameServer( `uint32_t` workshop_depot_id, `string` folder )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | workshop_depot_id | uint32_t | The depot ID of the game server. |
    | folder | string | The absolute path to store the workshop content. |

    Lets game servers set a specific workshop folder before issuing any UGC commands.

	This is helpful if you want to support multiple game servers running out of the same install folder.

	!!! returns "Returns: bool"
        Returns true upon success; otherwise, false if the calling user is not a game server or if the workshop is currently updating its content.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#BInitWorkshopForGameServer){ .md-button .md-button--doc_classes target="_blank" }

### releaseQueryUGCRequest

!!! function "releaseQueryUGCRequest( `uint64_t` query_handle )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |

    Releases a UGC query handle when you are done with it to free up memory.

	!!! returns "Returns: bool"
        Weirdly, always returns true.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#ReleaseQueryUGCRequest){ .md-button .md-button--doc_classes target="_blank" }

### removeAllItemKeyValueTags

!!! function "removeAllItemKeyValueTags( `uint64_t` update_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The update handle to remove all the key-value tags on. |

    Remove all existing key-value tags. You can add new ones with [addItemKeyValueTag](#additemkeyvaluetag).

	!!! returns "Returns: bool"

	---
	[ :material-tag-plus: Added GodotSteam 4.16](../changelog/godot4.md/#version-416){ .md-button .md-button--changes target="_blank" }

### removeAppDependency

!!! function "removeAppDependency( `uint64_t` published_file_id, `uint32_t` app_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item to get the user vote for. |
    | app_id | uint32_t | The app ID dependency that will be removed for this item. |

    Removes the dependency between the given item and the appid. This list of dependencies can be retrieved by calling [getAppDependencies](#getappdependencies).

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[remove_app_dependency_result](#remove_app_dependency_result) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#RemoveAppDependency){ .md-button .md-button--doc_classes target="_blank" }

### removeContentDescriptor

!!! function "removeContentDescriptor( `uint64_t` update_handle, `UGCContentDescriptorID` descriptor_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The update handle to remove the content descript for. |
    | descriptor_id | [UGCContentDescriptorID enum](#ugccontentdescriptorid) | The descriptor to be removed for this item. |

    Removes a mature content descriptor from a piece of UGC.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#RemoveContentDescriptor){ .md-button .md-button--doc_classes target="_blank" }

### removeDependency

!!! function "removeDependency( `uint64_t` published_file_id, `uint64_t` child_published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item to remove the dependency from. |
    | child_published_file_id | uint64_t | The dependency to remove from the parent. |

    Removes a workshop item as a dependency from the specified item.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[remove_ugc_dependency_result](#remove_ugc_dependency_result) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#RemoveDependency){ .md-button .md-button--doc_classes target="_blank" }

### removeItemFromFavorites

!!! function "removeItemFromFavorites( `uint32_t` app_id, `uint64_t` published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID that this item belongs to. |
    | published_file_id | uint64_t | The workshop item to remove from the user's favorites list. |

    Removes a workshop item from the users favorites list.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[user_favorite_items_list_changed](#user_favorite_items_list_changed) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#RemoveItemFromFavorites){ .md-button .md-button--doc_classes target="_blank" }

### removeItemKeyValueTags

!!! function "removeItemKeyValueTags( `uint64_t` update_handle, `string` key )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The update handle to remove the key-value tags for. |
    | key | string | The key to remove from the item. |

    Removes an existing key value tag from an item.

	You can only call this up to 100 times per item update. If you need remove more tags than that you'll need to make subsequent item updates.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC update handle is invalid or if you are trying to remove more than 100 key-value tags in a single update.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#RemoveItemKeyValueTags){ .md-button .md-button--doc_classes target="_blank" }

### removeItemPreview

!!! function "removeItemPreview( `uint64_t` update_handle, `uint32_t` index )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The update handle to remove the item preview from. |
    | index | uint32_t | The index of the preview to be removed. |

    Removes an existing preview from an item.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#RemoveItemPreview){ .md-button .md-button--doc_classes target="_blank" }

### sendQueryUGCRequest

!!! function "sendQueryUGCRequest( `uint64_t` query_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query request handle to send. |

    Send a UGC query to Steam.

	This must be called with a handle obtained from [createQueryUserUGCRequest](#createqueryuserugcrequest), [createQueryAllUGCRequest](#createqueryallugcrequest), or [createQueryUGCDetailsRequest](#createqueryugcdetailsrequest) to actually send the request to Steam.

	Before calling this you should use one or more of the following APIs to customize your query: [addRequiredTag](#addrequiredtag), [addExcludedTag](#addexcludedtag), [setReturnOnlyIDs](#setreturnonlyids), [setReturnKeyValueTags](#setreturnkeyvaluetags), [setReturnLongDescription](#setreturnlongdescription), [setReturnMetadata](#setreturnmetadata), [setReturnChildren](#setreturnchildren), [setReturnAdditionalPreviews](#setreturnadditionalpreviews), [setReturnTotalOnly](#setreturntotalonly), [setLanguage](#setlanguage), [setAllowCachedResponse](#setallowcachedresponse), [setCloudFileNameFilter](#setcloudfilenamefilter), [setMatchAnyTag](#setmatchanytag), [setSearchText](#setsearchtext), [setRankedByTrendDays](#setrankedbytrenddays), or [addRequiredKeyValueTag](#addrequiredkeyvaluetag).

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[ugc_query_completed](#ugc_query_completed) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SendQueryUGCRequest){ .md-button .md-button--doc_classes target="_blank" }

### setAdminQuery

!!! function "setAdminQuery( `uint64_t` query_handle, `bool` admin_query )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | admin_query | bool | Whether or not to return hidden items. |

    Admin queries return hidden items.

	!!! returns "Returns: bool"

    !!! info "Notes"
        Seems like this should be a query_handle instead of update_handle but they are both uint64_t handles so this may be a naming mistake on Valve's part.

### setAllowCachedResponse

!!! function "setAllowCachedResponse( `uint64_t` query_handle, `uint32_t` max_age_seconds )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | max_age | uint32_t | The maximum amount of time that an item can be returned without a cache invalidation. |

    Sets whether results will be returned from the cache for the specific period of time on a pending UGC Query.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid.

	!!! info "Notes"
        This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetAllowCachedResponse){ .md-button .md-button--doc_classes target="_blank" }

### setAllowLegacyUpload

!!! function "setAllowLegacyUpload( `uint64_t` update_handle, `bool` allow_legacy_upload )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | allow_legacy_upload | bool | Whether to allow legacy upload or not. |

	Use legacy upload for a single small file. The parameter to [`setItemContent()`](#setitemcontent) should either be a directory with one file or the full path to the file.  The file must also be less than 10MB in size.

	!!! returns "Returns: bool"

	---
	[ :material-tag-plus: Added GodotSteam 4.16](../changelog/godot4.md/#version-416){ .md-button .md-button--changes target="_blank" }

### setCloudFileNameFilter

!!! function "setCloudFileNameFilter( `uint64_t` query_handle, `string` match_cloud_filename )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | match_cloud_filename | string | The filename to match. |

    Sets to only return items that have a specific filename on a pending UGC Query.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid, if the UGC query handle is not from [createQueryUserUGCRequest](#createqueryuserugcrequest) or if **match_cloud_filename** is empty.

	!!! info "Notes"
        This can only be used with [createQueryUserUGCRequest](#createqueryuserugcrequest).

        This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetCloudFileNameFilter){ .md-button .md-button--doc_classes target="_blank" }

### setItemContent

!!! function "setItemContent( `uint64_t` update_handle, `string` content_folder )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | content_folder | string | The absolute path to a local folder containing the content for the item. |

    Sets the folder that will be stored as the content for an item; this should be an absolute path.

	For efficient upload and download, files should not be merged or compressed into single files (e.g. zip files).

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC update handle is invalid.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetItemContent){ .md-button .md-button--doc_classes target="_blank" }

### setItemDescription

!!! function "setItemDescription( `uint64_t` update_handle, `string` description )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | description | string | The new description of the item. |

    Sets a new description for an item.

	The description must be limited to the length defined by [PUBLISHED_DOCUMENT_DESCRIPTION_MAX](#constants) (8000).

	You can set what language this is for by using [setItemUpdateLanguage](#setitemupdatelanguage), if no language is set then "english" is assumed.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC update handle is invalid.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetItemDescription){ .md-button .md-button--doc_classes target="_blank" }

### setItemMetadata

!!! function "setItemMetadata( `uint64_t` update_handle, `string` metadata )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | metadata | string | The new metadata for this item. |

    Sets arbitrary metadata for an item. This metadata can be returned from queries without having to download and install the actual content.

	The metadata must be limited to the size defined by [DEVELOPER_METADATA_MAX](#constants) (5000).

	!!! returns "Returns: bool"
        Return true upon success. False if the UGC update handle is invalid, or if **metadata** is longer than [DEVELOPER_METADATA_MAX](#constants).

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetItemMetadata){ .md-button .md-button--doc_classes target="_blank" }

### setItemPreview

!!! function "setItemPreview( `uint64_t` update_handle, `string` preview_file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | preview_file | string | The absolute path to a local preview image file for the item. |

    Sets the primary preview image for this item. **preview_file** points to a local image file, which _must be_ under 1MB in size. The format should be one that both the web and the application (if necessary) can render. Suggested formats include JPG, PNG and GIF. The given path must be absolute.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC update handle is invalid.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetItemPreview){ .md-button .md-button--doc_classes target="_blank" }

### setItemTags

!!! function "setItemTags( `uint64_t` update_handle, `array` tag_array, `bool` allow_admin_tags = false )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | tag_array | array | The list of tags to set on this item. | 
    | allow_admin_tags | bool | Whether or not to allow admin tags. Defaults to false. |

    Sets arbitrary developer specified tags on an item.

	Each tag must be limited to 255 characters. Tag names can only include printable characters, excluding ','. For reference on what characters are allowed, refer to [http://en.cppreference.com/w/c/string/byte/isprint](http://en.cppreference.com/w/c/string/byte/isprint){ target="_blank" }

	!!! returns "Returns: bool"
        Return true upon success. False if the UGC update handle is invalid, or if one of the tags is invalid either due to exceeding the maximum length or because it is empty.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetItemTags){ .md-button .md-button--doc_classes target="_blank" }

### setItemTitle

!!! function "setItemTitle( `uint64_t` update_handle, `string` title )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | title | string | The new title of the item. |

    Sets a new title for an item.

	The title must be limited to the size defined by [PUBLISHED_DOCUMENT_TITLE_MAX](remote_storage.md#constants) (128).

	You can set what language this is for by using [setItemUpdateLanguage](#setitemupdatelanguage), if no language is set then "english" is assumed.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC update handle is invalid.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetItemTitle){ .md-button .md-button--doc_classes target="_blank" }

### setItemUpdateLanguage

!!! function "setItemUpdateLanguage( `uint64_t` update_handle, `string` language )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | language | string | The language of the title and description that will be set in this update. |

    Sets the language of the title and description that will be set in this item update.

	This must be in the format of the [API language code](https://partner.steamgames.com/doc/store/localization#supported_languages){ target="_blank" }.

	If this is not set then "english" is assumed.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC update handle is invalid.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetItemUpdateLanguage){ .md-button .md-button--doc_classes target="_blank" }

### setItemVisibility

!!! function "setItemVisibility( `uint64_t` update_handle, `RemoteStoragePublishedFileVisibility` visibility )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | visibility | [RemoteStoragePublishedFileVisibility enum](remote_storage.md#remotestoragepublishedfilevisibility) | The visibility to set.

    Sets the visibility of an item.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC update handle is invalid.

	!!! info "Notes"
        This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetItemVisibility){ .md-button .md-button--doc_classes target="_blank" }

### setItemsDisabledLocally

!!! function "setItemsDisabledLocally( `PackedInt64Array` published_file_ids, `bool` disabled_locally )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | disabled_locally | bool | Whether the items should be disabled locally or not |

    Sets whether the item should be disabled locally or not. This means that it will not be returned in [getSubscribedItems()](#getsubscribeditems) by default.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetItemsDisabledLocally){ .md-button .md-button--doc_classes target="_blank" }

### setLanguage

!!! function "setLanguage( `uint64_t` query_handle, `string` language )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | language | string | The language to return. |

    Sets the language to return the title and description in for the items on a pending UGC Query.

	This must be in the format of the [API language code](https://partner.steamgames.com/doc/store/localization#supported_languages){ target="_blank" }.

	If this is not set then "english" is assumed.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid.

	!!! info "Notes"
        This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetLanguage){ .md-button .md-button--doc_classes target="_blank" }

### setMatchAnyTag

!!! function "setMatchAnyTag( `uint64_t` query_handle, `bool` match_any_tag )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | match_any_tag | bool | Should the item just need to have one required tag (true), or all of them? (false) |

    Sets whether workshop items will be returned if they have one or more matching tag, or if all tags need to match on a pending UGC Query.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid or if the UGC query handle is not from [createQueryAllUGCRequest](#createqueryallugcrequest).

	!!! info "Notes"
        This can only be used with [createQueryAllUGCRequest](#createqueryallugcrequest).

        This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetMatchAnyTag){ .md-button .md-button--doc_classes target="_blank" }

### setRankedByTrendDays

!!! function "setRankedByTrendDays( `uint64_t` query_handle, `uint32_t` days )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | days | uint32_t | Sets the number of days to rank items over. Valid values are 1-365 inclusive. |

    Sets whether the order of the results will be updated based on the rank of items over a number of days on a pending UGC Query.  The maximum value is 365.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid, if the UGC query handle is not from [createQueryAllUGCRequest](#createqueryallugcrequest) or if the [UGCQuery](#ugcquery) of the query is not one of:

        * [UGC_QUERY_RANKED_BY_TREND](#ugcquery)
        * [UGC_QUERY_RANKED_BY_PLAYTIME_TREND](#ugcquery)
        * [UGC_QUERY_RANKED_BY_AVERAGE_PLAYTIME_TREND](#ugcquery)
        * [UGC_QUERY_RANKED_BY_VOTES_UP](#ugcquery)
        * [UGC_QUERY_RANKED_BY_PLAYTIME_SESSIONS_TREND](#ugcquery)

	!!! info "Notes"
        This can only be used with [createQueryAllUGCRequest](#createqueryallugcrequest).

        This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetRankedByTrendDays){ .md-button .md-button--doc_classes target="_blank" }

### setRequiredGameVersions

!!! function "setRequiredGameVersions( `uint64_t` query_handle, `String` game_branch_min, `String` game_branch_max )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | game_branch_min | string | The minimum game branch version. |
    | game_branch_max | string | The maximum game branch version. |

    An empty string for either parameter means that it will match any version on that end of the range. This will only be applied if the actual content has been changed.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetRequiredGameVersions){ .md-button .md-button--doc_classes target="_blank" }

### setReturnAdditionalPreviews

!!! function "setReturnAdditionalPreviews( `uint64_t` query_handle, `bool` return_additional_previews )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | return_additional_previews | bool | Return the additional previews for the items? |

    Sets whether to return any additional images/videos attached to the items on a pending UGC Query.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid.

	!!! info "Notes"
       This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetReturnAdditionalPreviews){ .md-button .md-button--doc_classes target="_blank" }

### setReturnChildren

!!! function "setReturnChildren( `uint64_t` query_handle, `bool` return_children )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | return_children | bool | Return the IDs of children of the items? |

    Sets whether to return the IDs of the child items of the items on a pending UGC Query.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid.

	!!! info "Notes"
       This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetReturnChildren){ .md-button .md-button--doc_classes target="_blank" }

### setReturnKeyValueTags

!!! function "setReturnKeyValueTags( `uint64_t` query_handle, `bool` return_key_value_tags )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | return_key_value_tags | bool | Return any key-value tags for the items? |

    Sets whether to return any key-value tags for the items on a pending UGC Query.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid.

	!!! info "Notes"
       This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetReturnKeyValueTags){ .md-button .md-button--doc_classes target="_blank" }

### setReturnLongDescription

!!! function "setReturnLongDescription( `uint64_t` query_handle, `bool` return_long_description )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | return_long_description | bool | Return the long description for the items? |

    Sets whether to return the full description for the items on a pending UGC Query.

	If you don't set this then you only receive the summary which is the description truncated at 255 bytes.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid.

	!!! info "Notes"
       This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetReturnLongDescription){ .md-button .md-button--doc_classes target="_blank" }

### setReturnMetadata

!!! function "setReturnMetadata( `uint64_t` query_handle, `bool` return_metadata )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | return_metadata | bool | Return the metadata for the items? |

    Sets whether to return the developer specified metadata for the items on a pending UGC Query.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid.

	!!! info "Notes"
       This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetReturnMetadata){ .md-button .md-button--doc_classes target="_blank" }

### setReturnOnlyIDs

!!! function "setReturnOnlyIDs( `uint64_t` query_handle, `bool` return_only_ids )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | return_only_ids | bool | Return only the IDs of items? |

    Sets whether to only return IDs instead of all the details on a pending UGC Query.

	This is useful for when you don't need all the information (e.g. you just want to get the IDs of the items a user has in their favorites list.)

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid or if the UGC query handle is from [createQueryUGCDetailsRequest](#createqueryugcdetailsrequest).

	!!! info "Notes"
       This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetReturnOnlyIDs){ .md-button .md-button--doc_classes target="_blank" }

### setReturnPlaytimeStats

!!! function "setReturnPlaytimeStats( `uint64_t` query_handle, `uint32_t` days )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | days | uint32_t | The number of days worth of playtime stats to return. |

    Sets whether to return the the playtime stats on a pending UGC Query.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid.

	!!! info "Notes"
       This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetReturnPlaytimeStats){ .md-button .md-button--doc_classes target="_blank" }

### setReturnTotalOnly

!!! function "setReturnTotalOnly( `uint64_t` query_handle, `bool` return_total_only )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | return_total_only | bool | Only return the total number of items? |

    Sets whether to only return the the total number of matching items on a pending UGC Query.

	The actual items will not be returned when [ugc_query_completed](#ugc_query_completed) is called.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid or if the UGC query handle is from [createQueryUGCDetailsRequest](#createqueryugcdetailsrequest).

	!!! info "Notes"
       This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetReturnTotalOnly){ .md-button .md-button--doc_classes target="_blank" }

### setSearchText

!!! function "setSearchText( `uint64_t` query_handle, `string` search_text )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | query_handle | uint64_t | The UGC query handle to customize. |
    | search_text | string | The text to be searched for. |

    Sets a string to that items need to match in either the title or the description on a pending UGC Query.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC query handle is invalid, if the UGC query handle is not from [createQueryAllUGCRequest](#createqueryallugcrequest), or if **search_text** is empty.

	!!! info "Notes"
        This can only be used with [createQueryAllUGCRequest](#createqueryallugcrequest).

        This must be set before you send a UGC Query handle using [sendQueryUGCRequest](#sendqueryugcrequest).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetSearchText){ .md-button .md-button--doc_classes target="_blank" }

### setSubscriptionsLoadOrder

!!! function "setSubscriptionsLoadOrder( `PackedInt64Array` published_file_ids )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_ids | PackedInt64Array | Array of of file IDs in the order they should be loaded. |

    Set the local load order for these items. If there are any items not in the given list, they will sort by the time subscribed.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetSubscriptionsLoadOrder){ .md-button .md-button--doc_classes target="_blank" }

### setTimeCreatedDateRange

!!! function "setTimeCreatedDateRange( `uint64_t` update_handle, `uint32_t` start, `uint32_t` end )"
	   | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | start | uint32_t | The Unix time creation start date. |
    | end | uint32_t | The Unix time creation end date. |

    Set the time range this item was created.  Both dates should be in Unix time.

	!!! returns "Returns: bool"    

### setTimeUpdatedDateRange

!!! function "setTimeUpdatedDateRange( `uint64_t` update_handle, `uint32_t` start, `uint32_t` end )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | start | uint32_t | The Unix time updated start date. |
    | end | uint32_t | The Unix time updated end date. |

    Set the time range this item was updated.

	!!! returns "Returns: bool"

### setUserItemVote

!!! function "setUserItemVote( `uint64_t` published_file_id, `bool` vote_up )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item ID to vote on. |
    | vote_up | bool | Vote up (true) or down (false)? |

    Allows the user to rate a workshop item up or down.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[set_user_item_vote](#set_user_item_vote) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetUserItemVote){ .md-button .md-button--doc_classes target="_blank" }

### showWorkshopEULA

!!! function "showWorkshopEULA( )"
	Show the app's latest Workshop EULA to the user in an overlay window, where they can accept it or not.

	!!! returns "Returns: bool"
        Returns true upon success. False if the overlay cannot be shown.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#ShowWorkshopEULA){ .md-button .md-button--doc_classes target="_blank" }

### startItemUpdate

!!! function "startItemUpdate( `uint32_t` app_id, `uint64_t` published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID that will be using this item. |
    | published_file_id | uint64_t | The item to update. |

    Starts the item update process.

	This gets you a handle that you can use to modify the item before finally sending off the update to the server with [submitItemUpdate](#submititemupdate).

	!!! returns "Returns: uint64_t"
        A handle that you can use with future calls to modify the item before finally sending the update.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#StartItemUpdate){ .md-button .md-button--doc_classes target="_blank" }

### startPlaytimeTracking

!!! function "startPlaytimeTracking( `array` published_file_ids )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_ids | array |  The array of workshop items you want to start tracking. Maximum of 100 items. |

	Start tracking playtime on a set of workshop items.

	When your app shuts down, playtime tracking will automatically stop.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[start_playtime_tracking](#start_playtime_tracking) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#StartPlaytimeTracking){ .md-button .md-button--doc_classes target="_blank" }

### stopPlaytimeTracking

!!! function "stopPlaytimeTracking( `array` published_file_ids )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_ids | array |  The array of workshop items you want to stop tracking. Maximum of 100 items. |

	Stop tracking playtime on a set of workshop items.

	When your app shuts down, playtime tracking will automatically stop.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[stop_playtime_tracking](#stop_playtime_tracking) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#StopPlaytimeTracking){ .md-button .md-button--doc_classes target="_blank" }

### stopPlaytimeTrackingForAllItems

!!! function "stopPlaytimeTrackingForAllItems( )"
	Stop tracking playtime of all workshop items.

	When your app shuts down, playtime tracking will automatically stop.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[stop_playtime_tracking](#stop_playtime_tracking) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#StopPlaytimeTrackingForAllItems){ .md-button .md-button--doc_classes target="_blank" }

### submitItemUpdate

!!! function "submitItemUpdate( `uint64_t` update_handle, `string` change_note )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | change_note | string | A brief description of the changes made. This is optional and can be left blank. Defaults to empty string. |

    Uploads the changes made to an item to the Steam Workshop.

	You can track the progress of an item update with [getItemUpdateProgress](#getitemupdateprogress).

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[item_updated](#item_updated) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SubmitItemUpdate){ .md-button .md-button--doc_classes target="_blank" }

### subscribeItem

!!! function "subscribeItem( `uint64_t` published_file_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item to subscribe to. |

    Subscribe to a workshop item. It will be downloaded and installed as soon as possible.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[subscribe_item](remote_storage.md#subscribe_item) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SubscribeItem){ .md-button .md-button--doc_classes target="_blank" }

### suspendDownloads

!!! function "suspendDownloads( `bool` suspend )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | suspend | bool | Suspend (true) or Resume (false) workshop downloads? |

	Suspends and resumes all workshop downloads.

	If you call this with suspend set to true then downloads will be suspended until you resume them by setting suspend to false or when the game ends.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SuspendDownloads){ .md-button .md-button--doc_classes target="_blank" }

### unsubscribeItem

!!! function "unsubscribeItem( `uint64_t` published_file_id )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | published_file_id | uint64_t | The workshop item to unsubscribe from. |

	Unsubscribe from a workshop item. This will result in the item being removed after the game quits.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[unsubscribe_item](remote_storage.md#unsubscribe_item) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#UnsubscribeItem){ .md-button .md-button--doc_classes target="_blank" }

### updateItemPreviewFile

!!! function "updateItemPreviewFile( `uint64_t` update_handle, `uint32_t` index, `string` preview_file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | index | uint32_t | The index of the preview file from 0 to [getQueryUGCNumAdditionalPreviews](#getqueryugcnumadditionalpreviews). |
    | preview_file | string | Absolute path to the local image. |

    Updates an additional video preview from YouTube for the item.

    If the preview type is an image then the format should be one that both the web and the application (if necessary) can render, and must be under 1MB. Suggested formats include JPG, PNG and GIF.

    This must be an absolute path to the file.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC update handle is invalid.

	!!! info "Notes"
       This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#UpdateItemPreviewFile){ .md-button .md-button--doc_classes target="_blank" }

### updateItemPreviewVideo

!!! function "updateItemPreviewVideo( `uint64_t` update_handle, `uint32_t` index, `string` video_id )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | update_handle | uint64_t | The workshop item update handle to customize. |
    | index | uint32_t | The index of the preview file from 0 to [getQueryUGCNumAdditionalPreviews](#getqueryugcnumadditionalpreviews). |
    | video_id | string | The YouTube video to add. (e.g. "jHgZh4GV9G0"). |

	Updates an additional video preview from YouTube for the item.

	!!! returns "Returns: bool"
        Returns true upon success. False if the UGC update handle is invalid.

	!!! info "Notes"
       This must be set before you submit the UGC update handle using [submitItemUpdate](#submititemupdate).

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#UpdateItemPreviewVideo){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### add_app_dependency_result

!!! function "add_app_dependency_result"
	The result of a call to [addAppDependency](#addappdependency).

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| result | [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The parent workshop item that the dependency was added to.
        | app_id | uint32_t | The app or dlc ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddAppDependencyResult_t){ .md-button .md-button--doc_classes target="_blank" }	

### add_ugc_dependency_result

!!! function "add_ugc_dependency_result"
	The result of a call to [addDependency](#adddependency).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The parent workshop item that the dependency was added to.
        | child_id | uint32_t | The child workshop item which was added as a dependency to the parent item.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#AddUGCDependencyResult_t){ .md-button .md-button--doc_classes target="_blank" }

### get_app_dependencies_result

!!! function "get_app_dependencies_result"
	Called when getting the app dependencies for an item.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The workshop item to get app dependencies for.
        | app_ids | PackedInt32Array | Number of app dependencies in **app_dependencies**.
        | app_dependencies | uint32_t | Array of app dependencies.
        | total_app_dependencies | uint32_t | Total app dependencies found.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetAppDependenciesResult_t){ .md-button .md-button--doc_classes target="_blank" }

### get_item_vote_result

!!! function "get_item_vote_result"
	Called when getting the users vote status on an item.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The workshop item to get the vote status.
        | vote_up | bool | Has the user voted the item up?
        | vote_down | bool | Has the user voted the item down?
        | vote_skipped | bool | Has the user skipped voting on this item?

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#GetUserItemVoteResult_t){ .md-button .md-button--doc_classes target="_blank" }

### item_created

!!! function "item_created"
	Called when a new workshop item has been created.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The new item's unique ID.
        | accept_tos | bool | Does the user need to accept the Steam Workshop legal agreement (true) or not (false)? See the [Workshop Legal Agreement for more information](https://partner.steamgames.com/doc/features/workshop/implementation#Legal){ target="\_blank" }.

    **result** can return as:

    * [RESULT_OK](main.md#result) if the operation completed successfully.
    * [RESULT_INSUFFICIENT_PRIVILEGE](main.md#result) if the user is currently restricted from uploading content due to a hub ban, account lock, or community ban. They would need to contact Steam Support.
    * [RESULT_BANNED](main.md#result) if the user doesn't have permission to upload content to this hub because they have an active VAC or Game ban.
    * [RESULT_TIMEOUT](main.md#result) if the operation took longer than expected. Have the user retry the creation process.
    * [RESULT_NOT_LOGGED_ON](main.md#result) if the user is not currently logged into Steam.
    * [RESULT_SERVICE_UNAVAILABLE](main.md#result) if the workshop server hosting the content is having issues - have the user retry.
    * [RESULT_INVALID_PARAM](main.md#result) if one of the submission fields contains something not being accepted by that field.
    * [RESULT_ACCESS_DENIED](main.md#result) if there was a problem trying to save the title and description. Access was denied.
    * [RESULT_LIMIT_EXCEEDED](main.md#result) if the user has exceeded their Steam Cloud quota. Have them remove some items and try again.
    * [RESULT_FILE_NOT_FOUND](main.md#result) if the uploaded file could not be found.
    * [RESULT_DUPLICATE_REQUEST](main.md#result) if the file was already successfully uploaded. The user just needs to refresh.
    * [RESULT_DUPLICATE_NAME](main.md#result) if the user already has a Steam Workshop item with that name.
    * [RESULT_SERVICE_READONLY](main.md#result) due to a recent password or email change, the user is not allowed to upload new content. Usually this restriction will expire in 5 days, but can last up to 30 days if the account has been inactive recently. 

	**accept_tos** should return false if the user has accepted the Workshop TOS. If it is true, direct the user to the TOS so they can accept it.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#CreateItemResult_t){ .md-button .md-button--doc_classes target="_blank" }

### item_deleted

!!! function "item_deleted"
	Called when an attempt at deleting an item completes.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The workshop item which was being deleted.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#DeleteItemResult_t){ .md-button .md-button--doc_classes target="_blank" }

### item_downloaded

!!! function "item_downloaded"
	Called when a workshop item has been downloaded.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The workshop item that has finished downloading.
        | app_id | uint32_t | The app ID associated with this workshop item.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#DownloadItemResult_t){ .md-button .md-button--doc_classes target="_blank" }

### item_installed

!!! function "item_installed"
	Called when a workshop item has been installed or updated.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | app_id | uint32_t | The app ID associated with this workshop item.
        | file_id | uint64_t | The workshop item that has finished installing. This can be used with [getItemInstallInfo](#getiteminstallinfo) to access the information about the item.
        | legacy_content | uint64_t | The UGC handle for the legacy content.
        | manifest_id | uint64_t | The manifest ID for this item.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#ItemInstalled_t){ .md-button .md-button--doc_classes target="_blank" }

### item_updated

!!! function "item_updated"
	Called when an item update has completed.
	Emits signal in response to function [submitItemUpdate](#submititemupdate).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
        | need_to_accept_tos | bool | Does the user need to accept the Steam Workshop legal agreement? See the [Workshop Legal Agreement for more information](https://partner.steamgames.com/doc/features/workshop/implementation#Legal){ target="\_blank" }.
        | file_id | int | The published file ID for the item that was updated.

        **result** can be one of the following:

        * [RESULT_OK](main.md#result) if the operation completed successfully.
        * [RESULT_FAIL](main.md#result) for a generic failure.
        * [RESULT_INVALID_PARAM](main.md#result) if either the provided app ID is invalid or doesn't match the consumer app ID of the item or, you have not enabled ISteamUGC for the provided app ID on the Steam Workshop Configuration App Admin page.
        The preview file is smaller than 16 bytes.
        * [RESULT_ACCESS_DENIED](main.md#result) if the user doesn't own a license for the provided app ID.
        * [RESULT_FILE_NOT_FOUND](main.md#result) if failed to get the workshop info for the item or failed to read the preview file.
        * [RESULT_LOCKING_FAILED](main.md#result) if failed to aquire UGC Lock.
        * [RESULT_FILE_NOT_FOUND](main.md#result) if the provided content folder is not valid.
        * [RESULT_LIMIT_EXCEEDED](main.md#result) if the preview image is too large, it must be less than 1 Megabyte; or there is not enough space available on the user's Steam Cloud.

    	If **need_to_accept_tos** comes back true, you should direct the user to accept the legal agreement.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SubmitItemUpdateResult_t){ .md-button .md-button--doc_classes target="_blank" }

### remove_app_dependency_result

!!! function "remove_app_dependency_result"
	Purpose: The result of a call to [removeAppDependency](#removeappdependency).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The parent workshop item that the dependency was removed from.
        | app_id | uint32_t | The app or dlc ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#RemoveAppDependencyResult_t){ .md-button .md-button--doc_classes target="_blank" }

### remove_ugc_dependency_result

!!! function "remove_ugc_dependency_result"
	Purpose: The result of a call to [removeDependency](#removedependency).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- | 
        | result | [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The parent workshop item that the dependency was removed from.
        | child_id | uint64_t | The child workshop item which was removed as a dependency from the parent item.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#RemoveUGCDependencyResult_t){ .md-button .md-button--doc_classes target="_blank" }

### set_user_item_vote

!!! function "set_user_item_vote"
	Called when the user has voted on an item.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The workshop item that the user voted on.
        | vote_up | bool | Was the vote up (true) or down (false)?

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SetUserItemVoteResult_t){ .md-button .md-button--doc_classes target="_blank" }

### start_playtime_tracking

!!! function "start_playtime_tracking"
	Called when workshop item playtime tracking has started.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#StartPlaytimeTrackingResult_t){ .md-button .md-button--doc_classes target="_blank" }

### stop_playtime_tracking

!!! function "stop_playtime_tracking"
	Called when workshop item playtime tracking has stopped.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#StopPlaytimeTrackingResult_t){ .md-button .md-button--doc_classes target="_blank" }

### ugc_query_completed

!!! function "ugc_query_completed"
	Called when a UGC query request completes.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | handle | uint64_t | The UGC query handle associated with this call result.
    	| result | [Result enum](main.md#result) | The result of the operation.
    	| results_returned | uint32_t | The number of items returned in this result set.
    	| total_matching | uint32_t | The total number of items that matched the query in the database.
    	| cached | bool | Indicates whether this data was retrieved from the local on-disk cache.
    	| next_cursor | string | If a paging cursor was used, then this will be the next cursor to get the next result set.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#SteamUGCQueryCompleted_t){ .md-button .md-button--doc_classes target="_blank" }

### user_favorite_items_list_changed

!!! function "user_favorite_items_list_changed"
	Called when the user has added or removed an item to/from their favorites.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The item which was added or removed.
        | was_add_request | bool | Was it added (true) or removed (false) from the user's favorites?

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#UserFavoriteItemsListChanged_t){ .md-button .md-button--doc_classes target="_blank" }

### user_subscribed_items_list_changed

!!! function "user_subscribed_items_list_changed"
	Signal that the list of subscribed items changed.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | app_id | uint32_t | The related app ID.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#UserSubscribedItemsListChanged_t){ .md-button .md-button--doc_classes target="_blank" }

### workshop_eula_status

!!! function "workshop_eula_status"
	Status of the user's acceptable/rejection of the app's specific Workshop EULA.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
        | app_id | uint32_t | The related app ID.
        | eula_data | dictionary | Contains the EULA data.

        **eula_data** contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
        | version | uint32_t | The version of the signed EULA, if applicable.
		| action | uint32_t | Unix timestamp of when the user signed the EULA, if applicable.
		| accepted | bool | True if the user accepted the given version, false otherwise. Note that this can be true if the user accepted an older version of the EULA.
		| needs_action | bool | True if the user needs to accept the latest Workshop EULA, false otherwise.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUGC#WorkshopEULAStatus_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
DEVELOPER_METADATA_MAX | k_cchDeveloperMetadataMax | 5000 | The maximum amount of bytes you can set with [setItemMetadata](#setitemmetadata).
NUM_UGC_RESULTS_PER_PAGE | kNumUGCResultsPerPage | 50 | The maximum number of results that you'll receive for a query result.
UGC_QUERY_HANDLE_INVALID | k_UGCQueryHandleInvalid | 0xffffffffffffffffull | Used to specify an invalid query handle. This is frequently returned if a call fails.
UGC_UPDATE_HANDLE_INVALID | k_UGCUpdateHandleInvalid | 0xffffffffffffffffull | Used to specify an invalid item update handle. This is frequently returned if a call fails.

{==
## :material-numeric: Enums
==}

### ItemPreviewType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
ITEM_PREVIEW_TYPE_IMAGE | k_EItemPreviewType_Image | 0 | Standard image file expected (e.g. jpg, png, gif, etc.)
ITEM_PREVIEW_TYPE_YOUTUBE_VIDEO | k_EItemPreviewType_YouTubeVideo | 1 | Video ID is stored.
ITEM_PREVIEW_TYPE_SKETCHFAB | k_EItemPreviewType_Sketchfab | 2 | Model ID is stored.
ITEM_PREVIEW_TYPE_ENVIRONMENTMAP_HORIZONTAL_CROSS | k_EItemPreviewType_EnvironmentMap_HorizontalCross | 3 | Standard image file expected - cube map in the layout.
ITEM_PREVIEW_TYPE_ENVIRONMENTMAP_LAT_LONG | k_EItemPreviewType_EnvironmentMap_LatLong | 4 | Standard image file expected.
ITEM_PREVIEW_TYPE_CLIP | k_EItemPreviewType_Clip | 5 | Clip id is stored.
ITEM_PREVIEW_TYPE_RESERVED_MAX | k_EItemPreviewType_ReservedMax | 255 | You can specify your own types above this value.

### ItemState

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
ITEM_STATE_NONE | k_EItemStateNone | 0 | Item not tracked on client.
ITEM_STATE_SUBSCRIBED | k_EItemStateSubscribed | 1 | Current user is subscribed to this item. Not just cached.
ITEM_STATE_LEGACY_ITEM | k_EItemStateLegacyItem | 2 | Item was created with [Remote Storage](remote_storage.md).
ITEM_STATE_INSTALLED | k_EItemStateInstalled | 4 | Item is installed and usable (but maybe out of date).
ITEM_STATE_NEEDS_UPDATE | k_EItemStateNeedsUpdate | 8 | Items needs an update. Either because it's not installed yet or creator updated content.
ITEM_STATE_DOWNLOADING | k_EItemStateDownloading | 16 | Item update is currently downloading.
ITEM_STATE_DOWNLOAD_PENDING | k_EItemStateDownloadPending | 32 | [downloadItem](#downloaditem) was called for this item, content isn't available until [item_downloaded](#item_downloaded) is fired.
ITEM_STATE_DISABLED_LOCALLY | k_EItemStateDisabledLocally | 64 | Item is disabled locally, so it shouldn't be considered subscribed.

### ItemStatistic

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
ITEM_STATISTIC_NUM_SUBSCRIPTIONS | k_EItemStatistic_NumSubscriptions | 0 | -
ITEM_STATISTIC_NUM_FAVORITES | k_EItemStatistic_NumFavorites | 1 | -
ITEM_STATISTIC_NUM_FOLLOWERS | k_EItemStatistic_NumFollowers | 2 | -
ITEM_STATISTIC_NUM_UNIQUE_SUBSCRIPTIONS | k_EItemStatistic_NumUniqueSubscriptions | 3 | -
ITEM_STATISTIC_NUM_UNIQUE_FAVORITES | k_EItemStatistic_NumUniqueFavorites | 4 | -
ITEM_STATISTIC_NUM_UNIQUE_FOLLOWERS | k_EItemStatistic_NumUniqueFollowers | 5 | -
ITEM_STATISTIC_NUM_UNIQUE_WEBSITE_VIEWS | k_EItemStatistic_NumUniqueWebsiteViews | 6 | -
ITEM_STATISTIC_REPORT_SCORE | k_EItemStatistic_ReportScore | 7 | -
ITEM_STATISTIC_NUM_SECONDS_PLAYED | k_EItemStatistic_NumSecondsPlayed | 8 | -
ITEM_STATISTIC_NUM_PLAYTIME_SESSIONS | k_EItemStatistic_NumPlaytimeSessions | 9 | -
ITEM_STATISTIC_NUM_COMMENTS | k_EItemStatistic_NumComments | 10 | -
ITEM_STATISTIC_NUM_SECONDS_PLAYED_DURING_TIME_PERIOD | k_EItemStatistic_NumSecondsPlayedDuringTimePeriod | 11 | -
ITEM_STATISTIC_NUM_PLAYTIME_SESSIONS_DURING_TIME_PERIOD | k_EItemStatistic_NumPlaytimeSessionsDuringTimePeriod | 12 | -

### ItemUpdateStatus

The current status for an UGC item update.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
ITEM_UPDATE_STATUS_INVALID | k_EItemUpdateStatusInvalid | 0 | The item update handle was invalid, job might be finished, listen to [item_updated](#item_updated).
ITEM_UPDATE_STATUS_PREPARING_CONFIG | k_EItemUpdateStatusPreparingConfig | 1 | The item update is processing configuration data.
ITEM_UPDATE_STATUS_PREPARING_CONTENT | k_EItemUpdateStatusPreparingContent | 2 | The item update is reading and processing content files.
ITEM_UPDATE_STATUS_UPLOADING_CONTENT | k_EItemUpdateStatusUploadingContent | 3 | The item update is uploading content changes to Steam.
ITEM_UPDATE_STATUS_UPLOADING_PREVIEW_FILE | k_EItemUpdateStatusUploadingPreviewFile | 4 | The item update is uploading new preview file image.
ITEM_UPDATE_STATUS_COMMITTING_CHANGES | k_EItemUpdateStatusCommittingChanges | 5 | The item update is committing all changes.

### UGCMatchingUGCType

Matching UGC types for queries.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
UGC_MATCHING_UGC_TYPE_ITEMS | k_EUGCMatchingUGCType_Items | 0 | Both mtx items and ready-to-use items.
UGC_MATCHING_UGC_TYPE_ITEMS_MTX | k_EUGCMatchingUGCType_Items_Mtx | 1 | -
UGC_MATCHING_UGC_TYPE_ITEMS_READY_TO_USE | k_EUGCMatchingUGCType_Items_ReadyToUse | 2 |  -
UGC_MATCHING_UGC_TYPE_COLLECTIONS | k_EUGCMatchingUGCType_Collections | 3 |  -
UGC_MATCHING_UGC_TYPE_ARTWORK | k_EUGCMatchingUGCType_Artwork | 4 |  -
UGC_MATCHING_UGC_TYPE_VIDEOS | k_EUGCMatchingUGCType_Videos | 5 |  -
UGC_MATCHING_UGC_TYPE_SCREENSHOTS | k_EUGCMatchingUGCType_Screenshots | 6 |  -
UGC_MATCHING_UGC_TYPE_ALL_GUIDES | k_EUGCMatchingUGCType_AllGuides | 7 | Both web guides and integrated guides.
UGC_MATCHING_UGC_TYPE_WEB_GUIDES | k_EUGCMatchingUGCType_WebGuides | 8 |  -
UGC_MATCHING_UGC_TYPE_INTEGRATED_GUIDES | k_EUGCMatchingUGCType_IntegratedGuides | 9 | - 
UGC_MATCHING_UGC_TYPE_USABLE_IN_GAME | k_EUGCMatchingUGCType_UsableInGame | 10 | Ready-to-use items and integrated guides.
UGC_MATCHING_UGC_TYPE_CONTROLLER_BINDINGS | k_EUGCMatchingUGCType_ControllerBindings | 11 |  -
UGC_MATCHING_UGC_TYPE_GAME_MANAGED_ITEMS | k_EUGCMatchingUGCType_GameManagedItems | 12 | Game managed items (not managed by users).
UGC_MATCHING_UGC_TYPE_ALL | k_EUGCMatchingUGCType_All | ~0 | Will only be valid for [createQueryUserUGCRequest](#createqueryuserugcrequest) requests.

### UGCQuery

Combination of sorting and filtering for queries across all UGC.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
UGC_QUERY_RANKED_BY_VOTE | k_EUGCQuery_RankedByVote | 0 | -
UGC_QUERY_RANKED_BY_PUBLICATION_DATE | k_EUGCQuery_RankedByPublicationDate | 1 | -
UGC_QUERY_ACCEPTED_FOR_GAME_RANKED_BY_ACCEPTANCE_DATE | k_EUGCQuery_AcceptedForGameRankedByAcceptanceDate | 2 | -
UGC_QUERY_RANKED_BY_TREND |k_EUGCQuery_RankedByTrend 3 | -
UGC_QUERY_FAVORITED_BY_FRIENDS_RANKED_BY_PUBLICATION_DATE | k_EUGCQuery_FavoritedByFriendsRankedByPublicationDate 4 | -
UGC_QUERY_CREATED_BY_FRIENDS_RANKED_BY_PUBLICATION_DATE | k_EUGCQuery_CreatedByFriendsRankedByPublicationDate | 5 | -
UGC_QUERY_RANKED_BY_NUM_TIMES_REPORTED | k_EUGCQuery_RankedByNumTimesReported | 6 | -
UGC_QUERY_CREATED_BY_FOLLOWED_USERS_RANKED_BY_PUBLICATION_DATE | k_EUGCQuery_CreatedByFollowedUsersRankedByPublicationDate | 7 | -
UGC_QUERY_NOT_YET_RATED | k_EUGCQuery_NotYetRated | 8 | -
UGC_QUERY_RANKED_BY_TOTAL_VOTES_ASC | k_EUGCQuery_RankedByTotalVotesAsc | 9 | -
UGC_QUERY_RANKED_BY_VOTES_UP | k_EUGCQuery_RankedByVotesUp | 10 | -
UGC_QUERY_RANKED_BY_TEXT_SEARCH | k_EUGCQuery_RankedByTextSearch | 11 | -
UGC_QUERY_RANKED_BY_TOTAL_UNIQUE_SUBSCRIPTIONS | k_EUGCQuery_RankedByTotalUniqueSubscriptions | 12 | -
UGC_QUERY_RANKED_BY_PLAYTIME_TREND | k_EUGCQuery_RankedByPlaytimeTrend | 13 | -
UGC_QUERY_RANKED_BY_TOTAL_PLAYTIME | k_EUGCQuery_RankedByTotalPlaytime | 14 | -
UGC_QUERY_RANKED_BY_AVERAGE_PLAYTIME_TREND | k_EUGCQuery_RankedByAveragePlaytimeTrend | 15 | -
UGC_QUERY_RANKED_BY_LIFETIME_AVERAGE_PLAYTIME | k_EUGCQuery_RankedByLifetimeAveragePlaytime | 16 | -
UGC_QUERY_RANKED_BY_PLAYTIME_SESSIONS_TREND | k_EUGCQuery_RankedByPlaytimeSessionsTrend | 17 | -
UGC_QUERY_RANKED_BY_LIFETIME_PLAYTIME_SESSIONS | k_EUGCQuery_RankedByLifetimePlaytimeSessions | 18 | -
UGC_QUERY_RANKED_BY_LAST_UPDATED_DATE | k_EUGCQuery_RankedByLastUpdatedDate | 19 | -

### UserUGCList

Different lists of published UGC for a user. If the current logged in user is different than the specified user, then some options may not be allowed.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
USER_UGC_LIST_PUBLISHED | k_EUserUGCList_Published | 0 | -
USER_UGC_LIST_VOTED_ON | k_EUserUGCList_VotedOn | 1 | -
USER_UGC_LIST_VOTED_UP | k_EUserUGCList_VotedUp | 2 | -
USER_UGC_LIST_VOTED_DOWN | k_EUserUGCList_VotedDown | 3 | -
USER_UGC_LIST_WILL_VOTE_LATER | k_EUserUGCList_WillVoteLater | 4 | -
USER_UGC_LIST_FAVORITED | k_EUserUGCList_Favorited | 5 | -
USER_UGC_LIST_SUBSCRIBED | k_EUserUGCList_Subscribed | 6 | -
USER_UGC_LIST_USED_OR_PLAYED | k_EUserUGCList_UsedOrPlayed | 7 | -
USER_UGC_LIST_FOLLOWED | k_EUserUGCList_Followed | 8 | -

### UserUGCListSortOrder

Sort order for user published UGC lists (defaults to creation order descending).

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
USER_UGC_LIST_SORT_ORDER_CREATION_ORDER_DESC | k_EUserUGCListSortOrder_CreationOrderDesc | 0 | -
USER_UGC_LIST_SORT_ORDER_CREATION_ORDER_ASC | k_EUserUGCListSortOrder_CreationOrderAsc | 1 | -
USER_UGC_LIST_SORT_ORDER_TITLE_ASC | k_EUserUGCListSortOrder_TitleAsc | 2 | -
USER_UGC_LIST_SORT_ORDER_LAST_UPDATED_DESC | k_EUserUGCListSortOrder_LastUpdatedDesc | 3 | -
USER_UGC_LIST_SORT_ORDER_SUBSCRIPTION_DATE_DESC | k_EUserUGCListSortOrder_SubscriptionDateDesc | 4 | -
USER_UGC_LIST_SORT_ORDER_VOTE_SCORE_DESC | k_EUserUGCListSortOrder_VoteScoreDesc | 5 | -
USER_UGC_LIST_SORT_ORDER_FOR_MODERATION | k_EUserUGCListSortOrder_ForModeration | 6 | -

### UGCContentDescriptorID

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
UGC_CONTENT_DESCRIPTOR_NUDITY_OR_SEXUAL_CONTENT | k_EUGCContentDescriptor_NudityOrSexualContent | 1 | -
UGC_CONTENT_DESCRIPTOR_FREQUENT_VIOLENCE_OR_GORE | k_EUGCContentDescriptor_FrequentViolenceOrGore | 2 | -
UGC_CONTENT_DESCRIPTOR_ADULT_ONLY_SEXUAL_CONTENT | k_EUGCContentDescriptor_AdultOnlySexualContent | 3 | -
UGC_CONTENT_DESCRIPTOR_GRATUITOUS_SEXUAL_CONTENT | k_EUGCContentDescriptor_GratuitousSexualContent | 4 | -
UGC_CONTENT_DESCRIPTOR_ANY_MATURE_CONTENT | k_EUGCContentDescriptor_AnyMatureContent | 5 | -