---
tags:
- pyannote
- pyannote-audio
- pyannote-audio-model
- speaker-segmentation
datasets:
- voxconverse
license: mit
inference: false
---

This branch contains a checkpoint of `pyannote/segmentation@2022.07` finetuned on `Ego4D V1` training set.

```python
from pyannote.audio import Model
model = Model.from_pretrained("pyannote/segmentation@Ego4D2022")
```

