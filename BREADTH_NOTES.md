# Build notes - Biophysics & Cryo-EM

Repo2RLEnv pr_diff (v0.9.6), Harbor-format output. Domain: biophysics-cryoem.

## Funnel

| Stage | Count |
|---|---|
| Domain candidates (deduped) | 422 |
| After active+star+noise filter | 169 |
| Final target list (cap unlimited) | 169 |
| Repos with >=1 task | 74 |
| Repos with 0 tasks | 92 |
| Total tasks (unlimited (no per-repo cap)) | 5985 |

## Generation

- Shared task bank: each repo generated once (pr_diff, limit=3,
  max_files_per_pr=15, min_loc_changed=2), 4-way concurrency,
  rate-limit-aware with .done checkpoint resume.
- Per-domain consolidation: rank by reward_calibration.loc_changed desc,
  keep ALL qualifying tasks (no per-repo cap). Zero-yield repos are NOT fabricated.
- Overlap with the 3 published cancer editions is allowed (user decision);
  domains are independent selections, not a partition of one repo set.

## Zero-yield repos (informational, 92)

- BradyAJohnston/MolecularNodes
- CERN/TIGRE
- invesalius/invesalius3
- bioMate-AI/biomate-connector
- bjing2016/alphaflow
- SpikingChen/SNN-Daily-Arxiv
- odlgroup/odl
- PMEAL/porespy
- tomopy/tomopy
- ml-struct-bio/cryodrgn
- OpenChemistry/tomviz
- atomicarchitects/equiformer_v2
- Allen-Tildesley/examples
- simongravelle/lammps-input-files
- atomicarchitects/equiformer
- matteo-ronchetti/torch-radon
- pycroscopy/pycroscopy
- OpenEIT/OpenEIT
- llnl/LEAP
- eitcom/pyEIT
- wangqiannudt/nerf-arxiv-daily
- seto77/ReciPro
- sandialabs/pyGSTi
- 3dem/model-angelo
- Electrostatics/pdb2pqr
- ELEKTRONN/elektronn3
- pinder-org/pinder
- David-OConnor/molchanica
- TomographicImaging/CIL
- Electrostatics/apbs
- multiview-stitcher/multiview-stitcher
- geoffwoollard/learn_cryoem_math
- dkazanc/TomoPhantom
- LiberTEM/LiberTEM
- atomicarchitects/equiformer_v3
- bowang-lab/BioReason-Pro
- JeffFessler/MIRT.jl
- noegroup/ScoreMD
- 52CV/WACV-2024-Papers
- villekf/OMEGA
- cicwi/tomosipo
- fmagrini/seislib
- iszhanjiawei/flow_matching_arxiv_daily
- OJB-Quantum/QC-Hardware-How-To
- TLCFEM/suanPan
- DiamondLightSource/discorpy
- jnagy1/IRtools
- mrke/NicheMapR
- lujiarui/Str2Str
- ToFuProject/tofu
- tgraphite/reax_tools
- paiardin/DockingPie
- warpem/warp
- denysdovhan/voxel-viewer
- BioinfoMachineLearning/cryoppp
- BAMresearch/automatic-sem-image-segmentation
- westlake-repl/Cryo-IEF
- hyperspy/rosettasciio
- maxscheurer/pycontact
- algotom/algotom
- bytedance/cryostar
- ShenLab/SeqDance
- MenkeTechnologies/zsh-more-completions
- ComputationalCryoEM/ASPIRE-Python
- sai-advaith/guided_alphafold
- huhlim/cg2all
- nghia-vo/sarepy
- TomographicImaging/CCPi-Regularisation-Toolkit
- thorn-lab/coronavirus_structural_task_force
- zhtjtcz/Mine-Arxiv
- xin2zhang/MCTomo
- ml-struct-bio/drgnai
- instamatic-dev/instamatic
- ExcitedStates/qfit-3.0
- GeoscienceAustralia/hiperseis
- LucaDeSiena/MuRAT
- rigetti/forest-benchmarking
- RI-imaging/ODTbrain
- graik/biskit
- arttumiettinen/pi2
- jianlin-cheng/CryoTransformer
- ephonic/EDACraft
- DC-Jade/bioinfo_pi_atlas
- kylechampley/XrayPhysics
- Yusuftmle/FlyBrain-HalfLife
- YisiLuo/Continuous-Representation-Zoo
- michael-0brien/cryojax
- dkazanc/ToMoBAR
- NeuralEnsemble/neuroConstruct
- saadwazir/MCADS-Decoder
- rs-station/ROCKET
- mxhulab/cryosieve

## Caveats

- Static validation only; real grading needs an LLM judge key + Docker.
- inline_dockerfile mode skips image build (no Docker daemon needed to publish).
- Provider secrets in upstream diffs were redacted before public release.
