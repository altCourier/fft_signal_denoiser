# Before beginning

Before you can denoise a signal, you have to understand the data the HackRF gives you. Unlike a microphone that just records amplitude over time (1D data), an SDR records IQ data (In-phase and Quadrature).

IQ data represents the signal as a series of complex numbers.

The fundamental premise of FFT denoising relies on how energy is distributed.

* Man-made RF signals (like Bluetooth, FM radio, or a drone control link) are highly structured. Their energy is tightly concentrated into specific, narrow frequency bands.

* The noise such as thermal noise from the HackRF’s amplifier, cosmic background radiation, generic static is usually White Noise or Gaussian Noise. This means its energy is randomly and evenly smeared across every single frequency at a relatively low power level.

In the time domain (the raw IQ data), the signal and the noise are hopelessly tangled together.

The concentrated signal forms massive peaks, denoising is simply the act of taking below some level of that peak.

