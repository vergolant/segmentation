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

This model is described in the technical report *[End-to-end speaker segmentation for overlap-aware resegmentation](paper/report.pdf)*, by Hervé Bredin and Antoine Laurent.

![Example](example.png)

## Citation

If you use this model for academic research, please consider citing the `pyannote.audio` library:

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

## Support

If you (would like to) use this model in commercial products and need help to make the most of it, please contact [me](mailto:herve@niderb.fr).

## Requirements

This model relies on `pyannote.audio` 2.0 (which is still in development as of April 2nd, 2021):

```bash
$ pip install https://github.com/pyannote/pyannote-audio/archive/develop.zip
```

## Basic usage

```python
from pyannote.audio import Inference
inference = Inference("pyannote/segmentation")
segmentation = inference("audio.wav")
# `segmentation` is a pyannote.core.SlidingWindowFeature
# instance containing raw segmentation scores like the 
# one pictured above (output)

from pyannote.audio.pipelines import Segmentation
pipeline = Segmentation(segmentation="pyannote/segmentation")
HYPER_PARAMETERS = {
  # onset/offset activation thresholds
  "onset": 0.5, "offset": 0.5,
  # remove speaker turn shorter than that many seconds.
  "min_duration_on": 0.0,
  # fill within speaker pauses shorter than that many seconds.
  "min_duration_off": 0.0
}

pipeline.instantiate(HYPER_PARAMETERS)
segmentation = pipeline("audio.wav")
# `segmentation` now is a pyannote.core.Annotation
# instance containing a hard binary segmentation 
# like the one picutred above (reference)
```


## Advanced usage

### Voice activity detection

```python
from pyannote.audio.pipelines import VoiceActivityDetection
pipeline = VoiceActivityDetection(segmentation="pyannote/segmentation")
pipeline.instantiate(HYPER_PARAMETERS)
vad = pipeline("audio.wav")
```

In order to reproduce results of the [technical report](paper/report.pdf), one should use the following hyper-parameter values:

Dataset         | `onset` | `offset` | `min_duration_on` | `min_duration_off`
----------------|---------|----------|-------------------|-------------------
AMI Mix-Headset | 0.851   | 0.430    | 0.115             | 0.146
DIHARD3         | 0.855   | 0.292    | 0.036             | 0.001
VoxConverse     | 0.883   | 0.688    | 0.106             | 0.526

We also provide the [expected output](tree/main/paper/expected_outputs/vad) on those three datasets in RTTM format.

### Overlapped speech detection

```python
from pyannote.audio.pipelines import OverlappedSpeechDetection
pipeline = OverlappedSpeechDetection(segmentation="pyannote/segmentation")
pipeline.instantiate(HYPER_PARAMETERS)
osd = pipeline("audio.wav")
```

In order to reproduce results of the [technical report](paper/report.pdf), one should use the following hyper-parameter values:

Dataset         | `onset` | `offset` | `min_duration_on` | `min_duration_off`
----------------|---------|----------|-------------------|-------------------
AMI Mix-Headset | 0.552   | 0.311    | 0.131             | 0.180
DIHARD3         | 0.564   | 0.264    | 0.158             | 0.080
VoxConverse     | 0.617   | 0.387    | 0.367             | 0.334

We also provide the [expected output](tree/main/paper/expected_outputs/osd) on those three datasets in RTTM format.

### Resegmentation

```python
from pyannote.audio.pipelines import Resegmentation
pipeline = Resegmentation(segmentation="pyannote/segmentation", 
                          diarization="baseline")
pipeline.instantiate(HYPER_PARAMETERS)
```

In order to reproduce (VBx) results of the technical report, one should use the following hyper-parameter values:

Dataset         | `onset` | `offset` | `min_duration_on` | `min_duration_off`
----------------|---------|----------|-------------------|-------------------
AMI Mix-Headset | 0.542   | 0.527    | 0.044             | 0.705
DIHARD3         | 0.592   | 0.489    | 0.163             | 0.182
VoxConverse     | 0.537   | 0.724    | 0.410             | 0.563



[VBx RTTM files](tree/main/paper/expected_outputs/vbx) are also provided in this repository for convenience:

```python
from pyannote.database.utils import load_rttm
vbx = load_rttm("paper/expected_outputs/vbx/DIHARD.rttm")
resegmented_vbx = pipeline({"audio": "DH_EVAL_000.wav", 
                            "baseline": vbx["DH_EVAL_000"]})
```


We also provide the [expected output](tree/main/paper/expected_outputs/rsg) on those three datasets in RTTM format.

