---
title: HTTP
description: A class reference for HTTP.
icon: material/connection
---

# HTTP

A small and easy to use HTTP client to send and receive data from the web.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } and [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### createCookieContainer

!!! function "createCookieContainer( `bool` allow_responses_to_modify )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | allow_responses_to_modify | bool | Set whether the server can set cookies in this container.

    Creates a cookie container to store cookies during the lifetime of the process. This API is just for during process lifetime, after Steam restarts no cookies are persisted and you have no way to access the cookie container across repeat executions of your process.

	If **allow_responses_to_modify** is true then any response to your requests using this cookie container may add new cookies to the container which may be transmitted with future requests. Otherwise, if it's false, then only cookies you explicitly set will be sent.

	You can associate the cookie container with a http request by using [setHTTPRequestCookieContainer](#sethttprequestcookiecontainer), and you can set a cookie using [setHTTPCookie](#sethttpcookie). Don't forget to free the container when you're done with it to prevent leaking memory by calling [releaseCookieContainer](#releasecookiecontainer)!
	
	!!! returns "Returns: uint32_t"
        Returns a new cookie container handle to be used with future calls to SteamHTTP functions.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#CreateCookieContainer){ .md-button .md-button--store target="_blank" }

### createHTTPRequest

!!! function "createHTTPRequest( `HTTPMethod` request_method, `string` absolute_url )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_method | [HTTPMethod enum](#httpmethod) | The type of request to make with this request.
    | absolute_url | string | The url to request. Must start with "http://" or "https://".

	Initializes a new HTTP request. Requires the method such as GET or POST and the absolute URL for the request. Both http and https are supported, so this string must start with "http://" or "https://" and should look like "http://store.steampowered.com/app/10/" or similar.

	This call returns a handle that you can use to make further calls to setup and then send the HTTP request with [sendHTTPRequest](#sendhttprequest) or [sendHTTPRequestAndStreamResponse](#sendhttprequestandstreamresponse). Don't forget to free the HTTP request when you're done with it to prevent leaking memory by calling [releaseHTTPRequest](#releasehttprequest).

	!!! returns "Returns: uint32_t"
        Returns a new request handle to be used with future calls to Steam HTTP functions. Returns [HTTPREQUEST_INVALID_HANDLE](#constants) if **absolute_url** is empty.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#CreateHTTPRequest){ .md-button .md-button--store target="_blank" }

### deferHTTPRequest

!!! function "deferHTTPRequest( `uint32_t` request_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to defer.

	Defers a request you have sent; the actual HTTP client code may have many requests queued and this will move the specified request to the tail of the queue.

	!!! returns "Returns: bool"
		Returns true if the request has been successfully defered. Otherwise false if **request_handle** is an invalid handle or if the request has not been sent yet.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#DeferHTTPRequest){ .md-button .md-button--store target="_blank" }

### getHTTPDownloadProgressPct

!!! function "getHTTPDownloadProgressPct( `uint32_t` request_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to get the download percentage for.

	Gets progress on downloading the body for the request. This will be zero unless a response header has already been received which included a content-length field. For responses that contain no content-length it will report zero for the duration of the request as the size is unknown until the connection closes.

	!!! returns "Returns: float"
        May be 0.0 if the **request_handle** is invalid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#GetHTTPDownloadProgressPct){ .md-button .md-button--store target="_blank" }

### getHTTPRequestWasTimedOut

!!! function "getHTTPRequestWasTimedOut( `int` request_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to check the failure reason for.

	Check if the reason the request failed was because we timed it out (rather than some harder failure). You'll want to call this within the context of [http_request_completed](#http_request_completed) callback if **request_success** is false.

	!!! returns "Returns: bool"
        Returns true upon success if we successfully checked; returns false under the following conditions:

        * **request_handle** was invalid.
        * The request has not been sent or has not completed.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#GetHTTPRequestWasTimedOut){ .md-button .md-button--store target="_blank" }

### getHTTPResponseBodyData

!!! function "getHTTPResponseBodyData( `uint32_t` request_handle, `uint32_t` buffer_size )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to get the response body data for.
    | buffer_size | uint32_t | This should be the size of returned PackedByteArray in bytes.

	Gets the body data from an HTTP response. This must be called after the HTTP request has completed and returned the HTTP response via the [http_request_completed](#http_request_completed) call result associated with this request handle.

	You should first call [getHTTPResponseBodySize](#gethttpresponsebodysize) or use the **body_size** variable provided in the call result, you can then allocate a buffer with that size to pass into this function.

	This is only for HTTP requests which were sent with [sendHTTPRequest](#sendhttprequest). Use [getHTTPStreamingResponseBodyData](#gethttpstreamingresponsebodydata) if you're using streaming HTTP requests via [sendHTTPRequestAndStreamResponse](#sendhttprequestandstreamresponse).

	!!! returns "Returns: PackedByteArray"
        This array may be empty if:

        * **request_handle** was invalid.
        * The request has not been sent or has not completed.
        * The request is a streaming request.
        * **buffer_size** is not the same as what was provided by [getHTTPResponseBodySize](#gethttpresponsebodysize).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#GetHTTPResponseBodyData){ .md-button .md-button--store target="_blank" }

### getHTTPResponseBodySize

!!! function "getHTTPResponseBodySize( `uint32_t` request_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to get the response body size for.

    Gets the size of the body data from an HTTP response. This must be called after the HTTP request has completed and returned the HTTP response via the [http_request_completed](#http_request_completed) or [http_request_data_received](#http_request_data_received) associated with this request handle.

	!!! returns "Returns: uint32_t"
        May return 0 if:

        * **request_handle** was invalid.
        * The request has not been sent or has not completed.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#GetHTTPResponseBodySize){ .md-button .md-button--store target="_blank" }

### getHTTPResponseHeaderSize

!!! function "getHTTPResponseHeaderSize( `uint32_t` request_handle, `string` header_name )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to check for the response header name.
    | header_name | string | The header name to check.

	Checks if a header is present in an HTTP response and returns its size. This must be called after the HTTP request has completed and returned the HTTP response via the [http_request_completed](#http_request_completed) call result associated with this request handle.

	If the response header exists in the response, then you can allocate a correctly sized buffer to get the associated value with [getHTTPResponseHeaderValue](#gethttpresponseheadervalue). Here is a list of standard response header names on [Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Response_fields){ target="\_blank" }.

	!!! returns "Returns: uint32_t"
        May be 0 if:

        * **request_handle** was invalid.
        * The request has not been sent or has not completed.
        * **header_name** is empty.
        * The header name is not present in the response.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#GetHTTPResponseHeaderSize){ .md-button .md-button--store target="_blank" }

### getHTTPResponseHeaderValue

!!! function "getHTTPResponseHeaderValue( `uint32_t` request_handle, `string` header_name, `uint32_t` buffer_size )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to get the response header value for.
    | header_name | string | The header name to get the header value for.
    | buffer_size | uint32_t | This should be the size of returned PackedByteArray in bytes.

	Gets a header value from an HTTP response. This must be called after the HTTP request has completed and returned the HTTP response via the [http_request_completed](#http_request_completed) call result associated with this request handle.

	You should first call [getHTTPResponseHeaderSize](#gethttpresponseheadersize) to check for the presence of the header and to get the size. You can then allocate a buffer with that size and pass it into this function. Here is a list of standard response header names on [Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Response_fields){ target="\_blank" }.

	!!! returns "Returns: PackedByteArray"
        This array may be empty if:

        * **request_handle** is invalid.
        * The request has not been sent or has not completed.
        * **header_name** is empty.
        * **buffer_size** was not big enough.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#GetHTTPResponseHeaderValue){ .md-button .md-button--store target="_blank" }

### getHTTPStreamingResponseBodyData

!!! function "getHTTPStreamingResponseBodyData( `uint32_t` request_handle, `uint32_t` offset, `uint32_t` buffer_size )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t |   The request handle to get the response body data for.
    | offset | uint32_t | This must be the offset provided by [http_request_data_received](#http_request_data_received).
    | buffer_size | uint32_t | This must be the size provided by [http_request_data_received](#http_request_data_received).

    Gets the body data from a streaming HTTP response. This must be called after data is received from a streaming HTTP request via the [http_request_completed](#http_request_completed) callback associated with this request handle.

    Typically you'll want to allocate a buffer associated with the request handle using the Content-Length HTTP response field to receive the total size of the data when you receive the header via [http_request_headers_received](#http_request_headers_received). You can then append data to that buffer as it comes in. This is only for streaming HTTP requests which were sent with [sendHTTPRequestAndStreamResponse](#sendhttprequestandstreamresponse). Use [getHTTPResponseBodyData](#gethttpresponsebodydata) if you're using [sendHTTPRequest](#sendhttprequest).

	!!! returns "Returns: PackedByteArray"
        The array may be empty if:

        * **request_handle** was invalid.
        * The request has not been sent or has not completed.
        * The request is not a streaming request.
        * **offset** is not the same offset that was provided by [http_request_data_received](#http_request_data_received).
        * **buffer_size** is not the same size that was provided by [http_request_data_received](#http_request_data_received).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#GetHTTPStreamingResponseBodyData){ .md-button .md-button--store target="_blank" }

### prioritizeHTTPRequest

!!! function "prioritizeHTTPRequest( `uint32_t` request_handle )"		
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to prioritize.

	Prioritizes a request you have sent; the actual HTTP client code may have many requests queued, and this will move the specified request to the head of the queue.

	!!! returns "Returns: bool"
		Returns true if the request has been successfully prioritized; otherwise, false if **request_handle** is an invalid handle or if the request has not been sent yet.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#PrioritizeHTTPRequest){ .md-button .md-button--store target="_blank" }

### releaseCookieContainer

!!! function "releaseCookieContainer( `uint32_t` cookie_handle )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | cookie_handle | uint32_t | The cookie container handle to release.

	Releases a cookie container, freeing the memory allocated within Steam. You **must** call this when you are done using each HTTP cookie container handle that you obtained via [createCookieContainer](#createcookiecontainer)!

	!!! returns "Returns: bool"
        Returns true if the handle has been freed; otherwise, false if the handle was invalid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#ReleaseCookieContainer){ .md-button .md-button--store target="_blank" }

### releaseHTTPRequest

!!! function "releaseHTTPRequest( `uint32_t` request_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to release.

    Releases an HTTP request handle, freeing the memory allocated within Steam; should always be called to free resources after receiving a [http_request_completed](#http_request_completed) callback and finishing using the response. You **must** call this when you are done using each HTTP request handle that you obtained via [createHTTPRequest](#createhttprequest).

	!!! returns "Returns: bool"
        Returns true if the the handle was released successfully; otherwise, false only if the handle is invalid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#ReleaseHTTPRequest){ .md-button .md-button--store target="_blank" }

### sendHTTPRequest

!!! function "sendHTTPRequest( `uint32_t` request_handle )"		
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to send.

    Sends an HTTP request. This call is asynchronous and provides a call result handle which you must use to track the call to its completion. If you have multiple requests in flight at the same time you can use [prioritizeHTTPRequest](#prioritizehttprequest) or [deferHTTPRequest](#deferhttprequest) to set the priority of the request.

	If the user is in offline mode in Steam, then this will add an only-if-cached cache-control header and only do a local cache lookup rather than sending any actual remote request.

	If the data you are expecting is large, you can use [sendHTTPRequestAndStreamResponse](#sendhttprequestandstreamresponse) to stream the data in chunks.

	!!! returns "Returns: bool"
        Returns true upon successfully setting the parameter; otherwise, false if:

        * **request_handle** was invalid.
        * The request has already been sent.

    !!! trigger "Triggers"
        [http_request_completed](#http_request_completed) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SendHTTPRequest){ .md-button .md-button--store target="_blank" }

### sendHTTPRequestAndStreamResponse

!!! function "sendHTTPRequestAndStreamResponse( `uint32_t` request_handle )"		
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to send.

    Sends an HTTP request and streams the response back in chunks. This call is asynchronous and provides a call result handle which you must use to track the call to its completion. Typically you'll want to allocate a buffer associated with the request handle using the Content-Length HTTP response field to receive the total size of the data when you receive the header via [http_request_headers_received](#http_request_headers_received). You can then append data to that buffer as it comes in.

	If you have multiple requests in flight at the same time you can use [prioritizeHTTPRequest](#prioritizehttprequest) or [deferHTTPRequest](#deferhttprequest) to set the priority of the request.

	If the user is in offline mode in Steam, then this will add an only-if-cached cache-control header and only do a local cache lookup rather than sending any actual remote request.

	If the data you are expecting is small (on the order of a few megabytes or less) then you'll likely want to use [sendHTTPRequest](#sendhttprequest).

	!!! returns "Returns: bool"
        Returns true upon successfully setting the parameter; otherwise, false if:

        * **request_handle** was invalid.
        * The request has already been sent.

    !!! trigger "Triggers"
        * [http_request_data_received](#http_request_data_received) callback
        * [http_request_headers_received](#http_request_headers_received) callback
        * [http_request_completed](#http_request_completed) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SendHTTPRequestAndStreamResponse){ .md-button .md-button--store target="_blank" }

### setHTTPCookie

!!! function "setHTTPCookie( `uint32_t` cookie_handle, `string` host, `string` url, `string` cookie_name )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | cookie_handle | uint32_t | The cookie container to set the the cookie in.
    | host | string | The host to set this cookie for.
    | url | string | The url to set this cookie for.
    | cookie_name | string | The cookie to set.

    Adds a cookie to the specified cookie container that will be used with future requests.

	!!! returns "Returns: bool"
        Returns true if the cookie was set successfully; Otherwise, false if the request handle was invalid or if there was a security issue parsing the cookie.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SetCookie){ .md-button .md-button--store target="_blank" }

### setHTTPRequestAbsoluteTimeoutMS

!!! function "setHTTPRequestAbsoluteTimeoutMS( `uint32_t` request_handle, `uint32_t` milliseconds )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to set the timeout on.
    | milliseconds | uint32_t | The length of the timeout period in milliseconds.

    Set an absolute timeout in milliseconds for the HTTP request.

	This is the total time timeout which is different than the network activity timeout which is set with [setHTTPRequestNetworkActivityTimeout](#sethttprequestnetworkactivitytimeout) which can bump everytime we get more data.

	!!! returns "Returns: bool"
        Returns true upon successfully setting the timeout; otherwise, false if:

        * **request_handle** was invalid.
        * The request has already been sent.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SetHTTPRequestAbsoluteTimeoutMS){ .md-button .md-button--store target="_blank" }

### setHTTPRequestContextValue

!!! function "setHTTPRequestContextValue( `uint32_t` request_handle, `uint64_t` context_value )"	
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to set the context value on.
    | context_value | uint64_t | The context value to set.

    Set a context value for the request, which will be returned in the [http_request_completed](#http_request_completed) callback after sending the request.

	This is just so the caller can easily keep track of which callbacks go with which request data.

	Must be called before sending the request.

	!!! returns "Returns: bool"
        Returns true upon successfully setting the context value; otherwise, false if:

        * **request_handle** was invalid.
        * The request has already been sent.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SetHTTPRequestContextValue){ .md-button .md-button--store target="_blank" }

### setHTTPRequestCookieContainer

!!! function "setHTTPRequestCookieContainer( `uint32_t` request_handle, `uint32_t` cookie_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to associate the cookie container with.
    | cookie_handle | uint32_t | The cookie container handle to associate with the request handle.

    Associates a cookie container to use for an HTTP request.

	!!! returns "Returns: bool"
        Returns true upon successfully setting the cookie container; otherwise, false if:

        * **request_handle** was invalid.
        * **cookie_handle** was invalid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SetHTTPRequestCookieContainer){ .md-button .md-button--store target="_blank" }

### setHTTPRequestGetOrPostParameter

!!! function "setHTTPRequestGetOrPostParameter( `uint32_t` request_handle, `string` name, `string` value )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to set the parameter on.
    | param_name | string | Parameter name field.
    | param_value | string | Value to associate with the name field.

    Set a GET or POST parameter value on the request, which is set will depend on the [HTTPMethod enum](#httpmethod) specified when creating the request.

	Must be called prior to sending the request.

	!!! returns "Returns: bool"
		Returns true upon successfully setting the parameter; otherwise, false if:

        * **request_handle** was invalid.
        * The request has already been sent.
        * **name** or **value** are empty.
        * The request method set in [createHTTPRequest](#createhttprequest) is not [HTTP_METHOD_GET](#httpmethod), [HTTP_METHOD_HEAD](#httpmethod), or [HTTP_METHOD_POST](#httpmethod).
        * If the request method is [HTTP_METHOD_POST](#httpmethod) and a POST body has already been set with [setHTTPRequestRawPostBody](#sethttprequestrawpostbody).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SetHTTPRequestGetOrPostParameter){ .md-button .md-button--store target="_blank" }

### setHTTPRequestHeaderValue

!!! function "setHTTPRequestHeaderValue( `uint32_t` request_handle, `string` header_name, `string` header_value )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to set the header value for.
    | header_name | string | The header name field.
    | header_value | string | Value to associate with the header name field.

    Set a request header value for the request, must be called prior to sending the request.

	Must be called before sending the request.

	A full list of standard request fields are available here on [Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields#Request_fields){ target="_blank" }. The User-Agent field is explicitly disallowed as it gets overwritten when the request is sent.

	!!! returns "Returns: bool"
		Returns true upon successfully setting the header value; otherwise, false if:

        * **request_handle** was invalid.
        * The request has already been sent.
        * **header_name** is "User-Agent".
        * **header_name** or **header_value** are empty.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SetHTTPRequestHeaderValue){ .md-button .md-button--store target="_blank" }

### setHTTPRequestNetworkActivityTimeout

!!! function "setHTTPRequestNetworkActivityTimeout( `uint32_t` request_handle, `uint32_t` timeout_seconds )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to set the timeout on.
    | timeout_seconds | uint32_t | The length of the timeout period in seconds.

    Set the timeout in seconds for the HTTP request.

	The default timeout is 60 seconds if you don't call this. This can get bumped everytime we get more data. Use [setHTTPRequestAbsoluteTimeoutMS](#sethttprequestabsolutetimeoutms) if you need a strict maximum timeout.

	!!! returns "Returns: bool"
        Returns true upon successfully setting the timeout; otherwise, false if:

        * **request_handle** was invalid.
        * The request has already been sent.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SetHTTPRequestNetworkActivityTimeout){ .md-button .md-button--store target="_blank" }

### setHTTPRequestRawPostBody

!!! function "setHTTPRequestRawPostBody( `uint32_t` request_handle, `string` content_type, `string` body )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to set the post body on.
    | content_type | string | Sets the value of the calls "content-type" http header.
    | body | string | The raw POST body data to set.

    Sets the body for an HTTP Post request.

	Will fail and return false on a GET request, and will fail if POST params have already been set for the request. Setting this raw body makes it the only contents for the post, the **content_type** parameter will set the "content-type" header for the request to inform the server how to interpret the body.

	!!! returns "Returns: bool"
        Returns true upon success indicating that the content-type field and the body data have been set; otherwise, false if:

        * **request_handle** was invalid.
        * The HTTP Method set in [createHTTPRequest](#createhttprequest) is not [HTTP_METHOD_POST](#httpmethod), [HTTP_METHOD_PUT](#httpmethod), or [HTTP_METHOD_PATCH](#httpmethod).
        * A POST body has already been set for this request either via this function or with [setHTTPRequestGetOrPostParameter](#sethttprequestgetorpostparameter).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SetHTTPRequestRawPostBody){ .md-button .md-button--store target="_blank" }

### setHTTPRequestRequiresVerifiedCertificate

!!! function "setHTTPRequestRequiresVerifiedCertificate( `uint32_t` request_handle, `bool` require_verified_certificate )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to set whether the request requires a verified certificate.
    | require_verified_certificate | bool | Require verified certificates?

    Disable or re-enable verification of SSL/TLS certificates. By default, certificates are checked for all HTTPS requests.

	!!! returns "Returns: bool"
        Returns true upon success; otherwise, false if the request handle is invalid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SetHTTPRequestRequiresVerifiedCertificate){ .md-button .md-button--store target="_blank" }

### setHTTPRequestUserAgentInfo

!!! function "setHTTPRequestUserAgentInfo( `uint32_t` request_handle, `string` user_agent_info )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | request_handle | uint32_t | The request handle to set the user agent info for.
    | user_agent_info | string | The string to append to the end of the user agent.

    Set additional user agent info for a request.

	This doesn't clobber the normal user agent, it just adds the extra info on the end. Sending an empty string resets the user agent info to the default value.

	!!! returns "Returns: bool"
        Returns true upon success indicating that the user agent has been updated; otherwise, false if the request handle is invalid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#SetHTTPRequestUserAgentInfo){ .md-button .md-button--store target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### http_request_completed

!!! function "http_request_completed"
	Result when an HTTP request completes. If you're using [getHTTPStreamingResponseBodyData](#gethttpstreamingresponsebodydata) then you should be using the [http_request_headers_received](#http_request_headers_received) or [http_request_data_received](#http_request_data_received).

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | request_handle | uint32_t | Handle for the request that has completed. You should call [releaseHTTPRequest](#releasehttprequest) on this handle to free the resources when you're done using it, which is typically in this callback.
        | context_value | uint64_t | Context value that the user defined on the request with [setHTTPRequestContextValue](#sethttprequestcontextvalue) that this callback is associated with. 0 if no context value was set.
        | request_success | bool | This will be true if the request actually got any sort of response from the server (even an error); otherwise, it will be false if the request failed due to an internal error or client side network failure.
        | status_code | int | Will be the HTTP status code value returned by the server. [HTTP_STATUS_CODE_200_OK](#httpstatuscode) is the normal OK response, if you get something else you probably need to treat it as a failure.
        | body_size | uint32_t | The size of the request body in bytes. This is the same as [getHTTPResponseBodySize](#gethttpresponsebodysize).

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#HTTPRequestCompleted_t){ .md-button .md-button--store target="_blank" }

### http_request_data_received

!!! function "http_request_data_received"
	Triggered when a chunk of data is received from a streaming HTTP request.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| request_handle | uint32_t | Handle value for the request that has received data.
        | context_value | uint64_t | Context value that the user defined on the request that this callback is associated with, 0 if no context value was set.
        | offset | uint32_t | Offset to provide to [getHTTPStreamingResponseBodyData](#gethttpstreamingresponsebodydata) to get this chunk of data.
        | bytes_received | uint32_t | Size in bytes to provide to [getHTTPStreamingResponseBodyData](#gethttpstreamingresponsebodydata) to get this chunk of data.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#HTTPRequestDataReceived_t){ .md-button .md-button--store target="_blank" }

### http_request_headers_received

!!! function "http_request_headers_received"
	Triggered when HTTP headers are received from a streaming HTTP request.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| request_handle | uint32_t | Handle value for the request that has received headers.
        | context_value | uint64_t | Context value that the user defined on the request that this callback is associated with, 0 if no context value was set.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTTP#HTTPRequestHeadersReceived_t){ .md-button .md-button--store target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
HTTPCOOKIE_INVALID_HANDLE | INVALID_HTTPCOOKIE_HANDLE | 0 | -
HTTPREQUEST_INVALID_HANDLE | INVALID_HTTPREQUEST_HANDLE | 0 | -

{==
## :material-numeric: Enums
==}

### HTTPMethod

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
HTTP_METHOD_INVALID | k_EHTTPMethodInvalid | 0 | -
HTTP_METHOD_GET | k_EHTTPMethodGET | 1 | -
HTTP_METHOD_HEAD | k_EHTTPMethodHEAD | 2 | -
HTTP_METHOD_POST | k_EHTTPMethodPOST | 3 | -
HTTP_METHOD_PUT | k_EHTTPMethodPUT | 4 | -
HTTP_METHOD_DELETE | k_EHTTPMethodDELETE |5 | - 
HTTP_METHOD_OPTIONS | k_EHTTPMethodOPTIONS | 6 | -
HTTP_METHOD_PATCH | k_EHTTPMethodPATCH | 7 | -

### HTTPStatusCode

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
HTTP_STATUS_CODE_INVALID | k_EHTTPStatusCodeInvalid | 0 | -
HTTP_STATUS_CODE_100_CONTINUE | k_EHTTPStatusCode100Continue | 100 | -
HTTP_STATUS_CODE_101_SWITCHING_PROTOCOLS | k_EHTTPStatusCode101SwitchingProtocols | 101 | -
HTTP_STATUS_CODE_200_OK | k_EHTTPStatusCode200OK | 200 | -
HTTP_STATUS_CODE_201_CREATED | k_EHTTPStatusCode201Created | 201 | -
HTTP_STATUS_CODE_202_ACCEPTED | k_EHTTPStatusCode202Accepted | 202 | -
HTTP_STATUS_CODE_203_NON_AUTHORITATIVE | k_EHTTPStatusCode203NonAuthoritative | 203 | -
HTTP_STATUS_CODE_204_NO_CONTENT | k_EHTTPStatusCode204NoContent | 204 | -
HTTP_STATUS_CODE_205_RESET_CONTENT | k_EHTTPStatusCode205ResetContent | 205 | -
HTTP_STATUS_CODE_206_PARTIAL_CONTENT | k_EHTTPStatusCode206PartialContent | 206 | -
HTTP_STATUS_CODE_300_MULTIPLE_CHOICES | k_EHTTPStatusCode300MultipleChoices | 300 | -
HTTP_STATUS_CODE_301_MOVED_PERMANENTLY | k_EHTTPStatusCode301MovedPermanently | 301 | -
HTTP_STATUS_CODE_302_FOUND | k_EHTTPStatusCode302Found | 302 | -
HTTP_STATUS_CODE_303_SEE_OTHER | k_EHTTPStatusCode303SeeOther | 303 | -
HTTP_STATUS_CODE_304_NOT_MODIFIED | k_EHTTPStatusCode304NotModified | 304 | -
HTTP_STATUS_CODE_305_USE_PROXY | k_EHTTPStatusCode305UseProxy | 305 | -
HTTP_STATUS_CODE_307_TEMPORARY_REDIRECT | k_EHTTPStatusCode307TemporaryRedirect | 307 | -
HTTP_STATUS_CODE_308_PERMANENT_REDIRECT | k_EHTTPStatusCode308PermanentRedirect | 308 | -
HTTP_STATUS_CODE_400_BAD_REQUEST | k_EHTTPStatusCode400BadRequest | 400 | -
HTTP_STATUS_CODE_401_UNAUTHORIZED | k_EHTTPStatusCode401Unauthorized | 401 | -
HTTP_STATUS_CODE_402_PAYMENT_REQUIRED | k_EHTTPStatusCode402PaymentRequired | 402 | -
HTTP_STATUS_CODE_403_FORBIDDEN | k_EHTTPStatusCode403Forbidden | 403 | -
HTTP_STATUS_CODE_404_NOT_FOUND | k_EHTTPStatusCode404NotFound | 404 | -
HTTP_STATUS_CODE_405_METHOD_NOT_ALLOWED | k_EHTTPStatusCode405MethodNotAllowed | 405 | -
HTTP_STATUS_CODE_406_NOT_ACCEPTABLE | k_EHTTPStatusCode406NotAcceptable | 406 | -
HTTP_STATUS_CODE_407_PROXY_AUTH_REQUIRED | k_EHTTPStatusCode407ProxyAuthRequired | 407 | -
HTTP_STATUS_CODE_408_REQUEST_TIMEOUT | k_EHTTPStatusCode408RequestTimeout | 408 | -
HTTP_STATUS_CODE_409_CONFLICT | k_EHTTPStatusCode409Conflict | 409 | -
HTTP_STATUS_CODE_410_GONE | k_EHTTPStatusCode410Gone | 410 | -
HTTP_STATUS_CODE_411_LENGTH_REQUIRED | k_EHTTPStatusCode411LengthRequired | 411 | -
HTTP_STATUS_CODE_412_PRECONDITION_FAILED | k_EHTTPStatusCode412PreconditionFailed | 412 | -
HTTP_STATUS_CODE_413_REQUEST_ENTITY_TOO_LARGE | k_EHTTPStatusCode413RequestEntityTooLarge | 413 | -
HTTP_STATUS_CODE_414_REQUEST_URI_TOO_LONG | k_EHTTPStatusCode414RequestURITooLong | 414 | -
HTTP_STATUS_CODE_415_UNSUPPORTED_MEDIA_TYPE | k_EHTTPStatusCode415UnsupportedMediaType | 415 | -
HTTP_STATUS_CODE_416_REQUESTED_RANGE_NOT_SATISFIABLE | k_EHTTPStatusCode416RequestedRangeNotSatisfiable | 416 | -
HTTP_STATUS_CODE_417_EXPECTATION_FAILED | k_EHTTPStatusCode417ExpectationFailed | 417 | -
HTTP_STATUS_CODE_4XX_UNKNOWN | k_EHTTPStatusCode4xxUnknown | 418 | -
HTTP_STATUS_CODE_429_TOO_MANY_REQUESTS | k_EHTTPStatusCode429TooManyRequests | 429 | -
HTTP_STATUS_CODE_444_CONNECTION_CLOSED | k_EHTTPStatusCode444ConnectionClosed | 444 | -
HTTP_STATUS_CODE_500_INTERNAL_SERVER_ERROR | k_EHTTPStatusCode500InternalServerError | 500 | -
HTTP_STATUS_CODE_501_NOT_IMPLEMENTED | k_EHTTPStatusCode501NotImplemented | 501 | -
HTTP_STATUS_CODE_502_BAD_GATEWAY | k_EHTTPStatusCode502BadGateway | 502 | -
HTTP_STATUS_CODE_503_SERVICE_UNAVAILABLE | k_EHTTPStatusCode503ServiceUnavailable | 503 | -
HTTP_STATUS_CODE_504_GATEWAY_TIMEOUT | k_EHTTPStatusCode504GatewayTimeout | 504 | -
HTTP_STATUS_CODE_505_HTTP_VERSION_NOT_SUPPORTED | k_EHTTPStatusCode505HTTPVersionNotSupported | 505 | -
HTTP_STATUS_CODE_5XX_UNKNOWN | k_EHTTPStatusCode5xxUnknown | 599 | -