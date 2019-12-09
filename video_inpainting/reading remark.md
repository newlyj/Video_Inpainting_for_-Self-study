# 3.Deep Video Inpainting

### 3.1 Input

- 2 lagging and 2 leading frame and 1 current frame
- Past predictions 
- The memory by the recurrent feedback

### 3.2 Model

- Name : VINET

- Source and reference encoders : 

  - encoder-decoder network 
  - Input : 
    - 5 source streams :
      - 2 lagging and 2 leading frame
      - previously generated frame 
    - 1 reference streams 
  - Output : 
    - source and reference features 

- Feature flow learning :

  - coarse-to-fine structure of PWCNet

    > Pwc-net: Cnns for optical flow using pyramid, warping, and cost volume.

  - To estimate the flows between the source and reference feature maps in four different spatial scales (1/8, 1/4, 1/2, and 1). 

  - FlowNet2 

- Learnable Feature Composition :

  - aligned feature maps from the five source streams ->
  - a spatio-temporally aggregated feature map $F_{s}'$
  - $F_{s}' $ and $F_{r}'$ to produce $F_{c}'$

- Recurrence and Memory

  - ConvLSTM

### 3.3 Loss

- Reconstruction loss
- Flow estimation loss
- Warping loss

### 3.4 Dataset

- Train : Youtube-VOS dataset and four different mask dataset

- Test : DAVIS dataset

  

### My understanding

1. 5 source streams and 1 reference stream are used to produce source and reference features
2. Source features uses flow to get an aligned feature map
3. Aligned features and reference feature are combined to produce composite feature
4. Input composite feature into ConvLSTM to make a recurrent feedback and generate the temporal memory 
5. Warp the previous output and blend it with the raw output



# 10. Onion-Peel Networks for Deep Video Completion 

### Onion-Peel Network (OPN)  

- Encoder, asymmetric attention block, and decoder 

### Encoder

- Two parallel gated convolutional layers and down-sample to  1/4 scale
- To obtain key and the value feature maps  

### Asymmetric Attention Block 

- To Compute a soft attention map over the reference images. 

### Dataset

- Train : Places2 and collected YouTube video
- Test : Youtube-VOS, DAVIS 

### Loss

- Pixel loss in peel and valid content
- Perceptual loss of content and style loss

### My Understanding

- The inpainting part are divided into peel part and valid part.
- Recurrence 
  - At each Recurrence, the onion-peel network fills only the peel region of the target image by referring to valid regions on the reference images. 
- The key part is using an asymmetric attention block  to make a feature matching
  - Specify the region of interests to reduce computation cost



# 11. Copy-and-Paste Networks for Deep Video Inpainting 

### Alignment network  

	- Optical flow based alignment is not suitable for slow-moving videos 
	- Shared alignment encoders + alignment regressors 
	- Minimize the self-supervised loss 

### Copy-and-Paste Network 

- The context matching module 
- Compute the visibility of each pixel throughout the video 

### Loss

- Construction loss
  - Hole (visible and invisible) and no-hole
- Perceptual loss of content and style loss
- A total variation loss for smoothing the checkerboard effect 

### Dataset

- Train : Places2 and collected YouTube video
- Test : Youtube-VOS, DAVIS 

### My understanding

- The idea is the same as the OPN

- The pixels marked on Cmask are pixels that are never visible in all reference frame because those pixels always fall into holes. 















 

