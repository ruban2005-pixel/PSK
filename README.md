# PSK
# Aim
Write a Python program for the modulation and demodulation of PSK.
# Tools required
- Python IDE with Numpy and Scipy
- Google Colab (for running the program)
# Program

```python
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Butterworth low-pass filter for demodulation
def butter_lowpass_filter(data, cutoff, fs, order=5):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff / nyquist
    b, a = butter(order, normal_cutoff, btype='low', analog=False)
    return lfilter(b, a, data)

# Parameters
fs = 1000  # Sampling frequency
f_carrier = 50  # Carrier frequency
bit_rate = 10  # Data rate
T = 1  # Total time duration
t = np.linspace(0, T, int(fs * T), endpoint=False)

# Message signal (binary data)
bits = np.random.randint(0, 2, bit_rate)
bit_duration = fs // bit_rate
message_signal = np.repeat(bits, bit_duration)

# PSK Modulation (0 -> 0 phase, 1 -> 180° phase shift)
carrier = np.sin(2 * np.pi * f_carrier * t)
psk_signal = np.sin(2 * np.pi * f_carrier * t + np.pi * message_signal)

# PSK Demodulation
demodulated = psk_signal * carrier
filtered_signal = butter_lowpass_filter(demodulated, f_carrier, fs)
decoded_bits = (filtered_signal[::bit_duration] < 0).astype(int)

# Plotting
plt.figure(figsize=(12, 8))
plt.subplot(4, 1, 1)
plt.plot(t, message_signal, label='Message Signal (Binary)', color='b')
plt.title('Message Signal')
plt.grid(True)

plt.subplot(4, 1, 2)
plt.plot(t, carrier, label='Carrier Signal', color='g')
plt.title('Carrier Signal')
plt.grid(True)

plt.subplot(4, 1, 3)
plt.plot(t, psk_signal, label='PSK Modulated Signal', color='r')
plt.title('PSK Modulated Signal')
plt.grid(True)

plt.subplot(4, 1, 4)
plt.step(np.arange(len(decoded_bits)), decoded_bits, label='Decoded Bits', color='r', marker='x')
plt.title('Decoded Bits')
plt.grid(True)
plt.legend()
plt.tight_layout()
plt.show()
```


## Sample Output:

The following waveforms will be displayed:

1. **Message Signal (Binary):** This is the original data in binary format (0s and 1s).
2. **Carrier Signal:** A high-frequency sinusoidal signal used to modulate the message.
3. **PSK Modulated Signal:** The result of modulating the binary data onto the carrier using PSK.
4. **Decoded Bits:** The output after demodulating the signal and recovering the original binary data.
![435445373-df2c600e-8109-454e-ab52-5dd50a611a58](https://github.com/user-attachments/assets/a15f2422-b2b8-4934-b706-dd5501b63c23)

# Results
The experiment successfully demonstrated digital-analog conversion using PSK. The modulation and demodulation process was visually confirmed by comparing the message signal and the decoded bits after transmission through the PSK system.

