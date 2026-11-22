---
name: remove-background
description: >
  Remove the background from a product photo and save a transparent PNG.
  Use when the user asks to cut out a product, delete or replace a
  background, or make a photo transparent for a listing.
---

# Remove background

Read revemaker-rules first.

If you cannot run shell commands, say this host cannot upload local
files, ask the user to try again from a host that can run them, and stop.

1. Call `revemaker_account` and tell the user their plan and balance.
2. Call `revemaker_upload` with the file's name, size and type, then send it:
   `curl -fsS -X PUT --upload-file "<path>" "<upload_url>"`.
   If `upload_headers` are returned, add each as a `-H "<name>: <value>"`
   argument exactly as given. A successful upload may have an empty response.
   Poll `revemaker_asset` every 2 seconds until `ready`, for up to 60 seconds,
   before calling the tool. If still pending, stop and report that validation
   is pending. If the asset is refused or missing, request a new upload.
3. Ask the user for any creative choice the tool lists.
4. Create one new `idempotency_key` and call `revemaker_remove_background` with
   the ready asset, those choices and that key. The call runs at once and
   charges the price in the tool description. Report the job ID and the credits
   it returns; retry only with the same key.
5. Poll `revemaker_job` every 2 seconds, up to 60 seconds. Stop polling at
   `done` or `failed`. On `failed`, tell the user it failed and nothing was
   charged, then stop. If 60 seconds pass without `done`, tell the user the job
   is still running and give them its job ID so they can ask for it later;
   never submit the request again.
6. Only for `done` with a returned URL, download to a temporary file:
   `curl -fsSL "<url>" -o "<name>-cutout.png.part"`.
   Check it is a non-empty PNG, then rename it to `<name>-cutout.png`.
   If the download fails, is empty or is not a PNG, delete only the `.part`
   file and keep any earlier cutout.
   If `done` carries no URL and says the result expired, tell the user the
   output is no longer available and stop; a new run needs their say-so.
   Never read the image bytes into the conversation.
7. On any refusal, follow revemaker-rules: open `action.url` only if present,
   wait for `retry_after`, stop on `contact_support`, otherwise retry the same
   call with the same arguments and key. The same refusal again means the
   action is still pending.
8. Give the user the saved path. Apart from steps 4 and 5, do not mention tool names
   or job IDs.
