# DALL-E Mega Model Card

**This Model Card is a simplified version of various resources from the WandB Reports**

[Project Report](https://wandb.ai/dalle-mini/dalle-mini/reports/DALL-E-mini-Generate-images-from-any-text-prompt--VmlldzoyMDE4NDAy)

[Demo](https://huggingface.co/spaces/dalle-mini/dalle-mini)

# Model Description

DALL-E Mega is a simplified replication of the original DALL-E training in the open. DALL-E Mega has improved from DALL-E Mini in several key ways. 

* Optimizer updated to Distributed Shampoo which proved to be more efficient following a comparison of different optimizers﻿

* A new architecture based on NormFormer and GLU variants following a comparison of transformer variants, including DeepNet, Swin v2, NormFormer, Sandwich-LN, RMSNorm with GeLU/Swish/SmeLU

* We use super conditioning which affects FID and CLIP score (see Pareto curves)

* Improvements over the dataset with CLIP score exploration

The model works by using a BART Encoder Model that DALL-E Mega uses to understand the text prompt. Another larger BART model then decodes it into VQGAN Tokens which are lastly decoded into images you can display.

# Training

The Simplified Procedure is as followed.

* Hardware: 1 pod TPU v3-256 = 32 nodes of TPU VM v3-8 (8 TPU per node) = 256 TPU v3

* Optimizer: Distributed Shampoo

* Model Partition Spec: 8 model parallel x 32 data parallel

* Batch: 44 samples per model x 32 data parallel x 3 gradient accumulation steps =  4224 samples per update

* Learning rate: warmup to 0.0001 for 10,000 steps and then kept constant until plateau

* Gradient checkpointing used on each Encoder/Decoder layer (ie, MHA + FFN)

* Distributed Shampoo + Normformer Optimizations have proved to be effective and efficiently scaling this model. 

It should also be noted that the learning rate and other parameters are sometimes adjusted on the fly.

[The Full Procedure and Technical Material](https://wandb.ai/dalle-mini/dalle-mini/reports/DALL-E-Mega-Training--VmlldzoxODMxMDI2#training-parameters)

# Limitations and Bias

Open-ended Generation Models tend to spread bias and harmful messages learned through the data it was given. It is recommended to not use DALL-E as an image generator for vague prompts, especially related to people related. We also do not recommend purposely making harmful prompts unless for research investigation(1) or other academic purposes. More about these assumptions and biases can be found in [this document](https://docs.google.com/document/u/1/d/1C1iJYbzGN_7dfiQAjaQIbETbbzqaE3ci1Fns5TlR-UI/edit?ouid=113653653333035119417&usp=docs_home&ths=true).

1. Even without enforced restrictions it's just common sense, let's keep AI art comfortable and accessible for everyone.