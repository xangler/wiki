# ffmpeg 全家桶
## ffmpeg 生成视频
```bash
ffmpeg -y -loop 1 -framerate 1 -pattern_type glob -i 'dc*.png' -vf "scale=1920x1080:force_original_aspect_ratio=decrease,pad=1920:1080:(ow-iw)/2:(oh-ih)/2"  -vcodec libx264 -t 30 out.mp4
ffmpeg -loop 1 -i sand.jpeg -r 25  -vcodec libx264 -t 30 out.mp4
```
## ffmpeg 截取视频
```bash
ffmpeg -y -rtsp_transport tcp -i rtsp://127.0.0.1:9051/demo.mp4 -vcodec copy -f mp4 -t 30 out.mp4
```
## ffprobe查看视频信息
```bash
ffprobe -v quiet -show_format -show_streams -print_format json demo.mp4
```
## ffprobe查看视频关键帧
```bash
ffprobe -v quiet -skip_frame nokey -show_entries frame=pkt_pts_time -of csv=print_section=0 demo.mp4
```
## ffmpeg 重建关键帧
```bash
ffmpeg -i demo.mp4 -g 25 ndemo.mp4
```
## ffmpeg格式转换
```bash
ffmpeg -i demo.vai -c:v copy -c:a aac -map 0 -f mp4 -vcodec h264 demo.mp4
```