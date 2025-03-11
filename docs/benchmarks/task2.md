# Task 2: Task-driven affordance grounding

## Task description

In the second track of the SceneFun3D benchmark, we propose the following challenge:

!!! abstract "Task-driven affordance grounding"
    **TASK:** Given a text-based description of a task, the aim is to localize and segment the functional interactive elements that an agent needs to interact with to successfully accomplish the task. 

    **INPUT:** The Faro laser scan of a given scene, multiple RGB-D sequences captured with an iPad Pro, camera parameters, and a language description of a task.

    **OUTPUT:** Instance segmentation of the point cloud that corresponds to the vertices of the provided laser scan, segmenting the functional interactive elements that the agent needs to manipulate.

We highlight that, in some cases, more than one instance of functional interactive elements may correspond to a single language task description.

## Benchmark test set

For the benchmark test set, we provide input language task descriptions without any ground-truth annotations. Participants must run their models on these inputs and submit their predictions via our submission page, following the provided submission instructions. You can find the input language task descriptions [**here**](https://cvg-data.inf.ethz.ch/scenefun3d/v1/benchmark_test_descriptions.json). The format is as follows:

```
[
  {
    "visit_id": the identifier of the scene,
    "desc_id": unique id of the description,
    "description": language instruction of the task
  }, 
  ...
]
```


## Submission Instructions

Given a language task description, the participants are asked to segment functional interactive element instances that an agent needs to interact with to successfully accomplish the task. Expected result is functional interactive element masks, and confidence scores for each mask. 

We ask the participants to upload their results as a single `.zip` file, which when unzipped must contain the prediction files in the **root**. **There must not be any additional files or folders in the archive except those specified below**.

Results must be provided as a text file for each scene. Each text file should contain a line for each instance, containing the relative path to the instance mask file that specifies the mask indices, and the confidence of the prediction. The result text files must be named according to the corresponding laser scan (`visit_id`) and language description (`desc_id`), as `{visit_id}_{desc_id}.txt`. Predicted `.txt` files listing the instances of each scan must live in the root of the unzipped submission. Predicted instance mask files must live in a subdirectory named `predicted_masks/` of the unzipped submission. For example, a submission should look like the following:

```
/PATH/TO/RESULTS/
    |__ {visit_id_1}_{desc_id_1}.txt
    |__ {visit_id_2}_{desc_id_2}.txt 
         ⋮
    |__ {visit_id_N}_{desc_id_N}.txt
    |__ predicted_masks/
        |__ {visit_id_1}_{desc_id_1}_000.txt
        |__ {visit_id_1}_{desc_id_1}_001.txt
            ⋮
```

for all the available N pairs (laser scan, language description).

Each prediction file for a scene should contain a list of instances, where an instance is: (1) the relative path to the predicted mask file, (2) the float confidence score. If your method does not produce confidence scores, you can use `1.0` as the confidence score for all masks. Each line in the prediction file should correspond to one instance, and the two values above separated by a space. Thus, the filenames in the prediction files must not contain spaces.
The predicted instance mask file should provide the mask vertex indices of the provided laser scan, i.e., `{visit_id}_laser_scan.ply`, following the original order of the vertices in this file. Following [ScanNet++](https://kaldir.vc.in.tum.de/scannetpp/benchmark/docs), we use Run-Length Encoding (RLE) to efficiently encode instance masks. Each instance mask file contains the RLE representation of the predicted mask over the vertices of the laser scan `{visit_id}_laser_scan.ply`, stored as pairs of start and length values. start is the 1-indexed position of a contiguous group of vertices in the point cloud, and length is the number of vertices in that group. For example, the RLE encoding of the 12-length binary mask `[0, 1, 1, 0, 0, 1, 1, 1, 0, 1, 1, 0]` is: `'2 2 6 3 10 2'`.

Consider a scene identified by visit_id `123456`, with a language description input identified by desc_id `5baea371-b33b-4076-92b1-587a709e6c65`. In this case, the submission files could look like:

`123456_5baea371-b33b-4076-92b1-587a709e6c65.txt`
```
predicted_masks/123456_5baea371-b33b-4076-92b1-587a709e6c65_000.txt 0.7234
predicted_masks/123456_5baea371-b33b-4076-92b1-587a709e6c65_001.txt 0.9038
⋮
```

and `predicted_masks/123456_5baea371-b33b-4076-92b1-587a709e6c65_000.txt` could look like:
```
2123 4 2235 3 3724 12 ...
```

where `predicted_masks/123456_000.txt` contains a **single line** with the RLE-encoded instance mask, where each pair corresponds to a start index and run length.

As a utility, we provide a [script](https://github.com/SceneFun3D/scenefun3d/blob/main/eval/affordance_grounding/create_example_submission.py) that generates an example submission by reading the ground-truth data of the validaiton set in the evaluation format. This example achieves a perfect score on the benchmark's validation set.

!!! note

    The prediction files must adhere to the vertex ordering of the original laser scan point cloud `{visit_id}_laser_scan.ply`. 
    If your pipeline alters this vertex ordering (e.g., through cropping the laser scan using the `crop_mask` data asset), 
    ensure that the model predictions are re-ordered to match the original vertex ordering before generating the prediction files.

!!! note

    Functional interactive elements which are made of transparent or reflective material resulting in bad 3D geometry 
    are excluded from the evaluation procedure.
    The model's predictions on these areas are excluded as well.

## Local evaluation

For local evaluation on the validation set, GT annotations need to be extracted in a specific format. To do this extraction, you can run:

```
python -m eval.affordance_grounding.prepare_gt_val_data --data_dir <data_dir> --val_scenes_list <val_scenes_list_path> --out_gt_dir <out_gt_dir>
```

where:

* `--data_dir <data_dir>`: Specify the directory where data is stored.
* `--val_scenes_list <val_scenes_list_path>`: Specify the list of scenes that will be used for validation as a line-separated .txt file.
* `--out_gt_dir <out_gt_dir>`: Specify the directory where the processed GT annotations will be stored.

For evaluation, run:

```
python -m eval.affordance_grounding.evaluate --pred_dir <pred_dir> --gt_dir <gt_dir>
```
where:

* `--pred_dir <pred_dir>`: Specify the directory (unzipped) where the model predictions are stored in the [submission format](#submission-instructions).
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

Submissions are evaluated using the mean Average Precision at the IoU thresholds of 0.25 and 0.50, denoted as AP<sub>25</sub> and AP<sub>50</sub> respectively. Submissions are ranked based on the AP<sub>50</sub> metric.