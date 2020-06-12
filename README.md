# AutoGAN

Paper:  
* [AutoGAN: Neural Architecture Search for Generative Adversarial Networks](https://arxiv.org/abs/1908.03835). 
  
  
Data: 
* [CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html) 
* [STL-10](http://ai.stanford.edu/~acoates/stl10/)


## Reproducibility Review

AutoGAN is published at ICCV 2019. As per the guidlines in [A Step Toward Quantifying Independently Reproducible Machine Learning Research](https://arxiv.org/abs/1909.06674), the features of the paper are listed below.

| Features | Comments|
| -- | --|
|Year Published| 2019|
|Year First Attempted| - |
|Venue Type| Conference |
|Rigor vs Empirical| Empirical |
|Has Appendix| No |
|Looks Intimidating| No |
|Readability| Good |
|Algorithm Difficulty| High |
|Pseudo Code| Yes |
|Primary Topic| NAS, GAN |
|Exemplar Problem| No |
|Compute Specified| No |
|Hyperparameters Specified| Partial |
|Compute Needed| Yes |
|Authors Reply| na |
|Code Available| Yes |
|Pages|11|
|Publication Venue| ICCV|
|Number of References| 73 |
|Number Equations| 3 |
|Number Proofs| 0 |
|Number Tables| 2 |
|Number Graph/Plots| 4 |
|Number Other Figures| 2 |
|Conceptualization Figures| 4 |
|Number of Authors| 4 |

The paper should be easily reproducable which needs a GPU for training and testing.  
Specifically use ``` tensorflow-gpu <= 1.14.0 ``` and ``` Cuda <= 10.0```. Higher versions will cause exceptions. 

## Set-up
python >= 3.6  
torch >= 1.1.0  
``` pip install -r requirements.txt ```

## How to search & train the derived architecture
``` sh exps/autogan_search.sh ```  
To train from scratch and get the performance of your discovered architecture, run the following command (you should replace the architecture vector following "--arch" with yours):  
```
python train_derived.py \
-gen_bs 128 \
-dis_bs 64 \
--dataset cifar10 \
--bottom_width 4 \
--img_size 32 \
--max_iter 50000 \
--gen_model shared_gan \
--dis_model shared_gan \
--latent_dim 128 \
--gf_dim 256 \
--df_dim 128 \
--g_spectral_norm False \
--d_spectral_norm True \
--g_lr 0.0002 \
--d_lr 0.0002 \
--beta1 0.0 \
--beta2 0.9 \
--init_type xavier_uniform \
--n_critic 5 \
--val_freq 20 \
--arch 1 0 1 1 1 0 0 1 1 1 0 1 0 3 \
--exp_name derive
```

## How to train & test the discovered architecture reported in the paper
### Train
``` sh exps/autogan_cifar10_a.sh ```
### Test
```
python test.py \
--dataset cifar10 \
--img_size 32 \
--bottom_width 4 \
--gen_model autogan_cifar10_a \
--latent_dim 128 \
--gf_dim 256 \
--g_spectral_norm False \
--load_path /path/to/*.pth \
--exp_name test_autogan_cifar10_a
```  
Pre-trained models are in ``` ./pre_trained_models ```

## References:
1. [AutoGAN code implementation](https://github.com/TAMU-VITA/AutoGAN)
2. [AdversarialNAS: Adversarial Neural Architecture Search for GANs](https://arxiv.org/abs/1912.02037)
3. [Neural Architecture Search with Reinforcement Learning](https://arxiv.org/abs/1611.01578)
4. [Efficient Neural Architecture Search via Parameter Sharing](https://arxiv.org/pdf/1802.03268.pdf)
5. [NAT: Neural Architecture Transformer for Accurate and Compact Architectures](https://arxiv.org/abs/1910.14488)
6. [Learning Transferable Architectures for Scalable Image Recognition](https://arxiv.org/abs/1707.07012)
7. [FID Score - GANs Trained by a Two Time-Scale Update Rule Converge to a Local Nash Equilibrium](https://arxiv.org/abs/1706.08500): [Code](https://github.com/bioinf-jku/TTUR)
8. [Improved Techniques for Training GANs](https://arxiv.org/abs/1606.03498) : [code](https://github.com/openai/improved-gan)
