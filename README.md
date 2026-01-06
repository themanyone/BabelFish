# BabelFish
<img src="bf.png" align="right">(Douglas Adams, "The Hitchhiker's Guide to the Galaxy").

A voice-activated, "organic" (free, private, open-source, offline, off-grid) universal translator, written as a minimal Bash script.

Preorder BabelFish earbuds here [Coming soon](). In the meantime, run the code on a Linux laptop, PC, or Raspberry Pi. Listen in on other languages, wherever and whenever they are spoken.

## Installation

**mimic3.** None of this requires `root` access. Install as a normal user. Use [this upgraded version that supports python 3.14](https://github.com/themanyone/mimic3). Run `install.sh`to install to a virtual environment to ensure compatability. Or install it to the base environment like this:

```shell
git clone https://github.com/themanyone/mimic3.git
cd mimic3
pip install --upgrade pip wheel setuptools
pip install -r requirements.txt
pip install .[all]

# If some deps fail to build with `gcc`
CC=clang pip install .[all]
# or
CC=tcc pip install .[all]
```

**whisper.cpp.** Install [whisper.cpp](https://github.com/ggml-org/whisper.cpp) according to the instructions found there.

Install whisper-server as a service by copying the service file below to `$HOME/.config/systemd/user/whisper.service`. Use a different port if you prefer. Just make clients and servers agree on which port to use. The `bab` script will automatically start the whisper service only when needed.

```shell
[Unit]
Description=Run Whisper server
Documentation=https://github.com/openai/whisper

[Service]
ExecStart=/home/k/.local/bin/whisper-server \
-vm $USER/Downloads/src/whisper.cpp/models/ggml-silero-v6.2.0.bin --vad \
-m $USER/Downloads/src/whisper.cpp/models/ggml-medium-q5_0.bin \
-sns --convert --port 7777

[Install]
WantedBy=default.target
```

You must run systemctl --user daemon-reload after making changes to any systemd unit files (e.g., in ~/.config/systemd/user/) to ensure the systemd user instance picks up the new configuration.

# Testing

Change directory to `whisper.cpp`and run the following tests to make sure the setup can hear, translate, and speak a foreign language.

```shell
models/download-ggml-model.sh medium-q5_0
whisper-cli -np -pc -l ru -f samples/jfk.wav -m models/ggml-medium-q5_0.bin

mimic3 --voice ru_RU/multi_low "И так, мои другие американцы"
```

We start the `whisper-server` with a larger model, such as `ggml-medium-q5_0.bin` when operating as a translator.

Since MIMIC 3 is capable of sending audio output to a pipe, we can use `aplay` to send the sound output to another device, such as the BableFish Bluetooth earbuds.

`mimic3 "Hello world!" --stdout|aplay -D sysdefault:CARD=Generic`

## Configuration

Edit `bab`. Find the line containing `aplay` and change the `--device` to your Bablefish Bluetooth audio device. Once paired, use `aplay -L` to discover the device name.

You may also comment out "# --device..." entirely and choose outputs using your desktop volume control/mixer.

## Running

Once everything is set up, tested and configured, our universal translator should be up and running.The device's language-selection dial should start `bab` with the desired language to translate into. But we can run `bab` from the command line, e.g. `bab es_ES/m-ailabs_low` or `bab ru_RU/multi_low`.

## Similar Projects

- [Voice Typing](https://github.com/themanyone/voice_typing)
- [Whisper Dictation](https://github.com/themanyone/whisper_dictation.git)
- [Caption Anything](https://github.com/themanyone/caption_anything)

### Thanks for trying out Voice Typing!

- GitHub https://github.com/themanyone
- YouTube https://www.youtube.com/themanyone
- Mastodon https://mastodon.social/@themanyone
- Linkedin https://www.linkedin.com/in/henry-kroll-iii-93860426/
- Buy me a coffee https://buymeacoffee.com/isreality
- [TheNerdShow.com](http://thenerdshow.com/)

Copyright (C) 2026 Henry Kroll III, www.thenerdshow.com.
See [LICENSE](LICENSE) for details.

