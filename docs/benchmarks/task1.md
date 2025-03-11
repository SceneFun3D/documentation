# Task 1: Functionality segmentation

## Task description

In the first track of the SceneFun3D benchmark, we propose the following challenge:

!!! abstract "Functionality segmentation"

    **TASK:** Segment the functional interactive elements in a 3D scene.

    **INPUT:** The Faro laser scan of a given scene, multiple RGB-D sequences captured with an iPad Pro and camera parameters.

    **OUTPUT:** Instance segmentation of the point cloud that corresponds to the vertices of the provided laser scan, identifying the functional interactive elements along with predictions of affordance labels.

## Submission Instructions

Given a 3D scene, the participants are tasked with segmenting the instances of functional interactive elements in the scene. Expected result is functional interactive element masks, predicted affordance classes and confidence scores for each mask. 

We ask the participants to upload their results as a single `.zip` file, which when unzipped must contain the prediction files in the **root**. **There must not be any additional files or folders in the archive except those specified below**.

Results must be provided as a text file for each scene. Each text file should contain a line for each instance, containing the relative path to the instance mask file that specifies the mask indices, the predicted affordance class ID and the confidence of the prediction. The result text files must be named according to the corresponding laser scan (`visit_id`) as `{visit_id}.txt`. Predicted `.txt` files listing the instances of each scan must live in the root of the unzipped submission. Predicted instance mask files must live in a subdirectory named `predicted_masks/` of the unzipped submission. For example, a submission should look like the following:

```
/PATH/TO/RESULTS/
    |__ {visit_id_1}.txt
    |__ {visit_id_2}.txt 
         ⋮
    |__ {visit_id_N}.txt
    |__ predicted_masks/
        |__ {visit_id_1}_000.txt
        |__ {visit_id_1}_001.txt
            ⋮
```

for all the available laser scans.

Each prediction file for a scene should contain a list of instances, where an instance is: (1) the relative path to the predicted mask file, (2) the affordance class ID and (3) the float confidence score. If your method does not produce confidence scores, you can use `1.0` as the confidence score for all masks. Each line in the prediction file should correspond to one instance, and the two values above separated by a space. Thus, the filenames in the prediction files must not contain spaces.
The predicted instance mask file should provide the mask vertex indices of the provided laser scan, i.e., `{visit_id}_laser_scan.ply`, following the original order of the vertices in this file. Following [ScanNet++](https://kaldir.vc.in.tum.de/scannetpp/benchmark/docs), we use Run-Length Encoding (RLE) to efficiently encode instance masks. Each instance mask file contains the RLE representation of the predicted mask over the vertices of the laser scan `{visit_id}_laser_scan.ply`, stored as pairs of start and length values. start is the 1-indexed position of a contiguous group of vertices in the point cloud, and length is the number of vertices in that group. For example, the RLE encoding of the 12-length binary mask `[0, 1, 1, 0, 0, 1, 1, 1, 0, 1, 1, 0]` is: `'2 2 6 3 10 2'`.

Consider a scene identified by visit_id `123456`. In this case, the submission files could look like:

`123456.txt`
```
predicted_masks/123456_000.txt 5 0.7234
predicted_masks/123456_001.txt 3 0.9038
⋮
```

and `predicted_masks/123456_000.txt` could look like:
```
2123 4 2235 3 3724 12 ...
```

where `predicted_masks/123456_000.txt` contains a **single line** with the RLE-encoded instance mask, where each pair corresponds to a start index and run length.

As a utility, we provide a [script](https://github.com/SceneFun3D/scenefun3d/blob/main/eval/functionality_segmentation/create_example_submission.py) that generates an example submission by reading the ground-truth data of the validation set in the evaluation format. This example submission achieves a perfect score on the benchmark's validation set.

!!! note

    The prediction files must adhere to the vertex ordering of the original laser scan point cloud `{visit_id}_laser_scan.ply`. 
    If your pipeline alters this vertex ordering (e.g., through cropping the laser scan using the `crop_mask` data asset), 
    ensure that the model predictions are re-ordered to match the original vertex ordering before generating the prediction files.

!!! note

    Functional interactive elements which are made of transparent or reflective material resulting in bad 3D geometry 
    are excluded from the evaluation procedure.
    The model's predictions on these areas are excluded as well.

The table below lists the ID for each affordance class:

<center>

| Label | Class ID |
|--------------|----|
| *rotate*     | 1  |
| *key_press*  | 2  |
| *tip_push*   | 3  |
| *hook_pull*  | 4  |
| *pinch_pull* | 5  |
| *hook_turn*  | 6  |
| *foot_push*  | 7  |
| *plug_in*    | 8  |
| *unplug*     | 9  |

</center>



## Local evaluation

For local evaluation on the validation set, GT annotations need to be extracted in a specific format. To do this extraction, you can run:

```
python -m eval.functionality_segmentation.prepare_gt_val_data --data_dir <data_dir> --val_scenes_list <val_scenes_list_path> --out_gt_dir <out_gt_dir>
```

where:

* `--data_dir <data_dir>`: Specify the directory where data is stored
* `--val_scenes_list <val_scenes_list_path>`: Specify the list of scenes that will be used for validation as a line-separated .txt file
* `--out_gt_dir <out_gt_dir>`: Specify the directory where the processed GT annotations will be stored 

For evaluation, run:

```
python -m eval.functionality_segmentation.evaluate --pred_dir <pred_dir> --gt_dir <gt_dir>
```
where:

* `--pred_dir <pred_dir>`: Specify the directory where the model predictions are stored in the correct submission format.
* `--gt_dir <gt_dir>`: Specify the directory where the processed GT annotations are stored. 

## Online benchmark

You can upload the `.zip` file containing your model's prediction on our online benchmark which performs evaluation on the hidden test set.

Please make sure that it follows the correct [submission format](#submission-instructions). Otherwise, the submission will fail.

To create the `.zip` file for submission, you can run:
```
cd path/to/submission/files
zip -r submission.zip *
```

## Evaluation metrics

Submissions are evaluated using the mean Average Precision at the IoU thresholds of 0.25 and 0.50, denoted as AP<sub>25</sub> and AP<sub>50</sub> respectively. Additionally, they are assessed using the average across IoU thresholds from 0.5 to 0.95 with a step of 0.05 (AP). Submissions are ranked based on the AP metric.