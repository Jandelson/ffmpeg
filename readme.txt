#Remover audio do video
ffmpeg -i videoplayback.mp4 -c:v copy -an tadandoonda_sem_audio.mp4

#Adicionar audio ao video
ffmpeg -i tadandoonda_sem_audio.mp4 -i audio1.mp3  -c copy  -map 0:v:0  -map 1:a:0  videoWithAudio.mp4

#Extrair audio do video
ffmpeg -i videoWithAudio.mp4 -vn -c:a libmp3lame -q:a 1 onlyAudio.mp3

#Arquivo com as cenas
ffmpeg -report -hide_banner -i videoplayback.mp4 -filter:v "select='gt(scene,0.5)',metadata=print:file=scenechanges.txt" -f null -
ffmpeg -i videoplayback.mp4 -filter:v "select='gt(scene,0.5)',metadata=print:file=scenechanges.txt" -f null -

#Cortar Videos
ffmpeg -ss 3.08643 -to 7.46579  -i videoplayback.mp4 -c copy output.mp4
ffmpeg -ss 00:00:00 -to 00:00:09  -i jan.mp4 -c copy jan1.mp4

#Concat
ffmpeg -f concat -safe 0 -i mylist.txt -c copy outputx.mp4

#get pts_time
ffmpeg -i videoplayback.mp4 -filter:v "select='gt(scene,0.5)',showinfo" -f null - 2>&1 | \
    grep Parsed_showinfo | grep pts_time:[0-9.]* -o | grep "[0-9]*\.[0-9]*" -o

