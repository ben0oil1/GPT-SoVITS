# GPT-SoVITS-WebUI 
A Powerful Few-shot Voice Conversion and Text-to-Speech WebUI. 

[源](https://github.com/RVC-Boss/GPT-SoVITS)

[**中文简体**](./docs/cn/README.md) |


------


For users in China region, you can use AutoDL Cloud Docker to experience the full functionality online: https://www.codewithgpu.com/i/RVC-Boss/GPT-SoVITS/GPT-SoVITS-Official

## Features:
1. **Zero-shot TTS:** Input a 5-second vocal sample and experience instant text-to-speech conversion.

2. **Few-shot TTS:** Fine-tune the model with just 1 minute of training data for improved voice similarity and realism.

3. **Cross-lingual Support:** Inference in languages different from the training dataset, currently supporting English, Japanese, and Chinese.

4. **WebUI Tools:** Integrated tools include voice accompaniment separation, automatic training set segmentation, Chinese ASR, and text labeling, assisting beginners in creating training datasets and GPT/SoVITS models.

## Environment Preparation

If you are a Windows user (tested with win>=10) you can install directly via the prezip. Just download the [prezip](https://huggingface.co/lj1995/GPT-SoVITS-windows-package/resolve/main/GPT-SoVITS-beta.7z?download=true), unzip it and double-click go-webui.bat to start GPT-SoVITS-WebUI.

### Tested Environments

- Python 3.9, PyTorch 2.0.1, CUDA 11
- Python 3.10.13, PyTorch 2.1.2, CUDA 12.3
- Python 3.9, PyTorch 2.3.0.dev20240122, macOS 14.3 (Apple silicon, GPU)

_Note: numba==0.56.4 require py<3.11_

### For Mac Users
If you are a Mac user, make sure you meet the following conditions for training and inferencing with GPU: 
- Mac computers with Apple silicon or AMD GPUs
- macOS 12.3 or later
- Xcode command-line tools installed by running `xcode-select --install`

_Other Macs can do inference with CPU only._

Then install by using the following commands:
#### Create  Environment
```bash
conda create -n GPTSoVits python=3.9
conda activate GPTSoVits
```
#### Install Requirements
```bash
pip install -r requirements.txt
pip uninstall torch torchaudio
pip3 install --pre torch torchaudio --index-url https://download.pytorch.org/whl/nightly/cpu
```
_Note: For preprocessing with UVR5, it is recommended to [download the original project GUI](https://github.com/Anjok07/ultimatevocalremovergui) and select "GPU Conversion". Additionally, there might be memory leak issues, especially during inference. Restarting the inference webUI can help._
### Quick Install with Conda

```bash
conda create -n GPTSoVits python=3.9
conda activate GPTSoVits
bash install.sh
```
### Install Manually

#### Pip Packages

```bash
pip install -r requirements.txt
```


#### FFmpeg

##### Conda Users
```bash
conda install ffmpeg
```

##### Ubuntu/Debian Users

```bash
sudo apt install ffmpeg
sudo apt install libsox-dev
conda install -c conda-forge 'ffmpeg<7'
```

##### MacOS Users

```bash
brew install ffmpeg
```

##### Windows Users

Download and place [ffmpeg.exe](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffmpeg.exe) and [ffprobe.exe](https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffprobe.exe) in the GPT-SoVITS root.

### Pretrained Models


Download pretrained models from [GPT-SoVITS Models](https://huggingface.co/lj1995/GPT-SoVITS) and place them in `GPT_SoVITS/pretrained_models`.

For Chinese ASR (additionally), download models from [Damo ASR Model](https://modelscope.cn/models/damo/speech_paraformer-large_asr_nat-zh-cn-16k-common-vocab8404-pytorch/files), [Damo VAD Model](https://modelscope.cn/models/damo/speech_fsmn_vad_zh-cn-16k-common-pytorch/files), and [Damo Punc Model](https://modelscope.cn/models/damo/punc_ct-transformer_zh-cn-common-vocab272727-pytorch/files) and place them in `tools/damo_asr/models`.

For UVR5 (Vocals/Accompaniment Separation & Reverberation Removal, additionally), download models from [UVR5 Weights](https://huggingface.co/lj1995/VoiceConversionWebUI/tree/main/uvr5_weights) and place them in `tools/uvr5/uvr5_weights`.


### Using Docker

#### docker-compose.yaml configuration

1. Environment Variables：
  - is_half: Controls half-precision/double-precision. This is typically the cause if the content under the directories 4-cnhubert/5-wav32k is not generated correctly during the "SSL extracting" step. Adjust to True or False based on your actual situation.

2. Volumes Configuration，The application's root directory inside the container is set to /workspace. The default docker-compose.yaml lists some practical examples for uploading/downloading content.
3. shm_size： The default available memory for Docker Desktop on Windows is too small, which can cause abnormal operations. Adjust according to your own situation.
4. Under the deploy section, GPU-related settings should be adjusted cautiously according to your system and actual circumstances.


#### Running with docker compose
```
docker compose -f "docker-compose.yaml" up -d
```

#### Running with docker command

As above, modify the corresponding parameters based on your actual situation, then run the following command:
```
docker run --rm -it --gpus=all --env=is_half=False --volume=G:\GPT-SoVITS-DockerTest\output:/workspace/output --volume=G:\GPT-SoVITS-DockerTest\logs:/workspace/logs --volume=G:\GPT-SoVITS-DockerTest\SoVITS_weights:/workspace/SoVITS_weights --workdir=/workspace -p 9870:9870 -p 9871:9871 -p 9872:9872 -p 9873:9873 -p 9874:9874 --shm-size="16G" -d breakstring/gpt-sovits:dev-20240123.03
```


## Dataset Format

The TTS annotation .list file format:

```
vocal_path|speaker_name|language|text
```

Language dictionary:

- 'zh': Chinese
- 'ja': Japanese
- 'en': English

Example:

```
D:\GPT-SoVITS\xxx/xxx.wav|xxx|en|I like playing Genshin.
```
