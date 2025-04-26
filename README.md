# Dockerization of BetaSuite

Dockerization of https://github.com/anatoidea/BetaSuite

(Originally based on https://github.com/solarorb93/BetaSuite)

I've tested this on Linux with an AMD graphics card, but this should serve as a good starting point for other operating systems (with Docker) and other hardware.


# Instructions

After cloning this repo,

`git submodule update --init`


Now build the betasuite image. If you have an Nvidia card, I don't have an answer yet. Check the Outdated section below.

`docker build -t betasuite -f dockerfiles/Dockerfile.rocm .`

Configure `BetaSuite/betaconfig.py` as needed.

Download the NudeNet model and place it in the `model` folder. [instructions](https://github.com/anatoidea/BetaSuite/tree/main#:~:text=Download%20NudeNet%20neural%20net%20model)

And run it! This is the line for ROCm on Linux.

`docker run -it --device=/dev/kfd --device=/dev/dri --group-add video -v .:/betasuite betasuite python3 betastare.py`

or

`docker run -it --device=/dev/kfd --device=/dev/dri --group-add video -v .:/betasuite betasuite python3 betatv.py`


And for BetaVision:

```
xhost +
docker run -it --device=/dev/kfd --device=/dev/dri --group-add video -v .:/betasuite --env="DISPLAY" --net=host --shm-size=1GB betasuite
```

Then

```
python3 betavision.py
```

If you need another terminal in the same container
```
running_betasuite="$(docker ps -qf "ancestor=betasuite")"
docker exec -it "$running_betasuite" bash

```



# Outdated

## Manually building the onnxruntime container

This shouldn't be needed. Just base the containers off of the ones on Docker Hub

If you need the dockerfiles to manually build the base: https://github.com/microsoft/onnxruntime/tree/main/dockerfiles

Build the onnxruntime base image:

`docker build -t betasuite-base -f onnxruntime_dockerfiles/Dockerfile.rocm onnxruntime_dockerfiles`

This may take a long time...
