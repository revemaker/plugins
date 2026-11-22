# revemaker plugin for Claude Code

Make short videos from product photos, transcribe recordings into text or subtitles and remove
backgrounds from product photos in Claude Code. The plugin adds the revemaker MCP server and
the skills below. You sign in in your browser, and no API key is ever needed.

## Skills

- `get-started`: install the command line client, sign in and check the account.
- `generate-video`: make a short video from a product photo, a description or both.
- `transcribe-video`: transcribe a video and download text or subtitles.
- `remove-background`: cut the product out of a photo as a transparent PNG.
- `revemaker-rules`: the shared rules the other skills follow.

## Install

```sh
claude plugin marketplace add revemaker/revemaker-tools
claude plugin install revemaker@revemaker-tools
```

If the plugin's server is missing from `claude mcp list`, look for a manual
Revemaker entry with the same URL and OAuth client. Claude Code can hide the
plugin's server when both match. Check the manual entry's scope with
`claude mcp get <name>` and remove the entry from that scope. For example, for a
user-scope entry named `revemaker-hosted`, run
`claude mcp remove --scope user revemaker-hosted`.

The plugin's skills use the `revemaker` command for sign-in. Install it with
`pipx install revemaker`, then run `revemaker login`.

## Update

```sh
claude plugin marketplace update revemaker-tools
claude plugin update revemaker@revemaker-tools
```

To use another server, set `MEDIA_MCP_URL` (ending in `/mcp`) before starting Claude Code.

This repository is generated from the service's private repository; open issues here.

Privacy: <https://revemaker.com/privacy>. License: MIT, see [LICENSE](LICENSE).
