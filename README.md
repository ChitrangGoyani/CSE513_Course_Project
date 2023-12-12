## CSE597 Course Project  
* Task: Zero-shot image captioning
* Chitrang Goyani - cbg5586@psu.edu
* Paper: https://openaccess.thecvf.com/content/ICCV2023/html/Fei_Transferable_Decoding_with_Visual_Entities_for_Zero-Shot_Image_Captioning_ICCV_2023_paper.html
* Original repo: https://github.com/FeiElysia/ViECap

## Description of files/directories:
1. `demo` - contains the figures from the paper
2. `images` - contains images for inference
3. `logs` - train and eval logs
4. `scripts` - train and eval shell scripts
5. `tutorial` - tutorials
6. `CaptionsDataset.py` - process datasets used for training ref: `main.py`
7. `ClipCap.py` - translate images into descriptive relevant text (CLIP prefix for image captioning) ref: `main.py`
9. `entities_extraction.py` - extract the entities from each caption within the chosen dataset
10. `generating_prompt_ensemble.py` - To evaluate the trained ViECap, first construct the vocabulary and extract the embeddings of each category in the vocabulary
11. `images_features_extraction.py` - download the image source files for the COCO and Flickr30k dataset from their official websites. Afterwards, place these files into the 'ViECap/annotations/coco/val2014' directory for COCO images and the 'ViECap/annotations/flickr30k/flickr30k-images' directory for Flickr30k images.
12. `infer_by_batch.py` - util
13. `load_annotations.py` - util
14. `main.py` - training code
15. `modelling_opt.py` - util
16. `requirements.txt` - to install the requirements for running the code
17. `retrieval_categories.py` - util
18. `search.py` - util
19. `texts_features_extraction.py` - pre-extract the training text features
20. `utils.py` - util
21. `validation.py` - validation code

## Replicate Results - Tutorial
* Clone the repo
* Setup a conda environment and install requirements from `requirements.txt` file. Refer CapDec repo if don't know how to setup conda environment: https://github.com/DavidHuji/CapDec
* Activate the conda environment and download the `annotations`, `evaluation` and `checkpoints` (optional, only required if you want to evaluate on pre-trained weights)
* Dataset preparation:  
    * Download the karpathy splits: https://www.kaggle.com/datasets/shtvkumar/karpathy-splits/tasks
    * We will be using only flickr30k for this tutorial.
    * Place the `dataset_{name_of_dataset}.json` in ./annotations/{dataset}/karpathy_split as `dataset.json`. Convert to dataset to list and generate train, test and validation splits by using the `parse_karpathy.py` 
    * run `python entities_extraction.py`
    * run `python texts_features_extraction.py`
    * run `python generate_prompt_ensemble.py` after downloading the vocabulary from the Releases of the original repo.
    * run (optional) `python images_features_extraction.py`
* Training:
    * run `bash train_flickr30k.sh 0` to start training - can change hyperparameters in the shell script file
* Evalution:
    * Cross-domain captioning: `bash eval_{to_dataset}.sh train_{from_dataset} 0 '--top_k 3 --threshold 0.2' {14/29}` 
    * In-domain captioning: `bash eval_{dataset}.sh train_{dataset} 0 '' {14/29}`
* Inference:
    * `python infer_by_instance.py --prompt_ensemble --using_hard_prompt --soft_prompt_first --image_path ./images/instance1.jpg`

## References:
1. ViECap  
  ```
@InProceedings{Fei_2023_ICCV,
    author    = {Fei, Junjie and Wang, Teng and Zhang, Jinrui and He, Zhenyu and Wang, Chengjie and Zheng, Feng},
    title     = {Transferable Decoding with Visual Entities for Zero-Shot Image Captioning},
    booktitle = {Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV)},
    month     = {October},
    year      = {2023},
    pages     = {3136-3146}
}
```
```
@article{fei2023transferable,
  title={Transferable Decoding with Visual Entities for Zero-Shot Image Captioning},
  author={Fei, Junjie and Wang, Teng and Zhang, Jinrui and He, Zhenyu and Wang, Chengjie and Zheng, Feng},
  journal={arXiv preprint arXiv:2307.16525},
  year={2023}
}
```
2. The repository builds on [CLIP](https://github.com/openai/CLIP), [ClipCap](https://github.com/rmokady/CLIP_prefix_caption), [CapDec](https://github.com/DavidHuji/CapDec), [MAGIC](https://github.com/yxuansu/MAGIC) and pycocotools repositories. Thanks for open-sourcing!
