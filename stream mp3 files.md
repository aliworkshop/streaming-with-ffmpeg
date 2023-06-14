```
ffmpeg -i file.mp3 -c:a libmp3lame -b:a 128k -map 0:0 -f segment -segment_time 10 \
-segment_list outputlist.m3u8 -segment_format mpegts m3u8/output%03d.ts
```
