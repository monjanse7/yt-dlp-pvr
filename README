# yt-dlp dvr

**Author / maintainer:** monjanse7  
**Current version:** 0.4.30

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
