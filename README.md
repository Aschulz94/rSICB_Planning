# rSICB_Planning


 

<p align="center">

  <h1 align="center">Regional Biology Conference Hosting Guide & Documents
    <br>
    <a href='https://arxiv.org/abs/2304.10417'>
    <img src='https://img.shields.io/badge/arxiv-report-red' alt='ArXiv PDF' 
  </h1>
  <p align="center">
    <a href="https://www.schulzscience.com/"><strong>Andrew Schulz*</strong></a>
    ·
    <a href="https://prnvkhndlwl.github.io/"><strong>Pranav Khandelwal</strong></a>

  </p>
  <h2 align="center">Society of Integrative and Comparative Biology </h2>
 <div align="center">Division of Comparative Biomechanics and Vertebrate Morphology Hosting Guide" </div>
 <div align="center">
  </div>
</p>
<p float="center">
  <div align="center">
  <img src="assets/sinc_tsr.gif" />
  </div>
</p>

<!-- | Paper Video                                                                                                | Qualitative Results                                                                                                |
|------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| [![PaperVideo](https://img.youtube.com/vi/vidid/0.jpg)](https://www.youtube.com/) | -->

## Features

Planning documents and templates for hosting a rSICB confernece. There is a zip folder attached with the following items: 
Overview of Regional SICB Folders: 

Conference Website: Includes links to previous conference websites and information on resources to create a quick and fast conference website on hosting institutions’ webpages 

Day of Documents: This includes examples and templates of all documents that are necessary for the day of the conference. This includes organizing lists, templates for creating poster session layouts, Nametag templates, and a parking template to be emailed out to participants.  

Email Templates: This folder includes examples of Day, Week, & Month out reminder emails for all participants as well as an initial email template to the masses as well as examples for how to have presenters upload their documents before the conference.  

Feedback: This folder contains a detailed report of past rSICB conference feedback given post meeting 

Registration: This folder includes a PDF template for a questionnaire for the hosting institutions and the participants and contact lists for attending members and lab groups in the southeast.  

Schedule Documents: This folder includes the past rSICB scheduling templates for both in-person and virtual options for the regional SICB meetings. It also includes templates for scheduling at a glance for the final day of the conference for organizers to remain on schedule.  


<h2 align="center">3D Scanning and 3D Print files</h2>

<details>
  <summary>Details</summary>
In the folder, we have provided both raw 3D scan files as ready-to-print STL files of the Pel's scaly-tailed squirrel.

Access the files here:
```bash
git clone https://github.com/Aschulz94/ScalySquirrel
```

After that do this to install DistillBERT:

```shell
cd deps/
git lfs install
git clone https://huggingface.co/distilbert-base-uncased
cd ..
```

Install the requirements using `virtualenv` :
```bash
# pip
source scripts/install.sh
```
You can do something equivalent with `conda` as well.
</details>



[comment]: <> (## Running the Demo)

[comment]: <> (We have prepared a nice demo code to run SINC on arbitrary videos. )



<h2 align="center">DLC Training, Labeling, and Raw Videos</h2>

 <details>
  <summary>Details</summary>

<div align="center"><em>There is no need to do this step if you have followed the instructions and have done it for TEACH. Just use the ones from TEACH.</em></div>

<div align="center"><h3>Step 1: Data Setup</h3></center></div>

Download the data from [AMASS website](https://amass.is.tue.mpg.de). Then, run this command to extract the amass sequences that are annotated in babel:

```shell
python scripts/process_amass.py --input-path /path/to/data --output-path path/of/choice/default_is_/babel/babel-smplh-30fps-male --use-betas --gender male
```

Download the data from [TEACH website](https://teach.is.tue.mpg.de), after signing in. The data SINC was trained was a processed version of BABEL. Hence, we provide them directly to your via our website, where you will also find more relevant details. 
Finally, download the male SMPLH male body model from the [SMPLX website](https://smpl-x.is.tue.mpg.de/). Specifically the AMASS version of the SMPLH model. Then, follow the instructions [here](https://github.com/vchoutas/smplx/blob/main/tools/README.md#smpl-h-version-used-in-amass) to extract the smplh model in pickle format.

The run this script and change your paths accordingly inside it extract the different babel splits from amass:

```shell
python scripts/amass_splits_babel.py
```

Then create a directory named `data` and put the babel data and the processed amass data in.
You should end up with a data folder with the structure like this:

```
data
|-- amass
|  `-- your-processed-amass-data 
|
|-- babel
|   `-- babel-teach
|       `...
|   `-- babel-smplh-30fps-male 
|       `...
|
|-- smpl_models
|   `-- smplh
|       `--SMPLH_MALE.pkl
```

Be careful not to push any data! 
Then you should softlink inside this repo. To softlink your data, do:

`ln -s /path/to/data`

You can do the same for your experiments:

`ln -s /path/to/logs experiments`

Then you can use this directory for your experiments.

<div align="center"><h3>Step 2 (a): Training</h3></center></div>

To start training after activating your environment. Do:

```shell
python train.py experiment=baseline logger=none
```

Explore `configs/train.yaml` to change some basic things like where you want
your output stored, which data you want to choose if you want to do a small
experiment on a subset of the data etc.
You can disable the text augmentations and using `single_text_desc: false` in the
model configuration file. You can check the `train.yaml` for the main configuration
and this file will point you to the rest of the configs (eg. `model` refers to a config found in
the folder `configs/model` etc.).

<div align="center"><h3>Step 2 (b): Training MLD</h3></center></div>

Prior to running this code for MLD please create and activate an environment according to their [repo](https://github.com/ChenFengYe/motion-latent-diffusion). Please do the `1. Conda Environment` and `2. Dependencies` out of the steps in their repo.

```shell
python train.py experiment=some_name run_id=mld-synth0.5-4gpu model=mld data.synthetic=true data.proportion_synthetic=0.5 data.dtype=seg+seq+spatial_pairs machine.batch_size=16 model.optim.lr=1e-4 logger=wandb sampler.max_len=150
```

</details>
 
## Citation

```bibtex
@inproceedings{SINC:ICCV:2022,
  title={{SINC}: Spatial Composition of {3D} Human Motions for Simultaneous Action Generation},
  author={Athanasiou, Nikos and Petrovich, Mathis and Black, Michael J. and Varol, G\"{u}l },
  booktitle = {ICCV},
  year = {2023}
}

```
## License
This code is available for all researchers to use.

## References
Many parts of this code were based on the official implementation of [DeepLabCut](https://github.com/DeepLabCut/DeepLabCut).

## Contact

This code repository was implemented by [Andrew Schulz](https://www.schulzscience.com/), [Mrudul Chellapurath](https://www.linkedin.com/in/mrudul-chellapurath-31530292/), and [Pranav Khandelwal](https://prnvkhndlwl.github.io/).

Give a ⭐ if you like.




