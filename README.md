# cnn-vit-robustness-protocols

Code, per-sample evaluation records, pre-registration documents and
verification scripts for

> C. Sahin and O. Yildirim, "Less a Weak Source than a Different One: Vulnerability, Similarity and Direction in CNN-ViT Adversarial Transfer under an Architecture-by-Initialization Design", submitted to IEEE Access, 2026.

## Summary

Adversarially trained ViTs look like weak transfer sources against CNNs, but
two models cannot split this asymmetry between source and target. The paper
factors log target-correct transfer rates into target vulnerability, symmetric
similarity and directional residual, averaged over a fixed panel of seven
robust ResNet-family CNNs, and tests registered predictions with two or three
runs per cell (brackets: 90% intervals over training runs, conditional on test
images and panel). On CIFAR-10, 0.936 [0.925, 0.948] of ViT-Tiny's panel
source gap to ResNet-18 is symmetric, and the ViT's directional term (+0.012)
is equivalent to zero within +/-0.10. In an architecture-by-initialization
design on a matched pipeline, the scratch-trained ViT is less similar to the
panel than the ResNet (-0.364 [-0.415, -0.314]), a contrast confounded with
its lower clean accuracy. ImageNet initialization narrows it more for the ViT
(+0.260); averaged over initialization, architecture leaves the log
directional term within +/-0.10 (+0.003). On CIFAR-100 the symmetric share
(0.840) replicates, but equivalence of the ViT's directional term is refuted
(+0.143). A small directional term holds only on CIFAR-10, the log scale and a
CNN panel; on the logit scale it is strongly negative.

All numbers are taken from `results/C1_REFERANS_FOYU.md`, which
`scripts/build_reference_sheet.py` generates from the records.

## Pre-registration

| Path | Content |
|---|---|
| `raporlar/konsey_2026-10-29/ON_KAYIT_2026-10-29.md` | Main registration (panel tests, two-by-two design, attack side, masking), frozen on 29 October 2026 before any of the new runs |
| `raporlar/konsey_2026-10-29/ON_KAYIT_EK_B8_B10_2026-10-30.md` | Addendum (CIFAR-100, ImageNetV2, gradient alignment), frozen on 30 October 2026 before any of its matrices was computed |
| `raporlar/konsey_2026-10-29/hucre_tablosu*.json` | Cell tables (which checkpoint fills which design cell) whose digests the analysis outputs record |
| `results/q1_research/ONKAYIT_SAPMALAR.md` | Deviation log, including the pre-run digests of the addendum scripts |
| `experiments/q1_konsey_ayrisim.py` | The frozen analysis script |
| `experiments/q1_ek_ayrisim.py`, `experiments/q1_ek_gradyan_kosinus.py`, `experiments/q1_ek_transfer_matrix.py` | Addendum wrapper, gradient script and matrix script, digests recorded before their first run |
| `experiments/q1_ek_masa.py` | Exploratory and sensitivity analyses computed after the results |
| `REGISTRATION_SHA256SUMS.txt` | Full-file digests of the files above at release time |

The registration documents are written in Turkish. In the development
repository they were committed at freezing (main registration: commit
e32f2ce, 29 October 2026; addendum: commit 7325419, 30 October 2026).

## Layout

| Path | Content |
|---|---|
| `results/c1_*` | CIFAR-10 campaign: per-sample clean/robust flags for every test image, model and seed; transfer, gradient, attention and depth-profile records; aggregated summaries |
| `results/q1/konsey_*` | Council round: panel decomposition (`konsey_ayrisim_b2.json`), two-by-two design (`b4`), attack side (`b5`), masking (`konsey_b6_maskeleme.json`), CIFAR-100 (`konsey_b9_c100/`), ImageNetV2 (`konsey_b8_imagenetv2/`), gradient alignment (`konsey_b10_gradyan/`), post-hoc analyses (`konsey_ek_masa/`) |
| `results/q1/` (other) | CIFAR-100 and SVHN replications of the protocol comparison, the L2 check, the checkpoint-selection ablation, pool fits and the machine-read Mahmood et al. (2021) tables |
| `results/C1_REFERANS_FOYU.md` | Generated reference sheet. Every number in the paper is taken from this file |
| `src/analysis/protokoller.py` | The single definition of the four conditioning protocols |
| `scripts/build_reference_sheet.py` | Regenerates the reference sheet from the records |
| `experiments/` | Evaluation and analysis drivers (matrix computation needs the checkpoints) |
| `src/`, `cli/`, `configs/` | Training, attack and evaluation code |
| `data/val_split_indices*.json` | The fixed validation split (seed 777), excluded from every training stage |

The per-sample `.npz` records of the council round (`results/q1/konsey_*` matrix directories, about 60 MB) are not in this repository; they are available from the authors on request and will be deposited on Zenodo with the published version. Without them the analysis outputs listed above can be checked against their stored digests, but the reference sheet cannot be rebuilt from scratch.

Seeds: ResNet-18 runs 1001, 1002, 1003; ViT-Tiny runs 2001, 2002, 2003;
validation split 777; AutoAttack and analysis sampling 42.

The reference sheet and some record keys are labelled in Turkish. Key terms:
`temiz` clean, `gurbuz` robust, `ham` raw (unconditioned), `hedef dogru`
target-correct, `her ikisi dogru` both-correct, `basarili kaynak`
successful-source, `kosum` training run, `cift` run pair, `tohum` seed,
`yayilim` spread, `ozdeslik` identity, `secim` checkpoint selection,
`on kayit` pre-registration, `ek` addendum, `sapma` deviation,
`hucre tablosu` cell table, `dondurulmus` frozen, `sarmalayici` wrapper.

## Reproduce the numbers

```bash
pip install -r requirements.txt    # numpy, scipy, pandas and matplotlib suffice for the analysis
sha256sum -c <(grep -v '^#' REGISTRATION_SHA256SUMS.txt)
python scripts/build_reference_sheet.py   # needs the full record set (see above)
git status --short results/C1_REFERANS_FOYU.md   # no change expected
```

`scripts/verify_manuscript_numbers.py` and `scripts/check_manuscript_claims.py`
check every carrier number and claim in the manuscript against these
records. They expect the manuscript sources under `paper/manuscript/`, which
are not part of this repository; they are included so that the checks can be
rerun once the paper is published.

## Not included

Trained checkpoints (about 40 GB) and the archived adversarial examples used
by the calibration runs and the council round. Both are available from the
authors on request; neither is needed to recompute the reported numbers from
the records.

## License

Code is released under the MIT License (see `LICENSE`). The evaluation
records and the generated reference sheet are released under CC BY 4.0.

## Citation

See `CITATION.cff`.
