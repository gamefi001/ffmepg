# 通过docker运行一个ffmpeg,实现自动化直播推流



## 创建工作目录,并放入mp4格式文件
mkdir /root\/ffmpeg

cd /root/ffmpeg

## 把推流rtmp地址填入config.ini内
vi config.ini 

## 修改启动配置
vi start.sh

rtmp=\`cat /ffmpeg/config.ini`

while true; do video=$(find ./ -type f | shuf -n 1); ffmpeg -re -i "$video" -preset ultrafast -vcodec libx264 -g 60 -b:v 1500k -c:a aac -b:a 92k -strict -2 -f flv ${rtmp}; done

## 启动docker
docker run -itd --name ffmpeg -v /root/ffmpeg:/ffmpeg -w /ffmpeg --restart=always --privileged=true gamefi001/ffmpeg:latest bash start.sh

