# [*mEye*](https://www.pupillometry.it): A Deep Learning Tool for Pupillometry

> ⭐ MEYE is available on **MATLAB**! Check it out [here](matlab/README.md)

> Check out [pupillometry.it](https://www.pupillometry.it) for a ready-to-use web-based mEye pupillometry tool!

This branch provides the Python code to make predictions and train/finetune models.
If you are interested in the code of the pupillometry web app, check out the `gh-pages` branch.

## Requirements
You need a Python 3 environment with the following packages installed:

  - tensorflow >= 2.4
  - imageio, imageio-ffmpeg
  - scipy  
  - tqdm

If you want to train models, you also need 

  - adabelief_tf >= 0.2.1
  - pandas
  - sklearn

We provide a [Dockerfile](./Dockerfile) for building an image with docker.

## Make Predictions with Pretrained Models

You can make predictions with pretrained models on pre-recorded videos or webcam streams. 

  1. Download the [pretrained model](https://github.com/fabiocarrara/meye/releases/download/v0.1.1/meye-2022-01-24.h5). If you want to use the [old model](https://github.com/fabiocarrara/meye/releases/download/v0.1/meye-segmentation_i128_s4_c1_f16_g1_a-relu.hdf5), check out version [`v0.1` of this branch](https://github.com/fabiocarrara/meye/tree/v0.1). See available models in [Releases](https://github.com/fabiocarrara/meye/releases).
  2. Check out the `pupillometry-offline-videos.ipynb` notebook for a complete example of pupillometry data analysis.
  3. In alternative, we provide also the `predict.py` script that implements the basic loop to make predictions on video streams. E.g.:

       - ```bash
         # input: webcam (default)
         # prediction roi: biggest central square crop (default)
         # outputs: predictions.mp4, predictions.csv (default)
         predict.py path/to/model
         ```
     
       - ```bash
         # input: video file
         # prediction roi: left=80, top=80, right=208, bottom =208
         # outputs: video_with_predictions.mp4, pupil_metrics.csv
         predict.py path/to/model path/to/video.mp4 -rl 80 -rt 80 -rr 208 -rb 208 -ov video_with_predictions.mp4 -oc pupil_metrics.csv
         ```
       - ```bash
         # check all parameters with
         predict.py -h
         ```
    
## Training Models

  1. Download our dataset ([NN_human_mouse_eyes.zip](https://doi.org/10.5281/zenodo.4488164), 246.4 MB) or prepare your dataset following our dataset's structure.
     > If you need to annotate your dataset, check out [pLabeler](https://github.com/LeonardoLupori/pLabeler), a MATLAB software for labeling pupil images.
     
     The dataset should be placed in `data/<dataset_name>`.
     
  2. If you are using a custom dataset, edit `train.py` to perform the train/validation/test split of your data.
     
  3. Train with default parameters:
     ```bash
     python train.py -d data/<dataset_name>
     ```
     
  - For a list of available parameters, run
    ```bash
    python train.py -h
    ```

## MATLAB support
Starting from MATLAB version 2021b, MEYE is also available for use on MATLAB!  
A fully functional class and a tutorial for its use is available [here](matlab/README.md)!


## References

### Dataset
 [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.4488164.svg)](https://doi.org/10.5281/zenodo.4488164)

If you use our dataset, please cite:

     @dataset{raffaele_mazziotti_2021_4488164,
       author       = {Raffaele Mazziotti and Fabio Carrara and Aurelia Viglione and Lupori Leonardo and Lo Verde Luca and Benedetto Alessandro and Ricci Giulia and Sagona Giulia and Amato Giuseppe and Pizzorusso Tommaso},
       title        = {{Human and Mouse Eyes for Pupil Semantic Segmentation}},
       month        = feb,
       year         = 2021,
       publisher    = {Zenodo},
       version      = {1.0},
       doi          = {10.5281/zenodo.4488164},
       url          = {https://doi.org/10.5281/zenodo.4488164}
     }


## Pho Note 2024-09-25 22:31 - Basically Working! Processing videos I recorded.

# Pho Documentation

```
(meye-py3.10) PS C:\Users\pho\repos\EyeTracking\meye> python predict.py "${modelPath}" '<video5>' -ov video_with_predictions.mp4 -oc 2024-09-27_pho_pupil_metrics.csv
Traceback (most recent call last):
  File "K:\FastSwap\Environments\pypoetry\pypoetry\Cache\virtualenvs\meye-imRk_HCI-py3.10\lib\site-packages\imageio_ffmpeg\_io.py", line 163, in count_frames_and_secs
    out = subprocess.check_output(cmd, stderr=subprocess.STDOUT, **_popen_kwargs())
  File "C:\Users\pho\.pyenv\pyenv-win\versions\3.10.9\lib\subprocess.py", line 421, in check_output
    return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
  File "C:\Users\pho\.pyenv\pyenv-win\versions\3.10.9\lib\subprocess.py", line 526, in run
    raise CalledProcessError(retcode, process.args,
subprocess.CalledProcessError: Command '['K:\\FastSwap\\Environments\\pypoetry\\pypoetry\\Cache\\virtualenvs\\meye-imRk_HCI-py3.10\\lib\\site-packages\\imageio_ffmpeg\\binaries\\ffmpeg-win64-v4.2.2.exe', '-i', 'video=OBS Virtual Camera', '-map', '0:v:0', '-vf', 'null', '-f', 'null', '-']' returned non-zero exit status 1.

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "C:\Users\pho\repos\EyeTracking\meye\predict.py", line 81, in <module>
    main(args)
  File "C:\Users\pho\repos\EyeTracking\meye\predict.py", line 14, in main
    n_frames = video.count_frames()
  File "K:\FastSwap\Environments\pypoetry\pypoetry\Cache\virtualenvs\meye-imRk_HCI-py3.10\lib\site-packages\imageio\plugins\ffmpeg.py", line 385, in count_frames
    return cf(self._filename)[0]
  File "K:\FastSwap\Environments\pypoetry\pypoetry\Cache\virtualenvs\meye-imRk_HCI-py3.10\lib\site-packages\imageio_ffmpeg\_io.py", line 166, in count_frames_and_secs
    raise RuntimeError(
RuntimeError: FFMPEG call failed with 1:
ffmpeg version 4.2.2 Copyright (c) 2000-2019 the FFmpeg developers
  built with gcc 9.2.1 (GCC) 20200122
  configuration: --enable-gpl --enable-version3 --enable-sdl2 --enable-fontconfig --enable-gnutls --enable-iconv --enable-libass --enable-libdav1d --enable-libbluray --enable-libfreetype --enable-libmp3lame --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libopenjpeg --enable-libopus --enable-libshine --enable-libsnappy --enable-libsoxr --enable-libtheora --enable-libtwolame --enable-libvpx --enable-libwavpack --enable-libwebp --enable-libx264 --enable-libx265 --enable-libxml2 --enable-libzimg --enable-lzma --enable-zlib --enable-gmp --enable-libvidstab --enable-libvorbis --enable-libvo-amrwbenc --enable-libmysofa --enable-libspeex --enable-libxvid --enable-libaom --enable-libmfx --enable-amf --enable-ffnvcodec --enable-cuvid --enable-d3d11va --enable-nvenc --enable-nvdec --enable-dxva2 --enable-avisynth --enable-libopenmpt
  libavutil      56. 31.100 / 56. 31.100
  libavcodec     58. 54.100 / 58. 54.100
  libavformat    58. 29.100 / 58. 29.100
  libavdevice    58.  8.100 / 58.  8.100
  libavfilter     7. 57.100 /  7. 57.100
  libswscale      5.  5.100 /  5.  5.100
  libswresample   3.  5.100 /  3.  5.100
  libpostproc    55.  5.100 / 55.  5.100
video=OBS Virtual Camera: No such file or directory
```
