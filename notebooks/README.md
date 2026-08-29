# Experiment map

The folders preserve the full experiment set, including schedule comparisons and classical controls. Stored outputs remain visible for portfolio review.

> [!CAUTION]
> Each original training loop evaluates `test_loader` every epoch and selects its checkpoint using test accuracy. Metrics are historical development values. They require a validation-based rerun before they can support a fair comparison.

## Hybrid 512+1

| File | Schedule | Stored result | Notes |
|---|---:|---:|---|
| `warmup3_finetune15.ipynb` | 3+15 | 98.11%, AUC 0.9916 | Main reported run |
| `warmup3_finetune10.ipynb` | 3+10 | 97.79%, AUC 0.9917 | Schedule comparison |
| `warmup3_finetune7.ipynb` | 3+7 | 97.16% | Schedule comparison |
| `warmup2_finetune7.py` | 2+7 | See script output | Exported Python version |

This configuration retains all 512 ResNet features and adds one QNN output, so the classifier receives 513 features. Its high classical-to-quantum feature ratio makes it a hybrid implementation study rather than a strong test of quantum contribution.

## Hybrid 32+4

| File | Schedule | Stored result | Notes |
|---|---:|---:|---|
| `warmup3_finetune10.ipynb` | 3+10 | 97.63%, AUC 0.9955 | Most complete 32+4 run |
| `warmup2_finetune7.ipynb` | 2+7 | 97.00%, AUC 0.9955 | Original plot/output labels incorrectly say 3+10; source configuration is 2+7 |
| `warmup3_finetune7.ipynb` | 3+7 | 95.90%, AUC 0.9918 | Schedule comparison |
| `warmup3_finetune15.ipynb` | 3+15 | Incomplete stored evaluation | Do not use as a headline result |
| `quantum_variant.ipynb` | 3+15 | 98.26% stored best accuracy | Retained exploratory variant; generic checkpoint provenance makes it unsuitable as the headline result |

The 32 classical plus 4 quantum outputs match the 36-dimensional input of the classical control. This controls output width only; it does not equalise parameter count, optimisation, or representational capacity.

## Baselines

| File | Stored result | Purpose |
|---|---:|---|
| `classical_36_warmup3_finetune15.ipynb` | 97.79% | Matched 36-feature classical control |
| `resnet18_512_features.ipynb` | 97.48%, AUC 0.9959 | Full-feature ResNet18 baseline |
| `resnet18_no_warmup.ipynb` | 97.32%, AUC 0.9967 | No-warmup schedule control |

## Reproduction priorities

For a rigorous rerun, create patient-disjoint train, validation, and test sets; select checkpoints only on validation performance; evaluate the test set once; fix seeds; record package versions; and repeat each configuration across multiple seeds. Those steps are deliberately documented rather than retroactively changing the stored historical outputs.
