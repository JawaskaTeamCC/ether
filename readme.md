# Ether net

The _Ether net_ library is a [_Tokio-based_](tokio) analogous to the
builtin **rednet** library of CraftOS from ComputerCraft.

To use this library, add this below your `[dependencies]`
section in your `Janus.toml` file:

```toml
result = { git = "https://github.com/sigmasoldi3r/result-saturnus" }
tokio = { git = "https://github.com/JawaskaTeamCC/tokio-saturnus" }
ether = { git = "https://github.com/JawaskaTeamCC/ether-saturnus" }
```

[tokio]: https://github.com/JawaskaTeamCC/tokio-saturnus

## Can be used from pure Lua?

Yes. Just use the compiled version, drop it in your computer
and use like:

```lua
local Ether = require("ether").Ether;

-- Do your things
Ether.add_protocol("foobar"):unwrap();
Ether.host("supercool"):unwrap();
-- Subscribe your listeners
Ether.on("foobar", function(_, meta)
    print("Hi from " .. meta.sender);
end);
t.ev:run(function()
    Ether.send("*", "foobar", { hey = "You" });
end);
--- Etc
```

To get the compiled version paste:

```sh
pastebin get mtiTCVaD ether.lua
```

> [!WARNING]
> This paste might get outdated! Open an issue if you
> encounter it to be outdated (Current release: 2 jan 2024)

## Example

```rs
Ether.send("*", "super_cool", { command: "hi" });
let saluted = 0;
Ether.on("super_cool", (data, meta) => {
    if data.command == "hi" {
        print(meta.sender ++ " salutes you!");
        saluted += 1;
    }
});
ev->timeout(() => {
    if saluted {
        print(saluted ++ " people said hi!");
    } else {
        print("no one responded, sadly :(");
    }
}, 0.1);
```

---

## Usage

This library exposes an Ether class with static methods to run
the messages across.

Like rednet, it has the concepts of "host" and "protocol".

> [!NOTE]
> To see how `Result<A, B>` works, see [Result library](https://github.com/sigmasoldi3r/result-saturnus).

---

### Add protocol

```php
Ether.add_protocol(name: string, options: ProtocolOptions) -> Result
```

Returns `Result` which is one of:
- `Ok<()>`
- `Err<NoModemError>`
- `Err<ProtocolAlreadyExistsError>`

This function registers a new protocol, returns error if already registered.

> [!TIP]
> Protocols are used as "sub-channels" when using the `Ether.send()`
> function.

Also, hosts will tell others what protocols they support when
requested by `lookup()`.

```ts
interface ProtocolOptions {
    private?: boolean
    channel?: number
    rewrite_receive?: message => any
    rewrite_transmit?: message => any
}
```

### Host

```php
Ether.host(name: string): Result
```

Returns `Result` which is one of:
- `Ok<()>`
- `Err<NoModemError>`
- `Err<ReservedHostNameError>`

Starts the Ether agent, this means that the host will be visible
and ready to respond others with it's own host name, and the
provided list of available protocols.

> [!TIP]
> You can always stop the hosting mechanism by calling the
> `Ether.unhost()` function, this will make your host silent again.

### Unhost

```php
Ether.unhost()
```

Self explanatory.

### Lookup

```php
Ether.lookup(timeout: number)
```

Looks up for living hosts available in the current network.

### Broadcast

```php
Ether.broadcast(body: any)
```

### Send

```php
Ether.send(host: string, protocol: string, body: any)
```

### On

```php
Ether.on(protocol: string, callback: MessageHandler)
```

```ts
type MessageHandler = (body: any, meta: MessageInfo) => void
```

```ts
interface MessageInfo {
    sender: string
    sender_mac: number
    body: any
    protocol?: string
}
```
