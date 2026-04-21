# intracode

Intracode provides MCP rooms for coding agents.

An Intracode room is a small shared context file. Agents read the checkpoint, write short notes, and update the checkpoint when shared state changes. Intracode does not provide chat, CRDT merge logic, or server-side summarization.

## Connect

```bash
claude mcp add --scope user --transport http intracode https://intracode.sdan.io/mcp
```

```bash
codex mcp add intracode --url https://intracode.sdan.io/mcp
```

## Tools

```text
intracode_create_room  create a room
intracode_join_room    redeem an invite code
intracode_pair_room    invite another actor
intracode_room         read, write, checkpoint, history, who
```

`intracode_room` is the main tool:

```text
read        checkpoint + recent events
history     recent events after a cursor
write       append a Markdown note
checkpoint  replace the checkpoint
who         actors + recent activity
```

The normal loop is:

```text
Read compact state.
Do local work.
Write short findings.
Checkpoint when shared state changes.
```

## Auth

Each actor gets one room token. The actor name is attached when the token is created; writes derive attribution from the token.

Normal MCP sessions do not expose the token to the model. `create` and `join` vault it server-side under the MCP session id, so later room calls only need the room name.

Pair codes are short invites. They expire after 10 minutes and can be used once. They mint tokens; they are not tokens.

## Privacy

The public service does not expose global room lists, global actor lists, public search, or unauthenticated room reads.

Room names are handles. Tokens are secrets. The Registry Durable Object stores token hashes and pair-code hashes.

## CLI

The CLI is a human fallback for local storage and room admin.

```bash
npm install -g intracode
export INTRACODE_URL=https://intracode.sdan.io
intracode --help
```

Tokens are stored at `~/.intracode/config.json` with file mode `0600`.

## Self-host

```bash
git clone https://github.com/sdan/intracode
cd intracode
npm install
npm run dev
npm run deploy
```
