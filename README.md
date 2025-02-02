# Golang Transcoding Library

> This repository is no longer maintained. Please use [Goffmpeg](https://github.com/xfrr/goffmpeg).

## Features

<dl>
  <dt>Ease use</dt>
  <dd>Implement your own business logic around easy interfaces</dd>

  <dt>Transcoding progress</dt>
  <dd>Obtain progress events generated by transcoding application process</dd>
</dl>

## Download from Github

```shell
go get github.com/ahmadissa/transcoder
```

## Example

```go
package main

import (
	"log"

	ffmpeg "github.com/ahmadissa/transcoder/ffmpeg"
)

func main() {


	hwaccel := "cuvid"
	videoCodec := "h264_cuvid"

	inputOpts := ffmpeg.Options{
		Hwaccel:      &hwaccel,
		VideoCodec:   &videoCodec,
	}

	format := "mp4"
	overwrite := true

	outputOpts := ffmpeg.Options{
		OutputFormat: &format,
		Overwrite:    &overwrite,
	}

	ffmpegConf := &ffmpeg.Config{
		FfmpegBinPath:   "/usr/local/bin/ffmpeg",
		FfprobeBinPath:  "/usr/local/bin/ffprobe",
		ProgressEnabled: true,
	}

	progress, err := ffmpeg.
		New(ffmpegConf).
		Input("/tmp/avi").
		Output("/tmp/mp4").
		WithInputOptions(inputOpts).
		WithOutputOptions(outputOpts).
		Start()

	if err != nil {
		log.Fatal(err)
	}

	for msg := range progress {
		log.Printf("%+v", msg)
	}
}
```
