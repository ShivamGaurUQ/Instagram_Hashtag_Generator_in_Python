# Overview
1. Generate hashtags for Instagram images using soft-attention mechanism as described in the paper Show, Attend and Tell: Neural Image Caption Generation with Visual Attention.(https://arxiv.org/abs/1502.03044)
2. Explore the possibility of using the hashtags to generate narrative caption for the image.
3. More details about the project are included in the 'Overview.pdf'

# Methodology
1. First, generate hashtags for an input image by using attention model.

Attention mechanism focusses on important features of the image. Image features are extracted from lower CNN layers (ENCODER). The decoder uses a LSTM that is responsible for producing a hashtag (one word) at each time step t, which is conditioned on a context vector zt, the previous hidden state ht and the previously generated hashtag. Soft attention mechanism is used to generate hashtags. 

The entire network was trained from end-to-end. InceptionV3 (pretrained on Imagenet) was used to classify images in the HARRISON dataset and features were extracted from the last convolutional layer.To generate hashtags, the CNN-LSTM model with embedding dimension size of 256, 512 GRU(LSTM) units and Adam optimizer was trained for 40 epochs on a GEForce GTX Titan GPU with each epoch taking about 2.5 hours.
The model was trained on 80 percent of data (around 43K images) while the remaining was used for testing.


2. Second, leverage the hashtag from previous stage to produce a short story by using a character-level language model.

The character - level RNN model is trained on ‘PersonaBank’ corpus which is a collection of 108 personal narratives from various weblogs. The corpus is described in the paper: PersonaBank: A Corpus of Personal Narratives and Their Story Intention Graphs (https://arxiv.org/abs/1708.09082). These stories cover a wide range of topics from romance and wildlife to travel and sports.
Out of 108 stories, 55 are positive stories while the remaining are negative. Average length of story in the corpus is 269 words.

The language model is trained using a standard categorical cross-entropy loss.The language model was trained for 100 epochs with word embedding dimension size of 1024, 2 LSTM layers, softmax activation function, RMSProp optimizer and a learning rate of 0.01on a GEForce GTX Titan GPU to generate stories with 500 characters in length.





# Hashtag generation using Tensorflow and Pytorch

1. Hashtags generation using soft attention model (Show, Attend Tell)(Tensorflow)

-- Harrison dataset is used which is preprocessed and split into (80:10:10) train/validation/test ratio by preprocess.py file
-- Soft-attention model is trained using tensorflow_attention.py in the Show, attend and Tell (Soft Attention) directory.
-- The code in the tensorflow_attention.py is adapted from "https://github.com/tensorflow/docs/blob/master/site/en/r2/tutorials/text/image_captioning.ipynb"
-- The test data results, loss_epoch, perplexity_epoch readings obtained from the after training the model are saved in the directory Show, attend and Tell (Soft Attention) directory.
-- The model requires Keras, Tensorflow and Python 3.6 to train. The requirements can be installed in anaconda environment using environment_tensorflow.yaml


2.  To generate hashtags using soft attention model (Show, Attend Tell)(Pytorch)

-- Harrison dataset is used which is preprocessed and split into (80:10:10) train/validation/test ratio by preprocess.py file
-- CNN-LSTM model (defined in model.py) is used to train the model using train.py in the Show and tell directory.
-- Run build_vocab.py --> train.py to train the model. Run sample.py to generate test data results.
-- The code is adapted from "https://github.com/yunjey/pytorch-tutorial/tree/master/tutorials/03-advanced/image_captioning"
-- Requires PyTorch setup which can be done by creating a pytorch environment using environment_pytorch.yaml

3. To generate hashtags using Multi-label image classification

-- train2.py is used to generate hashtags using AlexNet implemented using Keras by running in tensorflow environment.
-- The test data results, loss_epoch (train and validation), accuracy_epoch(train and validation) readings obtained after training and validation of the model are saved in the Multi-label image classification directory.

4. Character-level language model using RNN (to generate story from hashtag)

-- model trained using tensorflow
-- model trained on PersonaBank corpus saved in new_persona.txt. (Preprocessed version of persona data) 
-- train model by running char_lang_model.py in tensorflow environment.
