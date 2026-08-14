# yt-dlp dvr

**yt-dlp dvr** is a Windows-friendly YouTube livestream recorder built around yt-dlp.

It can automatically monitor YouTube channels, start recording when a livestream begins, reconnect if the stream connection is interrupted, record live chat, and generate YouTube-style chat replay videos.

**Author / Maintainer:** monjanse7

This project is based on the original `yt-dvr` project by MCJack123 and contains substantial additional functionality, including reconnect support, chat recording, PO Token integration, enhanced chat rendering, Windows launchers, and post-processing tools.

---

# Features

- Automatic YouTube channel livestream monitoring
- Automatic recording when a channel goes live
- Automatic reconnect if the download connection is interrupted
- Keeps reconnect segments in the same recording session
- Configurable offline grace period
- yt-dlp based downloading
- FFmpeg remuxing to MP4
- YouTube account cookie support
- PO Token support using bgutil
- Deno JavaScript runtime support
- Live chat recording
- Chat saved as:
  - `.txt`
  - `.chat.jsonl`
- Optional subtitle-style chat track
- Optional burned-in chat
- Enhanced YouTube-style Live Chat Replay rendering
- Profile pictures, badges and special chat message styling
- Chat panel can be rendered beside the original video
- Configurable CPU usage for chat rendering
- Manual recovery tool for missing chat overlays
- Local web interface
- Windows batch launchers
- PyInstaller EXE build support

---

# System Requirements

yt-dlp dvr is primarily designed for **Windows 10 and Windows 11**.

Recommended:

- Windows 10 or Windows 11
- Python 3.12
- FFmpeg
- Internet connection
- Modern web browser
- YouTube cookies for streams that require authentication
- Deno 2.x for PO Token generation

The included setup scripts can prepare many of the required components automatically.

---

# Installation

## 1. Download yt-dlp dvr

Download the latest release ZIP from GitHub.

For example:

```text
yt-dlp-dvr-v0.4.30.zip



C:\Users\YourName\Videos\yt-dlp-dvr


START_YT_DLP_DVR.bat

Starting yt-dlp dvr
Running on http://127.0.0.1:6334

http://127.0.0.1:6334

you may also use http://localhost:6334


Important

Do not run multiple copies of yt-dlp dvr using the same database and recording directory at the same time.

Running two instances against the same recording session may cause:

duplicate downloads
database conflicts
FFmpeg conflicts
duplicate chat recorders
file locking problems

# Web Interface

yt-dlp dvr includes a local web interface.

The two most important sections are:

Channels
Settings
Settings

Open:

http://localhost:6334/settings
Save directory

This controls where recordings are stored.

Example:

C:/Users/YourName/Videos/yt-dlp-dvr/files

Forward slashes are recommended in JSON configuration paths:

C:/Users/YourName/Videos/recordings

instead of:

C:\Users\YourName\Videos\recordings
Server Port

Default:

6334

The normal web interface will therefore be:

http://localhost:6334
Log Level

Typical values include:

INFO
DEBUG

Use INFO for normal operation.

Use DEBUG when troubleshooting.

Expected states such as a channel simply not being live are shown as informational messages rather than serious errors.

Example:

INFO: YouTube channel @example is not currently live
Poll Interval

This determines how often yt-dlp dvr checks monitored channels.

Example:

120

means:

check every 120 seconds

A value around 60-180 seconds is normally sufficient.

Very aggressive polling is not recommended because YouTube may temporarily challenge repeated requests.

Adding a YouTube Channel

Open the Channels section.

Add the channel URL.

Recommended format:

https://www.youtube.com/@CHANNELNAME/live

Example:

https://www.youtube.com/@examplechannel/live

yt-dlp dvr will periodically check this URL.

When a Channel Is Offline

If the channel is not currently streaming, you may see:

INFO: YouTube channel @examplechannel is not currently live

This is normal.

It is not an application error.

yt-dlp dvr continues monitoring the channel and checks again after the configured polling interval.

Upcoming Livestreams

If YouTube reports:

This live event will begin in a few moments

yt-dlp dvr treats this as an expected upcoming-stream state.

It continues checking until the actual media stream becomes available.

Automatic Recording

When a monitored channel becomes live, yt-dlp dvr:

Detects the active YouTube video ID.
Creates a recording session.
Starts yt-dlp.
Sends the media stream to FFmpeg when required.
Starts live chat recording if enabled.
Monitors the downloader.
Attempts reconnect if the connection disappears.
Finalizes the recording when the stream is genuinely over.
Optionally remuxes the recording.
Optionally creates chat post-processing outputs.
Reconnect System

Temporary YouTube/HLS/network interruptions do not immediately end the recording.

If the downloader stops while the same YouTube video ID is still live, yt-dlp dvr waits briefly and reconnects.

Example log:

Downloader stopped but stream ABC123 is still live
Reconnect attempt 1

The system keeps using the same logical recording session.

The goal is to prevent a brief internet interruption from creating completely unrelated recordings.

Offline Grace Period

A stream is not considered permanently finished immediately after one failed check.

yt-dlp dvr uses an offline grace period.

For example:

180 seconds

This helps distinguish between:

a temporary YouTube/CDN interruption
a real end of livestream
FFmpeg

FFmpeg is used for operations such as:

stream processing
remuxing
MP4 generation
chat overlay rendering
subtitle/chat track processing

If FFmpeg is installed separately, enter its path in Settings.

Example:

C:/ffmpeg/bin/ffmpeg.exe
MP4 Remuxing

Enable:

Remux recordings

and select:

mp4

if you want the final recording stored as an MP4 whenever possible.

Remuxing normally does not require completely re-encoding the original video, so it is much faster than generating a burned-in chat overlay.

YouTube Cookies

Some YouTube videos require authentication.

This may include:

age-restricted videos
account-restricted videos
bot challenges
certain livestreams

yt-dlp dvr supports a Netscape-format cookie file.

Example:

youtube.com_cookies.txt

A typical yt-dlp parameter configuration may contain:

{
  "cookiefile": "C:/Users/YourName/Videos/yt-dlp-dvr/youtube.com_cookies.txt"
}
Security Warning

Your cookie file may contain active YouTube login credentials.

Never upload your cookie file to GitHub.

Do not share it in:

GitHub Issues
Discord
screenshots
log files
ZIP releases

The supplied .gitignore should be configured to exclude sensitive local files.

PO Token Support

Modern YouTube extraction can sometimes require a PO Token.

yt-dlp dvr includes support for the bgutil PO Token provider.

The included setup can use:

bgutil-ytdlp-pot-provider

with:

Deno

as the JavaScript runtime.

A healthy setup may report:

PO STATUS: ACTIVE

and something similar to:

bgutil script-deno registered
Deno >= 2
PO Token Configuration

A configuration may contain custom yt-dlp dvr options such as:

{
  "_ytdvr_po_token": true,
  "_ytdvr_po_client": "mweb",
  "_ytdvr_bot_backoff_seconds": 300
}

These _ytdvr_* options are internal yt-dlp dvr controls and are not passed directly to yt-dlp as unknown options.

Bot Challenge Backoff

If YouTube starts returning repeated bot/authentication challenges, yt-dlp dvr can temporarily reduce request frequency.

For example:

300 seconds

prevents the application from hammering the endpoint repeatedly.

Live Chat Recording

Enable:

Get chat

for a channel.

When the livestream begins, yt-dlp dvr starts a chat recorder.

Two chat files are normally created.

Plain text chat
recording.txt

This is intended for easy human reading.

Structured chat
recording.chat.jsonl

JSONL means:

JSON Lines

Each line contains structured information for an individual chat event.

The JSONL file is used by the enhanced chat renderer.

Empty Chat Files

A chat file may initially appear as:

0 KB

This does not necessarily mean the chat recorder is broken.

If nobody has sent a chat message yet, there may simply be nothing to write.

Chat Post-Processing

yt-dlp dvr can create additional videos after the livestream finishes.

Available options may include:

switchable subtitle/chat track
burned-in chat
enhanced YouTube-style Live Chat Replay
Enhanced YouTube-Style Chat Replay

The enhanced renderer recreates a YouTube-inspired live chat display.

Depending on available chat metadata, it can display:

usernames
profile pictures
normal messages
owner badges
moderator badges
member badges
highlighted messages
Super Chat-style messages
dark/light appearance
Recommended Side-Panel Layout

Recent versions of yt-dlp dvr can place the chat panel beside the video instead of over the video.

Example:

┌──────────────────────────────┬──────────────┐
│                              │              │
│                              │  LIVE CHAT   │
│       ORIGINAL VIDEO         │              │
│                              │  User: Hello │
│                              │  User: Hi!   │
│                              │              │
└──────────────────────────────┴──────────────┘

For example:

Original video:
1920x1080


Chat panel:
520x1080


Final video:
2440x1080

The original 1920x1080 video remains fully visible.

Important: Chat Overlay Requires Re-Encoding

Unlike a normal MP4 remux, a burned-in or side-panel chat video normally requires FFmpeg to re-encode the video.

This means:

CPU usage can be higher
the PC may become warmer
processing may take a long time
the resulting file can be large

This is normal for video encoding.

CPU Usage Modes

yt-dlp dvr includes CPU limiting options for enhanced chat rendering.

Typical modes:

Eco
1 CPU thread

Lowest CPU usage.

Advantages:

coolest
leaves most CPU available for other programs

Disadvantages:

slowest rendering
Cool
2 CPU threads

Recommended default.

Advantages:

noticeably lower CPU load
reasonable rendering speed
better for laptops and smaller PCs
Balanced
up to 4 CPU threads

Faster, but generates more CPU load and heat.

Full CPU

Uses much more available CPU capacity.

Advantages:

fastest rendering

Disadvantages:

highest CPU usage
highest heat output

Use Eco or Cool if your computer becomes too hot during chat rendering.

Chat Rendering Progress

Enhanced rendering displays progress.

Example:

Preparing enhanced chat frames: 12/73 | 16.4%

After preparation:

Enhanced chat frames ready; starting FFmpeg encoder

During video encoding:

00:12:34 / 01:26:37 | 14.5% | speed 0.82x | output 180 MiB

This contains:

current video position
total duration
percentage
encoding speed
current output size
Temporary Overlay Files

While encoding, yt-dlp dvr uses a temporary file.

Example:

video.chat-overlay.tmp.mp4

Only after FFmpeg finishes successfully is it converted/renamed to:

video.chat-overlay.mp4

Do not upload or use the .tmp.mp4 file unless you specifically want to inspect an interrupted render.

Recovering Missing Chat Overlays

If the livestream was recorded successfully but the enhanced chat overlay was not created, use:

GENERATE_MISSING_CHAT_OVERLAYS.bat

This tool searches for completed recordings and their corresponding:

.chat.jsonl

files.

It can scan both:

the yt-dlp dvr database
the recording filesystem

This allows old recordings to be repaired even if their original database entry is unavailable.

Example Recovery

Suppose you already have:

my-stream.mp4
my-stream.chat.jsonl

but do not have:

my-stream.chat-overlay.mp4

Run:

GENERATE_MISSING_CHAT_OVERLAYS.bat

yt-dlp dvr searches for the matching pair and generates:

my-stream.chat-overlay.mp4

The original files remain unchanged.
