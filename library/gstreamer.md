# Gstreamer Cheat Sheet

## **1. Installation**  
### **Linux (Ubuntu/Debian)**  
```bash
sudo apt install gstreamer1.0-tools gstreamer1.0-plugins-base \
gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```
### **macOS (via Homebrew)**  
```bash
brew install gstreamer gst-plugins-base gst-plugins-good gst-plugins-bad gst-plugins-ugly
```
### **Windows**  
Download from [GStreamer official site](https://gstreamer.freedesktop.org/download/)  

---

## **2. Basic Pipeline Commands**  
### **Play a Video File**  
```bash
gst-launch-1.0 filesrc location=video.mp4 ! decodebin ! autovideosink
```
### **Play an Audio File**  
```bash
gst-launch-1.0 filesrc location=sound.mp3 ! decodebin ! autoaudiosink
```
### **Capture from Camera**  
```bash
gst-launch-1.0 v4l2src ! videoconvert ! autovideosink
```
### **Stream from Microphone**  
```bash
gst-launch-1.0 alsasrc ! audioconvert ! autoaudiosink
```

---

## **3. Pipeline Components**  
### **Sources** (Data Input)  
- `filesrc` – Reads from a file  
- `v4l2src` – Captures from a webcam  
- `alsasrc` – Captures from a microphone  

### **Decoders** (Converts media format)  
- `decodebin` – Auto-decoder  
- `avdec_h264` – H.264 decoder  
- `avdec_mp3` – MP3 decoder  

### **Encoders** (Compress media)  
- `x264enc` – H.264 encoder  
- `avenc_mp3` – MP3 encoder  

### **Muxers** (Combines audio & video)  
- `mp4mux` – MP4 container  
- `matroskamux` – MKV container  

### **Sinks** (Output media)  
- `autovideosink` – Auto video output  
- `autoaudiosink` – Auto audio output  
- `filesink` – Writes to a file  

---

## **4. Streaming Over Network**  
### **Stream Video Over UDP**  
Sender:  
```bash
gst-launch-1.0 v4l2src ! videoconvert ! x264enc tune=zerolatency ! rtph264pay ! udpsink host=192.168.1.100 port=5000
```
Receiver:  
```bash
gst-launch-1.0 udpsrc port=5000 ! application/x-rtp,encoding-name=H264 ! rtph264depay ! decodebin ! autovideosink
```

### **RTMP Streaming to a Server**  
```bash
gst-launch-1.0 v4l2src ! videoconvert ! x264enc bitrate=500 ! flvmux ! rtmpsink location="rtmp://your-server/live/streamkey"
```

---

## **5. Recording Audio & Video**  
### **Record Video from Webcam**  
```bash
gst-launch-1.0 v4l2src ! videoconvert ! x264enc ! mp4mux ! filesink location=video.mp4
```
### **Record Audio from Microphone**  
```bash
gst-launch-1.0 alsasrc ! audioconvert ! avenc_mp3 ! filesink location=audio.mp3
```

---

## **6. Debugging GStreamer Pipelines**  
### **Check Installed Plugins**  
```bash
gst-inspect-1.0 | grep x264
```
### **Check Specific Plugin Details**  
```bash
gst-inspect-1.0 decodebin
```
### **Test Pipeline Without Running**  
```bash
gst-validate-launch-1.0 filesrc location=video.mp4 ! decodebin ! autovideosink
```

---

## **7. GStreamer in Python**  
```python
import gi
gi.require_version('Gst', '1.0')
from gi.repository import Gst

Gst.init(None)

pipeline = Gst.parse_launch("videotestsrc ! autovideosink")
pipeline.set_state(Gst.State.PLAYING)
```


## **8.Pipeline examples**

| #  | Pipeline              | Description                                          | Tags                  | Content                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| -- | --------------------- | ---------------------------------------------------- | --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1  | aac_encode            | extract and play mp4's audio                         | audio                 | gst-launch-1.0 filesrc location=filename.mp4 ! qtdemux name=demux demux.audio_0 ! queue ! avdec_aac ! audioconvert ! autoaudiosink                                                                                                                                                                                                                                                                                                                                |
| 2  | audio2rtmp            | push audio wavscope video to rtmp server             | video, audio, rtmp    | gst-launch-1.0 alsasrc device=hw:2,0 ! audioconvert ! wavescope ! queue ! videoconvert ! x264enc ! flvmux streamable=true ! queue ! rtmpsink location="rtmp://localhost:1935/live/audio_test"                                                                                                                                                                                                                                                                     |
| 3  | camera_to_mp4         | capture camera's video to mp4                        | video, jetson, nvidia | gst-launch-1.0 v4l2src device=/dev/video0 num-buffers=100 ! capsfilter caps="video/x-raw,width=1920, height=1080, framerate=60/1" ! nvvideoconvert ! videorate max-rate=30 ! nvv4l2h264enc ! h264parse ! mp4mux ! filesink location=test.mp4                                                                                                                                                                                                                      |
| 4  | camera_to_rtmp        | capture camera's video and push via rtmp             | video, jetson, nvidia | gst-launch-1.0 v4l2src device=/dev/video0 ! capsfilter caps="video/x-raw,width=1920, height=1080, framerate=60/1" ! nvvideoconvert ! videorate max-rate=30 ! nvv4l2h264enc ! h264parse ! flvmux streamable=true ! rtmpsink location=rtmp://localhost:1935/live/waltertest                                                                                                                                                                                         |
| 5  | h264_decode           | decode mp4 with h264 and play it                     | video                 | gst-launch-1.0 filesrc location=filename.mp4 ! qtdemux name=demux demux.video_0 ! queue ! h264parse ! openh264dec ! autovideosink                                                                                                                                                                                                                                                                                                                                 |
| 6  | h265_decode           | decode mp4 with h265 and play it                     | video                 | gst-launch-1.0 filesrc location=filename.mp4 ! qtdemux name=demux demux.video_0 ! queue ! h265parse ! avdec_h265 ! autovideosink                                                                                                                                                                                                                                                                                                                                  |
| 7  | hls2mp4               | convert hls to mp4                                   | video                 | gst-launch-1.0 filesrc location=material/snapshot_20240227151256.m3u8 ! hlsdemux ! filesink location=snapshot_20240227151256.mp4                                                                                                                                                                                                                                                                                                                                  |
| 8  | hlsplay               | playback hls file                                    | video                 | gst-launch-1.0 filesrc location="/tmp/hls_playlist.m3u8" ! hlsdemux ! decodebin ! videoconvert ! fpsdisplaysink text-overlay=true                                                                                                                                                                                                                                                                                                                                 |
| 9  | mac_audio_record      | capture audio and record as hls                      | audio, macos          | gst-launch-1.0 osxaudiosrc device=141 ! avenc_aac ! hlssink2 max-files=5                                                                                                                                                                                                                                                                                                                                                                                          |
| 10 | mac_capture_screen    | capture video and display it on macos                | video, macos          | gst-launch-1.0 avfvideosrc capture-screen=true ! videoscale ! videoconvert ! capsfilter caps="video/x-raw,width=640,height=480" ! osxvideosink                                                                                                                                                                                                                                                                                                                    |
| 11 | mac_display           | capture video and display it with clock              | video, macos          | gst-launch-1.0 avfvideosrc device-index=1 ! capsfilter caps="video/x-raw, width=1920, height=1080, framerate=30/1" ! clockoverlay ! autovideosink                                                                                                                                                                                                                                                                                                                 |
| 12 | mac_hls_record        | capture video and record it as hls                   | video, macos          | gst-launch-1.0 avfvideosrc device-index=1 ! capsfilter caps="video/x-raw, width=1920, height=1080, framerate=30/1" ! videoconvert ! capsfilter caps="video/x-raw(memory:NVMM),format=I420,width=1920,height=1080,framerate=30/1" ! tee name=t t. ! queue ! openh264enc ! h264parse ! hlssink2 max-files=10 location=/tmp/hls_record_%03d.ts playlist-location=/tmp/hls_playlist.m3u8 target-duration=10 t. ! queue ! autovideosink                                |
| 13 | mac_spacescope        | display audio's space scope                          | audio, macos          | gst-launch-1.0 audiotestsrc wave=9 ! audioconvert ! spacescope ! glimagesink                                                                                                                                                                                                                                                                                                                                                                                      |
| 14 | mac_wavescope         | display audio's wave scope                           | audio, macos          | gst-launch-1.0 autoaudiosrc ! audioconvert ! wavescope ! glimagesink                                                                                                                                                                                                                                                                                                                                                                                              |
| 15 | mic_test              | play audio from mic                                  | audio, test           | gst-launch-1.0 autoaudiosrc ! audioconvert ! wavescope ! videoconvert ! autovideosink                                                                                                                                                                                                                                                                                                                                                                             |
| 16 | mp3_decode            | play mp3 audio                                       | audio                 | gst-launch-1.0 filesrc location=filename.mp3 ! mpegaudioparse ! avdec_mp3 ! audioconvert ! alsasink                                                                                                                                                                                                                                                                                                                                                               |
| 17 | mp4_play              | playback mp4 video file                              | video, mp4            | gst-launch-1.0 filesrc location=./material/talk.mp4 ! decodebin name=dec ! videoconvert ! timeoverlay ! autovideosink dec. ! audioconvert ! audioresample ! autoaudiosink                                                                                                                                                                                                                                                                                         |
| 18 | pipeline_mp4_rtmp     | push mp4 via rtmp                                    | video                 | gst-launch-1.0 filesrc location=material/talk.mp4 ! decodebin ! videoconvert ! identity drop-allocation=1 ! x264enc speed-preset=5 tune=4 byte-stream=true threads=1 key-int-max=15 intra-refresh=true ! flvmux streamable=true ! rtmpsink location="rtmp://localhost:1935/live/waltertest"                                                                                                                                                                       |
| 19 | pipeline_rtp_receiver | receive camera's video via rtp                       | video                 | gst-launch-1.0 udpsrc port=5000 caps=application/x-rtp ! rtph264depay ! avdec_h264 ! autovideosink                                                                                                                                                                                                                                                                                                                                                                |
| 20 | pipeline_rtp_sender   | send camera's video via rtp                          | video, linux          | gst-launch-1.0 v4l2src device=/dev/video1 ! decodebin ! videoconvert ! omxh264enc video/x-h264,stream-format=byte-stream ! rtph264pay ! udpsink host=localhost port=5000                                                                                                                                                                                                                                                                                          |
| 21 | pipeline_test         | playback test video                                  | video                 | gst-launch-1.0 videotestsrc num-buffers=100 ! capsfilter caps="video/x-raw, width=1920, height=1080, framerate=30/1" ! autovideosink                                                                                                                                                                                                                                                                                                                              |
| 22 | pipeline_test_rtmp    | push test video via rtmp                             | video                 | gst-launch-1.0 videotestsrc ! clockoverlay ! queue ! videoconvert ! x264enc speed-preset=5 tune=4 ! flvmux streamable=true ! rtmpsink location=rtmp://localhost:1935/live/waltertest                                                                                                                                                                                                                                                                              |
| 23 | play_pcm              | play pcm audio file                                  | audio                 | gst-launch-1.0 filesrc location=material/16k16bit.pcm ! capsfilter caps="audio/x-raw,format=S16LE,channels=1,rate=16000,layout=interleaved" ! audioconvert ! audioresample ! autoaudiosink                                                                                                                                                                                                                                                                        |
| 24 | recod2flv             | record test video to flv                             | video                 | gst-launch-1.0 videotestsrc num-buffers=60 ! x264enc ! flvmux ! filesink location=abc.flv                                                                                                                                                                                                                                                                                                                                                                         |
| 25 | record2hls            | record test video to hls                             | video                 | gst-launch-1.0 videotestsrc is-live=true num-buffers=100 ! identity ! timeoverlay ! x264enc ! h264parse ! hlssink2 max-files=10 location=/tmp/hls_record_%03d.ts playlist-location=/tmp/hls_playlist.m3u8 target-duration=10                                                                                                                                                                                                                                      |
| 26 | record2mp4            | record test video to mp4                             | video                 | gst-launch-1.0 videotestsrc num-buffers=60 ! timeoverlay ! x264enc ! qtmux ! filesink location=abc.mp4                                                                                                                                                                                                                                                                                                                                                            |
| 27 | record_mp4_display    | record test video to mp4                             | video                 | gst-launch-1.0 videotestsrc is-live=true num-buffers=100 ! timeoverlay ! tee name=t t. ! queue leaky=1 ! x264enc ! mp4mux ! filesink location=xyz.mp4 t. ! queue leaky=1 ! autovideosink sync=false                                                                                                                                                                                                                                                               |
| 28 | record_wav            | record audio of mic of mac                           | audio                 | gst-launch-1.0 osxaudiosrc ! audioconvert ! audioresample ! capsfilter caps="audio/x-raw,format=S16LE,channels=1,rate=16000,layout=interleaved" ! tee ! queue ! splitmuxsink location=test_%5d.wav muxer=wavenc ! tee ! queue ! wavescope ! videoconvert ! autovideosink                                                                                                                                                                                          |
| 29 | rtmp_test             | push video to rtmp server                            | video, rtmp           | gst-launch-1.0 filesrc location=material/talk.mp4 ! decodebin ! videoconvert ! identity drop-allocation=1 ! openh264enc bitrate=4000000 ! videoconvert ! flvmux streamable=true ! rtmpsink location='rtmp://192.168.104.199:1935/live/talktest'                                                                                                                                                                                                                   |
| 30 | speaker_test          | play test audio                                      | audio                 | gst-launch-1.0 audiotestsrc wave=5 volume=0.3 num-buffers=50 ! audioconvert ! autoaudiosink                                                                                                                                                                                                                                                                                                                                                                       |
| 31 | tsplay                | playback mpeg ts file                                | video                 | gst-launch-1.0 filesrc location="material/record_20240229135840_00012.ts" ! tsdemux ! decodebin ! videoconvert ! capsfilter caps='video/x-raw(memory:NVMM),format=NV12,width=1920,height=1080' ! fpsdisplaysink text-overlay=true                                                                                                                                                                                                                                 |
| 32 | video_display         | capture video, change video rate and display         | video, jetson, nvidia | gst-launch-1.0 v4l2src device=/dev/video0 ! capsfilter caps="video/x-raw,width=1920,height=1080, framerate=60/1" ! nvvideoconvert ! capsfilter caps="video/x-raw(memory:NVMM),format=NV12,width=1920,height=1080, framerate=60/1" ! queue ! videorate max-rate=30 ! nvvideoconvert ! capsfilter caps="video/x-raw(memory:NVMM),width=1920,height=1080, framerate=30/1" ! nvoverlaysink sync=0                                                                     |
| 33 | video_jpeg            | capture video as image/jpeg format, then display it  | video, jetson, nvidia | gst-launch-1.0 v4l2src device=/dev/video1 ! capsfilter caps="image/jpeg,width=1920,height=1080" ! nvv4l2decoder mjpeg=1 ! nvvideoconvert ! xvimagesink sync=0                                                                                                                                                                                                                                                                                                     |
| 34 | video_rate_test       | capture video, change video rate and push via rtmp   | video, jetson, nvidia | gst-launch-1.0 v4l2src device=/dev/video0 ! capsfilter caps="video/x-raw,width=1920, height=1080, framerate=60/1" ! nvvideoconvert ! capsfilter caps="video/x-raw(memory:NVMM),width=1920,height=1080, framerate=60/1" ! videorate max-rate=30 ! nvv4l2h264enc maxperf-enable=1 control-rate=1 capture-io-mode=2 output-io-mode=5 insert-sps-pps=1 bitrate=2000000 ! h264parse ! flvmux streamable=true ! rtmpsink location=rtmp://localhost:1935/live/waltertest |
| 35 | video_record          | record video from camera                             | video, jetson, nvidia | gst-launch-1.0 v4l2src device=/dev/video1 ! capsfilter caps="image/jpeg,width=1920,height=1080,framerate=30/1" ! nvv4l2decoder mjpeg=1 ! nvvideoconvert ! capsfilter caps="video/x-raw(memory:NVMM),format=NV12,width=1920,height=1080,framerate=30/1" ! tee name=t t. ! queue ! nvv4l2h264enc ! h264parse ! hlssink2 max-files=10 location=/tmp/hls_record_%03d.ts playlist-location=/tmp/hls_playlist.m3u8 target-duration=10 t. ! queue ! autovideosink        |
| 36 | video_test            | display test video                                   | video, test           | gst-launch-1.0 videotestsrc num-buffers=100 ! capsfilter caps="video/x-raw, width=1280, height=720, framerate=30/1" ! timeoverlay ! identity ! autovideosink                                                                                                                                                                                                                                                                                                      |
| 37 | video_xraw            | capture video as video/x-raw format, then display it | video, jetson, nvidia | gst-launch-1.0 v4l2src device=/dev/video0 ! capsfilter caps="video/x-raw,width=1920,height=1080" ! nvvideoconvert ! xvimagesink sync=0                                                                                                                                                                                                                                                                                                                            |
| 38 | videojpg2rtmp         | push camera video to rtmp server                     | video, rtmp           | gst-launch-1.0 v4l2src device=/dev/video1 ! capsfilter caps="image/jpeg,width=1920, height=1080, framerate=30/1" ! nvv4l2decoder mjpeg=1 ! nvvideoconvert ! videorate max-rate=30 ! nvv4l2h264enc ! h264parse ! flvmux streamable=true ! rtmpsink location=rtmp://localhost:1935/live/video_test                                                                                                                                                                  |
| 39 | videoxraw2rtmp        | push hdmi video to rtmp server                       | video, rtmp           | gst-launch-1.0 v4l2src device=/dev/video0 ! capsfilter caps="video/x-raw,width=1920, height=1080, framerate=60/1" ! nvvideoconvert ! videorate max-rate=30 ! nvv4l2h264enc ! h264parse ! flvmux streamable=true ! rtmpsink location=rtmp://localhost:1935/live/video_test                                                                                                                                                                                         |
| 40 | wav_scope_test        | display test audio's wave scope                      | audio                 | gst-launch-1.0 audiotestsrc ! audioconvert ! wavescope ! glimagesink                                                                                                                                                                                                                                                                                                                                                                                              |
| 41 | wav_scope_to_rtmp     | push wave scope video of mic's audio via rtmp        | audio                 | gst-launch-1.0 alsasrc device=hw:2,0 ! audioconvert ! wavescope ! queue ! videoconvert ! x264enc ! flvmux streamable=true ! queue ! rtmpsink location="rtmp://localhost:1935/live/wavetest"                                                                                                                                                                                                                                                                       |