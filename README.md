# Coronary Stenosis Segmentation on the ARCADE Benchmark: A Comparative Study

Code, notebooks and result tables of the Bachelor thesis of Fahim Muntasir Tuhin (Department of Software Engineering, Daffodil International University, 2026). Supervisor: Dr. Md Fazla Elahe.

The thesis compares YOLOv8 / YOLOv8s-seg, U-Net, Attention U-Net, DeepLabV3+ (ResNet101) and a vessel-guided DeepLabV3+ (VG-DeepLab) on the ARCADE stenosis sub-task, scores them with one metric code, and adds an ablation, repeat runs with new seeds, a zero-shot test on CADICA and an explanation analysis.

All notebooks were run on Kaggle (1-2 NVIDIA Tesla T4, 16 GB; some older runs on Google Colab). Software: Python 3.13, PyTorch 2.11.0, Segmentation Models PyTorch 0.6.0, Ultralytics 8.4.212. The saved outputs in each notebook are the ones used in the thesis.

## Data (not included)
- ARCADE (stenosis and syntax sub-tasks): official release on Zenodo, record 10390295 (`arcade.zip`, MD5 `c5b1973ade06f7dff210f878161e1a76`). The notebooks read the public Kaggle copy `nirmalgaud/arcade-dataset`; `06_data_checks/checksum_kaggle_copy_vs_official_zenodo.ipynb` shows that all 1500 stenosis and 1500 syntax images are byte-identical to the official files.
- CADICA: released by its authors (see the thesis for the citation).
- Trained weights are not in this repository (large files); they are available from the author on request.

## Notebooks
Letters follow the thesis: B = DeepLabV3+ five-fold ensemble, N = VG-DeepLab, V = vessel head only, R = refinement head only, U = U-Net decoder on a pretrained ResNet101 encoder.

| Folder / notebook | What it does (thesis part) |
|---|---|
| `01_first_stage_baselines/yolov8_detection_v2`, `_v4` | YOLOv8 detection runs (raw and enhanced input) |
| `01_first_stage_baselines/yolov8s_seg_run_a`, `_run_b` | The two YOLOv8s-seg runs |
| `01_first_stage_baselines/unet_and_attention_unet_v2` | U-Net and Attention U-Net (deep supervision, Tversky loss) on the official 1000/200 split |
| `01_first_stage_baselines/deeplabv3plus_resnet101_single` | Single DeepLabV3+ on the official 1000/200 split |
| `02_main_B_and_N/B_deeplabv3plus_5fold_ensemble` | Five-fold training of B, OOF post-processing search, test scoring |
| `02_main_B_and_N/B_and_N_vg_deeplab_5fold` | Five-fold training of B and N (VG-DeepLab), OOF search, test scoring |
| `02_main_B_and_N/second_stage_verifier_not_used` | Segment-then-verify experiment; it lost on the OOF images and was not applied |
| `03_ablation_and_seed_repeats/ablation_V_...`, `ablation_R_...` | Five-fold training of V and R; re-scoring of B and N |
| `03_ablation_and_seed_repeats/seed_repeat_B/N/V_run*` | Repeat runs with new training seeds |
| `03_ablation_and_seed_repeats/pretrained_unet_control_U` | Control U: U-Net decoder with the same pretrained encoder |
| `04_evaluation_and_statistics/unified_evaluation_six_models` | Re-scoring of six models with one metric code, bootstrap intervals, paired tests |
| `04_evaluation_and_statistics/true_f1_common_operating_point_bonferroni` | Usual F1, common operating point, 99.67% Bonferroni intervals |
| `04_evaluation_and_statistics/oof_confidence_intervals_table_4_5` | Paired N-B intervals on the 1200 OOF images |
| `04_evaluation_and_statistics/three_checks_inference_only` | Matching rule, test-vs-OOF gap, single fold vs ensemble |
| `05_cadica_and_explainability/cadica_zero_shot_and_seg_gradcam_B_N` | Zero-shot CADICA test and Seg-Grad-CAM / vessel-map tests for B and N |
| `05_cadica_and_explainability/cadica_explainability_earlier_single_model` | Earlier version of the same test with one model |
| `06_data_checks/duplicate_check_1...`, `duplicate_check_2...` | Near-duplicate and shared-source checks (images only, no test scores) |
| `06_data_checks/checksum_...` | Kaggle copy vs official Zenodo file |

## Notebooks without saved outputs
The U-Net / Attention U-Net, single DeepLabV3+ and YOLOv8 detection v4 notebooks were saved without cell outputs. Their rendered runs are in `01_first_stage_baselines/rendered_outputs_pdf/` (PDF prints of the Colab runs).

## Results
`results/` holds the small tables written by the notebooks (CSV/JSON/PNG), one folder per run or notebook. Probability maps and weights are not included.

## How to run
Open a notebook on Kaggle, add the input `nirmalgaud/arcade-dataset`, select a T4 GPU and run all cells.  The CADICA notebooks need the CADICA data as an additional input; see the first cells of each notebook. Training the five folds takes 2.5-4.5 hours per model on one T4 (Section 3.12 of the thesis).

## Notes
- Some paths and logs contain the Kaggle user name of the author.
- The notebooks are kept as they ran, with their saved outputs.
- The ARCADE test images were scored only after the model, epoch and post-processing were fixed on other data; Table 3.5 of the thesis lists every notebook that scored the test set.

## Citation
Tuhin, F.M. (2026) *Coronary Stenosis Segmentation on the ARCADE Benchmark: A Comparative Study*. Bachelor thesis, Daffodil International University.
