# Evaluation guidelines

The SceneFun3D training and validation sets can be utilized to train and evaluate models locally. For evaluation on the validation set, we provide evaluation scripts in the SceneFun3D toolkit for each task [here](https://github.com/SceneFun3D/scenefun3d/blob/main/eval). 

Currently, the benchmark is evaluated using [version 0.2.0](site:changelog/#0.2.0) of the dataset.

**Benchmark results** are evaluated on the hidden test set for which we do not provide the ground-truth annotations. The benchmark is hosted on EvalAI and can be found [here](https://eval.ai/web/challenges/challenge-page/2466/overview). 

Prior to making a submission on the evaluation benchmark, make sure the submission is in the correct format by following the instructions outlined for each task. Otherwise, the submission will fail.

On the benchmark submission page, you can submit to both the validation and test splits. Since evaluation on the validation split can also be performed locally, this step acts as an optional sanity check. **Only test split submissions count towards official benchmarking**.

In the sections below, you can find information about evaluation and benchmark submissions and description for each task:

* [Task 1: Functionality segmentation](site:benchmarks/task1)
* [Task 2: Task-driven affordance grounding](site:benchmarks/task2)
* [Task 3: Motion estimation](site:benchmarks/task3)
