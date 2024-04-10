# UHF-guided Segmentation
This repository provides a PyTorch implementation of the following paper:
> **Transferring Ultrahigh-Field Representations for Intensity-Guided Brain Segmentation of Low-Field Magnetic Resonance Imaging**<br>
> [Kwanseok Oh](https://scholar.google.co.kr/citations?user=EMYHaHUAAAAJ&hl=ko)<sup>1, \*</sup>, [Jieun Lee](https://scholar.google.co.kr/citations?user=RV0DwyEAAAAJ&hl=ko)<sup>1, \*</sup>, [Da-Woon Heo](https://scholar.google.co.kr/citations?user=WapMdZ8AAAAJ&hl=ko)<sup>1</sup>, [Dinggang Shen](https://scholar.google.co.kr/citations?user=v6VYQC8AAAAJ&hl=ko)<sup>3, 4</sup><br/>, and [Heung-Il Suk](https://scholar.google.co.kr/citations?user=dl_oZLwAAAAJ&hl=ko)<sup>1, 2</sup><br/>
> (<sup>1</sup>Department of Artificial Intelligence, Korea University) <br/>
> (<sup>2</sup>Department of Brain and Cognitive Engineering, Korea University) <br/>
> (<sup>3</sup>School of Biomedical Engineering, ShanghaiTech University) <br/>
> (<sup>4</sup>Shanghai United Imaging Intelligence Co., Ltd) <br/>
> (* indicates equal contribution) <br/>
> Official Version: https://arxiv.org/pdf/2402.08409 <br/>
> 
> **Abstract:** *Ultrahigh-field (UHF) magnetic resonance imaging (MRI), i.e., 7T MRI, provides superior anatomical details of internal brain structures owing to its enhanced signal-to-noise ratio and susceptibility-induced contrast. However, the widespread use of 7T MRI is limited by its high cost and lower accessibility compared to low-field (LF) MRI. This study proposes a deep-learning framework that systematically fuses the input LF magnetic resonance feature representations with the inferred 7T-like feature representations for brain image segmentation tasks in a 7T-absent environment. Specifically, our adaptive fusion module aggregates 7T-like features derived from the LF image by a pre-trained network and then refines them to be effectively assimilable UHF guidance into LF image features. Using intensity-guided features obtained from such aggregation and assimilation, segmentation models can recognize subtle structural representations that are usually difficult to recognize when relying only on LF features. Beyond such advantages, this strategy can seamlessly be utilized by modulating the contrast of LF features in alignment with UHF guidance, even when employing arbitrary segmentation models. Exhaustive experiments demonstrated that the proposed method significantly outperformed all baseline models on both brain tissue and whole-brain segmentation tasks; further, it exhibited remarkable adaptability and scalability by successfully integrating diverse segmentation models and tasks. These improvements were not only quantifiable but also visible in the superlative visual quality of segmentation masks.*

## Architecture

TBU

## Requirements

torch==1.10.1

torchvision==0.11.2

scikit-image==0.19.1

scikit-learn==1.0.2

nibabel==3.2.1

nilearn==0.8.1

scipy==1.7.3

## Usage

Command format:
```
python main.py --type <3D / 2D> --mode <all / train / test> --net <T / K / F> --gpu <GPU_NUMBER> \\
--path_dataset_Paired <path to the paired 3T and 7T dataset> --path_dataset_IBSR <path to the IBSR dataset for tissue segmentation> --path_dataset_MALC <path to the MALC dataset for region segmentation> \\
--base <name of the baseline segmentation model> --base_encoder <path to a pre-trained weight file of the segmentation encoder> --base_decoder <path to a pre-trained weight file of the segmentation decoder> \\
--plane <plane for a 2D model: axial / coronal / sagittal>
```


1. For training teacher and knowledge keeper networks for a 3D version, you can use the following command:
```
python main.py --type 3D --mode train --net T --gpu 1 --path_dataset_Paired /PATH_PAIRED && python main.py --type 3D --net K --mode train --gpu 1 --path_dataset_Paired /PATH_PAIRED
```


2. For training fusion modules by using 3D U-Net as the baseline segmentation model, you can use the following command:
```
python main.py --type 3D --mode train --net F --gpu 1 --path_dataset_IBSR /PATH_IBSR --base UNet --base_encoder /PATH_BASE/UNET_ENCODER.pth --base_decoder /PATH_BASE/UNET_DECODER.pth
```

3. For testing the UHF-guided segmentation model trained on the above steps, you can use the following command:
```
python main.py --type 3D --mode test --net F --gpu 1 --path_dataset_IBSR /PATH_IBSR --base UNet
```

4. To implement all consecutive steps for training and testing at once, you can also use the following command:
```
python main.py --type 3D --mode all --gpu 1 --path_dataset_Paired /PATH_PAIRED --path_dataset_IBSR /PATH_IBSR --base UNet --base_encoder /PATH_BASE/UNET_ENCODER.pth --base_decoder /PATH_BASE/UNET_DECODER.pth
```
