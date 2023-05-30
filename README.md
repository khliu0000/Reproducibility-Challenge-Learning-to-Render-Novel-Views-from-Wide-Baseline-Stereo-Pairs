# Reproducibility-Challenge-Learning-to-Render-Novel-Views-from-Wide-Baseline-Stereo-Pairs

This is a reproducibility challenge in class "Advances in 3D Vison", NCYU.

Paper: Learning to Render Novel Views from Wide-Baseline Stereo Pairs

[Github](https://github.com/yilundu/cross_attention_renderer/tree/master)



## Reproducibility Challenge
Please follow the source github to build up the environment.  
The release weights from source github may be needed if you want to evaluate their release weights by yourself. Please download and unzip it.  
The dataset we use to reproduce evaluation and training are the subset of realestate10k, which also can be downloaded through source github. Please be aware that the images and camera pose files needed to be placed at specific directory.  
  
The reproducibility of evaluation and trainng require a GPU of gram more than 11GB. We tried to reproduce using 2080ti 11GB and failed. We reproduce successfully with A5000 24GB.  
  
The following command are used in our experiment. We use one A5000 GPU in our experiment.  
Please note that the config file at ./experiment_scripts/ need to be modified to run the experiment. You can create your own profile in it.  
  
### Training
```
python experiment_scripts/train_realestate10k.py --experiment_name realestate --batch_size 8 --gpus 1
```
The command in source github has batch_size 12 and gpus 4. We only have one gpu and the batch_size need to be adjusted to 8 to fit in single A5000 24G.  
The training process runs indefinitely, and we stopped the training after 48 hours, choosing the checkpoint with lowest total loss as our experimental weight.  
The next command adds the other two terms of loss functions in trianing.  
```
python experiment_scripts/train_realestate10k.py --experiment_name realestate_lpips_depth --batch_size 2 --gpus 1 --lpips --depth --checkpoint_path "path_to_current_checkpoint"
```
The command also adjusted to meet our hardware limitation. We trained another 24 hours with this command but the performance does't increase significantly.  

### Evaluation
```
python experiment_scripts/eval_realestate10k.py --experiment_name vis_realestate --batch_size 12 --gpus 1 --checkpoint_path "path_to_current_checkpoint"
```
The evaluation using test set of subset, which including 997 scenes from realestate10k. It takes about 1 hour to finish evalution with A5000. The final output are the avaerage performance on the test set.  

### Result
We evalutate their release weight and our weights using test set of the subset.  
|              |LPIPS|SSIM |PSNR |MSE   |
|Paper         |0.262|0.839|21.38|0.0110|
|Release Weight|0.317|0.809|21.11|0.0112|
|Our Trained(1)|0.355|0.773|19.50|0.0161|
|Our Trained(2)|0.356|0.769|19.39|0.0165|  

(1) means using only image loss. (2) adds two additional loss by the second training command.


## Our Experiment

We use the [code](https://colab.research.google.com/drive/1PeL5oJ_eraLEdzTEVPLBwoM2pyv26WcU?usp=sharing) which is applied by [Github](https://github.com/yilundu/cross_attention_renderer/tree/master) to run our experiments.
The images in folder `our_input` are used to renderer novel view video. The results are saved in the folder `our_output`.

### Experiment 1

#### Two input images

![image](https://github.com/khliu0000/Reproducibility-Challenge-Learning-to-Render-Novel-Views-from-Wide-Baseline-Stereo-Pairs/blob/main/our_input/set1/1.jpg)
![image](https://github.com/khliu0000/Reproducibility-Challenge-Learning-to-Render-Novel-Views-from-Wide-Baseline-Stereo-Pairs/blob/main/our_input/set1/2.jpg)

#### After preprocessing

Before rendering, resize the images to proper size

![image](https://github.com/khliu0000/Reproducibility-Challenge-Learning-to-Render-Novel-Views-from-Wide-Baseline-Stereo-Pairs/blob/main/our_input/set1/process1.png)

![image](https://github.com/khliu0000/Reproducibility-Challenge-Learning-to-Render-Novel-Views-from-Wide-Baseline-Stereo-Pairs/blob/main/our_input/set1/process2.png)

#### Output

![image](https://github.com/khliu0000/Reproducibility-Challenge-Learning-to-Render-Novel-Views-from-Wide-Baseline-Stereo-Pairs/blob/main/our_output/set1_output.gif)

### Experiment 2

#### Two input images

![image](https://github.com/khliu0000/Reproducibility-Challenge-Learning-to-Render-Novel-Views-from-Wide-Baseline-Stereo-Pairs/blob/main/our_input/set2/1.jpg)
![image](https://github.com/khliu0000/Reproducibility-Challenge-Learning-to-Render-Novel-Views-from-Wide-Baseline-Stereo-Pairs/blob/main/our_input/set2/2.jpg)

#### After preprocessing

Before rendering, resize the images to proper size

![image](https://github.com/khliu0000/Reproducibility-Challenge-Learning-to-Render-Novel-Views-from-Wide-Baseline-Stereo-Pairs/blob/main/our_input/set2/process1.png)

![image](https://github.com/khliu0000/Reproducibility-Challenge-Learning-to-Render-Novel-Views-from-Wide-Baseline-Stereo-Pairs/blob/main/our_input/set2/process2.png)

#### Output

![image](https://github.com/khliu0000/Reproducibility-Challenge-Learning-to-Render-Novel-Views-from-Wide-Baseline-Stereo-Pairs/blob/main/our_output/set2_output.gif)
