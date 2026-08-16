# yt-dlp dvr

**Author / maintainer:** monjanse7  
**Current version:** 0.4.35

### Chat timing reliability (v0.4.35)

yt-dlp dvr now uses YouTube's per-message Unix timestamp as the authoritative chat clock. This prevents delayed pytchat polling batches from making individual messages appear many seconds late. Existing `.chat.jsonl` files that contain `timestamp_ms` can also be repaired during overlay regeneration.

`yt-dlp dvr` is a Windows-oriented DVR frontend for monitoring YouTube live
channels, recording livestreams with yt-dlp/FFmpeg, reconnecting interrupted
streams into the same recording session, saving live chat, and optionally
creating a YouTube-style chat replay video.

## Main features

- Automatic channel live monitoring.
- Same-stream reconnect with a grace period and same-file recording workflow.
- yt-dlp + FFmpeg recording and MP4 remuxing.
- Automatic bgutil PO Token setup with portable Deno support.
- TXT + JSONL live-chat archive.
- Optional switchable chat subtitle track.
- Optional enhanced chat replay rendered beside the video instead of covering it.
- Real YouTube/custom emoji images in enhanced chat (v0.4.31+ recordings).
- Eco / Cool / Balanced / Full CPU profiles for chat-overlay rendering.
- Tool for regenerating missing chat overlays from existing recordings.

## Windows quick start

1. Extract the ZIP.
2. Run `START_YT_DLP_DVR.bat`.
3. Open the local web interface shown in the console.
4. Add your YouTube channel URLs and configure recording/chat options.

To build the standalone executable, first run the normal launcher once, close
it, then run `BUILD_EXE.bat`. The executable is created as:

```text
dist\yt-dlp-dvr.exe
```

## Project files

- `START_YT_DLP_DVR.bat` — normal Windows launcher.
- `run_yt_dlp_dvr.py` — Python launcher.
- `reconnect_patch.py` — reconnect, PO-token, chat and post-processing logic.
- `GENERATE_MISSING_CHAT_OVERLAYS.bat` — rebuild missing chat outputs.
- `BUILD_EXE.bat` — PyInstaller build helper.
- `CHANGELOG.txt` — detailed development/version history.
- `NOTICE.txt` — licensing and upstream attribution.
- `LICENSE` — GNU Affero General Public License v3.
- `.gitignore` — excludes recordings, local config/database, environments and build output.

## Compatibility note

Some internal names such as the Python module `yt_dvr`, `YTDVR_*` environment
variables, `ytdvr_config.json`, and `ytdvr.db` are intentionally retained for
compatibility with the upstream yt-dvr 0.3.0 package. They are implementation
interfaces, not the public project name.

## License and upstream credit

This fork is distributed under **AGPL-3.0-or-later**. `yt-dlp dvr` is based on
and installs the upstream **yt-dvr 0.3.0** project. Original upstream authorship
and copyright remain with its contributors. See `NOTICE.txt` for upstream and
third-party attribution.

The author/maintainer name for the `yt-dlp dvr` fork is **monjanse7**.

## Stop a recording before the livestream ends

Starting with **v0.4.32**, each active recording card has a **Stop recording & finalize** button. Use it when you want to keep only the part recorded so far even though the source livestream is still running.

The action:

1. Stops only the selected recording.
2. Prevents the monitor from immediately restarting the same livestream on the next poll.
3. Flushes the captured TXT/JSONL chat at the same point as the video.
4. Finalizes/remuxes the partial recording to the configured final container (normally MP4).
5. Queues the enhanced chat side-panel in the background when chat was captured.
6. Keeps yt-dlp dvr running so other channels continue to be monitored.

When the channel later points to a **different livestream/video ID**, normal automatic recording is enabled again.

Do not use the global server stop or close the command window if you want the background chat-overlay render to finish.


## Chat/video timing sync (v0.4.35)

yt-dlp dvr uses YouTube/pytchat's per-message Unix timestamp as the authoritative chat clock instead of the moment a polling callback happens to deliver the message. This fixes the case where only some messages arrive in a delayed batch and would otherwise appear 10-20+ seconds late in the rendered overlay.

For new recordings, yt-dlp dvr also measures the first real media bytes written to the recording. A `*.chat-sync.json` sidecar preserves that video-zero calibration. When both pieces are available, each message is mapped directly from its YouTube server timestamp to the video timeline.

Existing `.chat.jsonl` files from older versions usually already contain `timestamp_ms`. v0.4.35 can use those timestamps to repair individual outlier offsets during regeneration. If you already have a bad overlay, run `REGENERATE_CHAT_OVERLAYS.bat` in v0.4.35.

**Chat timing fine adjustment (seconds)** remains available for a global final adjustment. Negative values move all chat earlier; positive values move all chat later. Use it only if the whole chat track is consistently shifted, not to compensate for individual delayed messages.
