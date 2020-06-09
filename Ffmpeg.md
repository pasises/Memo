# FFMPEG
## Gifを倍速する
純粋に倍速にするとなぜか色がおかしくなる．   
  
``` 
//gifをmp4に変換  
ffmpeg -i input.gif  -movflags faststart -pix_fmt yuv420p -vf "scale=trunc(iw/2)*2:trunc(ih/2)*2" output.mp4  

//倍速にする  
ffmpeg -i output.mp4 -vf setpts=PTS/10.0 -af atempo=10.0 -q 0 output_10.mp4  

//mp4からgifへ変換するのに必要な画像情報を出力する  
ffmpeg -i output_10.mp4 -vf "palettegen" -y palette.png  

//mp4からgifへ変換する  
ffmpeg -i output_10.mp4 -i palette.png -lavfi fps=12,scale=900:-1:flags=lanczos [x]; [x][1:v] "paletteuse=dither=bayer:bayer_scale=5:diff_mode=rectangle" -y output.gif
```