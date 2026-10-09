# Structured Sensory Redundancy

Code, analysis notebooks, and trained policy checkpoints for:

**Extensive but Anatomically Structured Sensory Redundancy in a Feedforward Bipedal Locomotion Policy**
Kenshi Obaru, rehabiliworld, Kumamoto City, Japan
*Frontiers in Robotics and AI — Humanoid Robotics* (under review)

## Abstract

Motor-output ablation studies of trained locomotion policies have identified small numbers of disproportionately critical network units, but the analogous question for sensory input — how much proprioceptive loss a policy tolerates, and whether this loss is evenly distributed — has received less systematic, cross-seed-replicated attention. Using the Unitree G1 humanoid, we applied joint-state ablation (zeroing joint-position and joint-velocity observations, leaving motor output untouched) across widening conditions in eight independently trained policies. Single-joint and small paired-joint ablations never produced a fall within the 5.0-second evaluation horizon. Ablating an entire body half (13 of 29 joints) or the trunk's 3 joints alone also produced no falls. Combining half-body with trunk loss (16 joints) sharply increased fall rate, depending on anatomical identity rather than joint count: ten random 16-joint subsets yielded fall proportions from 0.0% to 93.1% (mean 33.9%, SD 37.8%). Right-half-plus-trunk loss produced falls in 65.3% of trials versus 19.4% for the mirror-image loss (consistent in direction in 7 of 8 seeds); repeating across five additional initial conditions attenuated this difference (49.4% vs. 43.1%, overlapping CIs), with only the direction, not the magnitude, preserved. Freezing rather than zeroing produced no falls in either 16-joint condition, showing the transition is specific to zero-substitution. The asymmetry did not persist at a further-degraded 19-joint condition; removing 22 or more of 29 joints' proprioception was almost uniformly fatal (98.6-100%). The 12 single-joint ablations tested caused only minor (~10-15%) degradation, not a floor effect. At the 16-joint condition, terminal joint angles diverged substantially from default — by approximately 25° for ablated joints and 16° for intact joints, versus approximately 3° during normal walking — indicating an active, evolving failure rather than the passive collapse-to-default pattern previously found for motor-output loss. These results show that joint-state-ablation tolerance depends on anatomical identity rather than joint count — an entire body half is tolerated, but little more — and that a left-right asymmetry near this transition is consistent in direction, not magnitude, across initial conditions.

## Repository contents

```
notebooks/
  g1_sensory_ablation.ipynb           Main analysis notebook (Colab). Setup, observation-vector
                                       verification, sensory-ablation implementation, single-joint
                                       and paired-combination screening, whole-body and half-body/
                                       trunk sweeps, 16/19/22-joint boundary characterization,
                                       failure-pattern (mechanism) analysis, and the full
                                       set of reviewer-response experiments (intact baseline,
                                       continuous performance metrics, repeated initial conditions,
                                       observation-normalization check, previous-action control,
                                       alternative fault models, joint-count-matched random subsets,
                                       gait characterization, disturbance robustness, mixed-effects
                                       logistic regression, initial-condition/mirrored-state
                                       diversification, extended pre-fall trajectory logging).
                                       NOTE: the failure-pattern analysis in this notebook predates the
                                       measurement correction and is superseded by
                                       g1_sensory_ablation4_with_figure4_fix.ipynb.
 g1_reviewer2_gap_experiments_v5 (2).ipynb
                                       Supplementary notebook: heading-change experiment and
                                       mirrored-state test (reviewer-requested gap analyses).
                                       Section A (default-posture check extended to 8 seeds) is
                                       superseded by the corrected analysis in
                                       g1_sensory_ablation4_with_figure4_fix.ipynb; it is retained
                                       for transparency only.
  g1_sensory_ablation4_with_figure4_fix.ipynb
                                       Re-measurement of the Section 3.6 failure-pattern analysis
                                       (terminal state captured before the environment's automatic
                                       reset) and regeneration of Figure 4. Includes executed outputs.
checkpoints/
  g1_velocity_seed1_model_2999.pt     Trained PPO policy checkpoints (8 independently trained
  ...                                 seeds, one file per seed) used throughout the
  g1_velocity_seed8_model_2999.pt     sensory-ablation experiments.
results/
  *.csv                               Result tables generated by the notebooks (fall rates,
                                       continuous performance metrics, boundary sweeps, etc.).
LICENSE
README.md
```

## Environment

Experiments were run on Google Colab (Tesla T4 GPU) using:

- `mjlab==1.2.0` (MuJoCo-Warp-based RL framework)
- `mujoco==3.5.0`
- `warp-lang==1.12.1`
- Python, PyTorch, `rsl-rl-lib`, `pandas`, `numpy`, `scipy`

The task is `Mjlab-Velocity-Flat-Unitree-G1`. Policies are purely feedforward
multilayer-perceptron actors (hidden layers 512/256/128) trained via proximal
policy optimization (PPO); sensory ablation zeroes the joint-position and
joint-velocity entries of the 99-dimensional observation vector for specified
joints at every control step, without modifying policy weights or motor output.

## Reproducing the experiments

1. Open a notebook in Google Colab (or a local Jupyter environment with a GPU).
2. Place the checkpoints under `checkpoints/` (or adjust the `logs/` path used
   in the notebook's setup cell).
3. Run cells in order — each experiment section is self-contained and
   resume-safe (results are appended to CSV files as trials complete, so a
   section can be re-run without repeating already-completed trials).
   
**Note on numerical reproducibility.** GPU simulation is not bit-wise deterministic. Re-running the failure-pattern analysis yields values that differ slightly from those in the manuscript (e.g., mean deviation from default 25.8° / 15.3° / 2.7° vs. ≈25° / ≈16° / ≈3°; maximum tracking error 205.6° vs. 208.9°), without changing the qualitative result.

## Companion paper

This is the sensory-ablation companion to a motor-output ablation study on the
same policy family:

Obaru, K. "A Single Critical Output Unit in a Feedforward Bipedal Locomotion
Policy: Cross-Seed Replication and Mechanism Analysis in the Unitree G1
Humanoid." *Frontiers in Robotics and AI* (under review).

## Citation

A full citation (with DOI) will be added here once the paper is published.
In the meantime, please cite the manuscript as "in preparation" / "under review"
at *Frontiers in Robotics and AI*.

## License

Code and checkpoints are released under the MIT License (see `LICENSE`).

## Contact

Kenshi Obaru — research@rehabiliworld.com
