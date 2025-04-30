## Vision Transformer based Feature Detection with Location Embeddings 

![Screenshot 2025-04-30 103039](https://github.com/user-attachments/assets/5438c4e2-f892-4797-aae1-875f6f0f764a)

There is a growing need to accurately map permafrost landforms and thaw disturbances at Pan-Arctic scale using sub-meter resolution satellite images. The challenge of handling petabyte-scale image data across the Arctic can be addressed by large-scale high-performance computing workflows with accurate feature detection models. To gain the advantage of transformer-based DL networks, similar to the success in large language models, vision transformers (ViTs) are introduced for feature detection tasks. ViTs can accurately capture the long-range dependencies and global context of an image through attention mechanisms. ViTs scale well compared to CNNs, enabling larger models that capture more details. Large ViTs can be pre-trained without labeled training data by leveraging self-supervised learning approaches, making the downstream fine-tuning less data intensive. ViTs have shown significant accuracy improvements for everyday images on benchmark image datasets and are considered the current state-of-the-art. Identifying features with the same semantic concept but with distinct spectral characteristics at different spatial locations is a major challenge affecting the overall accuracy of feature detection models when applied to extremely large and diverse geographic areas such as the Arctic. Incorporating location embeddings into the models can mitigate this problem by enabling the models to adapt the learning based on the geographic location. ViTs with location embeddings is a suitable candidate to build robust models for mapping semantically complex permafrost features across spatially heterogeneous Arctic landscapes. 

In this work, we have introduced an experimental setup for empirically exploring multiple configurations to combine the image feature embeddings and the location embeddings. 

### Possible Extentions:
1. Cross Attention / Learned Projection based merging: Theoretically and practically explore the outputs from the ViT encoder and the location encoder. Design a trainable cross-attention-based encoder that can improve the current best-case detection outputs. The design should be supported by theoretical intuition and validated through practical experiments.
2. Test the model with a geographicly larger dataset like [SpaceNet](https://spacenet.ai/spacenet-buildings-dataset-v2/)
3. Hyperparameter tuning
4. Further extend TransUNet based VitDet-UNet where the CNN is replaced with the SFPN
5. MAE pre-training with Arctic Images with more bands and extend the current architechture
6. .....
