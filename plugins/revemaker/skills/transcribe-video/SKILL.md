---
name: transcribe-video
description: Transcribe an uploaded video or public video URL and download text or subtitles with optional speaker labels and timestamps.
---

# Transcribe a video

Read revemaker-rules first. Discover the live tools and schemas at run time.

1. Call `revemaker_account` and show the user's plan and available balance.
2. For a local file, call `revemaker_upload` with its filename, exact size and MIME type.
   Use `curl -fsS -X PUT --upload-file "<path>" "<upload_url>"`, passing all returned
   upload_headers unchanged. If this host cannot run shell commands, explain that
   local upload requires a shell-capable host. Never read file contents into the conversation.
   Poll `revemaker_asset` until ready. For a public direct video URL, use
   `revemaker_import_video` instead. For a public YouTube URL it returns the video's existing
   captions as a transcript asset (source: captions, no speaker labels), which `revemaker_transcribe`
   takes like any asset. If the import is refused (no captions, private, live, too long or
   unavailable), ask the user to upload the file. Do not supply cookies or bypass private-source restrictions.
3. Use `auto` for YouTube captions, which keep their original language. For audio or video files,
   ask for the user's language choice. Prefer automatic detection for mixed-language recordings.
4. Create one new idempotency key and call `revemaker_transcribe` with the ready asset,
   the language and that key. The call runs at the rate in the tool description.
   Report the job ID, credits and any perk usage returned by the tool.
   Reuse that key if the answer is lost; never create a second paid request to
   recover an existing job.
5. Poll `revemaker_job` every 2 seconds, up to 60 seconds. If still running, return the
   job ID so the user can resume later. If uncertain, preserve the job ID and follow
   support guidance; do not resubmit. Explain failures as returned by the service.
6. Once done, call `revemaker_transcript_export` for TXT, SRT or VTT. TXT can omit
   timestamps; subtitles always retain cue times. Speaker labels are optional,
   anonymous identifiers. Original, Simplified and Traditional Chinese writing
   are export choices; script conversion is not translation or improved recognition.
7. Download the returned signed URL into a new local file via curl. Keep the data out
   of the conversation, verify download success, and report the path. Expired links
   are refreshed by asking for another export, without another transcription call.

Format, label or script changes reuse the existing job and do not transcribe again.
