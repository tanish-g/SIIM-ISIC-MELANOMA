# SIIM-ISIC MELANOMA CLASSIFICATION GOLD-MEDAL APPROACH

### BRIEF INTRODUCTION
    In this competition, you’ll identify melanoma in images of skin lesions. In particular, you’ll use images within the same patient and determine which are likely to represent a melanoma. Using patient-level contextual information may help the development of image analysis tools, which could better support clinical dermatologists.

    Melanoma is a deadly disease, but if caught early, most melanomas can be cured with minor surgery. Image analysis tools that automate the diagnosis of melanoma will improve dermatologists' diagnostic accuracy. Better detection of melanoma has the opportunity to positively impact millions of people.

### WHAT WE HAD TRIED 
  - Initially  tried working on tabular data classification using CatBoost,XGBoost,LGBM using features('sex','anatom_site','age_approx')
  - Mainly relied on efficientnets[B0-B5]
  - Split on the basis of StratifiedKFold ,GroupKFold and TFrecords.The major stability of CV and LB was found on *Chriss Deotte* TFrecords
  -  Augmentations-
     1.)AutoAugment
     2.)Cutmix
     3.)Cutouts(Num_holes=8,max_h_size=64,max_w_size=64)
     4.)Flip,Shear-Rotate,CenterCrop,RandomResizedCrop,RandomRotate
  - Have also used Catalyst Balance Sampler(mode='Downsampling')
  - SWA ,Snapshot-ensembling
  - Trained with freezed batchnorm(Finetune) 
  - Used Public TPU notebooks(Thanks to *chriss deotte*)
  - Tried different optimizers(Ranger,Ralamb,RangerLars,AdamW) and schedulers(CosineAnnealing,ReduceLROnPlateau and Customscheduler)
  - Loss function(Focal Loss,Balanced Focal Loss,BCEWithLogitsLoss,Label Smoothing NLL Loss )
  - Traind model (CNN+META_DATA(Feature_concatenated))
  - 25 and 15 TTA used augs(similar to Train )

# WHAT WORKED OUT 

  - AutoAugment gave boost(+0.01%)
  - SnapshotEnsembling(boost=1%)
  - Progressive learning
  - Only BCEwithlogitsloss gave significant boost
  - Cutmix(+0.01%)
  - Random_Resized_Crop(+0.05%)
  - TTA(boost=0.005%)
  - Ensembling 
  - Highly uncorrelated predication ensemble(boost=0.001%)

# WHAT DIDN'T WORKED
  - Balance Sampler
  - SWA
  - Weighted Focal loss
  - Freezed Batchnorm
  - Gradient Accumulation

## ENSEMBLING TECHNIQUE( Rescale between(0-1))
  - Weighted ensembling(GRID SEARCH ON CV)
  - Trimm ensemble
  - Min-max-ensemble
  - Post processing technique(JIGSAW Competition)
  - Weighted ensembling of meta-model and Image model(GRID SEARCH)
  - Average ensemble

### NOTEWORTHY
 Trained model on image-size(384) and when validated on different imagesizes(384,456,512) and found out suprising result,the model which was actually trained on 384 gave better val auc on 456 and 512 imagesize.Then  ensembled them which gave better results.
- Trained on 384 validated on different sizes
![image](https://drive.google.com/uc?export=view&id=15kj9adOhOVnFiH0Mssq4nYkkcB7LigAy) 

## OUR MODEL FOR BLENDING CNN AND META DATA
![image](https://drive.google.com/uc?export=view&id=1bLKU9LUZ_0ahr2VNiiPiOtTe0SbYcTvD)
