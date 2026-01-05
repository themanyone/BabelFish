# BabelFish
A voice-activated offline, off-grid universal translator
(Douglas Adams, "The Hitchhiker's Guide to the Galaxy").

Introducing the BableFish in-ear device. It translates everything into English. Or some other language (selectable). 

You can preorder the BabelFish here. In the meantime, you can run the code on your laptop and listen in on other languages, whenever they are spoken.

## Installation. 

You'll have to install `mimic3` if you want to hear the spoken translations. Use our upgraded version from github that supports `python 3.14`. Run `install.sh`to install to a virtual environment to ensure compatability. Or install it to the base environment like this:

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



Install [whisper.cpp](https://github.com/ggml-org/whisper.cpp) according to their instructions.

# Testing.

Change directory to `whisper.cpp`and run the following tests. Running these tests will determine if your installation can translate spoken text into another language. And speak it.

```shell
models/download-ggml-model.sh medium-q5_0
whisper-cli -np -pc -l ru -f samples/jfk.wav -m models/ggml-medium-q5_0.bin

mimic3 --voice ru_RU/multi_low "И так, мои другие американцы"
```

We start the `whisper-server` with a larger model, such as `ggml-medium-q5_0.bin` when operating as a translator.

Since MIMIC 3 is capable of sending audio output to a pipe, we can use `aplay` to send the sound output to another device, such as the BableFish's earphone.

`mimic3 "Hello world!" --stdout|aplay -D sysdefault:CARD=Generic`

## Configuration.

Edit `bab`. Find the line containing `aplay` and change the `--device` to your Bablefish's Bluetooth audio device. Use `aplay -L` to discover it.

You may also comment out "# --device..." entirely and choose outputs using your desktop volume control/mixer.

## Running.

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

