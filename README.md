
# (CVPR 2025)Tightening Robustness Verification of MaxPool-based Neural Networks via Minimizing the Over-Approximation Zone
## Setup Instructions

We propose Ti-Lin that, in certain cases, outperforms the 2024 MaxLin(https://github.com/xiaoyuanpigo/maxlin) approach. Based on this, we introduce Hybrid-Lin, which combines the strengths of both  MaxLin and  Ti-Lin. Experimental results demonstrate that Hybrid-Lin generally performs better than MaxLin. Therefore, for more precise robustness verification results, we recommend using Hybrid-Lin, which we have implemented in Alpha-Beta-CROWN (https://github.com/Verified-Intelligence/alpha-beta-CROWN).

## Run Certification

Please also install Gurobi, its associated python library gurobipy and its license in order to run LP/Dual-LP methods.
```bash
conda config --add channels http://conda.anaconda.org/gurobi
conda install gurobi
```

To test Ti-Lin on CNN-Cert framework [1]:
```bash
git clone https://github.com/AkhilanB/CNN-Cert.git
cd ..
cp -f cnn_bounds_full_core_tilin.py CNN-Cert/cnn_bounds_full_core.py
```

To test Ti-Lin on ERAN, DeepPoly framework (https://github.com/eth-sri/eran) or on 3DCertify framework(https://github.com/eth-sri/3dcertify.git)
```bash
git clone --recurse-submodules https://github.com/eth-sri/3dcertify.git
cd ..
cp -f verify_perturbation.py 3dcertify/verify_perturbation.py 
cp -f elina_tilin.c 3dcertify/ERAN/ELINA/fppoly/pool_approx.c
cd 3dcertify/ERAN/ELINA/
make all
cd ../../..
```



To test Ti-Lin on $\alpha,\beta$-CROWN framework (https://github.com/Verified-Intelligence/alpha-beta-CROWN)
By default, the configuration of Alpha-Beta-CROWN does not recompute the lower and upper bounds (L and U) for ReLU outputs. Instead, to improve efficiency, it uses IBP (Interval Bound Propagation) for these calculations, relying on Alpha-Beta-CROWN’s relaxation methods for ReLU and MaxPool. While this approach generally does not affect soundness, it can impact soundness when using Ti-Lin, MaxLin, or Hybrid-Lin.

Before test Ti-Lin, MaxLin, or Hybrid-Lin on $\alpha,\beta$-CROWN, to ensure soundness, the names of the ReLU layers before MaxPool need to be added to the solver: invprop: directly_optimize list in the configuration.

Ti-Lin's implementation in $\alpha,\beta$-CROWN:
```bash
git clone https://github.com/Verified-Intelligence/alpha-beta-CROWN.git
cd ..
cp -f auto_lirpa_tilin.py alpha-beta-CROWN/complete_verifier/auto_LiRPA/operators/pooling.py
```


Hybrid-Lin's implementation in $\alpha,\beta$-CROWN:
```bash
git clone https://github.com/Verified-Intelligence/alpha-beta-CROWN.git
cd ..
cp -f auto_lirpa_hybridlin.py alpha-beta-CROWN/complete_verifier/auto_LiRPA/operators/pooling.py
```

## Benchmarks
Download the CNN models used in the paper. We use CNN-Cert(https://www.dropbox.com/s/mhe8u2vpugxz5ed/models.zip), 3DCertify(https://files.sri.inf.ethz.ch/pointclouds/pretrained-models.zip), VNN-COMP2021 (accessed in https://github.com/stanleybak/vnncomp2021)and ERAN (accessed in https://github.com/eth-sri/eran) benchmarks.
Some models(.h5) need to be transformed into other typt(.pb) to be ceritified by ERAN.  Vnnlib files and the transformed models we provide in the supplementary materials.




## Citation
If you find our work helpful, please consider citing 

```bash
@inproceedings{xiao2025tightening,
  title={Tightening Robustness Verification of MaxPool-based Neural Networks via Minimizing the Over-Approximation Zone},
  author={Xiao, Yuan and Chen, Yuchen and Ma, Shiqing and Fang, Chunrong and Bai, Tongtong and Gu, Mingzheng and Cheng, Yuxin and Chen, Yanwei and Chen, Zhenyu},
  booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition},
  year={2025}
}
```

