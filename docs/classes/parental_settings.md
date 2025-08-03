---
title: Parental Settings
description: A class reference for Parental Settings.
icon: material/eye-settings-outline
---

# Parental Settings

Interface to Steam parental settings (Family View).  These functions are not documented in Valve's online documentation nor are they really explained in the Steamworks SDK files.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### isAppBlocked

!!! function "isAppBlocked( `uint32_t` app_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID for the application to check for. |

	Check if the given app is blocked by parental settings.

	!!! returns "Returns: bool"

### isAppInBlockList

!!! function "isAppInBlockList( `uint32_t` app_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID for the application to check for. |

	Check if this app is in the block list.

	!!! returns "Returns: bool"

### isFeatureBlocked

!!! function "isFeatureBlocked( `ParentalFeature` feature )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | feature | [ParentalFeature enum](#parentalfeature) | The feature to check for. |

	Check if the given feature is blocked by parental settings.

	!!! returns "Returns: bool"

### isFeatureInBlockList

!!! function "isFeatureInBlockList( `ParentalFeature` feature )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | feature | [ParentalFeature enum](#parentalfeature) | The feature to check for. |

	Check if the given feature is in the block list.

	!!! returns "Returns: bool"

### isParentalLockEnabled

!!! function "isParentalLockEnabled( )"
	Check if the parental lock is enabled.

	!!! returns "Returns: bool"

### isParentalLockLocked

!!! function "isParentalLockLocked( )"
	Check if the parental lock is actually locked.

	!!! returns "Returns: bool"

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### parental_setting_changed

!!! function "parental_setting_changed"
	Callback for querying UGC.

	!!! returns "Returns"
		Nothing.

{==
## :material-numeric: Enums
==}

### ParentalFeature

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
FEATURE_INVALID | k_EFeatureInvalid | 0 | -
FEATURE_STORE | k_EFeatureStore | 1 | -
FEATURE_COMMUNITY | k_EFeatureCommunity | 2 | -
FEATURE_PROFILE | k_EFeatureProfile | 3 | -
FEATURE_FRIENDS | k_EFeatureFriends | 4 | -
FEATURE_NEWS | k_EFeatureNews | 5 | -
FEATURE_TRADING | k_EFeatureTrading | 6 | -
FEATURE_SETTINGS | k_EFeatureSettings | 7 | -
FEATURE_CONSOLE | k_EFeatureConsole | 8 | -
FEATURE_BROWSER | k_EFeatureBrowser | 9 | -
FEATURE_PARENTAL_SETUP | k_EFeatureParentalSetup | 10 | -
FEATURE_LIBRARY | k_EFeatureLibrary | 11 | -
FEATURE_TEST | k_EFeatureTest | 12 | -
FEATURE_SITE_LICENSE | k_EFeatureSiteLicense | 13 | -
FEATURE_KIOSK_MODE | k_EFeatureKioskMode_Deprecated | 14 | -
FEATURE_BLOCK_ALWAYS | k_EFeatureBlockAlways | 15 | -
FEATURE_MAX | k_EFeatureMax | - | -
