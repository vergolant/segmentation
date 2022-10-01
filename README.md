---
tags:
- pyannote
- pyannote-audio
- pyannote-audio-model
- speaker-segmentation
license: mit
inference: false
---

This branch contains a checkpoint of `pyannote/segmentation@2022.07` finetuned on `3/24 TV channel` (whole), `Aragon Radio` (whole), and `RTV2018` (development and test) sets.

```python
from pyannote.audio import Model
model = Model.from_pretrained("pyannote/segmentation@Albayzin2022")
```

