# Defect Analysis – Hugging Face Transformers

## Setup

First create the virtual env.

``` shell
py -m venv .venv
pip install -r requirements.txt
```

Then clone the repo and generate the log file. This step can also be executed directly from  ```assignment.ipynb```

```shell
# Clone the transformers repository and checkout a specific version
git clone https://github.com/huggingface/transformers.git
cd transformers
git checkout v4.57.0
git tag --points-at HEAD

# Get git log of changes made after January 1, 2023
git log --name-only --pretty=format:"%ad - %an: %s" --after="2023-01-01" > ../git_log_output.txt
```

## Task 1

**Analyze these messages to detect the presence of specific keywords of your choice related to defect fixes.**

Our method for finding defective hotspots consist of checking for the substrings *fix*, *bug*, *error*, *issue*, *patch* in the commit message. Our check is case insensitive, and is not restricted to full words so a message which contains *Fix:* or *patched* would also count as defect.

**Calculate and plot the total number of defects per month. Why do you think the number of defects dropped sharply in October 2025? Why did defects drop sharply in October 2025?**

TODO Insert plot

The repository is checked out at release tag v4.57.0, whose latest commit in October 3rd. We can see that in October 2025 there is only one commit. This explains why there are no defects in October 2025.

**Calculate and plot the number of defects per month for the two files with the highest number of defects.**

TODO Insert plot

**In which month were the most defects introduced? How would you explain it? Manually examine the repository for that month (e.g., change logs, releases, commit messages) and come up with a hypothesis.**

March 2025 recorded the highest number of defect fixes (24 commits).
Historical data shows that March 2024 had an unusually high number of new features, many of which caused integration conflicts and latent bugs that reappeared a year later.

```log
MARCH 2025 DEFECT COMMITS

Mon Mar 31 23:31:24 2025 +0800 - cyyever: Fix more inefficient PT operations (#37060)
Mon Mar 31 17:02:49 2025 +0800 - huismiling: [MLU] Fix FA2 check error, remove deepspeed-mlu deps. (#36159)
Fri Mar 28 18:00:35 2025 +0100 - Cyril Vallez: Fix AttentionInterface following feedback (#37010)
Fri Mar 28 17:57:16 2025 +0100 - Cyril Vallez: Fix state_dict map location when quantized (#37086)
Fri Mar 28 16:36:44 2025 +0100 - Yih-Dar: fix tied weights issue (#37031)
Thu Mar 27 22:46:32 2025 +0800 - cyyever: Fix typing for None valued variables (#37004)
Wed Mar 26 16:24:57 2025 +0100 - Mohamed Mekkouri: Fix device_map check for ggml files (#37003)
Tue Mar 25 11:51:41 2025 +0100 - Marc Sun: Fix cuda index issue in cache allocator (#36937)
Tue Mar 25 10:43:27 2025 +0100 - Mohamed Mekkouri: Fixing _pre_quantization_dtype when torch_dtype is None (#36930)
Fri Mar 21 13:27:47 2025 +0100 - Raushan Turganbay: Fix: dtype cannot be str (#36262)
Fri Mar 21 10:11:47 2025 +0100 - Benjamin Bossan: FIX FSDP plugin update for QLoRA (#36720)
Thu Mar 20 11:55:47 2025 +0000 - Pavel Iakubovskii: Fix import for torch 2.0, 2.1 - guard typehint for "device_mesh" (#36768)
Tue Mar 18 18:46:03 2025 +0100 - Marc Sun: Fix casting dtype for quantization (#36799)
Fri Mar 14 17:36:02 2025 +0100 - Cyril Vallez: Fix post_init() code duplication (#36727)
Fri Mar 14 22:24:53 2025 +0900 - Sean (Seok-Won) Yi: Fix/best model checkpoint fix (#35885)
Thu Mar 13 21:47:35 2025 +0530 - Mehant Kammakomati: fix: fsdp sharded state dict won't work for save_only_model knob (#36627)
Thu Mar 13 22:27:50 2025 +0800 - wineandchord: fix type annotation for ALL_ATTENTION_FUNCTIONS (#36690)
Thu Mar 13 12:16:13 2025 +0100 - Marc Sun: Fix slicing for 0-dim param (#36580)
Wed Mar 12 11:40:46 2025 +0100 - Marc Sun: Fix bnb regression due to empty state dict (#36663)
Wed Mar 12 11:29:11 2025 +0100 - Arthur: fix block mask typing (#36661)
Mon Mar 3 18:35:37 2025 +0100 - Marc Sun: fix torch_dtype, contiguous, and load_state_dict regression (#36512)
Mon Mar 3 09:05:58 2025 -0500 - Zach Mueller: Fix loading zero3 weights (#36455)
Sun Mar 2 07:33:36 2025 +0000 - hlky: Fix _load_state_dict_into_meta_model with device_map=None (#36488)
Sat Mar 1 07:12:17 2025 +0100 - Marc Sun: Fix couple of issues from #36335 (#36453)
```

Based on these commit logs, the root cause of the spike in defects is not immediately clear without additional project context. However, cross-referencing the Hugging Face Transformers release history (PyPI: https://pypi.org/project/transformers/4.49.0/#history) provides useful insight:
v4.49.0 was released on February 17, 2025
v4.50.0 was released on March 21, 2025
Significant feature work and refactoring occurred between these two releases. The intense development period in late February and March likely introduced technical debt, which manifested as bugs soon after. Notably, 11 of the 24 fixes occurred after the v4.50.0 release, suggesting that the new release introduced regressions and integration issues that required immediate remediation. We assume thus the spike in March 2025 defects was primarily driven by heavy integration activity and major feature updates associated with the 4.50.0 release, leading to regressions and unstable components that required urgent post-release fixes.

**What are the limitations of this method for finding defective hotspots?**

While our method is very inclusive and flags many commits as defects, the main limitation is that there may be false positives. As we check for substrings, there may be messages that are flagged because of unintended words like *fixation* for example. Also, a commit may not actualy fix a bug, but includes in its message any of our selected words.

## Task 2

TBD

## Task 3

TBD

## Declaration of AI use

## References
