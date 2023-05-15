# Snoring recognition
Qishu Qian  

Github link: https://github.com/qqs0616/Snoring-recognition

Edge Impulse project link:
https://studio.edgeimpulse.com/public/184526/latest

## Introduction
It is estimated that snoring affects 57% of men and 40% of women in the United States. It even occurs in up to 27% of children. These statistics indicate that snoring is common, but its severity and health effects can vary. 

Snoring can be mild, occasional and inconsequential, or it can be a sign of a serious underlying sleep-related breathing disorder. Snoring is one of the most common symptoms of obstructive sleep apnoea. Most people who snore don't realise it unless someone tells them, which is part of the reason why sleep apnoea is under-diagnosed.

<p align="center">
<img src=dl%E5%9B%BE%E7%89%87/background.png width=45% />
</p>

Therefore, I wanted to train a snoring detection model to identify snoring sounds in natural sleep sounds through a microphone to help people identify if they have a snoring habit.

## Research Question
Is it possible to train tensorflow models to detect snoring based on real-time sounds?

## Application Overview
The purpose of this application is to identify snoring during sleep. The sound input relies on the microphone - in this case the phone. When the sound is captured, a TensorFlow model for sound classification will be run (this is a deep learning model, built, trained in edge impulse). The model will use the sound input as inference, presenting the results automatically on the device output.

<p align="center">
<img src=dl%E5%9B%BE%E7%89%87/Diagrama%20de%20flux.png width=80% />
</p>

## Data
The data was sourced from the kaggle website. The dataset contains two folders - one for snoring and the other for non-snoring. Folder 1 contains the snoring sounds. It has a total of 500 sounds.

Of these 500 snoring samples, 363 samples consist of snoring sounds from children, adult men and adult women without any background sounds. The remaining 137 samples consisted of non-snoring snoring sounds with a background. It has a total of 500 sounds. The duration of each sound was 1 second.

These 500 non-snoring sound samples include background sounds that may be present near the snorer. Ten categories of non-snoring sounds were collected, with 50 samples in each category. The ten categories of sounds were the sound of a baby crying, the ticking of a clock, the opening and closing of a door, complete silence and the slight sound of a gadget vibrating a motor, the flashing of a toilet, the siren of an emergency vehicle, the sound of rain and thunderstorms, the sound of a tram, the sound of people talking and the background television news.

### Data explorer

<p align="center">
<img src=dl%E5%9B%BE%E7%89%87/data%202.png width=80% />
<p align="center">
<img src=dl%E5%9B%BE%E7%89%87/data%201.png width=80% />
</p>

## Model

### Processing blocks
In order to obtain the best accuracy on this test set, different processing blocks were first used for testing. The different characteristics of the three processing blocks led to different results.

* Spectrogram：extracts time and frequency features from a signal. It performs well on audio data for non-voice recognition use cases, or on any sensor data with continuous frequencies.

* MFE：extracts time and frequency features from a signal. However it uses a non-linear scale in the frequency domain, called Mel-scale. It performs well on audio data, mostly for non-voice recognition use cases when sounds to be classified can be distinguished by human ear.

* MFCC：extracts coefficients from an audio signal. Similarly to the Audio MFE block, it uses a non-linear scale called Mel-scale. It is the reference block for speech recognition and can also performs well on some non-human voice use cases.

The following results were obtained after testing, indicating that the MFE processing block is best suited to this dataset.

<p align="center">
<img src=dl%E5%9B%BE%E7%89%87/block.png width=100% />

### Model comparison
In the next stage, the model was tested again using the same processing blocks but with a different model. The test results showed that the 1D convolutional network is applied to the original sound waveform and the 2D convolutional network is applied to the frequency domain features. For this dataset, one convolution works better when using Spectrogram or MFCC processing blocks.

<p align="center">
<img src=dl%E5%9B%BE%E7%89%87/model%20compare.png width=100% />

### Final model
After several tests and modifications, the following model was finally chosen.
<p align="center">
<img src=dl%E5%9B%BE%E7%89%87/final%20model%201.png width=30% /><img src=dl%E5%9B%BE%E7%89%87/final%20model%202.png width=40% />

The model is the most accurate and has the following advantages:

* Efficient architecture: The model uses a compact architecture consisting of multiple Conv2D layers with a MaxPooling2D layer in between.

* Dropout regularisation: The model uses Dropout regularisation to prevent overfitting by randomly discarding some neurons during training.

* Adam optimiser: Adam is a popular optimiser, known for its efficiency and fast convergence.

* Good activation functions: The model uses the ReLU activation function in the Conv2D layer and the softmax activation function in the output layer. reLU can help alleviate the gradient disappearance problem. softmax is commonly used for multi-class classification tasks.
<p align="center">
<img src=dl%E5%9B%BE%E7%89%87/final%20model%20result.png width=80% />

## Experiments

To further understand the factors affecting the accuracy of the final model, the following results were obtained by modifying the parameters of Auto Balance, Mask Time Period, Mask Frequency Band, Learning Rate, and Validation Set:
<p align="center">
<img src=dl%E5%9B%BE%E7%89%87/factors%20affecting.png width=100% />

Based on the accuracy derived from varying the conditional training, it can be seen that for this mfe-conv2d-ad2 model, automatic balancing, Mask time bands, Mask frequency bands, increasing the learning rate, and increasing the validation set will cause the accuracy to decrease. Increasing noise and training cycles will not affect accuracy.

## Results and Observations

### Experimental results
By using egde impulse, the most accurate model was deployed to the mobile phone using QR code. 

Experiments were carried out on models deployed to mobile phones using snoring videos searched online and simulated snoring by real people. The experiments can be viewed at the following link:https://youtu.be/jYlqdWWQwXU
<p align="center">
<img src=dl%E5%9B%BE%E7%89%87/test.png width=29% />     <img src=dl%E5%9B%BE%E7%89%87/test%202.png width=30% />

After several experiments, it was found that most of the snoring sounds within the video were basically recognised, but the real simulated snoring sounds were not always recognised.

The results of the experiments are also affected by the size of the sound, the distance from the microphone, the length of the sound, the ambient sound and the time difference, the results are somewhat inaccurate.

### Future Development
In order to refine the snoring recognition system and apply it to real life, the data set can continue to be added to improve the accuracy of the model. The system may be able to do this in the future to help people monitor the length of time they snore. It could also be linked to a vibrating motor to give a silent alarm when the user snores, monitoring the user's sleep and improving the quality of sleep for the user and those around them.
<p align="center">
 <img src=dl%E5%9B%BE%E7%89%87/future%20development.png width=60% />


## Bibliography
1. Davidson, John, and Dueep J. Singh. How to Cope with Snoring - Easy Ways to Cure and Manage Sleep Apnea. JD-Biz Corp Publishing, 2013.

2. Edgeimpulse.com. (2022). Getting Started - Edge Impulse Documentation. [online] Available at: https://docs.edgeimpulse.com/docs/.

3. www.kaggle.com. (n.d.). Snoring. [online] Available at: https://www.kaggle.com/datasets/tareqkhanemu/snoring [Accessed 27 Apr. 2023].

4. T. H. Khan, "A deep learning model for snoring detection and vibration notification using a smart wearable gadget," Electronics, vol. 8, no. 9, article. 987, ISSN 2079-9292, 2019.

5. Volpi, David O. Wake Up! You’re Snoring : A Guide to Diagnosis and Treatment. Iuniverse, Inc, 2003.

## Declaration of Authorship
I, Qishu Qian, confirm that the work presented in this assessment is my own. Where information has been derived from other sources, I confirm that this has been indicated in the work.

Qishu Qian

2023.4.27
