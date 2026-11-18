# Pseudodynamic Minimaps

Reproducibility package for the paper *Pseudodynamic Minimaps: Encoding Pen Dynamics in
Handwriting Images for Parkinson's Disease Classification*.

A pseudodynamic minimap renders an online handwriting recording as a single image: the planar
coordinates place each point, and colour carries the pen dynamics — speed, pressure, altitude
and azimuth — along the stroke that produced them. The paper evaluates the representation on
the PaHaW dataset under participant-separated cross-validation.

**Status.** Accepted as a short paper at the International Conference on Computer Vision
Theory and Applications (VISAPP), 2027. The camera-ready is in preparation; the full citation
and DOI will be added here once the proceedings are published.

## Authors

| Author | Affiliation | ORCID |
|---|---|---|
| Munif Gebara Jr. | Universidade Estadual de Maringá, Brazil | [0009-0007-4773-5468](https://orcid.org/0009-0007-4773-5468) |
| Ángel Sánchez | Universidad Rey Juan Carlos, Spain | [0000-0001-9069-6985](https://orcid.org/0000-0001-9069-6985) |
| Yandre Maldonado e Gomes da Costa | Universidade Estadual de Maringá, Brazil | [0000-0002-0630-3171](https://orcid.org/0000-0002-0630-3171) |
| Igor Scaliante Wiese | Universidade Tecnológica Federal do Paraná, Brazil | [0000-0001-9943-5570](https://orcid.org/0000-0001-9943-5570) |
| André Felipe Ribeiro Cordeiro | Universidade Estadual de Maringá, Brazil | [0000-0001-6719-2826](https://orcid.org/0000-0001-6719-2826) |
| Jamily G. S. Souza | Universidade Estadual de Maringá, Brazil | [0009-0009-2634-8965](https://orcid.org/0009-0009-2634-8965) |

## What is in this repository

- **`experimental/`** — the Python package `pdhms_restart`, the run and verification scripts
  for the ten experimental stages, their JSON configurations and protocol decisions
  (`DECISION.md`), the unit tests, the pinned environment, and `manifest.json`, which records
  a SHA-256 digest for every source file.
- **`aggregate_results/`** — the cohort-level CSVs behind the reported tables: per-task and
  global summaries, paired method comparisons with bootstrap intervals and Holm-adjusted
  p-values, and how often each encoding was selected inside training.
- **`scripts/`** — the manuscript figure generators.
- **`PACKAGING.txt`** — the note that accompanied the original archive.

## What is deliberately **not** in this repository

- **The PaHaW recordings.** They are not ours to redistribute. PaHaW must be obtained from its
  owners under their own access terms; see Drotár et al. (2016), *Evaluation of handwriting
  kinematics and pressure for differential diagnosis of Parkinson's disease*, Artificial
  Intelligence in Medicine 67:39–46.
- Participant-level predictions, model checkpoints, attribution maps and saved run outputs.
  Only aggregate results are published here.
- The manuscript sources and the published PDF.

## Reproducing the experiments

Read **`experimental/README.md`** first. It gives the data requirements, the stage order with
the exact commands, the verification step for each stage, and the limits of what a rerun
establishes. In short: Python 3.13, `pip install -r experimental/requirements-lock.txt`, then
`pip install -e "experimental[cnn]"` if you need the convolutional stages.

**On the absolute paths in the configurations.** The archived `config.json` files keep the
authors' original local `dataset_root` values, such as
`C:/Users/munif/PycharmProjects/.../dataset/PaHaW`. This is deliberate: `manifest.json` records
a SHA-256 for each file, so what is published here is byte-identical to what was actually
executed. Copy the `experimental` directory and edit the copy, as step 4 of
`experimental/README.md` describes. Do not edit the archived originals in place.

## Headline result

Experiment 3, participant-level majority vote over the three repeated-pattern tasks (T2–T4),
averaged over five repetitions of five-fold participant-separated cross-validation, with every
choice made inside training:

| Pipeline | Participant-level macro F1 | 95% CI | Accuracy |
|---|---:|---|---:|
| Static trace + LPQ + SVM | 0.6143 | [0.5194, 0.7049] | 61.60% |
| **Minimaps + LPQ + SVM** | **0.6449** | [0.5502, 0.7340] | **64.80%** |
| Kinematic/pressure + SVM | 0.6279 | [0.5409, 0.7106] | 62.93% |
| Minimaps + ResNet18 features | 0.6021 | [0.5136, 0.6873] | 60.27% |

The margin of the minimap pipeline over the static trace is 0.0307, with a 95% participant
bootstrap interval of [−0.0107, 0.0748] that includes zero. No contrast between encodings
survives multiplicity correction on a cohort of 75 participants. The paper presents the
representation as an explicit way to carry pen dynamics into image-based handwriting analysis,
not as a demonstrated improvement.

## What this package does and does not establish

The ten stages contain the implementation and configuration needed to rerun the reported
sequence once the data and environment are provided. Source packaging was checked by syntax
parsing, import resolution and byte-for-byte hashes. Those checks are not themselves another
experimental rerun, and they do not establish bitwise reproducibility on other hardware or on
an external cohort.

## Citation

See `CITATION.cff`. Until the proceedings appear, please cite the paper as accepted at
VISAPP 2027 and link to this repository.

## License

MIT — see `LICENSE`. The PaHaW dataset is not covered by this licence and is not distributed
here.
