# myu

myu is a command line client for askMyu. It is a single self-contained binary, so you do not
need Node, Deno, or any other runtime to run it.

## Requirements

macOS or Linux, and an askMyu account. There is no Windows build. If you are on Windows, use
WSL and follow the Linux instructions.

## Installing

```sh
curl -fsSL https://myu.askmyu.com/cli/install.sh | sh
```

The script works out your operating system and CPU architecture, downloads the matching
binary from the latest release, checks it against the `SHA256SUMS` file published with that
release, and installs it to `~/.myu/bin/myu` with permissions `700`. It does not use sudo and
does not write anything outside your home directory.

If your shell cannot find `myu` afterwards, add the install directory to your `PATH`:

```sh
export PATH="$HOME/.myu/bin:$PATH"
```

Add that line to `~/.zshrc` or `~/.bashrc` if you want it to persist.

## Signing in

Signing in takes two commands.

```sh
myu login
```

This prints a URL and a short code. Open the URL in a browser where you are already signed in
to askMyu, check that the code on the page matches the one in your terminal, and approve it.
The terminal finishes on its own and writes a session file.

```sh
myu unlock
```

This asks for your 12 word recovery phrase. askMyu encrypts your content with a key that only
your devices hold, so the terminal needs a copy of that key before it can read your journal or
your conversations. The phrase is used once to unwrap the key and is not saved anywhere. The
key itself is saved for this machine, so you only need to run `unlock` once per machine.

To sign out again, run `myu logout`. That deletes the local session and the stored key.

## Commands

- `myu today` shows what needs you today.
- `myu open <name|number>` opens a conversation. Running `myu open` with no argument lists them.
- `myu journal` writes a journal entry. Add `--edit` to compose it in `$EDITOR`.
- `myu commits` lists what you owe other people and what they owe you.
- `myu commit` takes a to-do, or hands it to your coding agent.
- `myu retro` shows the current retro. The subcommands are `ready`, `rank`, `approve`,
  `object`, `merge`, `start`, `agenda --now` and `rhythm`.
- `myu push` shows what would leave your Myu for a shared space, and asks before sending it.
- `myu log` shows what Myu read, assumed, said and did, one line each. `--today` and
  `--kind=said` narrow it.
- `myu status` shows who you are signed in as, which server you are on, and whether your key
  is unlocked.
- `myu help` lists everything. `myu <command> --help` explains one command.

## Using it with a coding agent

```sh
claude mcp add myu -- ~/.myu/bin/myu mcp
```

This registers myu as an MCP server for Claude Code. An agent session has narrower permissions
than your own session. It can read today's lines, read your own commitments, suggest a
reviewer, and record what it did. It cannot read your journal and cannot post anything to a
shared space.

## Using a different server

By default the CLI talks to `https://myu.askmyu.com`. To sign in to a different one, pass
`--api` to `myu login`:

```sh
myu login --api http://localhost/api
```

The address is stored with the session and used by every command afterwards, so you only pass
it once. `myu status` prints the address in use and where it came from. To override it for a
single command, set `MYU_API_URL`.

Installing from a different server works the same way. Pass `--base` to the install script and
it reads that server's copy of `latest.json`:

```sh
curl -fsSL http://localhost/cli/install.sh | sh -s -- --base http://localhost
```

The binaries are identical in every environment. Only the server address differs.

## Releases

Every release publishes four binaries and a `SHA256SUMS` file:

- `myu-<version>-aarch64-apple-darwin` for Macs with Apple silicon
- `myu-<version>-x86_64-apple-darwin` for Intel Macs
- `myu-<version>-x86_64-unknown-linux-gnu` for Linux on x86-64
- `myu-<version>-aarch64-unknown-linux-gnu` for Linux on arm64

The install script verifies the checksum and installs nothing if it does not match. To check a
download yourself:

```sh
shasum -a 256 myu-<version>-<target>
grep myu-<version>-<target> SHA256SUMS
```

## Files it writes

- `~/.myu/bin/myu` is the binary.
- `~/.config/myu/session.json` holds your session token and the server address.
- `~/.config/myu/device.json` holds the account key for this machine.
- `~/.config/myu/config.json` holds settings such as your editor choice.

If `XDG_CONFIG_HOME` is set, those three files live under that directory instead. Setting
`MYU_CONFIG_DIR` overrides the location entirely, which is useful for testing.

To remove everything:

```sh
rm -rf ~/.myu ~/.config/myu
```

## Troubleshooting

- `command not found: myu` means `~/.myu/bin` is not on your `PATH`. See Installing above.
- `not signed in` means there is no session. Run `myu login`.
- `locked` means the session exists but this machine has no key. Run `myu unlock`.
- If commands reach the wrong server, run `myu status` to see which one is in use, then
  `myu login --api <url>` to change it.

## Licence

Apache-2.0. This repository holds the released binaries and this README. The source lives in
askMyu's main repository.
