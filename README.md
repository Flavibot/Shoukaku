## Shoukaku

> A stable and updated wrapper around Lavalink

[![Discord](https://img.shields.io/discord/423116740810244097?style=flat-square)](https://discordapp.com/invite/FVqbtGu)
[![npm](https://img.shields.io/npm/v/shoukaku?style=flat-square)](https://www.npmjs.com/package/shoukaku)
![Github Stars](https://img.shields.io/github/stars/Deivu/Shoukaku?style=flat-square)
![GitHub issues](https://img.shields.io/github/issues-raw/Deivu/Shoukaku?style=flat-square)
![Snyk Vulnerabilities for npm package](https://img.shields.io/snyk/vulnerabilities/npm/shoukaku?style=flat-square)
![NPM](https://img.shields.io/npm/l/shoukaku?style=flat-square)

<p align="center">
    <img src="https://safe.saya.moe/lhvaWz3iP67f.webp"> 
</p>

> The ShipGirl Project, feat Shoukaku; ⓒ Azur Lane

### Features

✅ Stable

✅ Documented

✅ Updated

✅ Extendable

✅ ESM & CommonJS supported

✅ Very cute (Very Important)

### Supported Libraries

Refer to [/src/connectors](https://github.com/Deivu/Shoukaku/tree/master/src/connectors) for list of supported libraries + how to support other libraries

### Installation

> `npm install shoukaku`

### Documentation

https://shoukaku.shipgirl.moe/

### Small code snippet examples

> Initializing the library (Using Connector Discord.JS)

```js
const { Client } = require("discord.js");
const { Shoukaku, Connectors } = require("shoukaku");
const Nodes = [
  {
    name: "Localhost",
    url: "localhost:6969",
    auth: "re_aoharu",
  },
];
const client = new Client();
const shoukaku = new Shoukaku(new Connectors.DiscordJS(client), Nodes);

// Always handle "error" events or your program may crash due to uncaught error
shoukaku.on("error", (_, error) => console.error(error));
client.login("token");

// If you want shoukaku to be available on client, then bind it to it, here is one example of it
client.shoukaku = shoukaku;
```

> Never initialize Shoukaku like this, or else she will never initialize, start shoukaku before you call `client.login()`

```js
client.on("ready", () => {
  client.shoukaku = new Shoukaku(new Connectors.DiscordJS(client), Nodes);
});
```

> Join a voice channel, search for a track, play the track, then disconnect after the track ends

```js
// create a voice connection and player using Shoukaku#joinVoiceChannel
const player = await shoukaku.joinVoiceChannel({
  guildId: "your_guild_id",
  channelId: "your_channel_id",
  shardId: 0, // if unsharded it will always be zero (depending on your library implementation)
});
// player is created, now search for a track
const result = await player.node.rest.resolve("scsearch:snowhalation");
if (!result?.data.length) return;
const metadata = result.tracks.shift();
// play the searched track
await player.playTrack({ track: { encoded: metadata.encoded } });
// wait for track to end
await once(player, 'end');
// leaver the voice channel
await shoukaku.leaveVoiceChannel(player.guildId);
```

> Playing a track and changing a playback option (in this example, volume)

```js
await player.playTrack({ track: { encoded: metadata.encoded } });
await player.setGlobalVolume(50);
```

> Updating the whole player if you don\'t want to use my helper functions

```js
await player.update({ ...playerOptions });
```

> Setting a custom get node ideal function

```js
const shoukaku = new Shoukaku(
  new Connectors.DiscordJS(client),
  [{ ...yourNodeOptions }],
  {
    ...yourShoukakuOptions,
    nodeResolver: (nodes, connection) => getYourIdealNode(nodes, connection),
  }
);
const player = await shoukaku.joinVoiceChannel({
  guildId: "your_guild_id",
  channelId: "your_channel_id",
  shardId: 0,
});
```

> A full bot example (that dont use timeout to leave the voice channel) can be found at https://github.com/shipgirlproject/Shoukaku?tab=readme-ov-file#full-bot-implementation-of-shoukaku-in-discordjs

### Shoukaku's options

| Option                 | Type                   | Default  | Description                                                                                      | Notes                    |
| ---------------------- | ---------------------- | -------- | ------------------------------------------------------------------------------------------------ | ------------------------ |
| resume                 | boolean                | false    | If you want to enable resuming when your connection when your connection to lavalink disconnects |                          |
| resumeTimeout          | number                 | 30       | Timeout before lavalink destroys the players on a disconnect                                     | In seconds               |
| resumeByLibrary        | boolean                | false    | If you want to force resume players no matter what even if it's not resumable by lavalink        |                          |
| reconnectTries         | number                 | 3        | Number of tries to reconnect to lavalink before disconnecting                                    |                          |
| reconnectInterval      | number                 | 5        | Timeout between reconnects                                                                       | In seconds               |
| restTimeout            | number                 | 60       | Maximum amount of time to wait for rest lavalink api requests                                    | In seconds               |
| moveOnDisconnect       | boolean                | false    | Whether to move players to a different lavalink node when a node disconnects                     |                          |
| userAgent              | string                 | (auto)   | Changes the user-agent used for lavalink requests                                                | Not recommeded to change |
| structures             | Object{rest?, player?} | {}       | Custom structures for shoukaku to use                                                            |                          |
| voiceConnectionTimeout | number                 | 15       | Maximum amount of time to wait for a join voice channel command                                  | In seconds               |
| nodeResolver           | function               | function | Custom node resolver if you want to have your own method of getting the ideal node               |                          |

### 3rd Party Plugins

| Name     | Link                                          | Description                                              |
| -------- | --------------------------------------------- | -------------------------------------------------------- |
| Kazagumo | [Github](https://github.com/Takiyo0/Kazagumo) | A wrapper for Shoukaku that has an internal queue system |

> Open a PR if you want to add your plugin here

### Other Links

> [Support](https://discord.gg/FVqbtGu) (#Development) | [Lavalink](https://github.com/freyacodes/Lavalink)

### Full bot implementation of Shoukaku in Discord.JS

> [Kongou](https://github.com/Deivu/Kongou)

### Made with ❤ by

> @ichimakase (Saya)
