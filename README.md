# Speech Feature Kit (Alpha-version)

A Python wrapper for convenient speech feature extraction

## Installation 

```
pip install speech-features-kit
```

## Dependencies

1. numpy
2. scipy

## Credits

[aishoot's Project](https://github.com/aishoot/Speech_Feature_Extraction)

## Sample code

Example 1:

```python

import speech_features_kit.MFCC.MFCC as mf

import scipy.io.wavfile as wav

(rate,sig) = wav.read("output/english.wav")
mfcc_feat = mf.mfcc(sig, rate)
d_mfcc_feat = mf.delta(mfcc_feat, 2)
fbank_feat = mf.logfbank(sig, rate)

print(fbank_feat[1:3,:])

```

Example 2:

```python

import wave
import pylab as pl
import numpy as np
import speech_features_kit.Volume.Volume as vp

# ============ test the algorithm =============
# read wave file and get parameters.
fw = wave.open('../data/english.wav','rb')
params = fw.getparams()
print(params)
nchannels, sampwidth, framerate, nframes = params[:4]
strData = fw.readframes(nframes)
waveData = np.fromstring(strData, dtype=np.int16)
waveData = waveData*1.0/max(abs(waveData))  # normalization
fw.close()

# calculate volume
frameSize = 256
overLap = 128
volume11 = vp.calVolume(waveData,frameSize,overLap)
volume12 = vp.calVolumeDB(waveData,frameSize,overLap)

# plot the wave
time = np.arange(0, nframes)*(1.0/framerate)
time2 = np.arange(0, len(volume11))*(frameSize-overLap)*1.0/framerate
pl.subplot(311)
pl.plot(time, waveData)
pl.ylabel("Amplitude")

pl.subplot(312)
pl.plot(time2, volume11)
pl.ylabel("absSum")

pl.subplot(313)
pl.plot(time2, volume12, c="g")
pl.ylabel("Decibel(dB)")
pl.xlabel("time (seconds)")
pl.show()

```