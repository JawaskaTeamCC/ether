# Ether net

The _Ether net_ library is a _Tokio-based_ analogous to the
builtin **rednet** library of CraftOS from ComputerCraft.

To use this library, add this below your `[dependencies]`
section in your `Janus.toml` file:

```toml
result = { git = "https://github.com/sigmasoldi3r/result-saturnus" }
tokio = { git = "https://github.com/JawaskaTeamCC/tokio-saturnus" }
ether = { git = "https://github.com/JawaskaTeamCC/ether-saturnus" }
```

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

## Examples


