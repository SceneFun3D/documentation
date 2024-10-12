# The SceneFun3D dataset

The SceneFun3D dataset aims to catalyze research towards understanding *functionalities* and *affodances* in 3D environments. To this end, it provides fine-grained functional interaction annotations in high-fidelity reconstructions of real-world indoor spaces.

## Annotations

The dataset contains three categories of annotations aiming to encourage research on the following questions:

* **Where** are the functionalities located in 3D indoor environments and what actions they afford?
* **What** purpose do the functionalities serve in the scene context?
* **How** to interact with the functional elements?

### Functional interactive elements

<video controls loop poster="https://scenefun3d.github.io/static/images/task1_pic.png">
  <source src="https://scenefun3d.github.io/static/images/task1_vid.mp4" type="video/mp4">
</video>

SceneFun3D contains annotations of **functional interactive elements** (e.g., knobs, handles, buttons) which comprise a *3D instance mask* 
on the high-resolution 3D geometry reconstruction followed by an *affordance label* that described the action that the element affords.


### Natural language task descriptions

<video controls loop poster="https://scenefun3d.github.io/static/images/task2_pic.png">
  <source src="https://scenefun3d.github.io/static/images/task2_vid.mp4" type="video/mp4">
</video>


Beyond localizing the functionalities, it is crucial to understand the purpose that they serve in the scene context for task-oriented scene interactions. The dataset provides *diverse*, *free-form* **language descriptions of tasks** that involve interacting with the annotated functionalities, which require *common sense* and *multi-hop reasoning* to interpret and handle effectively.

### Motions

<video controls loop poster="https://scenefun3d.github.io/static/images/task3_pic.png">
  <source src="https://scenefun3d.github.io/static/images/task3_vid.mp4" type="video/mp4">
</video>

Additionally, the dataset provides **motion annotations** required to manipulate the interactive elements.

## 3D indoor environments

SceneFun3D builds upon the [ARKitScenes](https://github.com/apple/ARKitScenes) dataset, which offers a large-scale set of indoor scenes
captured with a professional stationary Faro laser scanner and a commodity iPad device. To enhance its usability for 3D scene understanding applications, 
we perform the following improvements and provide the related assets:

* Faro-ARKit coordinate system transformation
* Accurate COLMAP-estimated camera poses synced with the iPad video frames and Faro-rendered depth maps
* Increased number of registered high-resolution iPad RGB frames in the Faro coordinate system
* Easy-to-use data parser

<p align="center">
  <a href="">
    <img src="assets/scenefun3d_scenes.png" alt="Logo" width="100%">
  </a>
</p>
