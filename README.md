# MAT: Mask-Aware Transformer for Large Hole Image Inpainting (CVPR2022, Oral)

#### Wenbo Li, Zhe Lin, Kun Zhou, Lu Qi, Yi Wang, Jiaya Jia

#### [\[Paper\]](https://github.com/dvlab-research/MAT)
---

## News

The training and testing code is released. We also provide masks and inpainted results of CelebA-HQ-val and Places-val [here](https://github.com/dvlab-research/MAT).

---

## Usage

1. Clone the repository.
    ```shell
    git clone https://github.com/dvlab-research/MAT.git 
    ```
2. Install the dependencies.
    - Python 3.7
    - PyTorch 1.7.1
    - Cuda 11.0
    - Other packages
    ```shell
    pip install -r requirements.txt
    ```

## Quick Test

1. We provide models trained on CelebA-HQ and Places365-Standard at 512x512 resolution. Download models from [One Drive]() and put them into the 'pretrained' directory.

2. Obtain inpainted results by running
    ```shell
    python generate_image.py --network model_path --dpath data_path --outdir out_path [--mpath mask_path]
    ```
    where the mask path is optional. If not assigned, random 512x512 masks will be generated. Note that 0 and 1 values in a mask refer to masked and remained pixels.

    For example, run
    ```shell
    python generate_image.py --network pretrained/CelebA-HQ.pkl --dpath test_sets/CelebA-HQ/images --mpath test_sets/CelebA-HQ/masks --outdir samples
    ```

    Note. Our implementation only supports generating an image whose size is a multiple of 512. You need to pad or resize the image to make its size a multiple of 512. Please pad the mask with 0 values.

## Train

For example, if you want to train a model on Places, run a bash script with
```shell
python train.py \
    --outdir=output_path \
    --gpus=8 \
    --batch=32 \
    --metrics=fid36k5_full \
    --data=training_data_path \
    --data_val=val_data_path \
    --dataloader=datasets.dataset_512.ImageFolderMaskDataset \
    --mirror=True \
    --cond=False \
    --cfg=places512 \
    --aug=noaug \
    --generator=networks.mat.Generator \
    --discriminator=networks.mat.Discriminator \
    --loss=losses.loss.TwoStageLoss \
    --pr=0.1 \
    --pl=False \
    --truncation=0.5 \
    --style_mix=0.5 \
    --ema=10 \
    --lr=0.001
```

Description of arguments:
- outdir: output path for saving logs and models
- gpus: number of used gpus
- batch: number of images in all gpus
- metrics: find more metrics in 'metrics/metric\_main.py'
- data: training data
- data\_val: validation data
- dataloader: you can define your own dataloader
- mirror: use flip augmentation or not 
- cond: use class info, default: false
- cfg: configuration, find more details in 'train.py'
- aug: use augmentation of style-gan-ada or not, default: false
- generator: you can define your own generator
- discriminator: you can define your own discriminator
- loss: you can define your own loss
- pr: ratio of perceptual loss
- pl: use path length regularization or not, default: false
- truncation: truncation ratio proposed in stylegan
- style\_mix: style mixing ratio proposed in stylegan
- ema: exponoential moving averate, ~K samples
- lr: learning rate

## Evaluation

Find evaluation scrtips for FID/U-IDS/P-IDS/LPIPS/PSNR/SSIM/L1 metrics in the 'evaluation' directory. Only need to give paths of your results and GTs. 

## Citation

## Acknowledgement
Our code is bulit upon [StyleGAN2-ADA](https://github.com/NVlabs/stylegan2-ada-pytorch).
