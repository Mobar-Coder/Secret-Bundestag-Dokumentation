# Message standard

All messages are in JSON format and the first field is the message type

## Client to Server:

### join/create lobby
If the lobby name does not exist on the server, a new one is created.
```js
{
    "name": "join",
    "body": {
        "name": string,
        "lobby": string
    }
}
```

### reply to decision
Generic decision to a server request. Example: Server requests player to choose a card to play.
```js
{
    "name": "genericReply",
    "body": {
        "decision": string
    }
}
```

## Server to Client:

### start game

```js
{
    "name": "gameStart",
    "body": {
        "fraction": string,
        "role": string,
        "player": string,
        "hitler": string // (null for liberal players or hitler)
        "teamMates": [string] // (null for liberal players or hitler)
    }
}
```

### decisions
Server asks client to pick an option. Example: Type: Pick a card, choices: liberal, fascist.
```js
{
    "name": "requestDecision",
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
    "name": "gameState",
    "body": {
        "liberalPolicies": int,
        "fascistPolicies": int,
        "chancellor": string, // can be null
        "president": string,
        "pastOffice": string, // can be null
        "electionTracker": int,
        "players" [{
            "name": string,
            "alive": bool
        }],
        "cardPile": int,
        "discardPile": int
    }
}
```

### end of game
```js
{
    "name": "gameEnd",
    "body": {
        "winnerTeam": string
    }
}
```
