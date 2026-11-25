---
name: get-started
description: >
  Set up and check the revemaker tools: confirm the revemaker command line client is
  installed, sign in, and show the account. Use when the user is new to the
  revemaker tools, asks how to sign in, or a revemaker tool says the user is signed out.
---

# Get started

Read revemaker-rules first.

1. Unless the user asked to set up the command line client, call the `revemaker_account`
   tool if it is available. If it returns the account, the agent is already signed in:
   tell the user their plan, status and balance and go to step 6.
2. Check the revemaker command line client is installed: run `revemaker --help`. If the
   command is not found, tell the user to install it from the revemaker service's
   install notes, and stop. Do not install software for them.
3. Run `revemaker account`. If it succeeds, go to step 5.
4. If it says the user is signed out, run `revemaker login`. It opens the browser;
   tell the user to finish signing in there, then run `revemaker account` again.
   Never ask for an API key.
5. Call the `revemaker_account` tool and tell the user their plan, status and
   balance.
6. Tell the user they can now ask to remove the background from a photo.
