# alsa-audio-server
Scripts used to start and run the ffmpeg-based ALSA to HTTP service

The service uses ffmpeg to process the audio from an ALSA source, encode the audio, and stream the encoded audio to a remote client.

## Configuration
The default configuration file can be found in /usr/local/etc/alsa-audio-server.json.  There are sections to define the input, output, and server.

### Input
options include the type of input (should always be alsa), the sample rate for your sound card, number of channels, and the hardware address.

    "input": {
        "type": "alsa",
        "sampleRate": "44100",
        "channels": "2",
        "address": "hw:1"
    }

### Output
The output section defines the codec used for the stream and options for the various supported codecs (currently only flac and mp3 are supported).  Subsections for each codec specify additional processing options.

* FLAC Encoding
  When 'flac' is specified as the codec, the compression level parameter may be defined.
* MP3 Encoding
  The options for the 'mp3' codec include encoding (VBR/CBR), quality (VBR), and bitrate(CBR)

    "output": {
        "codec": "flac",
        "flac": {
            "compressionLevel": "5"
        },
        "mp3": {
            "encoding": "cbr",
            "bitrate": "256k",
            "quality": "0"
        }
    }

### Server Options
Server options include the logging level for ffmpeg and the port where the server listens for connections.

    "server": {
        "logLevel": "quiet",
        "listenPort": "8080"
    }

## Running the Service
The service definition can be found in /etc/systemd/system/alsa-audio-server.service, and can be maintained and monitored using the `systemctl` command.