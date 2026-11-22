---
name: get-started
description: >
  Set up and check the revemaker tools: confirm the revemaker command line client is
  installed, sign in, and show the account. Use when the user is new to the
  revemaker tools, asks how to sign in, or a revemaker tool says the user is signed out.
---

# Get started

Read revemaker-rules first.

1. Check the revemaker command line client is installed: run `revemaker --help`. If the
   command is not found, tell the user to install it from the revemaker service's
   install notes, and stop. Do not install software for them.
2. Run `revemaker account`. If it succeeds, go to step 4.
3. If it says the user is signed out, run `revemaker login`. It opens the browser;
   tell the user to finish signing in there, then run `revemaker account` again.
   Never ask for an API key.
4. Call the `revemaker_account` tool and tell the user their plan, status and
   balance.
5. Tell the user they can now ask to remove the background from a photo.
