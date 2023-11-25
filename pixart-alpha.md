# PIXART-α: Fast Training of Diffusion Transformer for Photorealistic Text-to-Image Synthesis

## Abstract

The most advanced text-to-image (T2I) models require significant training costs (e.g., millions of GPU hours), seriously hindering the fundamental innovation for the AIGC community while increasing CO2 emissions. This paper introduces PIXART-α, a Transformer-based T2I diffusion model whose image generation quality is competitive with state-of-the-art image generators (e.g., Imagen, SDXL, and even Midjourney), reaching near-commercial application standards. Additionally, it supports high-resolution image synthesis up to 1024px resolution with low training cost, as shown in Figure 1 and 2. To achieve this goal, three core designs are proposed: (1) Training strategy decomposition: We devise three distinct training steps that separately optimize pixel dependency, text-image alignment, and image aesthetic quality; (2) Efficient T2I Transformer: We incorporate cross-attention modules into Diffusion Transformer (DiT) to inject text conditions and streamline the computation-intensive class-condition branch; (3) High-informative data: We emphasize the significance of concept density in text-image pairs and leverage a large Vision-Language model to auto-label dense pseudo-captions to assist text-image alignment learning. As a result, PIXART-α's training speed markedly surpasses existing large-scale T2I models, e.g., PIXART-α only takes 10.8% of Stable Diffusion v1.5's training time (~675 vs. ~6,250 A100 GPU days), saving nearly $300,000 ($26,000 vs. $320,000) and reducing 90% CO2 emissions. Moreover, compared with a larger SOTA model, RAPHAEL, our training cost is merely 1%. Extensive experiments demonstrate that PIXART-α excels in image quality, artistry, and semantic control. We hope PIXART-α will provide new insights to the AIGC community and startups to accelerate building their own high-quality yet low-cost generative models from scratch.


## Introduction

PIXART-α is a diffusion based text-to-image (t2i) model. Diffusion based t2i models are very demanding when it comes to training and inferencing. They require large volumes of (text, image) pairs as well as long training times to produce quality results. Furthermore, to inference such models, several forward-passes of the model are necessary because of how denoising works. PIXART-α aims to cut the training costs by developing a fast diffusion transformer. The key contributions are:

1. Class-conditioned pre-training initialized from a pre-trained Diffusion Transformer (DiT) model in order to capture pixel dependencies before even conditioning on text. Suitable initialization from a pre-trained model effectively accelerates the training of the model.

2. Conditioned training on (text, image) pairs. This is where conditioning and correspondences between images and text is learned. Text and image dataset is produced from LAION dataset by applying a pre-trained LLaVa model to refine text inputs.

3. Aeshtetic training is the last stage where the model is taught to produce stylistically pleasing and high res images.


## Architecture

PIXART-α uses a DiT architecture which is transformer based (unlike many other t2i models which employ CNNs). The time embedding is fed into the model via a global MLP which provides parameters for adaptive layer norm. Text conditioning is performed via multi-head cross attention.

## Dataset

The dataset is constructed from the large scela LAION data. Existing LAION data text captions are not detailed. They are short and deficient. Hence, the scene and composition of the images is not well described which can lead to models failing to capture fine-grained t2i correspondences. Hence, the authors use LLaVA model to generate new text descriptions for each image. LLaVA is a multi modal language-image model able to perform several crucial tasks given image and a text as a prompt. The authors just prompt it to describe the image.

## Results in Brief

Briefly, the paper achieves the following results:

1. Open Source. You can pick their code up and whip up your own model in no time.
2. Shorter training times and lower data consumption compared to other SOTA methods (eg. DALLE 2, Imaged, RAPHAEL, Stable Diffusion).
3. Fine grained correspondences between input text and generated images.

| **Method**  	| **Type**  	| **#Params**   	| **#Images**  	| **FID-30K**  	| **GPU days**  	|
|---	|---	|---	|---	|---	|---	|
|  DALLE 	| Diff 	| 12.0B  	| 1.45B  	| 27.50  	|  -  	|
|  GLIDE 	| Diff 	| 5.0B  	| 5.94B  	| 12.24  	|  -  	|
|  LDM 	| Diff 	| 1.4B  	| 0.27B  	| 12.64  	|  41,667 A100  	|
|  DALLE 2 	| Diff 	| 6.5B  	| 5.63B  	| 10.39  	|  6,250 A100  	|
|  SDv1.5 	| Diff 	| 0.9B  	| 3.16B  	| 9.62  	|  6,250 A100 |
|  GigaGAN 	| GAN 	| 0.9B  	| 0.98B  	| 9.09  	|  4,783 A100 |
|  Imagen 	| Diff 	| 3.0B  	| 15.36B  	| 7.27  	|  7,132 A100  	|
|  RAPHAEL 	| Diff 	| 3.0B  	| 5.00B  	| 6.61  	|   60,000 A100  	|
|  **PIXART-α** 	| Diff 	| **0.6B**  	| **0.025B**  	|  10.65   	|   **675 A100**  	|


## References
1. [PIXART-α](https://github.com/PixArt-alpha/PixArt-alpha)
2. [LLaVA](https://llava-vl.github.io/)
3. [DiT](https://github.com/facebookresearch/DiT)
4. [RAPHAEL](https://arxiv.org/abs/2305.18295)
5. [Stable Diffusion](https://arxiv.org/abs/2112.10752)