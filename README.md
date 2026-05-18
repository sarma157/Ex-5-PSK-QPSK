# Ex: 5- PSK and QPSK
## AIM:
Write a simple Python program for the modulation and demodulation of PSK and QPSK.
## TOOLS REQUIRED:
Google Colab
## PROGRAM:
#### PSK:
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

def lpf(x, fc, fs):
    b, a = butter(4, fc/(0.5*fs), 'low')
    return lfilter(b, a, x)

fs, fc, br, T = 1000, 50, 10, 1
t = np.arange(0, T, 1/fs)
bd = fs // br

bits = np.random.randint(0, 2, br)
msg = np.repeat(bits, bd)

carrier = np.sin(2*np.pi*fc*t)

bpsk = np.sin(2*np.pi*fc*t + np.pi*msg)

demod = lpf(bpsk * carrier, fc, fs)
decoded = (demod[::bd] < 0).astype(int)

plt.figure(figsize=(10,9))
plt.suptitle("NAME : SARMASARAN.M\nREG NO : 212224060239",
             fontsize=12, fontweight='bold')

plt.subplot(4,1,1)
plt.plot(t, msg)
plt.title("Message Signal")
plt.grid(True)

plt.subplot(4,1,2)
plt.plot(t, carrier)
plt.title("Carrier Signal")
plt.grid(True)

plt.subplot(4,1,3)
plt.plot(t, bpsk)
plt.title("BPSK Modulated Signal")
plt.grid(True)

plt.subplot(4,1,4)
plt.step(range(len(decoded)), decoded, where='mid')
plt.title("Decoded Bits")
plt.grid(True)

plt.tight_layout(rect=[0,0,1,0.93])
plt.show()
```
#### QPSK:
```
import numpy as np
import matplotlib.pyplot as plt

fs = 1000
fc = 10
T = 1
t = np.arange(0, T, 1/fs)

bits = np.array([1,0, 1,1, 1,1, 1,0])
symbols = bits.reshape(-1, 2)

symbol_samples = len(t) // len(symbols)

qpsk = np.zeros(len(t))

for i, pair in enumerate(symbols):
    I = 1 if pair[0] == 1 else -1
    Q = 1 if pair[1] == 1 else -1

    ts = t[i*symbol_samples:(i+1)*symbol_samples]
    qpsk[i*symbol_samples:(i+1)*symbol_samples] = \
        I*np.cos(2*np.pi*fc*ts) + Q*np.sin(2*np.pi*fc*ts)

decoded = []

for i in range(len(symbols)):
    ts = t[i*symbol_samples:(i+1)*symbol_samples]
    segment = qpsk[i*symbol_samples:(i+1)*symbol_samples]

    I_demod = np.sum(segment * np.cos(2*np.pi*fc*ts))
    Q_demod = np.sum(segment * np.sin(2*np.pi*fc*ts))

    decoded.append(1 if I_demod > 0 else 0)
    decoded.append(1 if Q_demod > 0 else 0)

plt.figure(figsize=(10,8))
plt.suptitle("NAME : SARMASARAN.M\nREG NO : 212224060239",
             fontsize=12, fontweight='bold')

plt.subplot(3,1,1)
plt.step(range(len(bits)), bits, where='mid')
plt.title("Input Binary Data")
plt.ylim(-0.5,1.5)
plt.grid(True)

plt.subplot(3,1,2)
plt.plot(t, qpsk)
plt.title("QPSK Modulated Signal")
plt.grid(True)

plt.subplot(3,1,3)
plt.step(range(len(decoded)), decoded, where='mid')
plt.title("Demodulated Output")
plt.ylim(-0.5,1.5)
plt.grid(True)

plt.tight_layout(rect=[0,0,1,0.93])
plt.show()
```
## OUTPUT WAVEFORM:
#### PSK:
<img width="815" height="738" alt="image" src="https://github.com/user-attachments/assets/fda37ef2-d28e-44e8-97d7-d973a24c8f42" />

#### QPSK:
<img width="821" height="660" alt="image" src="https://github.com/user-attachments/assets/0e5cc69f-e6ac-4470-ae05-975273129331" />

## RESULT:
The PSK and QPSK signals were successfully modulated and demodulated using Google Colab.
