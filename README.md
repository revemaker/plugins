# revemaker plugin for Claude Code and Codex

Make short videos from product photos, transcribe recordings into text or subtitles and remove
backgrounds from product photos in Claude Code or Codex. The plugin adds the revemaker MCP server
and the skills below. You sign in in your browser, and no API key is ever needed.

## Skills

- `get-started`: install the command line client, sign in and check the account.
- `generate-video`: make a short video from a product photo, a description or both.
- `transcribe-video`: transcribe a video and download text or subtitles.
- `remove-background`: cut the product out of a photo as a transparent PNG.
- `revemaker-rules`: the shared rules the other skills follow.

## Install in Claude Code

```sh
claude plugin marketplace add revemaker/plugins
claude plugin install revemaker@revemaker-plugins
```

If the plugin's server is missing from `claude mcp list`, look for a manual
Revemaker entry with the same URL and OAuth client. Claude Code can hide the
plugin's server when both match. Check the manual entry's scope with
`claude mcp get <name>` and remove the entry from that scope. For example, for a
user-scope entry named `revemaker-hosted`, run
`claude mcp remove --scope user revemaker-hosted`.

The plugin's skills use the `revemaker` command for sign-in. Install it with
`pipx install revemaker`, then run `revemaker login`.

To update:

```sh
claude plugin marketplace update revemaker-plugins
claude plugin update revemaker@revemaker-plugins
```

To use another server, set `MEDIA_MCP_URL` (ending in `/mcp`) before starting Claude Code.

## Install in Codex

First check for a Revemaker server you added to Codex by hand: run `codex mcp get revemaker`.
If it prints an entry, Codex would keep using that address under the same name and sign in
there instead of the plugin's server. Remove or rename the entry in the file that defines it
(`~/.codex/config.toml`, or a project's `.codex/config.toml`) until the command finds nothing.

```sh
codex plugin marketplace add revemaker/plugins
codex plugin add revemaker@revemaker-plugins
codex mcp login revemaker
```

The last command opens your browser to sign in. Start a new Codex session afterwards.

To update, refresh the marketplace; Codex then refreshes the installed plugin:

```sh
codex plugin marketplace upgrade revemaker-plugins
```

## Other agents

This repository also carries Cursor manifests (`.cursor-plugin/`) and an OpenClaw entry
(`plugins/revemaker/.mcp.json`). Sign-in from those agents is not verified yet, so no install
commands are given for them here.

This repository is generated from the service's private repository; open issues here.

Privacy: <https://revemaker.com/privacy>. License: MIT, see [LICENSE](LICENSE).
