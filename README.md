# Reisen [![GoDoc](https://godoc.org/github.com/zergon321/reisen?status.svg)](https://pkg.go.dev/github.com/zergon321/reisen)

A simple library to extract video and audio frames from media containers (based on **libav**, i.e. **ffmpeg**).

## Dependencies

The library requires **libav** components to work:

- **libavformat**
- **libavcodec**
- **libavutil**
- **libswresample**
- **libswscale**

For **Arch**-based **Linux** distributions:

```bash
sudo pacman -S ffmpeg
```

For **Debian**-based **Linux** distributions:

```bash
sudo apt install libswscale-dev libavcodec-dev libavformat-dev libswresample-dev libavutil-dev
```

For **macOS**:

```bash
brew install libav
```

For **Windows** see the [detailed tutorial](https://medium.com/@maximgradan/how-to-easily-bundle-your-cgo-application-for-windows-8515d2b19f1e).

## Installation

Just casually run this command:

```bash
go get github.com/zergon321/reisen
```

## Usage

Any media file is composed of streams containing media data, e.g. audio, video and subtitles. The whole presentation data of the file is divided into packets. Each packet belongs to one of the streams and represents a single frame of its data. The process of decoding implies reading packets and decoding them into either video frames or audio frames.

The library provides read video frames as **RGBA** pictures. The audio samples are provided as raw byte slices in the format of `AV_SAMPLE_FMT_DBL` (i.e. 8 bytes per sample for one channel, the data type is `float64`). The channel layout is stereo (2 channels). The byte order is little-endian. The detailed scheme of the audio samples sequence is given below.

![Audio sample structure](https://github.com/zergon321/reisen/blob/master/pictures/audio_sample_structure.png)

You are welcome to look at the [examples](https://github.com/zergon321/reisen/tree/master/examples) to understand how to work with the library. Also please take a look at the detailed [tutorial](https://medium.com/@maximgradan/playing-videos-with-golang-83e67447b111).

## Sample Format Description

The meta information of Sample Format can be found in all kinds of audio files, as well as in video files. In some cases, other file types can contain a value for Sample Format as well. Oftentimes, when you look at the metadata of your file, Sample Format is shortened and thus represented as “sample fmt”. Here, the fmt is simply an abbreviation for format.

But what does the Sample Format say about your file? This information is used to determine the number of bits used per sample. The higher the number of bits per sample in a file, the more data can be stored in this sample. Thus, 8bit contains less data than a Sample Format of 16bit. There are some very commonly used Sample Formats. They are listed below, going from low to high quality:

- 8bit
- 16bit
- 24bit

Together with the number of bits per sample, more information that further defines the Sample Format is often given when checking the metadata of your file. For example:

- u16 – unsigned 16 bits
- s16 – signed 16 bits
- s16p – signed 16 bits, planar
- flt – float
- fltp – float, planar
- dbl – double
- dblp – double, planar
