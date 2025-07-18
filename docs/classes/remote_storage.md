---
title: Remote Storage
description: A class reference for Remote Storage.
icon: material/storage-tank-outline
---

# Remote Storage

Provides functions for reading, writing, and accessing files which can be stored remotely in the Steam Cloud. See [Steam Cloud](https://partner.steamgames.com/doc/features/cloud){ target="\_blank" } for more information.

Filenames are case-insensitive, and will be converted to lowercase automatically. So "foo.bar" and "Foo.bar" are the same file, and if you write "Foo.bar" then iterate the files, the filename returned will be "foo.bar".

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### beginFileWriteBatch

!!! function "beginFileWriteBatch( )"
	Use this along with [endFileWriteBatch](#endfilewritebatch) to wrap a set of local file writes/deletes that should be considered part of one single state change. For example, if saving game progress requires updating both savegame1.dat and maxprogress.dat, wrap those operations with calls to [beginFileWriteBatch](#beginfilewritebatch) and [endFileWriteBatch](#endfilewritebatch).

	These functions provide a hint to Steam which will help it manage the app's Cloud files. Using these functions is optional, however it will provide better reliability.

	Note that the functions may be used whether the writes are done using the Remote Storage API, or done directly to local disk (where AutoCloud is used).

	!!! returns "Returns: bool"
        Returns true if the write batch was begun; otherwise, false if there was a batch already in progress.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#BeginFileWriteBatch){ .md-button .md-button--doc_classes target="_blank" }

### endFileWriteBatch

!!! function "endFileWriteBatch( )"
	Use this along with [beginFileWriteBatch](#beginfilewritebatch) - see that documentation for more details.

	!!! returns "Returns: bool"
        Returns true if the write batch was ended; otherwise, false if there was no batch already in progress.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#EndFileWriteBatch){ .md-button .md-button--doc_classes target="_blank" }

### fileDelete

!!! function "fileDelete( ```string``` file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file that will be deleted. |

    Deletes a file from the local disk, and propagates that delete to the cloud.

	This is meant to be used when a user actively deletes a file. Use [fileForget](#fileforget) if you want to remove a file from the Steam Cloud but retain it on the users local disk.

	When a file has been deleted it can be re-written with [fileWrite](#filewrite) to reupload it to the Steam Cloud.

	!!! returns "Returns: bool"
        Returns true if the file exists and has been successfully deleted; otherwise, false if the file did not exist.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileDelete){ .md-button .md-button--doc_classes target="_blank" }

### fileExists

!!! function "fileExists( ```string``` file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file. |

    Check if a remote file exists.

	!!! returns "Returns: bool"
        Returns true if the file exists; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileExists){ .md-button .md-button--doc_classes target="_blank" }

### fileForget

!!! function "fileForget( ```string``` file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file that will be forgotten. |

    Deletes the file from remote storage, but leaves it on the local disk and remains accessible from the API.

	When you are out of Cloud space, this can be used to allow calls to [fileWrite](#filewrite) to keep working without needing to make the user delete files.

	How you decide which files to forget are up to you. It could be a simple Least Recently Used (LRU) queue or something more complicated.

	Requiring the user to manage their Cloud-ized files for a game, while it is possible to do, is never recommended. For instance, "Which file would you like to delete so that you may store this new one?" removes a significant advantage of using the Cloud in the first place: its transparency.

	Once a file has been deleted or forgotten, calling [fileWrite](#filewrite) will resynchronize it in the Cloud. Rewriting a forgotten file is the only way to make it persisted again.

	!!! returns "Returns: bool"
        Returns true if the file exists and has been successfully forgotten; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileForget){ .md-button .md-button--doc_classes target="_blank" }

### filePersisted

!!! function "filePersisted( ```string``` file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file. |

    Checks if a specific file is persisted in the Steam Cloud.

	!!! returns "Returns: bool"
        Returns true if the file exists and the file is persisted in the Steam Cloud; otherwise, false if [fileForget](#fileforget) was called on it and is only available locally.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FilePersisted){ .md-button .md-button--doc_classes target="_blank" }

### fileRead

!!! function "fileRead( ```string``` file, ```uint32_t``` data_to_read )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file to read from. |
    | data_to_read | uint32_t | The amount of bytes to read. Generally obtained from [getFileSize](#getfilesize) or [getFileTimestamp](#getfiletimestamp). |

	Opens a binary file, reads the contents of the file into a byte array, and then closes the file.

    This is a synchronous call and as such is a will block your calling thread on the disk IO, and will also block the SteamAPI, which can cause other threads in your application to block. To avoid "hitching" due to a busy disk on the client machine using [fileReadAsync](#filereadasync), the asynchronous version of this API is recommended.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| ret | int32 | The number of bytes read. Returns 0 if the file doesn't exist or the read fails.
    	| buf | PackedByteArray | The buffer that the file is read into.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileRead){ .md-button .md-button--doc_classes target="_blank" }

### fileReadAsync

!!! function "fileReadAsync( ```string``` file, ```uint32_t``` offset, ```uint32_t``` data_to_read )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file to read from. |
    | offset | uint32_t | The offset in bytes into the file where the read will start from. 0 if you're reading the whole file in one chunk. |
    | data_to_read | uint32_t | The amount of bytes to read starting from **offset**. |

    Starts an asynchronous read from a file. The offset and amount to read should be valid for the size of the file, as indicated by [getFileSize](#getfilesize) or [getFileTimestamp](#getfiletimestamp).

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[file_read_async_complete](#file_read_async_complete) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileReadAsync){ .md-button .md-button--doc_classes target="_blank" }

### fileShare

!!! function "fileShare( ```string``` file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file to read from. |

    Share a file.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[file_share_result](#file_share_result) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileShare){ .md-button .md-button--doc_classes target="_blank" }

### fileWrite

!!! function "fileWrite( ```string``` file, ```PoolByteArray``` data, ```int32``` size = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file to write to. |
    | data | PackedByteArray | The bytes to write to the file. |
    | size | int32 | The number of bytes to write to the file. Typically the total size of **data**. Defaults to 0. |

    Creates a new file, writes the bytes to the file, and then closes the file. If the target file already exists, it is overwritten.

    This is a synchronous call and as such is a will block your calling thread on the disk IO, and will also block the SteamAPI, which can cause other threads in your application to block. To avoid "hitching" due to a busy disk on the client machine using [fileWriteAsync](#filewriteasync), the asynchronous version of this API is recommended.

	!!! returns "Returns: bool"
        Returns true if the write was successful; otherwise, false under the following conditions:

        * The file you're trying to write is larger than 100MiB as defined by [MAX_CLOUD_FILE_CHUNK_SIZE](#constants).
        * **size** is less than 0.
        * **data** is empty.
        * You tried to write to an invalid path or filename. Because Steam Cloud is cross platform the files need to have valid names on all supported OSes and file systems. [See Microsoft's documentation on Naming Files, Paths, and Namespaces.](https://msdn.microsoft.com/en-us/library/windows/desktop/aa365247(v=vs.85).aspx){ target="\_blank" }
        * The current user's Steam Cloud storage quota has been exceeded. They may have run out of space, or have too many files.
        * Steam could not write to the disk, the location might be read-only.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileWrite){ .md-button .md-button--doc_classes target="_blank" }

### fileWriteAsync

!!! function "fileWriteAsync( ```string``` file, ```PoolByteArray``` data, ```int32``` size = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file to write to. |
    | data | PoolByteArray | The bytes to write to the file. |
    | size | int32 | The number of bytes to write to the file. Typically the total size of **data**. Defaults to 0. |

    Creates a new file and asynchronously writes the raw byte data to the Steam Cloud, and then closes the file. If the target file already exists, it is overwritten.

	!!! returns "Returns: void"
        May trigger an invalid API call under the following conditions:

        * The file you're trying to write is larger than 100MiB as defined by [MAX_CLOUD_FILE_CHUNK_SIZE](#constants).
        * **size** is less than 0.
        * **data** is empty.
        * You tried to write to an invalid path or filename. Because Steam Cloud is cross platform the files need to have valid names on all supported OSes and file systems. [See Microsoft's documentation on Naming Files, Paths, and Namespaces.](https://msdn.microsoft.com/en-us/library/windows/desktop/aa365247(v=vs.85).aspx){ target="\_blank" }
        * The current user's Steam Cloud storage quota has been exceeded. They may have run out of space, or have too many files.
        * Steam could not write to the disk, the location might be read-only.

	!!! trigger "Triggers"
		[file_write_async_complete](#file_write_async_complete) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileWriteAsync){ .md-button .md-button--doc_classes target="_blank" }

### fileWriteStreamCancel

!!! function "fileWriteStreamCancel( ```uint64_t``` write_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | write_handle | uint64_t | The file write stream to cancel. |

    Cancels a file write stream that was started by [fileWriteStreamOpen](#filewritestreamopen).

	This trashes all of the data written and closes the write stream, but if there was an existing file with this name, it remains untouched.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileWriteStreamCancel){ .md-button .md-button--doc_classes target="_blank" }

### fileWriteStreamClose

!!! function "fileWriteStreamClose( ```uint64_t``` write_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | write_handle | uint64_t | The file write stream to close. |

    Closes a file write stream that was started by [fileWriteStreamOpen](#filewritestreamopen).

	This flushes the stream to the disk, overwriting the existing file if there was one.

	!!! returns "Returns: bool"
        Returns true if the file write stream was successfully closed, the file has been committed to the disk; otherwise, false if **write_handle** is not a valid file write stream.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileWriteStreamClose){ .md-button .md-button--doc_classes target="_blank" }

### fileWriteStreamOpen

!!! function "fileWriteStreamOpen( ```string``` file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file to write to. |

    Creates a new file output stream allowing you to stream out data to the Steam Cloud file in chunks. If the target file already exists, it is not overwritten until [fileWriteStreamClose](#filewritestreamclose) has been called.

	To write data out to this stream you can use [fileWriteStreamWriteChunk](#filewritestreamwritechunk), and then to close or cancel you use [fileWriteStreamClose](#filewritestreamclose) and [fileWriteStreamCancel](#filewritestreamcancel) respectively.

	!!! returns "Returns: uint64_t"
        Returns a file write stream handle. May return [UGC_FILE_STREAM_HANDLE_INVALID](#constants) under the following conditions:

        * You tried to write to an invalid path or filename. Because Steam Cloud is cross platform the files need to have valid names on all supported OSes and file systems. [See Microsoft's documentation on Naming Files, Paths, and Namespaces.](https://msdn.microsoft.com/en-us/library/windows/desktop/aa365247(v=vs.85).aspx){ target="\_blank" }
        * The current user's Steam Cloud storage quota has been exceeded. They may have run out of space, or have too many files.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileWriteStreamOpen){ .md-button .md-button--doc_classes target="_blank" }

### fileWriteStreamWriteChunk

!!! function "fileWriteStreamWriteChunk( ```uint64_t``` write_handle, ```PoolByteArray``` data )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | write_handle | uint64_t | The file write stream to write to. |
    | data | PoolByteArray | The data to write to the stream. |

    Writes a blob of data to the file write stream.

	!!! returns "Returns: bool"
        Returns true if the data was successfully written to the file write stream; otherwise, false if:

        * **write_handle** is not a valid file write stream
        * **data** size is negative or larger than [MAX_CLOUD_FILE_CHUNK_SIZE](#constants)
        * The current user's Steam Cloud storage quota has been exceeded. They may have run out of space or have too many files.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#FileWriteStreamWriteChunk){ .md-button .md-button--doc_classes target="_blank" }

### getCachedUGCCount

!!! function "getCachedUGCCount( )"
	Gets the number of cached UGC.  Used to iterate through UGC that has finished downloading but has not yet been read via [ugcRead](#ugcread).

	!!! returns "Returns: int32"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetCachedUGCCount){ .md-button .md-button--doc_classes target="_blank" }

### getCachedUGCHandle

!!! function "getCachedUGCHandle( ```int32``` content )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | content | int32 | The cached content to get he handle for. |

    Gets the cached UGC's handle.  Used to iterate through UGC that has finished downloading but has not yet been read via [ugcRead](#ugcread).

	!!! returns "Returns: uint64_t"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetCachedUGCHandle){ .md-button .md-button--doc_classes target="_blank" }

### getFileCount

!!! function "getFileCount( )"
	Gets the total number of local files synchronized by Steam Cloud.

	Used for enumeration with [getFileNameAndSize](#getfilenameandsize).

	!!! returns "Returns: uint32_t"
        The number of files present for the current user, including files in subfolders.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetFileCount){ .md-button .md-button--doc_classes target="_blank" }

### getFileNameAndSize

!!! function "getFileNameAndSize( ```int``` file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | int | The index of the file, this should be between 0 and [getFileCount](#getfilecount).

    Gets the file name and size of a file from the index.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| name | string | The name of the file at the specified index, if it exists. Returns an empty string if the file doesn't exist.
    	| size | int32_t | Returns the file size in bytes.

	!!! info "Notes"  You must call [getFileCount](#getfilecount) first to get the number of files.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetFileNameAndSize){ .md-button .md-button--doc_classes target="_blank" }

### getFileSize

!!! function "getFileSize( ```string``` file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file. |

    Gets the specified files size in bytes.

	!!! returns "Returns: int32_t"
        The size of the file in bytes. Returns 0 if the file does not exist.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetFileSize){ .md-button .md-button--doc_classes target="_blank" }

### getFileTimestamp

!!! function "getFileTimestamp( ```string``` file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file. |

    Gets the specified file's last modified timestamp in Unix epoch format (seconds since Jan 1st 1970).

	!!! returns "Returns: int64_t"
        The last modified timestamp in Unix epoch format (seconds since Jan 1st 1970).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetFileTimestamp){ .md-button .md-button--doc_classes target="_blank" }

### getLocalFileChange

!!! function "getLocalFileChange( ```int``` file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | int | Zero-based index of the change. |

    After calling [getLocalFileChangeCount](#getlocalfilechangecount), use this method to iterate over the changes. The changes described have already been made to local files. Your application should take appropriate action to reload state from disk, and possibly notify the user.

	For example: The local system had been suspended, during which time the user played elsewhere and uploaded changes to the Steam Cloud. On resume, Steam downloads those changes to the local system before resuming the application. The application receives an [local_file_changed](#local_file_changed), and uses [getLocalFileChangeCount](#getlocalfilechangecount) and [getLocalFileChange](#getlocalfilechange) to iterate those changes. Depending on the application structure and the nature of the changes, the application could:

	* Re-load game progress to resume at exactly the point where the user was when they exited the game on the other device
	* Notify the user of any synchronized changes that don't require reloading
	* Etc.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| file | string | 
    	| change_type | [LocalFileChange enum](#localfilechange) | What happened to this file.
    	| path_type | [FilePathType enum](#filepathtype) | Type of path to the file returned.

	!!! info "Notes"
        Only applies to applications flagged as supporting dynamic Steam Cloud sync.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetLocalFileChange){ .md-button .md-button--doc_classes target="_blank" }

### getLocalFileChangeCount

!!! function "getLocalFileChangeCount( )"
	When your application receives a [local_file_changed](#local_file_changed), use this method to get the number of changes (file updates and file deletes) that have been made. You can then iterate the changes using [getLocalFileChange](#getlocalfilechange).

	!!! returns "Returns: uint32_t"
        The number of local file changes that have occurred.

	!!! info "Notes"
        Only applies to applications flagged as supporting dynamic Steam Cloud sync.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetLocalFileChangeCount){ .md-button .md-button--doc_classes target="_blank" }

### getQuota

!!! function "getQuota( )"
	Gets the number of bytes available, and used on the users Steam Cloud storage.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| total_bytes | uint64_t | Returns the total amount of bytes the user has access to.
    	| available_bytes | uint64_t | Returns the number of bytes available.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetQuota){ .md-button .md-button--doc_classes target="_blank" }

### getSyncPlatforms

!!! function "getSyncPlatforms( ```string``` file )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file. |

    Obtains the platforms that the specified file will synchronize to.

	!!! returns "Returns: [RemoteStoragePlatform enum](#remotestorageplatform)"
    	Bitfield containing the platforms that the file was set to with [setSyncPlatforms](#setsyncplatforms).

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetSyncPlatforms){ .md-button .md-button--doc_classes target="_blank" }

### getUGCDetails

!!! function "getUGCDetails( ```uint64_t``` content )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | content | uint64_t | The UGC content handle. |

    Gets metadata for a file after it has been downloaded. This is the same metadata given in the [download_ugc_result](#download_ugc_result) call result.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| handle | uint64_t | The handle to the file that was attempted to be downloaded.
    	| app_id | uint32_t | ID of the app that created this file.
    	| size | int32 | The size of the file that was downloaded, in bytes.
    	| filename | string | The name of the file that was downloaded.
    	| owner_id | uint64_t | Steam ID of the user who created this content.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetUGCDetails){ .md-button .md-button--doc_classes target="_blank" }

### getUGCDownloadProgress

!!! function "getUGCDownloadProgress( ```uint64_t``` content )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | content | uint64_t | The UGC content handle. |

    Gets the amount of data downloaded so far for a piece of content. **bytes_expected** can be 0 if function returns false or if the transfer hasn't started yet, so be careful to check for that before dividing to get a percentage.

	!!! returns "Returns: dictionary"
    	Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| bytes_downloaded | int32 | The number of bytes downloaded so far.
    	| bytes_expected | int32 | The number of bytes expected to be downloaded.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#GetUGCDownloadProgress){ .md-button .md-button--doc_classes target="_blank" }

### isCloudEnabledForAccount

!!! function "isCloudEnabledForAccount( )"
	Checks if the account wide Steam Cloud setting is enabled for this user; or if they disabled it in the Settings -> Cloud dialog.

	Ensure that you are also checking [isCloudEnabledForApp](#iscloudenabledforapp), as these two options are mutually exclusive.

	!!! returns "Returns: bool"
        Returns true if Steam Cloud is enabled for this account; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#IsCloudEnabledForAccount){ .md-button .md-button--doc_classes target="_blank" }

### isCloudEnabledForApp

!!! function "isCloudEnabledForApp( )"
	Checks if the per game Steam Cloud setting is enabled for this user; or if they disabled it in the Game Properties -> Update dialog.

	Ensure that you are also checking [isCloudEnabledForAccount](#iscloudenabledforaccount), as these two options are mutually exclusive.

	It's generally recommended that you allow the user to toggle this setting within your in-game options, you can toggle it with [setCloudEnabledForApp](#setcloudenabledforapp).

	!!! returns "Returns: bool"
        Returns true if Steam Cloud is enabled for this app; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#IsCloudEnabledForApp){ .md-button .md-button--doc_classes target="_blank" }


### setCloudEnabledForApp

!!! function "setCloudEnabledForApp( ```bool``` enabled )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | enabled | bool | Enable (true) or disable (false) the Steam Cloud for this application? |

    Enable or disable Steam Cloud for this application. This must only ever be called as the direct result of the user explicitly requesting that it's enabled or not. This is typically accomplished with a checkbox within your in-game options.

    This setting can be queried with [isCloudEnabledForApp](#iscloudenabledforapp).

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#SetCloudEnabledForApp){ .md-button .md-button--doc_classes target="_blank" }

### setSyncPlatforms

!!! function "setSyncPlatforms( ```string``` file, ```RemoteStoragePlatform``` platform )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | file | string | The name of the file. |
    | platform | [RemoteStoragePlatform enum](#remotestorageplatform) | The platforms that the file will be syncronized to. |

    Allows you to specify which operating systems a file will be synchronized to.

	Use this if you have a multiplatform game but have data which is incompatible between platforms.

	Files default to [REMOTE_STORAGE_PLATFORM_ALL (0xffffffff)](#remotestorageplatform) when they are first created. You can use the bitwise OR operator, "|" to specify multiple platforms.

	!!! returns "Returns: bool"
        Returns true if the file exists; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#SetSyncPlatforms){ .md-button .md-button--doc_classes target="_blank" }

### ugcDownload

!!! function "ugcDownload( ```uint64_t``` content, ```uint32_t``` priority )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | content | uint64_t | The UGC content handle. |
    | priority | uint32_t | How soon the content should be downloaded; 0 is immediately. |

    Downloads a UGC file.

	A priority value of 0 will download the file immediately, otherwise it will wait to download the file until all downloads with a lower priority value are completed. Downloads with equal priority will occur simultaneously.

	!!! returns "Returns: void"

    !!! trigger "Triggers"
        [download_ugc_result](#download_ugc_result) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#UGCDownload){ .md-button .md-button--doc_classes target="_blank" }

### ugcDownloadToLocation

!!! function "ugcDownloadToLocation( ```uint64_t``` content, ```string``` location, ```uint32_t``` priority )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | content | uint64_t | The UGC content handle. |
    | location | string | The absolute path to where the UGC file should be downloaded. |
    | priority | uint32_t | How soon the content should be downloaded; 0 is immediately.

    Downloads a UGC file to a specific location.

	!!! returns "Returns: void"

    !!! trigger "Triggers"
        [download_ugc_result](#download_ugc_result) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#UGCDownloadToLocation){ .md-button .md-button--doc_classes target="_blank" }

### ugcRead

!!! function "ugcRead( ```uint64_t``` content, ```int32``` data_size, ```uint32_t``` offset, ```UGCReadAction``` action )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | content | uint64_t | The UGC content handle. |
    | data_size | int32 | The size of the data to be read. |
    | offset | uint32_t | The offset in bytes into the file where the read will start from. 0 if you're reading the whole file in one chunk. |
    | action | [UGCReadAction enum](#ugcreadaction) | The reading action for the UGC content. |

	After download, gets the content of the file. Small files can be read all at once by calling this function with an **offset** of 0 and **data_size equal** to the size of the file. Larger files can be read in chunks to reduce memory usage (since both sides of the IPC client and the game itself must allocate enough memory for each chunk).

    Once the last byte is read, the file is implicitly closed and further calls to UGCRead will fail unless [ugcDownload](#ugcdownload) is called again. For especially large files (anything over 100MB) it is a requirement that the file is read in chunks.

	!!! returns "Returns: PackedByteArray"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#UGCRead){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### download_ugc_result

!!! function "download_ugc_result"
	Response when downloading UGC.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| result | int / [Result enum](main.md#result) | The result of the operation.
        | download_data | dictionary | The details of the UGC download.

        **download_data** contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
		| handle | uint64_t | The handle to the file that was attempted to be downloaded.
		| app_id | uint32_t | ID of the app that created this file.
		| size | int32 | The size of the file that was downloaded, in bytes.
		| filename | string | The name of the file that was downloaded.
		| owner_id | uint64_t | Steam ID of the user who created this content.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#RemoteStorageDownloadUGCResult_t){ .md-button .md-button--doc_classes target="_blank" }

### file_read_async_complete

!!! function "file_read_async_complete"
	Response when reading a file with [fileReadAsync](#filereadasync).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| handle | uint64_t | Call handle of the async read which was made, must be passed to [fileReadAsyncComplete](#filereadasynccomplete) to get the data.
    	| result | int / [Result enum](main.md#result) | The result of the operation. If the local read was successful this will be [RESULT_OK](main.md#constants), you can then call [fileReadAsyncComplete](#filereadasynccomplete) to get the data.
    	| offset | iunt32 | Offset into the file this read was at.
    	| read | uint32 | Amount of bytes read; will be the less than or equal to the amount requested.
    	| complete | bool | Whether or not the file has finished being read.

	------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#RemoteStorageFileReadAsyncComplete_t){ .md-button .md-button--doc_classes target="_blank" }

### file_share_result

!!! function "file_share_result"
	Response to a file being shared.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| result | int / [Result enum](main.md#result) | The result of the operation.
    	| handle | uint64_t | The handle that can be shared with users and features.
    	| name | string | The name of the file that was shared.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#RemoteStorageFileShareResult_t){ .md-button .md-button--doc_classes target="_blank" }

### file_write_async_complete

!!! function "file_write_async_complete"
	Response when writing a file asyncrounously with [fileWriteAsync](#file_write_async_complete).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| result | int / [Result enum](main.md#result) | The result of the operation. If the local write was successful then this will be [RESULT_OK](main.md#result); any other value likely indicates that the filename is invalid or the available quota would have been exceeded by the requested write. Any attempt to write files that exceed this size will return [RESULT_INVALID_PARAM](main.md#result). Writing files to the cloud is limited to 100MiB.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#RemoteStorageFileWriteAsyncComplete_t){ .md-button .md-button--doc_classes target="_blank" }

### local_file_changed

!!! function "local_file_changed"
	One or more files for this app have changed locally after syncing to remote session changes.

	!!! returns "Returns"
		 Nothing.

	!!! info "Notes"
        Only posted if this happens _during_ the local app session.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#RemoteStorageLocalFileChange_t){ .md-button .md-button--doc_classes target="_blank" }

### published_file_subscribed

!!! function "published_file_subscribed"
	User subscribed to a file for the app (from within the app or on the web).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| file_id | uint64_t | The published file id.
    	| app_id | uint32_t | ID of the app that will consume this file.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#RemoteStoragePublishedFileSubscribed_t){ .md-button .md-button--doc_classes target="_blank" }

### published_file_unsubscribed

!!! function "published_file_unsubscribed"
	User unsubscribed to a file for the app (from within the app or on the web).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | file_id | uint64_t | The published file id.
        | app_id | uint32_t | ID of the app that will consume this file.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#RemoteStoragePublishedFileUnsubscribed_t){ .md-button .md-button--doc_classes target="_blank" }

### subscribe_item

!!! function "subscribe_item"
	Called when a player attempts to subscribe to a Workshop item.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| result | int / [Result enum](main.md#result) | The result of the operation.
    	| file_id | uint64_t | The workshop item that the user subscribed to.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#RemoteStorageSubscribePublishedFileResult_t){ .md-button .md-button--doc_classes target="_blank" }

### unsubscribe_item

!!! function "unsubscribe_item"
	Called when a player attempts to unsubscribe from a Workshop item.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | int / [Result enum](main.md#result) | The result of the operation.
        | file_id | uint64_t | The workshop item that the user unsubscribed to.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemoteStorage#RemoteStorageUnsubscribePublishedFileResult_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
ENUMERATE_PUBLISHED_FILES_MAX_RESULTS | k_unEnumeratePublishedFilesMaxResults | 50 | Deprecated; only used with the deprecated RemoteStorage based Workshop API.
FILE_NAME_MAX | k_cchFilenameMax | 260 | The maximum length that a Steam Cloud file path can be.
MAX_CLOUD_FILE_CHUNK_SIZE | k_unMaxCloudFileChunkSize | 100 * 1024 * 1024 | Defines the largest allowed file size for the Steam Cloud.
Cloud files cannot be written in a single chunk over 100MiB and cannot be over 200MiB total.
PUBLISHED_DOCUMENT_CHANGE_DESCRIPTION_MAX | k_cchPublishedDocumentChangeDescriptionMax | 8000 | Unused.
PUBLISHED_DOCUMENT_DESCRIPTION_MAX | k_cchPublishedDocumentDescriptionMax | 8000 | The maximum size in bytes that a Workshop item description can be.
PUBLISHED_DOCUMENT_TITLE_MAX | k_cchPublishedDocumentTitleMax | 128 + 1 | The maximum size in bytes that a Workshop item title can be.
PUBLISHED_FILE_ID_INVALID | k_PublishedFileIdInvalid | 0 | An invalid Workshop item handle.
PUBLISHED_FILE_UPDATE_HANDLE_INVALID | k_PublishedFileUpdateHandleInvalid | 0xffffffffffffffffull | Deprecated - Only used with the deprecated RemoteStorage based Workshop API.
PUBLISHED_FILE_URL_MAX | k_cchPublishedFileURLMax | 256 | The maximum size in bytes that a Workshop item URL can be.
TAG_LIST_MAX | k_cchTagListMax | 1024 + 1 | The maximum size in bytes that a Workshop item comma separated tag list can be.
UGC_FILE_STREAM_HANDLE_INVALID | k_UGCFileStreamHandleInvalid | 0xffffffffffffffffull | Returned when an error has occured when using [fileWriteStreamOpen](#filewritestreamopen).
UGC_HANDLE_INVALID | k_UGCHandleInvalid | 0xffffffffffffffffull | An invalid UGC Handle. This is often returned by functions signifying an error.

{==
## :material-numeric: Enums
==}

### RemoteStoragePlatform

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
REMOTE_STORAGE_PLATFORM_NONE | k_ERemoteStoragePlatformNone | 0 | -
REMOTE_STORAGE_PLATFORM_WINDOWS | k_ERemoteStoragePlatformWindows | (1 << 0) | -
REMOTE_STORAGE_PLATFORM_OSX | k_ERemoteStoragePlatformOSX | (1 << 1) | -
REMOTE_STORAGE_PLATFORM_PS3 | k_ERemoteStoragePlatformPS3 | (1 << 2) | -
REMOTE_STORAGE_PLATFORM_LINUX | k_ERemoteStoragePlatformLinux | (1 << 3) | -
REMOTE_STORAGE_PLATFORM_SWITCH | k_ERemoteStoragePlatformSwitch | (1 << 4) | -
REMOTE_STORAGE_PLATFORM_ANDROID | k_ERemoteStoragePlatformAndroid | (1 << 5) | -
REMOTE_STORAGE_PLATFORM_IOS | k_ERemoteStoragePlatformIOS | (1 << 6) | -
REMOTE_STORAGE_PLATFORM_ALL | k_ERemoteStoragePlatformAll | 0xffffffff | -

### RemoteStoragePublishedFileVisibility

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
REMOTE_STORAGE_PUBLISHED_VISIBLITY_PUBLIC | k_ERemoteStoragePublishedFileVisibilityPublic | 0 | -
REMOTE_STORAGE_PUBLISHED_VISIBLITY_FRIENDS_ONLY | k_ERemoteStoragePublishedFileVisibilityFriendsOnly | 1 | -
REMOTE_STORAGE_PUBLISHED_VISIBLITY_PRIVATE | k_ERemoteStoragePublishedFileVisibilityPrivate | 2 | -
REMOTE_STORAGE_PUBLISHED_VISIBILITY_UNLISTED | 3 | -

### UGCReadAction

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
UGC_READ_CONTINUE_READING_UNTIL_FINISHED | k_EUGCRead_ContinueReadingUntilFinished | 0 | Keeps the file handle open unless the last byte is read.  You can use this when reading large files (over 100MB) in sequential chunks. If the last byte is read, this will behave the same as UGC_READ_CLOSE. Otherwise, it behaves the same as UGC_READ_CONTINUE_READING. This value maintains the same behavior as before the UGCReadAction parameter was introduced.
UGC_READ_CONTINUE_READING | k_EUGCRead_ContinueReading | 1 | Keeps the file handle open. Use this when using [ugcRead](#ugcread) to seek to different parts of the file. When you are done seeking around the file, make a final call with k_EUGCRead_Close to close it.
UGC_READ_CLOSE | k_EUGCRead_Close | 2 | Frees the file handle. Use this when you're done reading the content. To read the file from Steam again you will need to call UGCDownload again.

### WorkshopEnumerationType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
WORKSHOP_ENUMERATION_TYPE_RANKED_BY_VOTE | k_EWorkshopEnumerationTypeRankedByVote | 0 | -
WORKSHOP_ENUMERATION_TYPE_RECENT | k_EWorkshopEnumerationTypeRecent | 1 | -
WORKSHOP_ENUMERATION_TYPE_TRENDING | k_EWorkshopEnumerationTypeTrending | 2 | -
WORKSHOP_ENUMERATION_TYPE_FAVORITES_OF_FRIENDS | k_EWorkshopEnumerationTypeFavoritesOfFriends | 3 | -
WORKSHOP_ENUMERATION_TYPE_VOTED_BY_FRIENDS | k_EWorkshopEnumerationTypeVotedByFriends | 4 | -
WORKSHOP_ENUMERATION_TYPE_CONTENT_BY_FRIENDS | k_EWorkshopEnumerationTypeContentByFriends | 5 | -
WORKSHOP_ENUMERATION_TYPE_RECENT_FROM_FOLLOWED_USERS | k_EWorkshopEnumerationTypeRecentFromFollowedUsers | 6 | -

### WorkshopFileAction

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
WORKSHOP_FILE_ACTION_PLAYED | k_EWorkshopFileActionPlayed | 0 | -
WORKSHOP_FILE_ACTION_COMPLETED | k_EWorkshopFileActionCompleted | 1 | -

### WorkshopFileType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
WORKSHOP_FILE_TYPE_FIRST | k_EWorkshopFileTypeFirst | 0 | -
WORKSHOP_FILE_TYPE_COMMUNITY | k_EWorkshopFileTypeCommunity | 0 | Normal Workshop item that can be subscribed to.
WORKSHOP_FILE_TYPE_MICROTRANSACTION | k_EWorkshopFileTypeMicrotransaction | 1 | Workshop item that is meant to be voted on for the purpose of selling in-game.
WORKSHOP_FILE_TYPE_COLLECTION | k_EWorkshopFileTypeCollection | 2 | A collection of Workshop or Greenlight items.
WORKSHOP_FILE_TYPE_ART | k_EWorkshopFileTypeArt | 3 | Artwork.
WORKSHOP_FILE_TYPE_VIDEO | k_EWorkshopFileTypeVideo | 4 | External video.
WORKSHOP_FILE_TYPE_SCREENSHOT | k_EWorkshopFileTypeScreenshot| 5 | Screenshot.
WORKSHOP_FILE_TYPE_GAME | k_EWorkshopFileTypeGame | 6 | Greenlight game entry.
WORKSHOP_FILE_TYPE_SOFTWARE | k_EWorkshopFileTypeSoftware | 7 | Greenlight software entry.
WORKSHOP_FILE_TYPE_CONCEPT | k_EWorkshopFileTypeConcept | 8 | Greenlight concept.
WORKSHOP_FILE_TYPE_WEB_GUIDE | k_EWorkshopFileTypeWebGuide | 9 | Steam web guide.
WORKSHOP_FILE_TYPE_INTEGRATED_GUIDE | k_EWorkshopFileTypeIntegratedGuide | 10 | Application integrated guide.
WORKSHOP_FILE_TYPE_MERCH | k_EWorkshopFileTypeMerch | 11 | Workshop merchandise meant to be voted on for the purpose of being sold.
WORKSHOP_FILE_TYPE_CONTROLLER_BINDING | k_EWorkshopFileTypeControllerBinding | 12 | Steam Controller bindings.
WORKSHOP_FILE_TYPE_STEAMWORKS_ACCESS_INVITE | k_EWorkshopFileTypeSteamworksAccessInvite | 13 | Internal.
WORKSHOP_FILE_TYPE_STEAM_VIDEO | k_EWorkshopFileTypeSteamVideo | 14 | Steam video.
WORKSHOP_FILE_TYPE_GAME_MANAGED_ITEM | k_EWorkshopFileTypeGameManagedItem | 15 | Managed completely by the game, not the user, and not shown on the web.
WORKSHOP_FILE_TYPE_CLIP | k_EWorkshopFileTypeClip | 16 | Internal.
WORKSHOP_FILE_TYPE_MAX | k_EWorkshopFileTypeMax | 17

### WorkshopVideoProvider

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
WORKSHOP_VIDEO_PROVIDER_NONE | k_EWorkshopVideoProviderNone | 0 | -
WORKSHOP_VIDEO_PROVIDER_YOUTUBE | k_EWorkshopVideoProviderYoutube | 1 | -

### WorkshopVote

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
WORKSHOP_VOTE_UNVOTED | k_EWorkshopVoteUnvoted | 0 | -
WORKSHOP_VOTE_FOR | k_EWorkshopVoteFor | 1 | -
WORKSHOP_VOTE_AGAINST | k_EWorkshopVoteAgainst | 2 | -
WORKSHOP_VOTE_LATER | k_EWorkshopVoteLater | 3 | -

### LocalFileChange

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
LOCAL_FILE_CHANGE_INVALID | k_ERemoteStorageLocalFileChange_Invalid | 0 | -
LOCAL_FILE_CHANGE_FILE_UPDATED | k_ERemoteStorageLocalFileChange_FileUpdated | 1 | The file was updated from another device.
LOCAL_FILE_CHANGE_FILE_DELETED | k_ERemoteStorageLocalFileChange_FileDeleted | 2 | The file was deleted by another device.

### FilePathType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
FILE_PATH_TYPE_INVALID | k_ERemoteStorageFilePathType_Invalid | 0 | -
FILE_PATH_TYPE_ABSOLUTE | k_ERemoteStorageFilePathType_Absolute | 1 | The file is directly accessed by the game and this is the full path.
FILE_PATH_TYPE_API_FILENAME | k_ERemoteStorageFilePathType_APIFilename | 2 | The file is accessed via the Remote Storage API and this is the filename.
