## Components that are different from SRGAN:
1. Network architecture:
	Basic architecture is of SRResNet
	'Basic blocks' or residual blocks in generator have been replaced with Residual-in-residual Dense Block(RRDB) 
	Batch normalization(BN) blocks have been removed - this has been shown to increase performance and reduce complexity
	Lots of problems with BNs
	Residual scaling has been done - residuals are multiplied by a constant between zero and one to prevent instability
	Smaller initialization - empirically found to make it easier to train
2. Adversarial loss
	Use a relativistic discriminator - one that predicts the probability that a real image is relatively more realistic than a fake one
	![[Pasted image 20240113195550.png]]
	where E represents the operation of taking average for all fake data in the mini batch
	Discriminator loss:
	![[Pasted image 20240113195709.png]]
	Adversarial loss:
	![[Pasted image 20240113195724.png]]
3. Perceptual loss
	![[Pasted image 20240113200514.png]]
	![[Pasted image 20240113200532.png]]
	Features before the activation layers have been chosen to define perceptual loss, as they contain more information

Network interpolation:
	We first train a PSNR-oriented network G$_{PSNR}$ and then obtain a GAN-based network G$_{GAN}$ by fine-tuning. We interpolate all the corresponding parameters of these two networks to derive an interpolated model G$_{INTERP}$, whose parameters are:
	![[Pasted image 20240113201303.png]]
	This model does not produce artifacts
	We can balance perceptual quality and fidelity without having to retrain the model
	Directly interpolating images pixel by pixel produces blurry or noisy images

BN removal: We first remove all BN layers for stable and consistent performance without artifacts.
Before activation in perceptual loss: Using features before activation can result in more accurate brightness of reconstructed images. Using features before activation helps to produce sharper edges and richer textures. 
RaGAN: RaGAN uses an improved relativistic discriminator, which is shown to benefit learning sharper edges and more detailed textures.
Deeper network with RRDB: Deeper model with the proposed RRDB can further improve the recovered textures. 

## Final model:
We implemented RRDB, residual scaling, and smaller initial weights. 
Due to lack of resources, we had to train for 500 epochs instead of 10000 as was mentioned in the paper. 
We got the following results:
![[Pasted image 20240121185439.png|700]]


![[Pasted image 20240121185538.png|700]]

References:
1. Wang, X. _et al._ (2019). ESRGAN: Enhanced Super-Resolution Generative Adversarial Networks. In: Leal-Taixé, L., Roth, S. (eds) Computer Vision – ECCV 2018 Workshops. ECCV 2018. Lecture Notes in Computer Science(), vol 11133. Springer, Cham. https://doi.org/10.1007/978-3-030-11021-5_5
2. https://youtu.be/ZM4_s5dAWpI?si=-b6JbRF85ZWHneUT