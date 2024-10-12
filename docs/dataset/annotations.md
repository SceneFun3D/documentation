# Annotation data

Currently, SceneFun3D provides three categories of annotated data:

* Functional interactive element annotations
* Language task descriptions 
* Motion annotations

In the sections below, we describe the provided data for each category. Each annotation is  accompanied with a unique identifier of the form `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.


## Functional interactive elements

The format of the functional interactive element annotations can be seen below.

```json title="annotations.json"
{
  "visit_id": the identifier of the scene,
  "annotations": [
    {
      "annot_id": unique id of the annotation,
      "indices": the mask indices of the original laser scan point cloud ({visit_id}_laser_scan.ply) that comprise the functional interactive element instance,
      "label": affordance label
    },
    ...
  ]
}
```


Currently, the SceneFun3D dataset contains interactions with the following **affordance labels**:

<!-- <table style="text-align: center"> -->

| Label       | Description                                                                                           |
|-------------|-------------------------------------------------------------------------------------------------------|
| *rotate*      | functionalities that are adjusted by a rotary switch knob, e.g., thermostat                            |
| *key_press*   | surfaces that consist of keys that can be pressed, e.g., remote control, keyboard                       |
| *tip_push*    | functionalities that can be triggered by the tip of the finger, e.g., light switch                      |
| *hook_pull*   | surfaces that can be pulled by hooking up fingers, e.g., fridge handle                                  |
| *pinch_pull*  | surfaces that can be pulled through a pinch movement, e.g., drawer knob                                 |
| *hook_turn*   | surfaces that can be turned by hooking up fingers, e.g., door handle                                    |
| *foot_push*   | surfaces that can be pushed by foot, e.g., foot pedal of a trash can                                    |
| *plug_in*     | surfaces that comprise electrical power sources                                                        |
| *unplug*      | removing a plug from a socket                                                                          |


<!-- </table> -->

In addition to these affordance categories, we have annotated functionalities whose geometry or the parent object’s geometry is not well-captured
in the laser scans (e.g., reflective or transparent surfaces) under the label *`exclude`*. These cases are excluded during the evaluation process.

## Language task descriptions

The format of the natural language task descriptions can be seen below.

```json title="descriptions.json"
{
  "visit_id": the identifier of the scene,
  "descriptions": [
    {
      "desc_id": unique id of the description,
      "annot_id": [
        list of the associated annotation id's in the *annotations.json* file
      ],
      "description": language instruction of the task
    },
    ...
  ]
}
```

We highlight that, in some cases, more than one instance of functional interactive elements may correspond to a single language task description.

## Motion annotations

The format of the motion annotations can be seen below.

```json title="motions.json"
{
  "visit_id": the identifier of the scene,
  "motions": [
    {
      "motion_id": unique id of the description,
      "annot_id": the associated annotation id in the *annotations.json* file,
      "motion_type": motion type (rotational or translational),
      "motion_dir": motion direction (three element array),
      "motion_origin_idx": point index of the original laser scan point cloud ({visit_id}_laser_scan.ply) which comprises the motion axis origin ,
      "motion_viz_orient": motion visualization orientation (optional)`
    },
    ...
  ]
}
```