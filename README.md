# TwinPose: Person-Specific Subspaces for Multi-View 3D Pose Estimation

## News

- **2026-06-08**: 🔗 Code is now available at [https://github.com/HYPER-THEORY/TwinPose](https://github.com/HYPER-THEORY/TwinPose).

- **2026-05-06**: 🎉 TwinPose has been accepted to **SIGGRAPH 2026 Journal Track** (ACM Transactions on Graphics)!


## Introduction


Following the success of deep neural networks in 2D pose estimation, reconstruction-based approaches have significantly advanced multi-person 3D pose estimation from sparse multi-view images. These methods typically detect 2D poses independently in each view and then associate them for 3D reconstruction. However, despite strong progress, recent state-of-the-art methods still face critical limitations: 1) They often depend on global optimization over a large and complex set of multi-view 2D joints to jointly infer 3D poses for all individuals, making the process highly complex and prone to suboptimal solutions; 2) Their tight coupling with the bottom-up detector OpenPose hinders the use of more advanced top-down or single-stage 2D pose estimators and restricts the integration of richer instance-level cues learned by these models.
<br><br>
To address these limitations, we propose **TwinPose**, a novel framework that alleviates the complexity of global pose inference by optimizing within **person-specific 3D pose subspaces**, while fully supporting diverse 2D pose detectors and effectively leveraging pose-instance cues. The key idea is to introduce a twin pose — a 3D counterpart of each 2D pose — that inherits its instance representation and aggregates geometrically consistent 2D joints from other views. All twin poses are unified in a common 3D space, where those belonging to the same individual naturally share a number of bones. This structural property enables association by counting shared bones, forming person-specific subspaces from which each individual’s 3D pose can be inferred independently in an efficient and robust manner.
<br><br>
Extensive experiments demonstrate that **TwinPose achieves state-of-the-art performance** in both accuracy and efficiency across multiple public and proprietary datasets. Importantly, it is **fully detector-agnostic**, allowing seamless integration with current and future advances in 2D pose estimation while remaining highly robust to noisy or imperfect 2D predictions.

![TwinPose](./static/images/twinpose.jpg)

## Quantitative Performance

with the fastest per-frame time (e.g., 0.92 ms on Shelf) and full flexibility to work with any 2D detector (e.g, HRNet, RTMO, and OpenPose).

**Quantitative comparison on the Shelf dataset.**

<table>
  <thead>
    <tr><th>Method</th><th>A1</th><th>A2</th><th>A3</th><th>Avg</th><th>Time (ms)</th></tr>
  </thead>
  <tbody>
    <tr><td>Tanke and Gall [2019]†#</td><td>99.8</td><td>90.0</td><td>98.0</td><td>96.0</td><td>N/A</td></tr>
    <tr><td>Bridgeman et al. [2019]†</td><td>99.3</td><td>91.6</td><td>97.6</td><td>96.2</td><td>9.1</td></tr>
    <tr><td>Dong et al. [2019]</td><td>98.8</td><td>94.1</td><td>97.8</td><td>96.9</td><td>90</td></tr>
    <tr><td>Chen et al. [2020a]</td><td>99.6</td><td>93.2</td><td>97.5</td><td>96.8</td><td>3.08</td></tr>
    <tr><td>Tu et al. [2020]*</td><td>99.3</td><td>94.1</td><td>97.6</td><td>97.0</td><td>333</td></tr>
    <tr><td>Huang et al. [2020]*</td><td>98.8</td><td>96.2</td><td>97.2</td><td>97.4</td><td>640</td></tr>
    <tr><td>Zhang et al. [2020]†</td><td>99.0</td><td>96.2</td><td>97.6</td><td>97.6</td><td>31.9</td></tr>
    <tr><td>Dong et al. [2021]</td><td>99.1</td><td>93.5</td><td>98.1</td><td>96.9</td><td>N/A</td></tr>
    <tr><td>Wang et al. [2021]*</td><td>99.3</td><td>95.1</td><td>97.8</td><td>97.4</td><td>~170</td></tr>
    <tr><td>Wu et al. [2021]</td><td>99.3</td><td>96.5</td><td>97.3</td><td>97.7</td><td>~48.8</td></tr>
    <tr><td>Reddy et al. [2021]†*</td><td>99.1</td><td>96.3</td><td>98.3</td><td>97.9°</td><td>&gt;333</td></tr>
    <tr><td>Lin and Lee [2021]</td><td>99.3</td><td>96.5</td><td>98.0</td><td>97.9</td><td>23.4</td></tr>
    <tr><td>Zhang et al. [2021]†</td><td>99.5</td><td><strong>97.0</strong></td><td>97.8</td><td>98.1</td><td>&gt;31.9</td></tr>
    <tr><td>Zhou et al. [2022]</td><td>99.5</td><td>96.7</td><td>98.2</td><td>98.1</td><td>2.94</td></tr>
    <tr><td>Choudhury et al. [2023]†*</td><td>99.0</td><td>96.3</td><td>98.2</td><td>97.8°</td><td>N/A</td></tr>
    <tr><td>Liao et al. [2024]*</td><td>99.5</td><td>96.8</td><td>97.8</td><td>98.0</td><td>210</td></tr>
    <tr><td><strong>TwinPose (Ours)</strong></td><td><strong>99.8</strong></td><td>96.2</td><td><strong>98.5</strong></td><td><strong>98.2</strong></td><td><strong>0.92</strong></td></tr>
  </tbody>
</table>


**Quantitative comparison on the 4DA dataset.**

<table>
  <thead>
    <tr><th>Method</th><th>2D Detector</th><th>Precision (%)</th><th>Recall (%)</th></tr>
  </thead>
  <tbody>
    <tr><td colspan="4"><em>Methods tightly coupled to the bottom‑up detector OpenPose</em></td></tr>
    <tr><td>Zhang et al. [2020]</td><td>OpenPose</td><td>88.5</td><td>90.2</td></tr>
    <tr><td>Dong et al. [2021]</td><td>OpenPose</td><td>90.1</td><td>89.0</td></tr>
    <tr><td>Zhou et al. [2022]</td><td>OpenPose</td><td>92.0</td><td>91.2</td></tr>
    <tr><td colspan="4"><em>Detector‑agnostic methods (any 2D pose detector)</em></td></tr>
    <tr><td>Dong et al. [2019]</td><td>OpenPose</td><td>78.5</td><td>77.1</td></tr>
    <tr><td>Dong et al. [2019]</td><td>HRNet</td><td>84.9</td><td>84.9</td></tr>
    <tr><td>Dong et al. [2019]</td><td>RTMO</td><td>85.4</td><td>85.5</td></tr>
    <tr><td><strong>TwinPose (Ours)</strong></td><td>OpenPose</td><td>91.4</td><td>90.4</td></tr>
    <tr><td><strong>TwinPose (Ours)</strong></td><td>HRNet</td><td>94.3</td><td>93.2</td></tr>
    <tr><td><strong>TwinPose (Ours)</strong></td><td>RTMO</td><td><strong>94.8</strong></td><td><strong>95.0</strong></td></tr>
  </tbody>
</table>

**Quantitative comparison on the Hi4D dataset.**

<table>
  <thead>
    <tr><th>Method</th><th>MPJPE ↓</th><th>PCP ↑</th><th>AP₅₀ ↑</th><th>AP₁₀₀ ↑</th><th>Recall ↑</th></tr>
  </thead>
  <tbody>
    <tr><td>Dong et al. [2019]</td><td>53.05</td><td>87.57</td><td>67.97</td><td>80.28</td><td>93.80</td></tr>
    <tr><td>Zhang et al. [2020]</td><td>41.29</td><td>88.62</td><td>80.87</td><td>97.27</td><td>98.78</td></tr>
    <tr><td>Lu et al. [2024a]</td><td>32.10</td><td>96.90</td><td><strong>91.48</strong></td><td>97.33</td><td>98.78</td></tr>
    <tr><td><strong>TwinPose (Ours)</strong></td><td><strong>22.00</strong></td><td><strong>99.71</strong></td><td>90.80</td><td><strong>99.34</strong></td><td><strong>99.86</strong></td></tr>
  </tbody>
</table>

## Qualitative Results

**Comparison with skeleton-level association method [Dong et al. 2019].** Traditional skeleton-level association approaches indiscriminately use all joints and bones, leading to incorrect associations (red boxes). TwinPose preserves only cross-view geometrically consistent joints, substantially improving robustness.

![TwinPose](./static/images/compare_MVPose.jpg)

**Comparison with the state-of-the-art 4DA method.** Global optimization in 4DA causes incorrect cross-person associations (red boxes). TwinPose performs person-specific inference in pose subspaces, enhancing both robustness and efficiency.

![TwinPose](./static/images/compare_4DA.jpg)

**Whole-body 3D pose estimation results of our method on the Panoptic dataset.** Results from eight camera views demonstrate consistent multi-view reconstructior
of body, hands, feet, and facial keypoints.

![TwinPose](./static/images/whole_body.jpg)

## Video Demo

For a detailed video demonstration, please see [this YouTube video](https://youtu.be/XLDARAOr0j0).
<div style="text-align: center;">
  <img 
    width="60%" 
    src="https://github.com/user-attachments/assets/e4b58e0f-7f7e-435e-9403-63a6f373e825"
  />
</div>


## Citations

If you find our paper useful in your research, please consider citing:

```bibtex
@article{yang2026twinpose,
  title         = {TwinPose: Person-Specific Subspaces for Multi-View 3D Pose Estimation},
  author        = {Yang, Wenwu and He, Tianyi and Ding, Jiwei and Wang, Xun and Zhang, Rong and Zhou, Kun},
  journal       = {ACM Transactions on Graphics},
  volume        = {45},
  number        = {4},
  articleno     = {61},
  year          = {2026},
  note          = {SIGGRAPH 2026 Journal Track}
}
```
