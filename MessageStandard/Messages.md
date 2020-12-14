# Message standard
This is Version 1 of the message standard.
All messages are in JSON format.  Every message has the following header:
```js
{
    "name": string, //the name of the message type
    "timestamp": string,
    "version": 1,
    "body": {
        ...
    }
}
```
The timestamp is according to ISO8601. All following message definitions contain this message header even though it is not explicitly specified.

## Client to Server:

### join/create lobby
If the lobby name does not exist on the server, a new one is created.
```js
{
    "name": "JoinRequest",
    "body": {
        "name": string,
        "lobby": string
    }
}
```
The player who creates a lobby is promoted to lobby leader and may send a StartRequest to ask the server to start the game.

```js
{
    "name": "StartRequest",
    "body": {
    }
}
```

### reply to decision
Generic decision to a server request. Example: Server requests player to choose a card to play.
```js
{
    "name": "GenericReply",
    "body": {
        "decision": string
    }
}
```

## Server to Client:

### accept connection

```js
{
    "name": "Accept",
    "body": {
        "leader": bool
    }
}
```

### start game

```js
{
    "name": "GameStart",
    "body": {
        "fraction": string,
        "role": string,
        "hitler": string // (null for liberal players or hitler)
        "teamMates": [string] // (null for liberal players or hitler)
    }
}
```

### decisions
Server asks client to pick an option. Example: Type: Pick a card, choices: liberal, fascist.
```js
{
    "name": "RequestDecision",
    "body": {
        "type": string
        "choices": [string]
    }
}
```

### game state
Snapshot for clients
```js
{
    "name": "GameState",
    "body": {
        "liberalPolicies": int,
        "fascistPolicies": int,
        "chancellor": string, // can be null
        "electionTracker": int,
        "players" [{
            "name": string,
            "alive": bool
            "govRole": string //one of "President, Chancellor, Candidate or null"
        }],
        "cardPile": int,
        "discardPile": int
    }
}
```

### end of game
```js
{
    "name": "GameEnd",
    "body": {
        "winnerTeam": string
    }
}
```
