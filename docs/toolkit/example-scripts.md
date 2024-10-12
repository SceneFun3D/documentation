# Example scripts

As part of the toolkit, we provide basic example scripts to demonstrate how to utilize the data assets and tools. In this section, we outline the steps to run these scripts.

## Example 1: Projecting image RGB onto the laser scan

You can run as

```
python -m examples.project_rgb_on_laser_scan --data_dir <data_dir> --visit_id <visit_id> --video_id <video_id> --coloring_asset <coloring_asset> --crop_extraneous
```

where the supported arguments are:

* `--data_dir <data_dir>`: Specify the path of the data

* `--visit_id <visit_id>`: Specify the identifier of the scene

* `--video_id <video_id>`: Specify the identifier of the video sequence

* `--coloring_asset <coloring_asset>`: Specify the RGB data asset to use for projecting the color to the laser scan. Can be `hires_wide` or `lowres_wide`. Defaults to `hires_wide`.

* `--crop_extraneous`: Specify whether to crop the extraneous points from the laser scan based on the `crop_mask` asset.

