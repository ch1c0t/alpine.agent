# alpine.agent

A Crystal application that boots an Alpine Linux guest in QEMU, configures its
console, and exposes a small interactive REPL.

## Requirements

- Crystal 1.22 or newer
- QEMU and the `xephyr.run` / `xephyr.kill` executables provided by
  [xephyr.context](https://github.com/ch1c0t/xephyr.context)
- Tesseract OCR with English language data
- A running LavinMQ instance configured for
  [global-amqp_channel](https://github.com/ch1c0t/crystal.global-amqp_channel)
- tmux

The QEMU implementation is provided by
[ch1c0t/crystal.qemu](https://github.com/ch1c0t/crystal.qemu).

## Build

```sh
bundle install
shards install
bundle exec rake
```

## Run

```sh
./bin/alpine.agent boot
```

The current default ISO lookup checks
`~/Downloads/ISOs/alpine-extended-3.24.2-x86_64.iso`. When it is absent, the
agent falls back to regular, non-empty files under `./disks/` and QEMU attaches
those disk files. The default ISO and disk selection are intentionally simple
at this stage; explicit ISO and disk selection will be added separately.

After boot, the agent waits for the Alpine login prompt, logs in as root, and
requests installation of `font-terminus` before starting the `alpine` tmux
REPL. The REPL currently provides:

- `ocr` — recognize text in the latest captured guest display frame.

Set `ALPINE_AGENT_REPL_FOCUS=false` to leave the current tmux client in the
boot pane. Switch to the REPL with `tmux switch-client -t alpine`.

Set `XEPHYR_LOG_LEVEL=DEBUG` or `TRACE` for more diagnostics.
