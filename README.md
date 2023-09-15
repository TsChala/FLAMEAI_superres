# FLAME AI Workshop - Superresolution of Turbulent Data

[Workshop Website](https://flame-ai-workshop.github.io) | [Kaggle](https://www.kaggle.com/competitions/2023-flame-ai-challenge)

This is repository for a submission for the Stanford FLAME AI workshop for superresolving turbulent flow data.\
\
*The model used is an SR3 diffusion model taken directly from*: https://github.com/Janspiry/Image-Super-Resolution-via-Iterative-Refinement\
The corresponding paper can be found here: [Image Super-Resolution via Iterative Refinement(SR3)](https://arxiv.org/pdf/2104.07636.pdf ).\
\
An example result from the validation set can be seen below with corresponding low-resolution input and high-resolution ground truth.


 <img src="./misc/superres1.png" alt="show" style="zoom:90%;" /> 

The SR3 model was run twice, first with [ux,uy,uz] as input channels, then with [rho,uy,uz] as input channels. For the final results ux, uy, uz is taken from the first run, while rho is taken from the second run.\

A pretrained model was downloaded from the above SR3 [GitHub repository](https://github.com/Janspiry/Image-Super-Resolution-via-Iterative-Refinement), the pretrained weights can be found here: [Google Drive](https://drive.google.com/drive/folders/12jh0K8XoM1FqpeByXvugHHAF3oAZ8KRu)\
The instruction on how to use the SR3 model for training and inference can also be found in the above [repository](https://github.com/Janspiry/Image-Super-Resolution-via-Iterative-Refinement)
***
The model was first trained until 1000000 iterations using the above mentioned pre-trained weights and the [ux,uy,uz] version of the turbulence training data. The corresponding .json setup file can be found in config/sr_sr3_16_128_flame.json. The val/train phase switches between training and validation. For the first training the resume_state was set to "pretrain/I640000_E37". For inference on the test data the config/sr_sr3_16_test_flame.json was used. \
The model then was trained for another 200000 iteration using the [rho,uy,uz] formulation. The corresponding .json files are sr_sr3_16_128_flame_rho.json for training, and sr_sr3_16_128_flame_rho_test.json for testing.\
\
Checkpoints to both are provided on OneDrive, along with the png and .mdb version of datasets. The **flame_ai_challenge_process_inputs_and_results.ipynb** creates these png files from the original data by rescaling everything to be between 0 and 255. The same notebook also contains postprocessing steps for the results before submission and a few samples from the superresolved results.\
\
Data in png format can be found here: [OneDrive](https://1drv.ms/f/s!AuYkVS2by4myiLNMtnuJi3iGvcfJog?e=aXfmZw)
- /train/ folder has data for training with [ux,uy,uz] channels
- /train_density/ folder has data for training with [rho,uy,uz] channels
- /val/ folder has data for validation with [ux,uy,uz] channels
- /val_density/ folder has data for validation with [rho,uy,uz] channels
- /test/ folder has data for testing with [ux,uy,uz] channels
- /test_density/ folder has data for testing with [rho,uy,uz] channels
All have subfolders with hr_128 for the high-resolution 128x128 data and lr_16 for the low-resolution 16x16 data. These were generated by the **flame_ai_challenge_process_inputs_and_results.ipynb**\


The data has to be preprocessed for the SR3 model, the already preprocessed data folder can be found here: [OneDrive](https://1drv.ms/f/s!AuYkVS2by4myiLM-QddYUibf73oAFQ?e=xXcb9B), in the folders named flame_ai_16_128_prepared_... .This was generated by the preprocessing *prepare_data.py* script from the SR3 model.\
***
# Model weights 
The trained model parameters can be found in the experiments folders, the set of parameters that are used in the ipynb can be found here: [OneDrive](https://1drv.ms/f/s!AuYkVS2by4myic4i3eMPT3yM1p2GUg?e=yhlf0I). Some experiments are for training and some for testing only. The weights can be found in the /checkpoint/ folders. In the /results/ folder the superresolved images can be found in png format. These are loaded by the .ipynb for visualization and postprocessing.

Training:
- Training for the [ux,uy,uz] case: [sr_flame_ai_230911_185035](https://1drv.ms/f/s!AuYkVS2by4myieJCLSYAp4aVdlQp_g?e=rxurTc)
    - The checkpoints folder has many trained weights, the last one was used for inference: I1000000_E818   
- Training for the density case with channels [rho,uy,uz]: [sr_flame_ai_230912_201811](https://1drv.ms/f/s!AuYkVS2by4myieIpHTSop6OTNYDEvA?e=GtTemJ)
    - Chekcpoint used for inference : I1200000_E1252 


Testing (these don't have the weights, just the final results in png format):
- Validation results with [ux,uy,uz]:sr_flame_ai_230912_130804
- Test results with [ux,uy,uz]: sr_flame_ai_230912_175205
- Test results with [density,ux,uy,uz]: sr_flame_ai_230913_094253

The model weights can be simply loaded in the json files (see any json files in the config directory), by setting *resume_state* to the folder containing the model weights. For example, *config/sr_sr3_16_128_flame_rho_test.json* contains the path to the last trained weights for inference on the test set with the [rho,uy,uz] model.












