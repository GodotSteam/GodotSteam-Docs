---
title: Authentication
description: A guide on setting up authentication.
icon: material/security-network
---

# Tutorials - Authentication
:material-badge-account-horizontal: _By Gramps_

---

In this tutorial we will talk about authenticating users with Steam. Networking is out of scope for this tutorial; you will need to implement your own networking code. You can check out the [lobbies](lobbies.md) and various networking tutorials for more on that, or even use Godot's high-level networking. [You can read more about the whole authentication process in Steam's documentation page on the subject.](https://partner.steamgames.com/doc/features/auth){ target="\_blank" }

??? guide "Relevant GodotSteam classes and functions"
	* [User class](../classes/user.md)
		* [beginAuthSession()](../classes/user.md#beginauthsession)
		* [cancelAuthTicket()](../classes/user.md#cancelauthticket)
		* [endAuthSession()](../classes/user.md#endauthsession)
		* [getAuthSessionTicket()](../classes/user.md#getauthsessionticket)

{==
## Setting Up Signals
==}

First, in both your clients and server, you'll want to set two variables: `auth_ticket` and `client_auth_tickets`. You will keep the local client's ticket dictionary in `auth_ticket`, obviously, and a list of all connected clients in your `client_auth_tickets` array. More on that later.

```gdscript
# Set up some variables
var auth_ticket: Dictionary		# Your auth ticket
var client_auth_tickets: Array	# Array of tickets from other clients
```

Now, we'll set up the signals for authentication callbacks:

=== "Godot 2.x, 3.x"

	```gdscript
	func _ready() -> void:
		Steam.connect("get_auth_session_ticket_response", self, "_on_get_auth_session_ticket_response")
		Steam.connect("validate_auth_ticket_response", self, "_on_validate_auth_ticket_response")
	```

=== "Godot 4.x"

	```gdscript
	func _ready() -> void:
		Steam.get_auth_session_ticket_response.connect(_on_get_auth_session_ticket_response)
		Steam.validate_auth_ticket_response.connect(_on_validate_auth_ticket_response)
	```

Next we implement the respective functions for when we receive the signals:

```gdscript
# Callback from getting the auth ticket from Steam
func _on_get_auth_session_ticket_response(this_auth_ticket: int, result: int) -> void:
	print("Auth session result: %s" % result)
	print("Auth session ticket handle: %s" % this_auth_ticket)
```

Our `_on_get_auth_session_ticket_response()` function will print out the auth ticket's handle and whether getting the ticket was successful (returns a `1`) or not. You can add logic for success or failure based on your game's needs. If successful, you'll probably want to send your new ticket to the server or another client for validation at this point.

Speaking of validation:

```gdscript
# Callback from attempting to validate the auth ticket
func _on_validate_auth_ticket_response(auth_id: int, response: int, owner_id: int) -> void:
	print("Ticket Owner: %s" % auth_id)

	# Make the response more verbose, highly unnecessary but good for this example
	var verbose_response: String
	match response:
		0: verbose_response = "Steam has verified the user is online, the ticket is valid and ticket has not been reused."
		1: verbose_response = "The user in question is not connected to Steam."
		2: verbose_response = "The user doesn't have a license for this App ID or the ticket has expired."
		3: verbose_response = "The user is VAC banned for this game."
		4: verbose_response = "The user account has logged in elsewhere and the session containing the game instance has been disconnected."
		5: verbose_response = "VAC has been unable to perform anti-cheat checks on this user."
		6: verbose_response = "The ticket has been canceled by the issuer."
		7: verbose_response = "This ticket has already been used, it is not valid."
		8: verbose_response = "This ticket is not from a user instance currently connected to steam."
		9: verbose_response = "The user is banned for this game. The ban came via the Web API and not VAC."
	print("Auth response: %s" % verbose_response)
	print("Game owner ID: %s" % owner_id)
```

The `_on_validate_auth_ticket_response()` function is received in response to `beginAuthSession()` when the ticket has been validated. It sends back the Steam ID of the user being authorized, the result of the validation (success is `0` as shown above), and finally the Steam ID of the user that owns the game.

[As Valve notes](https://partner.steamgames.com/doc/api/ISteamUser#ValidateAuthTicketResponse_t){ target="\_blank" }, the owner of the game and the user being authorized may be different if the game is borrowed from Steam Family Library Sharing. Inside this function, you can again put in whatever logic your game requires. You will probably want to add the client to the server if successful, obviously.

{==
## Getting Your Auth Ticket
==}

First you'll want to get an auth ticket from Steam and store it in your `auth_ticket` dictionary variable; this way you can pass it along to the server or other clients as needed:

```gdscript
auth_ticket = Steam.getAuthSessionTicket()
```

This function also has an optional `identity` you can pass to it but defaults to `null`. This identity can correspond to one of your networking identities set up with the [Networking Types](../classes/networking_types.md) class.

Now that you have your auth ticket, you'll want to pass it along to the server or other clients for validation.

{==
## Validating the Auth Ticket
==}

Your server or other clients will now want to take your auth ticket and validate it before allowing you to join the game. In a peer-to-peer situation, every client will want to validate the ticket of every other player. The server or clients will want to pass your `auth_ticket` dictionary's buffer and size, as well as your Steam ID, to `beginAuthSession()`. For this we'll create a `validate_auth_session()` function:

```gdscript
func validate_auth_session(ticket: Dictionary, steam_id: int) -> void:
	var auth_response: int = Steam.beginAuthSession(ticket.buffer, ticket.size, steam_id)

	# Get a verbose response; unnecessary but useful in this example
	var verbose_response: String
	match auth_response:
		0: verbose_response = "Ticket is valid for this game and this Steam ID."
		1: verbose_response = "The ticket is invalid."
		2: verbose_response = "A ticket has already been submitted for this Steam ID."
		3: verbose_response = "Ticket is from an incompatible interface version."
		4: verbose_response = "Ticket is not for this game."
		5: verbose_response = "Ticket has expired."
	print("Auth verifcation response: %s" % verbose_response))

	if auth_response == 0:
		print("Validation successful, adding user to client_auth_tickets")
		client_auth_tickets.append({"id": steam_id, "ticket": ticket.id})
	
	# You can now add the client to the game
```

If the response is `0` (meaning the ticket is valid), you can allow the player to connect to the server or game. A callback will also be received and trigger our `_on_validate_auth_ticket_response()` function which, as we saw before, sends along the Steam ID of the auth ticket provider, the result, and the Steam ID of the owner of the game. This callback will also be triggered when another user cancels their auth ticket. More on that later.

After the ticket is validated, you'll want to save the player's Steam ID and their ticket handle in your `client_auth_tickets` array either as an array or dictionary so they can be called later to cancel the auth sessions. In our example above, we used a dictionary so we can just pull the ticket handle by the user's Steam ID.

{==
## Canceling Auth Tickets and Ending Auth Sessions
==}

Finally when the game is over or the client is leaving the game, you'll want to cancel your own auth ticket and end the auth sessions with other players.

When the client is ready to leave the game, they will pass their own ticket handle to the `cancelAuthTicket()` function like so:

```gdscript
Steam.cancelAuthTicket(auth_ticket.id)
```

This will trigger the `_on_validate_auth_ticket_response()` function for the server or other clients to let them know the player has left and invalidated their auth ticket. Additionally, you should call `endAuthSession()` to also close out the auth session with the server or other clients:

```gdscript
Steam.endAuthSession(steam_id)
```

You will need to pass the Steam ID of every client connected. You can do this in a loop from your `client_auth_tickets` array like so:

```gdscript
for this_client_ticket in client_auth_tickets:
	Steam.endAuthSession(this_client_ticket.id)

	# Then remove this client from the ticket array
	client_auth_tickets.erase(this_client_ticket)
```

The [Steamworks documentation](https://partner.steamgames.com/doc/features/auth){ target="\_blank" } states that each player must cancel their own auth ticket(s) and end any auth sessions started with other players.

That concludes this simple tutorial for authenticated sessions.

{==
## :material-content-save-settings: Additional Resources
==}

### Example Project

[Later this year you can see this tutorial in action with more in-depth information by checking out our upcoming free-to-play game Skillet on GitHub.](https://github.com/GodotSteam/Skillet){ target="\_blank" } There you will be able to view of the code used which can serve as a starting point for you to branch out from.