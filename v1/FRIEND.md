FRIEND: Manage Private Subscriptions
====================================

Version 1.0 Maxim Sokhatsky

Endpoints
---------

* `actions/1/api/phone/:phone` — MQTT
* `events/1//api/anon/:client/:token` — MQTT

Tuples
------

```erlang
-record('Friend', {phone_id = [] :: [] | binary(),
                   friend_id = [] :: [] | binary(),
                   status=[] :: [] | request | confirm | revoke }).
```

Overview
--------

FRIEND API serves the management of peer-to-peer private subscriptions.
At the time users establish a friendship the MQTT subscriptions are being created.

Protocol
--------

### `Friend/request` — Friendship Request

```
1. client sends `{'Friend',Id,UserId,FriendId,request}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `<<>>`
             or `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/phone/:phone` once.
```

```
3. server sends `{'Contact',_,_,_,_,_,_,_,_,_,_,_,request}`
             to `ses/:party` once.
```

```
3. server sends `{'Contact',_,_,_,_,_,_,_,_,_,_,_,autorization}`
             to `ses/:counterparty` once.
```

### `Friend/confirm` — Confirm friendship

This is the moment when subscriptions to private
conversation MQTT topic are being created for both counterparties.

```
1. client sends `{'Friend',Id,UserId,FriendId,confirm}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{io,{ok,{already_present,_}},<<>>}`
             or `{io,{error,roster_not_found},<<>>}`
             or `{io,{error,not_authorized},<<>>}`
             to `actions/1/api/phone/:party` once.
```

```
3. server sends `{'Contact',_,_,_,_,_,_,_,_,_,_,_,friend}`
             to `ses/:party` once
            and `ses/:counterparty` once.
```

### `Friend/revoke` — Revoke friendship

This is the moment when subscriptions to private
conversation MQTT topic are being removed for both counterparties.

```
1. client sends `{'Friend',Id,User,revoke}`
             to `events/1//api/anon/:client/:token` once.
```

```
3. server sends `{'Contact',_,_,_,_,_,_,_,_,_,_,_,revoke}`
             to `ses/:party` once.
            and `ses/:counterparty` once.
```

### `Friend/ban` — Ban friend

```
1. client sends `{'Friend',Id,User,ban}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Contact',_,_,_,_,_,_,_,_,_,_,_,banned}`
             to `ses/:party` once.
            and `ses/:counterparty` once.
```

### `Friend/unban` — Unban friend

```
1. client sends `{'Friend',Id,User,unban}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Contact',_,_,_,_,_,_,_,_,_,_,_,unbanned}`
             to `ses/:party` once.
            and `ses/:counterparty` once.
```

### `Friend/mute` — Mute friend

```
1. client sends `{'Friend',Id,User,mute}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Contact',_,_,_,_,_,_,_,_,_,_,_,muted}`
             to `ses/:party` once.
```

### `Friend/unmute` — Unute friend

```
1. client sends `{'Friend',Id,User,unmute}`
             to `events/1//api/anon/:client/:token` once.
```

```
2. server sends `{'Contact',_,_,_,_,_,_,_,_,_,_,_,unmuted}`
             to `ses/:party` once.
```
