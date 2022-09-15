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

This branch contains a checkpoint of `pyannote/segmentation@2022.07` finetuned on `VoxConverse 0.3` development set.

```python
from pyannote.audio import Model
model = Model.from_pretrained("pyannote/segmentation@VoxSRC2022")
```
