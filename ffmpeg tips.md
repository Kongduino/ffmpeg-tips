## CROP

```bash
ffmpeg -i in.mp4 -filter:v "crop=out_w:out_h:x:y" out.mp4
```
*  out_w width of the output rectangle
*  out_h height of the output rectangle
*  x / y top-left corner of the output rectangle
```bash
$ ffmpeg -i Shredder.mp4 -filter:v "crop=1000:720:200:0" out.mp4
```

## SCALE

```bash
$ ffmpeg -i out.mp4 -filter:v scale=720:-1 -c:a copy shred.mp4
```
*  scale=w:h
*  -1 to let ffmpeg calculate the correct value

## EXTRACT PORTION

```bash
$ ffmpeg -ss 00:00:30.0 -i input.wmv -c copy -t 00:00:10.0 output.wmv
```

## CONCATENATE FILES

```bash
$ cat mylist.txt
file '/path/to/file1'
file '/path/to/file2'
file '/path/to/file3'

$ ffmpeg -f concat -safe 0 -i mylist.txt -c copy output.mp4
```

## CONVERT TO GIF

```bash
$ cat gifenc.sh

#!/bin/sh
palette="/tmp/palette.png"
filters="fps=$4,scale=$3:-1:flags=lanczos"
ffmpeg -v warning -i "$1" -vf "$filters,palettegen" -y "$palette"
ffmpeg -v warning -i "$1" -i $palette -lavfi "$filters [x]; [x][1:v] paletteuse" -y "$2"

$ ./gifenc.sh input.mov output.gif 720 10
```

This will convert "input.mov" to a GIF that's 720p wide 10fps.

## ADD SUBTITLES

First convert the subtitles to .ass format. [Yes, ass. Who doesn't love ass and videos, right?]

More seriously, this tip uses the [libass library](https://github.com/libass/libass). Make sure your ffmpeg install has the library in the configuration: `--enable-libass`.


```bash
$ ffmpeg -i subtitles.srt subtitles.ass
```

Then add them using a video filter:

```bash
$ ffmpeg -i mymovie.mp4 -vf ass=subtitles.ass mysubtitledmovie.mp4
```
