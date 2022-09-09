# Changes made by Birk in musiGAN_colab.ipynb
**unsolved** stuff is bold, while *solved* stuff is italic


## 2022-09-09, Friday
* deleted unnecessary code (see solved *issues* from prev. day)
* reorganized notebook
  * new order: 
    * data prep: now includes `Pianoroll` class + `lpd5` initialization
    * architecture: now includes all model code
    * train&eval classes: now includes training support, GANTraining and eval support in that order
    * train&eval: actual execution of training and tests


## 2022-09-08, Thursday
* Deleted all scaleGAN specific code

* Data Preparation
  * class `Pianoroll`
	* added labels to dataset
  * `lpd5` initialization
    * added many additional attributes to lpd5 which other functions and classes use
    * **question:** Where should these attribute additions go? Multiple are custom to this dataset.

* Models
  * classes `MusiGen` and `MusiDis`
    * changed a couple of layers / layer sizes to account for lpd5 now having 48x84 bars instead of 96x72 bars.
    * commented exact changes and how to reverse them
    * commented out `Tanh()` layer at MusiGen output
      * assumed to aleviate some gradient explosion
      * **issue:** effect not rigorously tested yet
      * **question:** what were exactly the theoretical motivations?

* Training evalutation
  * function `generator_goodness()`
    * bug found: 
      * issue: torch.mean() over batch reduces tensor to one value, 
      * should be: first, only mean over all batch images, not over pixels
      * fix: reshapes, shape-checks and torch.mean( , dim = 0) in first step

* Training
  * class `GANTraining`
    * changed optimizer learning rate `lr` and `betas`
      * **request:** When saving training parameters, optimizer info should be included as well.

* Model evaluation
  * added image to audio conversion
    * function `quick_test()`
      * Now, for each example image, a playable audio file is displayed.
      * The audio files are stored under `"../data_preparation/prepared_data/lpd5_full_4bars/audio_examples/{timestamp}.wav"`.
      * **request:** A common save location for training results would be better!
    * *issue:* Kai's audio template code not yet removed!
    * *issue:* imports not at the right location yet!
    * *issue:* Download code at bottom of page not yet removed!
    * **question:** are the fluidsynth and muspy downloads now permanently stored on Colab or do they need to be reloaded every time?
