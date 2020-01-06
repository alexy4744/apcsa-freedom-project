# Entry 3
##### 01/05/2020

## Change of plans again
Our idea of making a game has changed again since the last blog post. When we started to code it, me and my partner realized that although the idea sounded cool, we felt that the end result of the game would be boring and not something we imagined. The new game will be somewhat like a battle royale, where in order to win, you have to be the last one standing. Players can push each other off by colliding into each other or with a melee weapon or a gun. The melee weapon will have a high knockback value, but slow rate of fire, while guns will have higher rate of fire but smaller knockback value.

## Engineering Design Process
Since the last blog post where I thought about using a microservice architecture, there was one thing I didn't account for. I didn't account for the cost to run all of my microservices. The game is something I want to run long term on a low budget something like less than $10 a month. But the way I wanted to run my microservices was by using [Kubernetes](https://kubernetes.io/), and the cost of each individual VPS isn't cheap if I'm looking to run each microservice independently + a master node. I could possibly get away with purchasing a VPS equipped with 2 core and 4 gigs of memory from [GalaxyGate](https://galaxygate.net/hosting/vps/) and running all my microservices on it using [k3s](https://github.com/rancher/k3s); however it pretty much defeats the purpose of my goal to learn microservices in the first place.

### Firebase
Instead of using microservices, I will be using [Firebase](https://firebase.google.com/) instead. Firebase's [free plan](https://firebase.google.com/pricing) is plentiful enough for now and their Flame plan is priced reasonably. The thing that caught my attention with Firebase is their Cloud Firestore.

With Cloud Firestore, scaling is automatic and I get realtime updates when data changes. Realtime updates may be useful for me to update the cached data in the server or the game client without having to call an API to retrieve the data manually.

Firebase also comes with a lot of cool features such as user and phone authentication, which will be useful if I ever want to allow players to save their game stats.

## WebSocket Server
We started writing code to outline the WebSocket server. It is written in TypeScript and will be responsible for handling the game state and logic, which will also act as an anticheat. Basically the idea is that we never trust what the client sent to the server. We do this by allowing the client only send the type of action they want to do and the server decides what to send back to all the other players.

Here is a small diagram:

```
                Player's 1 max
                damage is 43
                      |         --> Player 2
  Player 1 --> WebSocket Server --> Player 3
      |                         --> Player 4
action: "attack"
```

In this example, player 1 has no way of changing their attack damage because player 1 can't choose how much damage to deal. That way, no other players are affected, and if anything, it will only affect what player 1 sees. A similar idea will be applied to the rest of the game, such as movement speed, attack speed, knockback, etc...

We also wrote basic classes to create a game, player, and weapons, which will later on be refactored, including folder structuree so that everything makes more sense for where it belongs. We also need to write unit tests and some documentation.

```typescript
// https://github.com/swizzyxyz/websocket-server/blob/master/src/structures/Game.ts
import Player from "@structures/Player";

interface GameSettings {
  maxPlayers: number;
}

class Game {
  public readonly maxPlayers: number;

  private playerList: Player[] = [];

  public constructor(settings: GameSettings) {
    this.maxPlayers = settings.maxPlayers;
  }

  public get players() {
    return this.playerList;
  }

  public addPlayer(player: Player) {
    if (this.players.length >= this.maxPlayers) throw new Error("Game is currently full!");
    else this.playerList.push(player);

    return player;
  }

  public kickPlayer(player: Player) {
    const playerExists = this.players.find(({ uuid }) => uuid === player.uuid);
    if (!playerExists) throw new Error(`Player ${player.uuid} does not exists!`);

    this.playerList.filter(({ uuid }) => uuid === player.uuid);

    return player;
  }
}

export default Game;
```

```typescript
// https://github.com/swizzyxyz/websocket-server/blob/master/src/structures/Player.ts
import { v4 as uuid } from "uuid";

interface PlayerStatistics {
  name: number,
  speed: number
}

class Player {
  public readonly speed: number;

  public readonly name: number;

  public readonly uuid: string = uuid(); // A unique id to distinguish between each player

  private remainingHealth: number = 100;

  // TODO: placeholders, need to figure out how to locate players in 3d space
  private x: number = 0;

  private y: number = 0;

  private z: number = 0;

  public constructor(statistics: PlayerStatistics) {
    this.name = statistics.name;
    this.speed = statistics.speed;
  }

  public get health() {
    return this.remainingHealth;
  }

  public get location() {
    return {
      x: this.x,
      y: this.y,
      z: this.z
    };
  }

  public heal(percentage: number) {
    if (percentage > 1) throw new RangeError("Percentage to heal cannot be greater than 100% of the player's health!");
    if (percentage <= 0) throw new RangeError("Percentage to heal cannot be less than or equal to 0!");

    const replenishAmount = this.remainingHealth * percentage;

    if (this.remainingHealth + replenishAmount > 100) this.remainingHealth = 100;
    else this.remainingHealth += replenishAmount;
  }

  public move(x: number, y: number, z: number) {
    // TODO: Need to add boundary checks against the current map
    this.x = x;
    this.y = y;
    this.z = z;
  }
}

export default Player;
```

```typescript
// https://github.com/swizzyxyz/websocket-server/blob/master/src/structures/Weapon.ts
interface WeaponStatistics {
  damage: number;
  knockback: number;
  name: string;
  range: number;
  rateOfFire: number;
}

class Weapon {
  public readonly damage: number;

  public readonly knockback: number;

  public readonly name: string;

  public readonly range: number;

  public readonly rateOfFire: number;

  public constructor(statistics: WeaponStatistics) {
    const { damage, knockback, name, range, rateOfFire } = statistics;

    this.damage = damage;
    this.knockback = knockback;
    this.name = name;
    this.range = range;
    this.rateOfFire = rateOfFire;
  }
}

export default Weapon;
```

# Skills
By learning how to Google properly by using correct keywords and terms, I ssolved my problems with TypeScript and ESLint configuration.

[Previous](entry02.md) | [Next](entry04.md)

[Home](../README.md)
