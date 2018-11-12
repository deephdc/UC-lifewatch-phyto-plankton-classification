DEEP-OC-image-classification
==============================

**Author:** [Ignacio Heredia](https://github.com/IgnacioHeredia) (CSIC)

**Project:** [DEEP Hybrid-DataCloud](https://deep-hybrid-datacloud.eu/)

This is a plug-and-play tool to train and evaluate an image classifier on a custom dataset using deep neural networks.

To start using this framework run:

```bash
git clone https://github.com/indigo-dc/image-classification-tf
cd image-classification-tf
pip install -e .
```

This project has been tested in Ubuntu 18.04 with Python 3.6.5. Further package requirements are described in the `requirements.txt` file. It is a requirement to have [Tensorflow>=1.10.0 installed](https://www.tensorflow.org/install/pip) (either in gpu or cpu mode). This is not listed in the `requirements.txt` as it [breaks GPU support](https://github.com/tensorflow/tensorflow/issues/7166). 

## Project Organization


    ├── LICENSE
    ├── README.md              <- The top-level README for developers using this project.
    ├── data
    │   ├── images                <- The original, immutable data dump.
    │   │
    │   └── data_splits            <- Scripts to download or generate data
    │
    ├── docs                   <- A default Sphinx project; see sphinx-doc.org for details
    │
    ├── docker                 <- Directory for Dockerfile(s)
    │
    ├── models                 <- Trained and serialized models, model predictions, or model summaries
    │
    ├── notebooks              <- Jupyter notebooks. 
    │
    ├── references             <- Data dictionaries, manuals, and all other explanatory materials.
    │
    ├── reports                <- Generated analysis as HTML, PDF, LaTeX, etc.
    │   └── figures            <- Generated graphics and figures to be used in reporting
    │
    ├── requirements.txt       <- The requirements file for reproducing the analysis environment, e.g.
    │                             generated with `pip freeze > requirements.txt`
    ├── test-requirements.txt  <- The requirements file for the test environment
    │
    ├── setup.py               <- makes project pip installable (pip install -e .) so imgclas can be imported
    ├── imgclas    <- Source code for use in this project.
    │   ├── __init__.py        <- Makes imgclas a Python module
    │   │
    │   ├── dataset            <- Scripts to download or generate data
    │   │   └── make_dataset.py
    │   │
    │   ├── features           <- Scripts to turn raw data into features for modeling
    │   │   └── build_features.py
    │   │
    │   ├── models             <- Scripts to train models and then use trained models to make
    │   │   │                 predictions
    │   │   └── model.py
    │   │
    │   └── tests              <- Scripts to perfrom code testing + pylint script
    │   │
    │   └── visualization      <- Scripts to create exploratory and results oriented visualizations
    │       └── visualize.py
    │
    └── tox.ini                <- tox file with settings for running tox; see tox.testrun.org

<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>

## Workflow

### 1. Data preprocessing

The first step to train yout image classifier if to have the data correctly set up. 

#### 1.1 Prepare the images

Put your images in the`./data/images` folder. If you have your data somewhere else you can use that location by setting the `image_dir` parameter in the  `./etc/config.yaml` file.

Please use a standard image format (like `.png` or `.jpg`). 

#### 1.2 Prepare the data splits

First you need add to the `./data/dataset_files` directory the following files:

| *Mandatory files* | *Optional files*  | 
|:-----------------------:|:---------------------:|
|  `classes.txt`, `train.txt` |  `val.txt`, `test.txt`, `info.txt`|

The `train.txt`, `val.txt` and `test.txt` files associate an image name (or relative path) to a label number (that has to *start at zero*).
The `classes.txt` file translates those label numbers to label names.
Finally the `info.txt` allows you to provide information (like number of images in the database) about each class. This information will be shown when launching a webpage of the classifier.

You can find examples of these files at  `./data/demo-dataset_files`.

### 2. Train the classifier

Before training the classifier you can customize the default parameter of the configuration file. To have an idea of what parameters you can change, you can explore them using the [dataset exploration notebook](./notebooks/1.0-Dataset_exploration.ipynb). This step is optional and training can be launched with the default configuarion parameters.

Once you have customized the configuration parameters in the  `./etc/config.yaml` file you can launch the training running `./imgclas/train_runfile.py`. You can monitor the training status using Tensorboard.

After training check the  [training notebook](./notebooks/2.0-Model_training.ipynb) to see how to vsualize the training statistics.

### 3. Test the classifier

You can test the classifier on a number of tasks: predict a single local image (or url), predict multiple images (or urls), merge the predictions of a multiimages of a single observation, etc. All these tasks are explained in the [computing predictions notebook](./notebooks/3.0-Computing_predictions.ipynb).

<img src="./reports/figures/predict.png" alt="predict" width="400">

You can also make and store the predictions of the `test.txt` file (if you provided one). Once you have done that you can visualize the statistics of the precision like popular metrics (accuracy, recall, precision), the confusion matrix, etc by running the [predictions statistcs notebook](./notebooks/3.1-Prediction_statistics.ipynb).

By running the [saliency maps notebook](./notebooks/3.2-Saliency_maps.ipynb) you can also visualize the saliency maps of the predicted images, which show what were the most relevant pixels in order to make the prediction.

![Saliency maps](./reports/figures/demo-saliency.png)

Finally you can launch a simple webpage to use the trained classifier on your favorite brownser.

## Launching an API

To access this packages functionality through an API you have to install the `DEEPaaS` package:

```bash
git clone https://github.com/indigo-dc/deepaas
cd deepaas
pip install -e .
```
and run `deepaas-run --listen-ip 0.0.0.0`.
From there you will be able to run training and predictions of this package  using `model_name=imgclas`.

<img src="./reports/figures/deepaas.png" alt="deepaas" width="1000"/>

