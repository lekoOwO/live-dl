# Live Downloader

Download live streams from YouTube.

## Features

- URL guessing: this script will try it best to guess what you pass to it, the following URLs/URIs should all work:
  - https://www.youtube.com/channel/UC1opHUrw8rvnsadT-iGp7Cg/live
  - https://www.youtube.com/channel/UC1opHUrw8rvnsadT-iGp7Cg
  - https://www.youtube.com/watch?v=S3CAGeeMRvo
  - https://www.youtube.com/playlist?list=UU1opHUrw8rvnsadT-iGp7Cg
  - S3CAGeeMRvo
  - UC1opHUrw8rvnsadT-iGp7Cg
- Monitor your favorite YouTube channel and download streams when live starts
- Email/Slack notifications when stream starts or finish downloading
- Writing streamer metadata (author/channel name, description, year) via FFmpeg
- Embed YouTube thumbnail as video cover via ~~AtomicParsley~~ (now handled by FFmpeg)
- Download subtitles if available
- Keywords filter: only download streams that match specific keywords in title
- Convert TS to MP4 after downloading

## Dependencies

- aria2c
- bash
- exiv2
- ffmpeg
- jq
- streamlink
- youtube-dlc
- yq (python-yq)
- atomicparsley

Tested on macOS 10.14 and Alpine Linux, so it should work on most Linux distros.

### Alpine Linux

```
apk add --no-cache aria2 bash ffmpeg python3 py3-pip perl build-base curl jq git nano exiv2-dev coreutils grep
apk add exiv2 --no-cache --repository=http://dl-cdn.alpinelinux.org/alpine/edge/community
apk add --no-cache atomicparsley --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing
pip3 install streamlink youtube-dlc yq
```

## Usage

Run `./live-dl` without any parameter to print help message.

You can run this script in background:

```shell
# Start process
nohup bash live-dl https://www.youtube.com/channel/UC1opHUrw8rvnsadT-iGp7Cg &>/tmp/live-dl-minatoaqua.log &

# View processes
ps aux | grep live-dl
501 94552   964   0  9:38PM ttys009    0:00.06 bash live-dl https://www.youtube.com/channel/UC1opHUrw8rvnsadT-iGp7Cg
501 94765   964   0  9:39PM ttys009    0:00.00 grep live-dl

# Stop process
kill 94552
```

## Caution

Due to the live stream detecting mechanism limit, the record might have a 10\~18 sec delay. Thus, the first 10\~18 secs of the live stream might be miscaptured.

Also, the stream recording is stopped when the stream reached 6 hrs due to YouTube's limit. Though we tried to restart `live-dl` immediately, but a 30sec miss or more can't be avoided.

To handle this situation, we strongly recommend re-download the stream after a few hours it ends manually.

## License

AGPL-3.0
