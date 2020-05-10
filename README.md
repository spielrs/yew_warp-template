# WASM WARP TEMPLATE

Wasm Warp Template is a minimal template to run rustwasm projects. The templete use [yew](https://yew.rs)[https://github.com/seanmonstar/warp] as frontend with wasm and [warp](https://github.com/seanmonstar/warp) as server and watcher.

**Note**: You can replace yew framework with another framework based in rustwasm or just vanilla rustwasm

## How it works

1.Replace the js file with the name of the project that you write in Cargo.toml in `app/index.html`

```html
<script type="module">
    import init, { run } from './pkg/your_project_name.js'; //by default is wasm_warp_template.js

    const start = async() => {
    await init();
    run();
    };

    start();
</script>
```

If you need to include css file, it has to be inside of the `pkg`

```html
<head>
    <link href="pkg/app.css" rel="stylesheet">
</head>
```

for assets has to be included in `assets`

2.Run the template:

- without logs: `cargo run`
- with logs: `RUST_LOG=name_of_your_project_from_Cargo.toml=trace cargo run` 

**Note**: By the default the name is wasm_warp_template

Ready to start developing 🚀

## Customize running project

You can set the server address and the path where are the wasm sources in your .env file:

```
SERVER_ADRESS=[SERVER_ADDRESS]:[PORT] // by default is 127.0.0.1:8080
WASM_PATH=[PATH WHERE ARE YOUR WASM SOURCES] // by default is `app`
```

**Note**: remember not include `/` in the end of the path

## Remove optimization

You can remove wasm optimization, if you consider that it is slow, adding the next in the `Cargo.toml`
of wasm source (by defaut `app/Cargo.toml`):

```
[package.metadata.wasm-pack.profile.release]
wasm-opt = false
```

## About WebSocket

To allow reload the browser when there are changes in the sources the server use WebSocket which is
running in the port 3000. If you need change it you can do changing in these two files:

1. app/index.html, line 24:

```javascript
const reloadSocket = new WebSocket("ws://127.0.0.1:3000");
```

2. src/main.rs, line 47

```rust
let websocket_srv = Server::bind(&([127, 0, 0, 1], 3000).into()).serve(make_websocket_svc);
```

## Limitations

* For now the server only watch the folder `src` but soon will be available to watch all the wasm source
less target folder (no necessary to watch it), for it requires to add feature to skip folder in the diff
in the library [spielrs_diff](https://github.com/spielrs/spielrs-diff)

## Roadmap

- [ ] Fix watcher limitation
- [ ] Customize watcher
- [ ] Open a browser after start running the server
- [ ] Avoid duplicate message in sockect connection
- [ ] Implement spielrs which will be based in [parceljs](https://en.parceljs.org/) but focus in rustwasm projects