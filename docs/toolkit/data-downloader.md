# Data downloader

We provide a data downloader script that downloads and prepares the data. 

You can run as:

```
python -m data_downloader.data_asset_download --split <split> --download_dir <download_dir> --download_only_one_video_sequence --dataset_assets <identifier list of data assets to download>
```

where the supported arguments are:

* `--split <split>`: Specify the split of the data. This argument can be `train_val_set`, `test_set` or `custom`.

* `--download_dir <download_dir>`: Specify the path where the downloaded data will be stored.

* `--download_only_one_video_sequence`: (Optional) Specify whether to download only one video sequence (the longest video will be downloaded). By omitting this flag, all the available video sequences for each scene will be downloaded.

* `--dataset_assets <identifier list of data assets to download>`: Specify the identifier list of the data assets to download. See the table below for the supported data asset identifiers. You can specify the data assets to download as `--dataset_assets <identifier-1> <identifier-2> ... <identifier-n>`

Below you can find a list with the supported *data asset identifiers*. To download the desired data assets, add the corresponding identifiers after the `--dataset_assets` argument. You can find a detailed description of each data asset at [*Dataset > Data assets and File structure*](site:dataset/file-structure).

| Data asset identifier | Filename | Description |
|-----------------------------|----------|-------------|
| laser_scan_5mm | &lt;visit_id&gt;_laser_scan.ply | Combined Faro laser scan downsampled with a voxel size of 5mm   |
| crop_mask | &lt;visit_id&gt;_crop_mask.npy | Binary mask to crop extraneous points from the combined laser scan |
| annotations | annotations.json | functional interactive element annotations |
| descriptions | descriptions.json | natural language task descriptions |
| motions | motions.json | motion annotations |
| lowres_wide | lowres_wide/ | low res. RGB frames of the wide camera |
| lowres_wide_intrinsics | lowres_wide_intrinsics/ | camera intrinsics for the low res. frames |
| lowres_depth | lowres_depth/ | low res. depth maps |
| hires_wide | hires_wide/ | high res. RGB frames of the wide camera |
| hires_wide_intrinsics |  hires_wide_intrinsics | camera intrinsics for the high res. frames |
| hires_depth | hires_depth/ | high res. depth maps |
| lowres_poses | lowres_poses.traj | ARKit camera poses in the ARKit coordinate system |
| hires_poses | hires_poses.traj | COLMAP camera poses in the laser scan coordinate system |
| vid_mov | &lt;video_id&gt;.mov | video captured with the iPad camera in .mov format |
| vid_mp4 | &lt;video_id&gt;.mp4 | video captured with the iPad camera in .mp4 format |
| arkit_mesh | &lt;video_id&gt;_arkit_mesh.ply | ARKit 3D mesh reconstruction of the scene |
| transform | &lt;video_id&gt;_transform.npy | 4x4 transformation matrix that registers the Faro laser scan to the ARKit coordinate system |



### Download the train/val sets

To download the scenes in the train/val sets, you can run:

```
python -m data_downloader.data_asset_download --split train_val_set --download_dir data/ --dataset_assets <identifier list of data assets to download>
```
where `<identifier list of data assets to download>` should be substituted with the identifiers of the data assets you want to download. For example, to download the combined laser scan, the low resolution RGB frames, depth maps and camera intrinsics, the camera trajectory and the transformation matrix, you can run:

```
python -m data_downloader.data_asset_download --split train_val_set --download_dir data/ --dataset_assets laser_scan_5mm lowres_wide lowres_depth lowres_wide_intrinsics camera_trajectory transform
```
You can also add `--download_only_one_video_sequence`, if you want to download only one video sequence for each scene. This option will reduce the storage needed and the download time.

### Download the test set

Similarly, to download the scenes in the test set, you can run:

```
python -m data_downloader.data_asset_download --split test_set --download_dir data/ --dataset_assets <identifier list of data assets to download>
```
where `<identifier list of data assets to download>` should be substituted with the identifiers of the data assets you want to download. 

To download only one video sequence for each scene, you can add `--download_only_one_video_sequence`.

### Download selected scenes

**Download a set of selected scenes:**

To download only a set of selected scenes, you can create a custom `.csv` file by following the same format (e.g., check [here](https://github.com/SceneFun3D/scenefun3d/blob/main/benchmark_file_lists/train_val_set.csv)). Specifically, the first column should specify the `visit_id` and the second column the `video_id`. For example:

```csv
visit_id,video_id
420683,42445137
421013,42444703
421015,42444789
...
```

To download the data, you can run:

```
python -m data_downloader.data_asset_download --split custom --video_id_csv <path to the .csv file> --download_dir data/ --dataset_assets <identifier list of data assets to download>
```

The `video_id` column will be used only if the desired data assets to download are related to the video sequence.

**Download a single selected scene:**

To download a single selected scene, you can run:

```
python -m data_downloader.data_asset_download --split custom --download_dir data/ --visit_id <visit_id of the desired scene> --video_id <video_id of the desired video sequence> --dataset_assets <identifier list of data assets to download>
```