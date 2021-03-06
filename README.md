# kapre
Keras Audio PREprocessors

# Installation
```bash
$ git clone https://github.com/keunwoochoi/kapre.git
cd kapre
$ python setup.py install
```

# Usage

## Spectrogram
```python
from keras.models import Sequential
from kapre.TimeFrequency import Spectrogram
from kapre.Utils import Normalization2D

# input: stereo channel, 1-sec audio signal
input_shape = (2, 44100) 
sr = 44100
model = Sequential()
# A "power-spectrogram in decibel scale" layer
model.add(Spectrogram(n_dft=512, n_hop=256, input_shape=src_shape,
                      border_mode='same', power=2.0,
                      return_decibel=True, trainable_kernel=False,
                      name='trainable_stft'))
# If you wanna normalise it per-frequency
model.add(Normalization2D(str_axis='freq')) # or 'time', 'channel', 'batch', 'data_sample'
# Then add your model
# E.g., model.add(some convolution layers...)
```

## Mel-spectrogram
```python
from keras.models import Sequential
from kapre.TimeFrequency import Melspectrogram
from kapre.Utils import Normalization2D

# 6 channels (!), maybe 1-sec audio signal
input_shape = (6, 44100) 
sr = 44100
model = Sequential()
# A mel-spectrogram layer with
# no decibel conversion for some reasons and (return_decibel=False)
# amplitude, not power (power=1.0)
model.add(Melspectrogram(n_dft=512, n_hop=256, input_shape=src_shape,
                         border_mode='same', sr=sr, n_mels=128,
                         fmin=0.0, fmax=sr/2, power=1.0,
                         return_decibel=False, trainable_fb=False,
                         trainable_kernel=False
                         name='trainable_stft'))
# If you wanna normalise it per-channel
model.add(Normalization2D(str_axis='channel')) # or 'freq', 'time', 'batch', 'data_sample'
# Then add your model
# E.g., model.add(some convolution layers...)
```

# More info
Please read docstrings at this moment.

# Plan

  - [x] `TimeFrequency`: Spectrogram, Mel-spectrogram
  - [x] `Utils`: AmplitudeToDB, Normalization2D, A-weighting
  - [ ] `DataAugmentation`: Random-gain Gaussian noise, random cropping 1D/2D, Dynamic Range Compression1D
  - [ ] `Filterbank`: Parameteric Filter bank
  - [ ] `Decompose`: Harmonic-Percussive separation
  - [ ] `InverseSpectrogram`
  - [ ] `TimeFrequency`: Harmonic/Spiral representations, chromagram


# Citation
```
@article{choi2016kapre,
  title={kapre: Keras Audio PREprocessors},
  author={Choi, Keunwoo},
  journal={GitHub repository: https://github.com/keunwoochoi/kapre},
  year={2016}
}
```