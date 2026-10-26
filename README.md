# myu

myu is the command line client for askMyu. It is one self-contained binary. It does not need
Node, Deno or any other runtime.

## Requirements

macOS or Linux, and an askMyu account. There is no Windows build. On Windows, use WSL and
follow the Linux instructions.

## Installing

```sh
curl -fsSL https://myu.askmyu.com/cli/install.sh | sh
```

The script detects your operating system and CPU, downloads the matching binary from the
latest release, checks its SHA-256 against the `SHA256SUMS` file published with that release,
and installs it to `~/.myu/bin/myu` with permissions `700`. It does not use sudo and writes
nothing outside your home directory.

If your shell cannot find `myu` afterwards, add the install directory to your `PATH`:

```sh
export PATH="$HOME/.myu/bin:$PATH"
```

Add that line to `~/.zshrc` or `~/.bashrc` to keep it.

To update, run the install line again. It replaces the binary with the latest release.

## Signing in

Two commands.

```sh
myu login
```

This prints a URL and a short code. Open the URL in a browser where you are already signed in
to askMyu, check that the code on the page matches the one in your terminal, and approve. The
terminal finishes on its own and writes a session file.

```sh
myu unlock
```

askMyu encrypts your content with a key that only your devices hold. The terminal needs a copy
of that key before it can read your journal or your conversations. `unlock` asks one of your
devices that already holds the key to hand it over. It prints a four-digit code. Approve it in
the app under Settings → Security & Privacy by typing that code, or from another terminal that
is already unlocked with `myu devices approve <code>`. The key arrives wrapped for this
terminal only and is saved for this machine, so `unlock` is needed once per machine.

If you have set a password for new devices (`myu password set`), `unlock` asks for that
password instead. With no other device at hand, `myu unlock --phrase` asks for your 12-word
recovery phrase. The phrase is used once to unwrap the key and is not saved.

`myu logout` deletes the local session and the stored key.

## Commands

`myu help` lists every command. `myu <command> --help` explains one command with examples.

Lists are numbered. A command that takes a number reads the list it printed last; a list older
than an hour is printed again first. A command that changes something asks before it does;
`--yes` skips the question. Commands that read take `--json`.

| Command | What it does |
|---|---|
| `myu login [you@work.com] [--api <url>]` | Sign in. `--email` sends a code to type instead of the browser approval. |
| `myu login --agent "claude code" [--with-people]` | A separate sign-in for a coding agent. `myu mcp` uses it. See below. |
| `myu unlock [--phrase]` | Get your key onto this machine. |
| `myu status` / `myu accounts` / `myu use <n>` | Who is signed in and which server. Switch between accounts. |
| `myu today` | What needs your attention today, numbered, under the morning brief. |
| `myu open <n\|name>` | Open a conversation. Answer with a number or type your own words. |
| `myu conversations` | List your conversations. `--waiting`, `--people`, `--meetings`, `--spaces`, `--journal` filter. |
| `myu journal` | Write a journal entry and read Myu's reply. `list`, `open <n>`, `export <n>`, `import <file>`, `audio <file>`. |
| `myu commits` / `myu commit` | Your commitments. Take on a task from a retro. |
| `myu log` | What Myu read, assumed, said and did. `--today`, `--week`, `--kind=said`, `--all`. `log verify` checks nothing in it was changed. |
| `myu change` / `myu count` | Correct something Myu got wrong about a person. Mark a code review as counted. |
| `myu briefing` / `myu canvases` / `myu week` | Today's briefing from Myu. The canvases it made. This week's review. |
| `myu people` / `myu companies` | The people and companies Myu knows, numbered. Add words to search. |
| `myu person <n\|name>` / `myu company <n\|name>` | A person's or a company's card. `person` also takes `set`, `memories`, `forget`, `correct`, `linkedin`, `unlink`, `archive`, `delete`, `answer`, `reply`, `earlier`. |
| `myu me` | Your own card. |
| `myu merge "<a>" into "<b>"` | Merge two people who are the same person. |
| `myu teams [<team>]` | Your teams. One team shows a grid for a window (`--window 1w\|2w\|1m\|1q\|1y`). Subcommands: `new`, `add`, `remove`, `on-it`, `not-on-it`, `rename`, `delete`, `order`, `groups`. |
| `myu calendar [--week \| <day>]` | Today's and tomorrow's meetings. `--week` for the week. A day shows its events and the tickets and pull requests due, opened or done that day. |
| `myu meeting <n\|title>` | A meeting's card. `accept`, `decline`, `dismiss`, `mute`, `who`, `notes`. |
| `myu plan "<people>"` / `myu plans` | Plan a meeting and get the Google Calendar link. List your plans. |
| `myu spaces` | Your spaces, by kind of work. |
| `myu space "<name>"` | One space. `new`, `lead`, `remove`, `move`, `settings`, `tracker`. `--log` and `--story` for its history. |
| `myu retros` / `myu retro …` | Your retros. Run one: `new`, `prep`, `picture`, `import`, `share`, `wrap`, `hand-out`, `start`, `rhythm`, `ready`, `approve`, `object`, `merge`, `agenda`, `log`. `--how` prints how a retro works. |
| `myu prep` / `myu push` | Get ready for a retro. Preview what would be shared with a space, then send it. |
| `myu debriefs` / `myu debrief …` | Your interview debriefs. One candidate's: `new`, `score`, `time`, `opens`, `reply`, `decide`, `reveal`, `scorecard`, `pronouns`, `role`, `delete`. |
| `myu invite …` / `myu invites` | Invite someone to askMyu, or to a space. List the invitations you sent. |
| `myu invitations` / `myu join` / `myu decline` | Invitations waiting for your answer. Accept or decline one. |
| `myu mute` / `myu unmute` / `myu muted` | Stop notifications from a space, or a meeting series. List what is muted. |
| `myu leave` / `myu archive` / `myu delete` | Leave a space. Make it read-only. Delete it and everything in it (the lead only). |
| `myu agreements` / `myu agree` | A space's agreements. Propose, accept or retire one. |
| `myu files` | A space's files. `add`, `note`, `get <n>`, `archive <n>`. |
| `myu patterns` | Patterns Myu found in your journal. `forget` deletes them. |
| `myu settings [set <name> <value>]` | Your settings. `set` changes one: name, pronouns, timezone, the morning brief, quiet hours and others. |
| `myu notifications` | What the spaces notify you about, by email and push. |
| `myu subscription` / `myu team` | Your plan and usage. The people on your team plan. |
| `myu emails` | The email addresses on your account. `add`, `resend`, `remove`, `primary`. |
| `myu emails allowance [<address>] [--exclude <folder> …\|--clear]` | How many emails Myu may send about your spaces today and this week, and the email folders it skips. `--exclude` saves folders to skip. |
| `myu integrations` / `myu connect <service>` / `myu disconnect` | What is connected. Connect Google, Microsoft, Slack, GitHub, Jira, Linear, Zulip, IMAP, CalDAV or an iCal link. |
| `myu profile` | What Myu knows about your work. `resume <file>`, `linkedin <address>`. |
| `myu mail import <file>` | Give Myu mail to read: a Google Takeout .zip, or .mbox, .eml or .msg files. |
| `myu password set\|change\|remove` | A password for signing in on new devices. |
| `myu phrase new` | A new recovery phrase. The old one stops working. |
| `myu devices` | Devices that hold your key. `approve <code>`, `deny <n>`, `remove <n>`, `rename <n>`. |
| `myu approve <code>` | Let another terminal sign in as you, by the code it shows. |
| `myu export` / `myu account delete` | Request a copy of all your data. Delete your account. |
| `myu feedback "<message>"` | Send feedback to the askMyu team. |
| `myu terms` | Show and accept the terms of service. |
| `myu completion zsh\|bash` | Print the line that enables tab completion. |
| `myu mcp [--check]` | Run the MCP server for a coding agent. See below. |
| `myu logout [--agent]` | Sign out. `--agent` signs out only the coding agent's sign-in. |
| `myu --version` | Print the version. |

The terminal does not draw canvases. Where Myu offers one, it prints the link.

## Using it with a coding agent

The agent gets its own sign-in, separate from yours. Make it, approve it in a browser where you
are signed in, then register the server with Claude Code:

```sh
myu login --agent "claude code"
claude mcp add myu -- ~/.myu/bin/myu mcp
```

Add `--with-people` to the first command if the agent may also read your people's cards, your
teams and your calendar.

`myu mcp --check` prints which sign-in the server runs as, whether it has your key, and the
list of tools. `myu logout --agent` removes the agent sign-in.

What the agent can do through the server:

| Tool | What it does |
|---|---|
| `myu_today` | Read what needs your attention: today's lines, or the people, spaces or calendar lines. |
| `myu_answer` | Answer one of Myu's lines by choice: a choice id, yes/no, "not now" or "don't ask again". It cannot type an answer. |
| `myu_commitments_mine` | Read your commitments. |
| `myu_retros` / `myu_retro` / `myu_thread` | Read where your retros stand, one retro, or one question in it. |
| `myu_review_suggest` | Suggest who should review a change. |
| `myu_did` | Record what the agent did. It shows in your log as "via your claude code session". |
| `myu_log_mine` | Read what the agent did through Myu. |
| `myu_calendar_day`, `myu_person`, `myu_teams`, `myu_team` | A day of your work calendar, a person's card, your teams. Only with `--with-people`. |

The agent sign-in cannot read or write your journal, read your conversations or your mail, or
post in a space for you. The server refuses those calls.

## Using a different server

By default the CLI talks to `https://myu.askmyu.com`. To sign in to a different server, pass
`--api` to `myu login`:

```sh
myu login --api http://localhost/api
```

The address is stored with the session and used by every command afterwards. `myu status`
prints the address in use. To override it for one command, set `MYU_API_URL`.

To install from a different server, pass `--base` to the install script. It reads that
server's copy of `latest.json`:

```sh
curl -fsSL http://localhost/cli/install.sh | sh -s -- --base http://localhost
```

The binaries are the same for every server. Only the address differs.

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
- `~/.config/myu/current.json` says which account is active.
- `~/.config/myu/accounts/<account id>/session.json` holds that account's session token and server address.
- `~/.config/myu/accounts/<account id>/device.json` holds that account's key for this machine.
- `~/.config/myu/agent-session.json` holds the coding agent's sign-in, when you made one.
- A few small files next to these keep the last numbered list, settings and the saved time zone.

If `XDG_CONFIG_HOME` is set, the config directory is `$XDG_CONFIG_HOME/myu`. Setting
`MYU_CONFIG_DIR` overrides the location entirely, which is useful for testing.

To remove everything:

```sh
rm -rf ~/.myu ~/.config/myu
```

## Troubleshooting

- `command not found: myu`: `~/.myu/bin` is not on your `PATH`. See Installing.
- `not signed in`: there is no session. Run `myu login`.
- `locked`: the session exists but this machine has no key. Run `myu unlock`.
- Commands reach the wrong server: run `myu status` to see which one is in use, then
  `myu login --api <url>` to change it.
- `myu mcp` prints "there is none on this machine": make the agent sign-in with
  `myu login --agent "claude code"`.

## Licence

Apache-2.0. This repository holds the released binaries and this README. The source is in
askMyu's main repository.
