---
tags:
- pyannote
- audio
- voice
- speech
- speaker
- speaker segmentation
- voice activity detection
- overlapped speech detection
- resegmentation
datasets:
- ami
- dihard
- voxconverse
license: mit
inference: false
---

# Pretrained speaker segmentation model

This model relies on `pyannote.audio` 2.0 (which is still in development):

```bash
$ pip install https://github.com/pyannote/pyannote-audio/archive/develop.zip
```

## Basic inference

```python
>>> from pyannote.audio import Inference
>>> inference = Inference("pyannote/Segmentation")
>>> segmentation = inference("audio.wav")
```

## Advanced pipelines

### Voice activity detection

```python
>>> from pyannote.audio.pipelines import VoiceActivityDetection
>>> HYPER_PARAMETERS = {"onset": 0.5, "offset": 0.5, "min_duration_on": 0.0, "min_duration_off": 0.0}
>>> pipeline = VoiceActivityDetection(segmentation="pyannote/Segmentation").instantiate(HYPER_PARAMETERS)
>>> vad = pipeline("audio.wav")
```

Dataset         | `onset` | `offset` | `min_duration_on` | `min_duration_off`
----------------|---------|----------|-------------------|-------------------
AMI Mix-Headset | TODO    | TODO     | TODO              | TODO 
DIHARD3         | TODO    | TODO     | TODO              | TODO 
VoxConverse     | TODO    | TODO     | TODO              | TODO 


### Overlapped speech detection

```python
>>> from pyannote.audio.pipelines import OverlappedSpeechDetection
>>> pipeline = OverlappedSpeechDetection(segmentation="pyannote/Segmentation").instantiate(HYPER_PARAMETERS)
>>> osd = pipeline("audio.wav")
```

Dataset         | `onset` | `offset` | `min_duration_on` | `min_duration_off`
----------------|---------|----------|-------------------|-------------------
AMI Mix-Headset | TODO    | TODO     | TODO              | TODO 
DIHARD3         | TODO    | TODO     | TODO              | TODO 
VoxConverse     | TODO    | TODO     | TODO              | TODO 


### Segmentation

```python
>>> from pyannote.audio.pipelines import Segmentation
>>> pipeline = Segmentation(segmentation="pyannote/Segmentation").instantiate(HYPER_PARAMETERS)
>>> seg = pipeline("audio.wav")
```

Dataset         | `onset` | `offset` | `min_duration_on` | `min_duration_off`
----------------|---------|----------|-------------------|-------------------
AMI Mix-Headset | TODO    | TODO     | TODO              | TODO 
DIHARD3         | TODO    | TODO     | TODO              | TODO 
VoxConverse     | TODO    | TODO     | TODO              | TODO 

### Resegmentation

```python
>>> from pyannote.audio.pipelines import Resegmentation
>>> pipeline = Resegmentation(segmentation="pyannote/Segmentation", diarization="baseline")
>>> assert isinstance(baseline, pyannote.core.Annotation)
>>> resegmented_baseline = pipeline({"audio": "audio.wav", "baseline": baseline})
```

Dataset         | `onset` | `offset` | `min_duration_on` | `min_duration_off`
----------------|---------|----------|-------------------|-------------------
AMI Mix-Headset | TODO    | TODO     | TODO              | TODO 
DIHARD3         | TODO    | TODO     | TODO              | TODO 
VoxConverse     | TODO    | TODO     | TODO              | TODO 

## Citations


```bibtex
@inproceedings{Bredin2020,
  Title = {{pyannote.audio: neural building blocks for speaker diarization}},
  Author = {{Bredin}, Herv{\'e} and {Yin}, Ruiqing and {Coria}, Juan Manuel and {Gelly}, Gregory and {Korshunov}, Pavel and {Lavechin}, Marvin and {Fustes}, Diego and {Titeux}, Hadrien and {Bouaziz}, Wassim and {Gill}, Marie-Philippe},
  Booktitle = {ICASSP 2020, IEEE International Conference on Acoustics, Speech, and Signal Processing},
  Address = {Barcelona, Spain},
  Month = {May},
  Year = {2020},
}
```
