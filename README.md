# HTTPBridgeMod

Adds asynchronous HTTP request/response support to Unreal Blueprint logic mods

<img src="https://i.imgur.com/Ke5Tpnc.png" width="256" height="256"/>

## Features

- Supports `GET`, `POST`, `PUT`, `DELETE` and `PATCH` methods
- Supports custom ip, path, port, headers and body
- Timeout support
- Requests are made from a `HTTPBridgeActor` BP that you spawn into the game
- Responses are broadcasted from an event dispatcher in `HTTPBridgeActor`

## Installing

### Automatic

- Install [r2modman Mod Manager](https://thunderstore.io/package/ebkr/r2modman/)
- Visit [this mod's webpage on Thunderstore](https://thunderstore.io/c/palworld/p/PWR/HTTPBridge/)
- Click `Install with Mod Manager`

### Manual

#### Client:

- Click `Manual Download`
- Navigate to the root folder of the app (where `Palworld.exe` is located)... You can find this by right-clicking `Palworld` in Steam and navigating to `Manage > Browse Local Files`
- Extract the contents of the `mod` folder into `<PalworldRoot>\Pal\Binaries\Win64\Mods\HTTPBridge` (you will have to make a new folder named `HTTPBridge` in `Mods`)
- Extract the contents of the `pak` folder into `<PalworldRoot>\Pal\Content\Paks\LogicMods`

#### Server:

- Click `Manual Download`
- Navigate to the root folder of the app (where `PalServer.exe` is located)... You can find this by right-clicking `Palworld Dedicated Server` in Steam and navigating to `Manage > Browse Local Files`
- Extract the contents of the `mod` folder into `<PalworldRoot>\Pal\Binaries\Win64\Mods\HTTPBridge` (you will have to make a new folder named `HTTPBridge` in `Mods`)
- Extract the contents of the `pak` folder into `<PalworldRoot>\Pal\Content\Paks\LogicMods`

## Devs

- Copy the contents of the `Sources\Mods` folder to your `Content\Mods` folder
- HTTPBridge uses asset chunk ID `99`. Please choose a different chunk ID for your other mods
- Anywhere within your own mod, create a new BP variable of type `HTTPBridgeActor`
- Use a `Spawn Actor from Class` node with `HTTPBridgeActor` as it's class, assigning your new variable to it
- Bind an event delegate to `HTTPRequest` using the `Create Event` node, and either `Create Matching Function` or `Create Matching Event`. This creates an `HTTPResponse` callback function for HTTPBridge to execute after an HTTP request
- Make a new `HTTPRequest` node, passing in your `HTTPRequest` variable and a valid `FGuid` (you can use the `New Guid` node)
- Try to limit your mod to only having one `HTTPActor` instance at a time, and only one request at a time until you recieve a response. Multiple simultaneous instances/requests have not yet been tested, so your mileage may vary if you try too many at once

### Networking

- If you wish to replicate from server to clients, you will need to open up `Content\Mods\HTTPBridge\HTTPBridgeActor`, click `Class Defaults` and set your desired replication variables, then compile and save.
- Note that you might also require localcc's [ReplicationEnabler](https://thunderstore.io/c/palworld/p/localcc/ReplicationEnabler/) mod for proper replication.
- Best networking practice would be allow clients to make `Run on Server` RPC calls to the server, which in turn makes authoritative local (`127.0.0.1` aka `localhost`) HTTP requests to your own backend application, and sends the relevant response data back through a `Run on Client` RPC call, however you may also make remote calls using any base domain (as the IP) and path, and HTTPBridge will automatically resolve it into an IP for you

### Reporting Bugs

- Create a new issue on the [GitHub page](https://github.com/Spectre-Cular/HTTPBridgeMod), or DM me on [Discord](https://discord.com/users/163849518868201473)
