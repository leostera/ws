# ws [![Travis-CI](https://api.travis-ci.org/ostera/ws.svg)](https://travis-ci.org/ostera/ws)
> 🔄 An erlang WebSocket server

## Installation

Simply include in your `rebar.config` as:

```erlang
{deps, [
  % ...
  {ws, {git, "https://github.com/ostera/ws", {tag, "0.1.0"}}}
  % ...
]}.
```

## Tentative Interface

```erlang
Eshell V7.3  (abort with ^G)
1> {ok, Server} = ws:listen(Options).
ok
2> ws:on(Server, '_', fun(Data) -> io:format("~p", [Data]) end ).
ok

%% on another shell
Eshell V7.3  (abort with ^G)
1> {ok, Client, Server} = ws:open(URL).
ok
2> ws:send(Server, "Hey there").
ok
3> ws:on(Client, '_', fun(Data) -> io:format("~p", [Data]) end).
ok

%% back to shell 1
3>
Hey there
3> ws:connections().
[ <0.103.0> ].

%% on yet another shell
Eshell V7.3  (abort with ^G)
1> {ok, Client} = ws:open(URL).
ok
2> ws:on(Client, '_', fun(Data) -> io:format("~p", [Data]) end).
ok

%% back to shell 1
3> ws:connections().
[ <0.103.0>, <0.108.0> ].
4> [ ws:send(C, "Hello backatcha") || C <- ws:connections() ].
[ ok, ok ].

%% shell 2
4>
Hello backatcha

%% shell 3
3>
Hello backatcha
3> ws:close(Client).
ok

%% back to shell 1
3> ws:connections().
[ <0.103.0> ].
```

## Motivation

As of this writing, there's a plethora of web servers (and more) out there: Webmachine, Mochiweb, Cowboy, N2O, Nitrogen,
ChicagoBoss, Zootonic, Elli.

None of them provide a fully thought of websockets server as a commodity, but instead you have to build around things like:

1. Connection tracking
2. Broadcasting/Fanout
3. Channels/Rooms

And if you have to build it, you have to test it, and you have to spend time on it.

I don't want to do that over and over again. Hopefully neither do you.

## Contributing

Fork, make a topic branch, and send a Pull Request. Travis will let you know if
it's good to go, and from the on we can review, retouch, and merge.

Included here is a `Makefile` with handy targets. Run `make` to execute the complete
battery of tests.

## Next Steps

See the [issues page](https://github.com/ostera/ws/issues?q=is%3Aopen+is%3Aissue+label%3Aenhancement) for a list of planned enhancements and features.

## License

See [LICENSE](https://github.com/ostera/ws/blob/master/LICENSE).
