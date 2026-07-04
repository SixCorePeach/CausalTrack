# CausalTrack
This is the code for our paper CausalTrack: 

这是我在目标跟踪任务的第一篇论文，之后我会疯狂更新关于论文的内容。加油

# UAVDT dataset
The dataset could use in the single object tracking task now, but lack of the download.
So I download the dataset in baidu Pan, the link is 
通过网盘分享的文件：UAVDT2018
链接: https://pan.baidu.com/s/1s5TlfDZ5wT0anvYCyWI8og?pwd=DT20 提取码: DT20 
--来自百度网盘超级会员v1的分享
免责声明 该数据集来源是 https://datasetninja.com/uavdt
如果需要下载该数据集，并且该数据集会在你的论文中引用，
以下是论文信息

@dataset{Uavdt,
  author={Dawei Du and Yuankai Qi and Hongyang Yu and Yifan Yang and Kaiwen Duan and Guorong Li and Weigang Zhang and Qingming Huang and Qi Tian},
  title={UAVDT Dataset},
  year={2018},
  url={https://sites.google.com/view/grli-uavdt/%E9%A6%96%E9%A1%B5}
}


## Design Philosophy of HiSALT

HiSALT is designed for aerial vision-language tracking, where visual evidence is often weak or ambiguous due to small targets, fast motion, occlusion, background clutter, and visually similar distractors. Instead of treating language as a single global sentence embedding, HiSALT represents language descriptions as compact guide tokens and injects them into transformer tracking features in a sparse and localization-aware manner.

The key motivation is that language descriptions in vision-language tracking are usually short, but not necessarily semantically simple. A short sentence may still contain multiple useful cues, such as target identity, attributes, state, spatial relation, and surrounding context. Therefore, the main problem is not long-text understanding, but how to selectively use compact and semantically mixed language cues during tracking.

HiSALT follows three principles:

1. **Language as Guide Tokens**  
   Raw descriptions or L1/L2/L3 hierarchical descriptions are encoded by a frozen CLIP text encoder and projected into the visual token space. This produces a small set of language guide tokens that can be cached and reused during inference.

2. **Sparse Semantic Injection**  
   Visual tokens query language guide tokens through a sparse gating mechanism. Instead of densely fusing the entire sentence representation into all visual tokens, HiSALT allows each visual token to selectively interact with relevant language cues. This reduces redundant cross-modal interaction and keeps the visual tracking representation stable.

3. **Localization-Oriented Search Token Refinement**  
   Since the tracking head mainly relies on search-region tokens, HiSALT refines language-enhanced search tokens with a local-global hybrid adapter. The global branch performs token-wise semantic adaptation, while the local convolutional branch restores spatial neighborhood consistency for accurate localization.

The overall pipeline is:

```text
Template Image + Search Image
        ↓
One-stream Transformer Backbone
        ↓
Template-Search Visual Tokens

Raw / L1 / L2 / L3 Text
        ↓
Frozen CLIP Text Encoder
        ↓
Projection to Visual Token Space
        ↓
Language Guide Tokens

Visual Tokens + Language Guide Tokens
        ↓
Sparse Semantic Injection
        ↓
Language-enhanced Visual Tokens
        ↓
Search Token Refinement
        ↓
Local-Global Hybrid Adapter
        ↓
Tracking Head
        ↓
Bounding Box Prediction
