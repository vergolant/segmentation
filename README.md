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

# pyannote.audio // speaker segmentation

This model relies on `pyannote.audio` 2.0 (which is still in development):

```bash
$ pip install https://github.com/pyannote/pyannote-audio/archive/develop.zip
```

## Basic inference

```python
>>> from pyannote.audio import Inference
>>> inference = Inference("pyannote/segmentation")
>>> segmentation = inference("audio.wav")
```

## Advanced pipelines

### Voice activity detection

```python
>>> from pyannote.audio.pipelines import VoiceActivityDetection
>>> HYPER_PARAMETERS = {"onset": 0.5, "offset": 0.5, "min_duration_on": 0.0, "min_duration_off": 0.0}
>>> pipeline = VoiceActivityDetection(segmentation="pyannote/segmentation")
>>> pipeline.instantiate(HYPER_PARAMETERS)
>>> vad = pipeline("audio.wav")
```

In order to reproduce results of the paper, one should use the following hyper-parameter values:

Dataset         | `onset` | `offset` | `min_duration_on` | `min_duration_off`
----------------|---------|----------|-------------------|-------------------
AMI Mix-Headset | 0.851   | 0.430    | 0.115             | 0.146
DIHARD3         | 0.855   | 0.292    | 0.036             | 0.001
VoxConverse     | 0.883   | 0.688    | 0.106             | 0.526

### Overlapped speech detection

```python
>>> from pyannote.audio.pipelines import OverlappedSpeechDetection
>>> pipeline = OverlappedSpeechDetection(segmentation="pyannote/segmentation")
>>> pipeline.instantiate(HYPER_PARAMETERS)
>>> osd = pipeline("audio.wav")
```

In order to reproduce results of the paper, one should use the following hyper-parameter values:

Dataset         | `onset` | `offset` | `min_duration_on` | `min_duration_off`
----------------|---------|----------|-------------------|-------------------
AMI Mix-Headset | 0.552   | 0.311    | 0.131             | 0.180
DIHARD3         | 0.564   | 0.264    | 0.158             | 0.080
VoxConverse     | 0.617   | 0.387    | 0.367             | 0.334


### Segmentation

```python
>>> from pyannote.audio.pipelines import Segmentation
>>> pipeline = Segmentation(segmentation="pyannote/segmentation")
>>> pipeline.instantiate(HYPER_PARAMETERS)
>>> seg = pipeline("audio.wav")
```
In order to reproduce results of the paper, one should use the following hyper-parameter values:


Dataset         | `onset` | `offset` | `min_duration_on` | `min_duration_off`
----------------|---------|----------|-------------------|-------------------
AMI Mix-Headset | 0.784   | 0.661    | 0.127             | 0.003
DIHARD3         | 0.848   | 0.495    | 0.056             | 0.000 
VoxConverse     | 0.882   | 0.779    | 0.304             | 0.484

### Resegmentation

```python
>>> from pyannote.audio.pipelines import Resegmentation
>>> pipeline = Resegmentation(segmentation="pyannote/segmentation", 
...                           diarization="baseline")
>>> pipeline.instantiate(HYPER_PARAMETERS)
```

VBx RTTM files are also provided in this repository for convenience:

```python
>>> from pyannote.database.utils import load_rttm
>>> vbx = load_rttm("/path/to/vbx.rttm")
>>> resegmented_vbx = pipeline({"audio": "DH_EVAL_000.wav", 
...                             "baseline": vbx["DH_EVAL_000"]})
```

In order to reproduce (VBx) results of the paper, one should use the following hyper-parameter values:

Dataset         | `onset` | `offset` | `min_duration_on` | `min_duration_off`
----------------|---------|----------|-------------------|-------------------
AMI Mix-Headset | 0.542   | 0.527    | 0.044             | 0.705
DIHARD3         | 0.592   | 0.489    | 0.163             | 0.182
VoxConverse     | 0.537   | 0.724    | 0.410             | 0.563

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
