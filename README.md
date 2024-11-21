import numpy as np
import scipy.signal as signal
import matplotlib.pyplot as plt

# Define filter specifications
fs = 1000  # Sampling frequency in Hz
wp = 100   # Passband frequency in Hz
ws = 200   # Stopband frequency in Hz
Rp = 1     # Passband ripple in dB
As = 40    # Stopband attenuation in dB

# Normalize the frequencies
nyquist = 0.5 * fs
wp_norm = wp / nyquist
ws_norm = ws / nyquist

# Design the Chebyshev Type I analog filter (low-pass)
order, wn = signal.cheby1(N=4, rp=Rp, Wn=wp_norm, btype='low', analog=True)

# Transform the analog filter to digital using the Bilinear Transform
b, a = signal.bilinear(order=order, Wn=wn, fs=fs)

# Frequency response of the digital filter
w, h = signal.freqz(b, a, worN=8000)

# Plot the frequency response
plt.figure()
plt.semilogx(w, 20 * np.log10(abs(h)), 'b')
plt.title('Chebyshev Type I Filter Frequency Response')
plt.xlabel('Frequency [Hz]')
plt.ylabel('Amplitude [dB]')
plt.grid()
plt.show()

# Optionally, you can filter a signal here using the digital filter coefficients `b` and `a`
