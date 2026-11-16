# Issue

**Title:** Disable EMA if the user inputs 0 for alpha

## Description

EMA for the losses during training should be disabled if the user sends a 0 for it. Currently LNNP will happily take this number and make it so that the losses are detached:
```python
        alpha = getattr(self.hparams, f"ema_alpha_{type}")
        if stage in ["train", "val"] and alpha < 1:
            ema = (
                self.ema[stage][type][loss_name]
                if loss_name in self.ema[stage][type]
                else loss.detach()
            )
            loss = alpha * loss + (1 - alpha) * ema
            self.ema[stage][type][loss_name] = loss.detach()
        return loss
```
Becomes just
```python
 loss = loss.detach()
```
when alpha is set to 0.

I added a conditional to skip EMA if alpha is not between 0 and 1.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `440f9859a02d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.