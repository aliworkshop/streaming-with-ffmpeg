
```
ffmpeg -hide_banner -re -i file.mp4  \
-c:v h264 -profile:v main -crf 20 -sc_threshold 0 -g 48 -keyint_min 48 -c:a aac -ar 48000 \
-filter:v:0 scale=w=640:h=360 -maxrate:v:0 856k -bufsize:v:0 1200k -b:a:0 96k \
-filter:v:1 scale=w=842:h=480 -maxrate:v:1 1498k -bufsize:v:1 2100k -b:a:1 128k \
-filter:v:2 scale=w=1280:h=720 -maxrate:v:2 2996k -bufsize:v:2 4200k -b:a:2 128k \
-filter:v:3 scale=w=1920:h=1080 -maxrate:v:3 5350k -bufsize:v:3 7500k -b:a:3 192k \
-map 0:v -map 0:a -map 0:v -map 0:a -map 0:v -map 0:a -map 0:v -map 0:a \
-var_stream_map "v:0,a:0,name:360p v:1,a:1,name:480p v:2,a:2,name:720p v:3,a:3,name:1080p" \
-preset slow -hls_list_size 0 -threads 0 -f hls -hls_playlist_type vod -hls_time 4 \
-master_pl_name master.m3u8 \
-hls_segment_filename %v/%03d.ts %v/index.m3u8
```

## description for parameters

#### `b:v`
is for video bitrate
#### `b:a`
is for audio bitrate
#### `c:v` 
is for video codec
#### `c:a` 
is for audio codec
#### `var_stream_map`
Map string which specifies how to group the audio, video and subtitle streams into different variant streams. The variant stream groups are separated by space. Expected string format is like this "a:0,v:0 a:1,v:1 ....". Here a:, v:, s: are the keys to specify audio, video and subtitle streams respectively. Allowed values are 0 to 9 (limited just based on practical usage).

When there are two or more variant streams, the output filename pattern must contain the string "%v", this string specifies the position of variant stream index in the output media playlist filenames. The string "%v" may be present in the filename or in the last directory name containing the file. If the string is present in the directory name, then sub-directories are created after expanding the directory name pattern. This enables creation of variant streams in subdirectories.

#### hls_playlist_type `event`
Emit #EXT-X-PLAYLIST-TYPE:EVENT in the m3u8 header. Forces hls_list_size to 0; the playlist can only be appended to.
#### hls_playlist_type `vod`
Emit #EXT-X-PLAYLIST-TYPE:VOD in the m3u8 header. Forces hls_list_size to 0; the playlist must not change.

