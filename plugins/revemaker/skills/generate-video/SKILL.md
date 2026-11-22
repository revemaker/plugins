---
name: generate-video
description: >
  Make a short MP4 video from a product image, a text description, or both.
  Use when the user asks to animate a photo, make a video or clip from an
  image or a description, or generate a product video.
---

# Generate a video

Read revemaker-rules first.

The video can start from an image, a description, or both. If the user gives
no image, skip step 2 and call the tool with the `prompt` alone. If the tool
answers that the chosen model needs an image or a description, tell the user
which, ask for it, and call again.

If the user gives an image and you cannot run shell commands, say this host
cannot upload local files, ask the user to try again from a host that can run
them, and stop.

1. Call `revemaker_account` and tell the user their plan and balance.
2. Only with an image: call `revemaker_upload` with its name, size and type, then send it:
   `curl -fsS -X PUT --upload-file "<path>" "<upload_url>"`.
   If `upload_headers` are returned, add each as a `-H "<name>: <value>"`
   argument exactly as given. A successful upload may have an empty response.
   Poll `revemaker_asset` every 2 seconds until `ready`, for up to 60 seconds,
   before calling the tool. If still pending, stop and report that validation
   is pending. If the asset is refused or missing, request a new upload.
3. Ask the user what motion or scene the video should show; that is the
   `prompt`, optional when there is an image. Offer only the `model` choices the
   tool lists, as they are named there, and let the user pick one. Call
   `revemaker_image_to_video` with the ready asset as `image` (if any), the
   `prompt` (if any) and the `model`.
4. Create one new `idempotency_key` and call with those arguments and that key.
   The call runs at once and charges the price in the tool description. Report
   the job ID and the credits it returns. If the answer is lost, repeat that
   same call with the same key; never create a second key for the same request.
5. Poll `revemaker_job` every 15 seconds, for up to 10 minutes, and at most 40
   times. Stop polling at `done`, `failed` or `uncertain`. On `failed`, tell
   the user it failed and nothing was charged, then stop. On `uncertain`, give
   the user the job ID and tell them to contact support. If 10 minutes pass
   without `done`, tell the user the job is still running and give them its job
   ID so they can ask for it later; never submit the request again.
6. Only for `done` with a returned URL, create a temporary file of this run's
   own in the output folder, then download into it:
   `part=$(mktemp "<folder>/.<name>-video.part.XXXXXX") && curl -fsSL "<url>" -o "$part"`.
   If `mktemp` fails, report the file creation error and the output folder's
   path, then stop.
   Check it is a non-empty MP4. Then pick an output name that is not taken:
   `<name>-video.mp4`, or `<name>-video-2.mp4`, `-3` and so on. Never replace
   an existing file: rename with `mv -n "$part" "<name>-video.mp4"`. If
   `$part` is still there afterwards, the name was taken meanwhile: pick the
   next name and rename again.
   If the download fails, is empty or is not an MP4, delete only `$part`,
   keep any earlier video, and call `revemaker_job` once more for a fresh URL;
   never generate again.
   If `done` carries no URL and says the result expired, tell the user the
   output is no longer available and stop; a new run needs their say-so.
   Never read the video bytes into the conversation.
7. On any refusal, follow revemaker-rules: open `action.url` only if present,
   wait for `retry_after`, stop on `contact_support`, otherwise retry the same
   call with the same arguments and key. The same refusal again means the
   action is still pending.
8. Give the user the saved path. Apart from steps 4 and 5, do not mention tool names
   or job IDs.
