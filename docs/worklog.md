# Set up and try demo

1. Download the DTU sample dataset ([train & eval data](https://fast-dipole-sums-data.s3.us-east-2.amazonaws.com/public/dtu_data.zip), [point clouds](https://fast-dipole-sums-data.s3.us-east-2.amazonaws.com/public/dtu_pcd.zip))

2. Set up the code by following the instruction in the README. In particular, I ran the following commands:
```
mkvirtualenv fast_dipole_sums -r requirements.txt
cd cuda_extensions
bash build_cuda_extensions.sh  # remove the --user from the install command (we have created a virtual environment)
```

3. Created a new file `dtu_gfss.conf` in the confs folder, where I copied the contents of the `dtu.conf` file and changed the following paths to include the ones I have placed the downloaded data. The paths should be full paths with user directory `~` expanded.
    - `dataset.data_dir`
    - `model.point_cloud.ply_path`

4. Ran the following command to train a model (representation) for one of the DTU models. It takes about an hour in my desktop (NVIDIA RTX A5500).
```
python exp_runner.py --conf ./confs/dtu_gfss.conf --case dtu/dtu_scan105 --mode train
```

5. One can then render images from the training using the radiance field by running the command:
```
python exp_runner.py --conf ./confs/dtu_gfss.conf --case dtu/dtu_scan105 --mode render --image_idx 0 --is_continue
```

# Useful conclusions
- The input to the model is given as a PLY file with points, normals and colors.
- There is a script `misc/process_custom_data.py` in which one can set scale `s` and a translation vector `[dx dy dz]` to center the point cloud at the origin and fit it into the unit sphere.
- The output model can render the input images (`renders`), depth maps (`depth`) and normals (`normals`).
- The output meshes are available is the folder `meshes`.
- The checkpoints are available in the folder `checkpoints`.
- Inside each experiment, the code that ran it is saved in the folder `recording`.
- They seem to save some debug point clouds in the folder `logs`.