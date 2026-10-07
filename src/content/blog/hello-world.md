---
title: 'Hello, world'
description: 'First post: checking that math and code render.'
pubDate: 'Oct 06 2026'
---

This is the first post on the blog. It mostly exists to check that everything renders.

## Math

Inline math works with single dollar signs, like $\nabla_\theta \mathcal{L}(\theta)$, and display math with double:

$$
\mathrm{softmax}(z)_i = \frac{e^{z_i}}{\sum_{j=1}^{K} e^{z_j}}
$$

## Code

```python
import torch
import torch.nn.functional as F


def cross_entropy(logits: torch.Tensor, targets: torch.Tensor) -> torch.Tensor:
    log_probs = F.log_softmax(logits, dim=-1)
    return -log_probs.gather(-1, targets.unsqueeze(-1)).mean()
```
