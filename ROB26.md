## Observations
The initial repo did not work for my setup since I needed a way to abstract from my machine cuda version, the best solution seemed to dockerize the software.
During the dockerization process some issues popped up, in particular:
    - Needed to fix some dependency specifications in pyproject.yml (a753b69)
    - Fix bug where rotation matrix used in megapose was not loaded in the same device as the input rendered images (9311f52)
    - Need to specify weights_only=False in new versions of torch when using torch.load() for megapose to work as expected (9311f52)
    - Some parts of the mirrors for the BOP dataset used in the download script were no longer available, in these cases the instructions were updated to use the huggingface cmd utility to download the dataset (7cdebad)

## Inference
Here are the instructions to reproduce the inference steps using both sub-packages of HappyPose

### CosyPose
```
# download ycbv model weights
python -m happypose.toolbox.utils.download --cosypose_models \
          detector-bop-ycbv-pbr--970850 \
          coarse-bop-ycbv-pbr--724183 \
          refiner-bop-ycbv-pbr--604090

# move linemod example into dataset folder
# bop toolkit info used to build example: https://github.com/ethz-asl/bop_toolkit/blob/main/docs/bop_datasets_format.md
cp -r examples/eggs dataset/examples/

# run example
python -m happypose.pose_estimators.cosypose.cosypose.scripts.run_inference_on_example eggs --dataset ycbv --run-inference --run-detections --vis-detections --vis-poses
```

### MegaPose

