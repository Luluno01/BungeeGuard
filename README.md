# BungeeGuard

A blacklist and whitelist BungeeCord plugin with support of UUID look up.

Tested on Waterfall, version: `git:Waterfall-Bootstrap:1.15-SNAPSHOT:87d2873:326`.

- [BungeeGuard](#bungeeguard)
  - [Features](#features)
  - [Usage](#usage)
  - [Configuration](#configuration)
  - [Commands](#commands)
    - [Whitelist](#whitelist)
      - [whitelist add](#whitelist-add)
      - [whitelist lazy-add](#whitelist-lazy-add)
      - [whitelist remove](#whitelist-remove)
      - [whitelist lazy-remove](#whitelist-lazy-remove)
      - [whitelist on](#whitelist-on)
      - [whitelist off](#whitelist-off)
    - [Blacklist](#blacklist)
      - [blacklist add](#blacklist-add)
      - [blacklist lazy-add](#blacklist-lazy-add)
      - [blacklist remove](#blacklist-remove)
      - [blacklist lazy-remove](#blacklist-lazy-remove)
      - [blacklist on](#blacklist-on)
      - [blacklist off](#blacklist-off)
    - [Main Command](#main-command)
      - [bungeeguard reload](#bungeeguard-reload)
      - [bungeeguard status](#bungeeguard-status)
      - [bungeeguard dump](#bungeeguard-dump)
  - [Lazy Lists](#lazy-lists)
  - [Important Notes](#important-notes)

## Features

* First **UUID-based** blacklist and whitelist plugin for BungeeCord
* Add and remove players by their **username** or UUID (username-change-proof)
* Lazy translation from username to UUID (from v1.2, **offline server friendly**, see [lazy lists](#lazy-lists) for details)

## Usage

Download pre-compiled jar file from [release page](https://github.com/Luluno01/BungeeGuard/releases). Put downloaded jar file under `<path/to/BungeeCord/plugins>`.

## Configuration

The configuration file for BungeeGuard is `plugins/BungeeGuard/config.yml`.

```yaml
#########################################
#       BungeeGuard Configuration       #
#            Version: 1.2               #
#          Author: Untitled             #
#########################################

# You can safely ignore this
version: "1.2"

# Message to be sent to player when that player is blocked for not being whitelisted
whitelist-message: :( You are not whitelisted on this server

# Message to be sent to player when that player is blocked for being blacklisted
blacklist-message: :( We can't let you enter this server

# Whether to use whitelist
enable-whitelist: true

# Lazy-whitelist (array of usernames)
# lazy-whitelist:
# - <lazy-whitelisted username>
lazy-whitelist:

# Whitelist (array of UUIDs)
# whitelist:
# - <whitelisted UUID>
whitelist:

# Whether to use blacklist
enable-blacklist: false

# Lazy-blacklist (array of usernames)
# lazy-blacklist:
# - <lazy-blacklisted username>
lazy-blacklist:

# Blacklist (array of UUIDs)
# blacklist:
# - <banned UUID>
blacklist:
```

Note that if you enable both blacklist and whitelist (which is weird, but it is possible to do that), player in both lists will be blocked because blacklist has a higher priority over whitelist.

## Commands

### Whitelist

Alias: `wlist`.

#### whitelist add

Add player(s) to whitelist:

```
whitelist add <space separated usernames or UUIDs>
```

Example:

```
whitelist add DummyPlayer0 DummyPlayer1 7be767e5-327c-4abd-852b-afab3ec1e2ff DummyPlayer2
```

#### whitelist lazy-add

Alias: `whitelist lazyadd` or `whitelist ladd`.

Add player(s) to lazy-whitelist:

```
whitelist lazy-add <space separated usernames or UUIDs>
```

Example:

```
whitelist lazy-add DummyPlayer0 DummyPlayer1 7be767e5-327c-4abd-852b-afab3ec1e2ff DummyPlayer2
```

#### whitelist remove

Alias: `whitelist rm`.

Remove player(s) from whitelist:

```
whitelist remove <space separated usernames or UUIDs>
```

Example:

```
whitelist remove DummyPlayer0 DummyPlayer1 7be767e5-327c-4abd-852b-afab3ec1e2ff DummyPlayer2
```

#### whitelist lazy-remove

Alias: `whitelist lazyremove`, `whitelist lremove` or `whitelist lrm`.

Remove player(s) from lazy-whitelist:

```
whitelist lazy-remove <space separated usernames or UUIDs>
```

Example:

```
whitelist lazy-remove DummyPlayer0 DummyPlayer1 7be767e5-327c-4abd-852b-afab3ec1e2ff DummyPlayer2
```

#### whitelist on

Turn on whitelist:

```
whitelist on
```

#### whitelist off

Turn off whitelist:

```
whitelist off
```

### Blacklist

Alias: `blist`.

#### blacklist add

Add player(s) to blacklist:

```
blacklist add <space separated usernames or UUIDs>
```

Example:

```
blacklist add DummyPlayer0 DummyPlayer1 7be767e5-327c-4abd-852b-afab3ec1e2ff DummyPlayer2
```

#### blacklist lazy-add

Alias: `blacklist lazyadd` or `blacklist ladd`.

Add player(s) to lazy-blacklist:

```
blacklist lazy-add <space separated usernames or UUIDs>
```

Example:

```
blacklist lazy-add DummyPlayer0 DummyPlayer1 7be767e5-327c-4abd-852b-afab3ec1e2ff DummyPlayer2
```

#### blacklist remove

Alias: `blacklist rm`.

Remove player(s) from blacklist:

```
blacklist remove <space separated usernames or UUIDs>
```

Example:

```
blacklist remove DummyPlayer0 DummyPlayer1 7be767e5-327c-4abd-852b-afab3ec1e2ff DummyPlayer2
```

#### blacklist lazy-remove

Alias: `blacklist lazyremove`, `blacklist lremove` or `blacklist lrm`.

Remove player(s) from lazy-blacklist:

```
blacklist lazy-remove <space separated usernames or UUIDs>
```

Example:

```
blacklist lazy-remove DummyPlayer0 DummyPlayer1 7be767e5-327c-4abd-852b-afab3ec1e2ff DummyPlayer2
```

#### blacklist on

Turn on blacklist:

```
blacklist on
```

#### blacklist off

Turn off blacklist:

```
blacklist off
```

### Main Command

Alias: `bg`.

#### bungeeguard reload

Reload configuration (from file `plugins/BungeeGuard/config.yml`):

```
bungeeguard reload
```

#### bungeeguard status

Check status of blacklist and whitelist:

```
bungeeguard status
```

#### bungeeguard dump

Dump currently loaded blacklist and whitelist:

```
bungeeguard dump
```

## Lazy Lists

Records are added to/removed from lazy lists via `whitelist lazy-*` and `blacklist lazy-*` commands.

Lazy-whitelist and lazy-blacklist work in a very similar way. Let's take lazy-whitelist as example, and you will understand how both of them work. Lazy-whitelist is a different list from the plain whitelist you access via `whitelist add` and `whitelist remove`. Upon record addition, username is added to lazy-whitelist rather than translated UUID, which may take some considerable time or even fail to translate. What's more, because the translation requests are sent to Mojang, implicitly requiring that the server is running in online mode (unless you hijack the requests and redirect them to your own authentication server). The workaround (or maybe it is actually a great feature) is not to do the translation immediately but to save the username in a temporary list, i.e. lazy-whitelist. Because server will be told the UUID of the player upon client connection (if I am right), we are able to lazily translate username to UUID without sending HTTP request. In other words, usernames in lazy-whitelist are translated into UUIDs and moved to whitelist (the plain one) once the server knows the corresponding UUID of the username, i.e. when player with the username connect to the server for the first time.

In this way, offline servers should be able to use this plugin painlessly.

## Important Notes

BungeeGuard does asynchronous UUID look up when you execute add/remove on the lists.
It's recommended to execute those command only in console, and wait patiently for the completion feedback from the command before executing other commands of BungeeGuard.

Offline servers should be able to use this plugin by using lazy lists or supplying BungeeGuard with players' UUIDs rather than their usernames. However, offline servers will still suffering from UUID abuse if they have no authentication plugin installed or have no external authentication mechanism. Offline server owners need to fully understand whitelist and blacklist is **NOT** a prevention of UUID abuse.
