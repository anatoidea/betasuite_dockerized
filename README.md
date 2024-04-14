# Dockerization of BetaSuite

See https://github.com/solarorb93/BetaSuite for BetaSuite's original configuration and usage.

I have some updates at https://github.com/anatoidea/BetaSuite that aren't (yet) merged in.


I've tested this on Linux with an AMD graphics card, but this should serve as a good starting point for other operating systems (with Docker) and other hardware.


# Instructions

After cloning this repo,

`git submodule update --init`

Depending on what graphics hardware you have, you'll need to modify these instructions slightly. Choose the corresponding onnxruntime Dockerfile for your hardware. I'm using `Dockerfile.rocm`. I've included the `Dockerfile.cuda` file in this repo, but https://github.com/microsoft/onnxruntime/tree/main/dockerfiles has more.

Build the onnxruntime base image:

`docker build -t betasuite-base -f onnxruntime_dockerfiles/Dockerfile.rocm onnxruntime_dockerfiles`

This may take a long time...

Now build the betasuite image:

`docker build -t betasuite -f dockerfiles/Dockerfile.rocm .`

Configure `BetaSuite/betaconfig.py` as needed. You'll want to at least change the `providers` line and the `ffmpeg_path` and `ffprobe_path` lines.

Download the NudeNet model and place it in the `model` folder. 


And run it! This is the line for ROCm on Linux. (Check https://github.com/microsoft/onnxruntime/tree/main/dockerfiles for instructions for other setups.)

`docker run -it --device=/dev/kfd --device=/dev/dri --group-add video -v .:/betasuite betasuite python3 betastare.py`

or

`docker run -it --device=/dev/kfd --device=/dev/dri --group-add video -v .:/betasuite betasuite python3 betatv.py`

