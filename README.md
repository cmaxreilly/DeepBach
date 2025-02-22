# DeepBach
This repository contains implementations of the DeepBach model described in

*DeepBach: a Steerable Model for Bach chorales generation*<br/>
Gaëtan Hadjeres, François Pachet, Frank Nielsen<br/>
*ICML 2017 [arXiv:1612.01010](http://proceedings.mlr.press/v70/hadjeres17a.html)*


The code uses python 3.6 together with [PyTorch v1.0](https://pytorch.org/) and
 [music21](http://web.mit.edu/music21/) libraries.

For the original Keras version, please checkout the `original_keras` branch.

Examples of music generated by DeepBach are available on [this website](https://sites.google.com/site/deepbachexamples/)

## Installation

You can clone this repository, install dependencies using Anaconda and download a pretrained 
model together with a dataset  
 with the following commands:

```
git clone https://github.com/Ghadjeres/DeepBach.git;
cd DeepBach;
conda env create --name deepbach_pytorch -f environment.yml;
bash dl_dataset_and_models.sh;
```

This will create a conda env named `deepbach_pytorch`.

### music21 editor

You might need to
Open a four-part chorale. Press enter on the server address, a list of computed models should appear. Select and (re)load a model. 
[Configure properly the music editor
 called by music21](http://web.mit.edu/music21/doc/moduleReference/moduleEnvironment.html). On Ubuntu you can eg. use MuseScore:

```shell
sudo apt install musescore;
python -c 'import music21; music21.environment.set("musicxmlPath", "/usr/bin/musescore")'
```

For usage on a headless server (no X server), just set it to a dummy command:

```shell
python -c 'import music21; music21.environment.set("musicxmlPath", "/bin/true")'
```

## Usage
```
Usage: deepBach.py [OPTIONS]

Options:
  --note_embedding_dim INTEGER    size of the note embeddings
  --meta_embedding_dim INTEGER    size of the metadata embeddings
  --num_layers INTEGER            number of layers of the LSTMs
  --lstm_hidden_size INTEGER      hidden size of the LSTMs
  --dropout_lstm FLOAT            amount of dropout between LSTM layers
  --linear_hidden_size INTEGER    hidden size of the Linear layers
  --batch_size INTEGER            training batch size
  --num_epochs INTEGER            number of training epochs
  --train                         train or retrain the specified model
  --num_iterations INTEGER        number of parallel pseudo-Gibbs sampling
                                  iterations
  --sequence_length_ticks INTEGER
                                  length of the generated chorale (in ticks)
  --help                          Show this message and exit.
```

## Examples
You can generate a four-bar chorale with the pretrained model and display it in MuseScore  by 
simply running
```
python deepBach.py
```

You can train a new model from scratch by adding the `--train` flag.


## Usage with NONOTO
The command 
```
python flask_server.py
```
starts a Flask server listening on port 5000. You can then use 
[NONOTO](https://github.com/SonyCSLParis/NONOTO) to compose with DeepBach in an interactive way.

This server can also been started using Docker with:
```
docker run -p 5000:5000 -it --rm ghadjeres/deepbach
```
(CPU version), with
or
```
docker run --runtime=nvidia -p 5000:5000 -it --rm ghadjeres/deepbach
```
(GPU version, requires [nvidia-docker](https://github.com/NVIDIA/nvidia-docker).


## Usage within MuseScore
*Deprecated*

Put `deepBachMuseScore.qml` file in your MuseScore plugins directory, and run
```
python musescore_flask_server.py
```
Open MuseScore and activate deepBachMuseScore plugin using the Plugin manager.
You can then click on the Compose button without any selection to create a new chorale from 
scratch. You can then select a region in the chorale score and click on the Compose button to 
regenerated this region using DeepBach.


### Issues

### Music21 editor not set

```
music21.converter.subConverters.SubConverterException: Cannot find a valid application path for format musicxml. Specify this in your Environment by calling environment.set(None, '/path/to/application')
```

Either set it to MuseScore or similar (on a machine with GUI) to to a dummy command (on a server). See the installation section.

# Citing

Please consider citing this work or emailing me if you use DeepBach in musical projects.
```
@InProceedings{pmlr-v70-hadjeres17a,
  title = 	 {{D}eep{B}ach: a Steerable Model for {B}ach Chorales Generation},
  author = 	 {Ga{\"e}tan Hadjeres and Fran{\c{c}}ois Pachet and Frank Nielsen},
  booktitle = 	 {Proceedings of the 34th International Conference on Machine Learning},
  pages = 	 {1362--1371},
  year = 	 {2017},
  editor = 	 {Doina Precup and Yee Whye Teh},
  volume = 	 {70},
  series = 	 {Proceedings of Machine Learning Research},
  address = 	 {International Convention Centre, Sydney, Australia},
  month = 	 {06--11 Aug},
  publisher = 	 {PMLR},
  pdf = 	 {http://proceedings.mlr.press/v70/hadjeres17a/hadjeres17a.pdf},
  url = 	 {http://proceedings.mlr.press/v70/hadjeres17a.html},
}
```
