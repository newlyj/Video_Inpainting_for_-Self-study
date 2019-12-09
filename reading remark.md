# 1.Video Inpainting by Jointly Learning Temporal Structure and Spatial Details
### Network
- a temporal structure inference network 
  - 3D-CNN
  - Input down-sampled marked video
  - Output a down-sampled inpainted video
- a spatial detail recovering network
 - 2D-CNN
 - Use two convolutional layers to extract two feature maps of the 3DCN output separately

### My understanding
- The result is not good especially in the case of fast-moving video.
- The idea is using two different network to train the temporal and spatial information and do a jointly training.
- The idea can also been seen in other reference papers.

# 2. Deep Flow-Guided Video Inpainting
### Model
- DFC-Net	
   - Consists of three similar subnetwork
    - The first DFC-S : 
         - down-sampled marked video to obtain the coarse flow
         - Input the flow maps and binary marks
    - The second and third DFC-S
         - The forward and backward flows are refined jointly
         - The frames are gradually enlarged such as 1/2 2/3 and 1 respectively.

### What is interesting ?
- Leverage the hard flow example mining mechanism to automatically focus more on the difficult areas to produce sharp boundaries.
- Use image inpainting [35] to inpaint unseen regions.

### My understanding 
- This work uses three encoder-decoder networks to produce a refined flow maps which can be used for inpainting the source frame.
- Then it uses two other networks to improve the inpainting performance.


# 3. Deep Video Inpainting

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

# 4. Deep Blind Video Decaptioning by Temporal Aggregation and Recurrence
### Model
- BVDNet
   - Without the inpainting masks
   - A hydrid encoder-decoder network :
  - 3D CNN
       - To capture the spatio-temporal features from the neighboring frames. 
       - A windows of 2N+1 length
  - 2D CNN
       - Use a generated frame to provide a reference for the current geration
    - Skip connections in 3D CNN part
     - pool the temporal dimension into one frame(for warping)

### Loss
- Image reconstruction loss
 - SSIM loss + Image gradients loss + normal reconstruction loss
- temporal consitency loss

### My understanding
- A blind inpainting scenrio
- Use skip connection and a lot of loss metric.

# 5. VORNet: Spatio-temporally Consistent Video Inpainting for Object Removal

### Model
- For video object removal and they also build
a big dataset.
- 1. Warping network

  - Collect the information from other frames
  - FlowNet2 to calculate the optical flow between two frames.
  - Secondly, do some operations for fore ground region.
  - Finally, warp the flow

- 2. Inpainting network

  - Use the generative model to do a image inpainting.
  - Get the coarse result only based on the current frame

- Refinement network

   -  Input: warping network and inpainting network.
   - Select the top 2 candidate
 - ConvLSTM
    - Skip connections are added.

### Loss
- Reconstruction loss
- Perceptual loss 
- Designed GAN losses
 - PatchGAN LOSS 
     - For image quality 
 - Temporal GAN loss
     - Calculate the siamese features differences
     - Estimate the final consistent score 

### My understanding
- Only choose one top candidate for refine network
- The refinement network add a discriminator which is used in the GAN loss.

# 6. Learnable Gated Temporal Shift Module for Deep Video Inpainting

### Model
- LGTSM (Learnable Gated Temporal Shift Module)
      - Features in channel x time dimensions
      - The features are shifted to the previous and next frame so that the 2D- CNN can learn the temporal structures.
  - Add a gate for free-form video inpainting
      - A soft validity map
  - The shift is learnable 
  - Use a buffer
 - The network
        - U-net + TSMGAN discriminator
         - No skip conection

### Loss
- Perceptual loss and style loss
- TSMGAN loss
- reconstruction loss

### My understanding
- LGTSM + GAN as discriminator
- 2D CNN as generator
- Insteresting part
   - The gating method
   - No skip connection
- It has the training code 



# 7. Align-and-Attend Network for Globally and Locally Coherent Video Inpainting 

### Model

- Two main module
  - Alignment module 
  - Nonlocal attention module 
- Both consist of a large spatial-temporal window size 
- The existing research uses the flow-guided warping, this work design a homography-guided warping.

### Network

- Homography estimator 
  - Homography Encoder 
    - Image to a smaller-size feature map
  - Masked matching 
    - Find the similarity between each feature map.
  - Transformation estimator 
    - Takes the correlation map C as input and produces homography parameters θ between the reference and target 
    - Output 6 parameters 
- Align-and-Attend Video Inpainter 
  - Align the the reference feature points
- Optical flow estimator 

### Loss

- Homography estimation use another L.
- hole loss 
- valid loss 
- two GAN loss 
- flow loss
- warp loss

### My understanding

- The homography estimator is new but it is the same as 10.
- This work is like the mix of 3, 5 and 10.



# 8. Frame-Recurrent Video Inpainting by Robust Optical Flow Inference 

### Model

- ImageCN
  - Generate inpainted frame separately
- Flownet
  - Flow of the output in ImageCN
  - Flow of the current frame
- Flow blending network  
  - blend the flow to remove errors
  - U-net
- Recurrent Neural Network 
  - Inpainted current frame + blended optical flow
  - ConvLTSM

### Loss

- Spatial loss
- Short-term temporal loss
- Long-term temporal loss

### My understanding

- This work do the imageCN firstly.
- The flownet uses two times
- Two temporal losses.



# 9. Free-form Video Inpainting with 3D Gated Convolution and Temporal PatchGAN 
### Model

- Temporal PatchGAN (TPatchGAN) discriminator to enhance the temporal consistency and video quality 
- The first learning-based model for free-form video inpainting 
- 3D Gated CNN (U-net)+ Temporal Patch Discriminator

### Loss

- Masked loss
- Perceptual loss and sytle loss
- Temporal PatchGAN loss. 




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



# 12. An Internal Learning Approach to Video Inpainting 

### Model

- Just a generative model G 
- Predict both inpainted frame and optical flow maps

### Loss

- Image Generation Loss. 
- Flow Generation Loss. 
- Consistency Loss. 
- Perceptual Loss.















 

