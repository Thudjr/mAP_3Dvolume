# mAP_3Dvolume

## Introduction:
This repo contains a tool to evaluate the mean average precision score (mAP) of 3D segmentation volumes. This tool uses the cocoapi approach for mAP evaluation. The master branch runs super fast. If you wish to test out the master branch, you can run the legacy branch (https://github.com/ygCoconut/mAP_3Dvolume/tree/legacy). 

## Important notes:
- The tool supposes you load arrays saved as h5 files. Feel free to change the loadh5 function to load something else.
- The tool assumes that the z-axis is the first axis, then x then y (i.e. gt.shape = (z, x, y), where z represents the slices of your stack). This should not matter in terms of map score though if you load a 3D array.
- There is a variety of flags that you can use. The most important flags are probably -ph and -ps. Choose -ps if you already computed the scores, otherwise you can use -ph to feed the tool with your output layer heatmap.
- In our model output, each voxel has 3 score/affinity values. For this reason, the average instance score is calculated in a way that might not be compatible with your model output. Feel free to adapt the score function.

## Requirements:
- You can use one of the following two commands to install the required packages:
```
conda install --yes --file requirements.txt
pip install requirements.txt
```


- The master branch is running with python 2.7 as a default, but can easily be adapted to run with python 3 if needed.

## How it works:
Run the following command to use the tool:
```
python run_eval.py -p "path/to/prediction.h5" -gt "path/to/ground_truth.h5" -ph "path/to/model_output.h5"
```
The following steps will be executed by the script:
1) Load the following 3D arrays:
- GT segmentation volume
- prediction segmentation volume
- model prediction matrix / scores matrix in order to get the prediciton score of each voxel

2) Create the necessary tables to compute the mAP:
- iou_p.txt contains the different prediction ids, the prediction scores, and their matched ground trught (gt) ids. Each prediciton is matched with gt ids from 4 different size ranges (based on number of instance voxels). Each of these ranges contains the matched  gt id, its size and the intersection over union (iou) score. 
- iou_fn.txt contains false negatives, as well as instances that have been matched with a worse iou than another instance.  

3) Evaluate the model performance with mAP by using the 3D optimized evaluation script  and the 2 tables mentioned above.
