# Sasando Acoustic Replication: Write-up Outline

## Abstract

- Brief problem statement: Electric sasando lacks acoustic quality of traditional sasando
- Approach: Dual Fourier series model to replicate resonator effects
- Key method: Extract parameters from reference signals, apply to electric input
- Results: Model successfully replicates harmonic structure using 400 harmonics
- Significance: Foundation for preserving traditional instrument sound in electric versions

---

## 1. Introduction

### Problem Statement
- Background on traditional sasando instrument
  - Hand-formed, dried lontar leaf resonator
  - Natural amplification of frequencies 98-1047 Hz
  - Bamboo tube: 70 cm length, 8 cm diameter
- Modern electric sasando: pickup and speaker system
- Problem: Sacrifices acoustic quality for portability
- Challenge: Replicate authentic acoustic sound in electric version

### Objective
- Develop mathematical model to represent acoustic characteristics
- Replicate resonator's frequency-dependent amplification effects
- Preserve harmonic structure and decay characteristics
- Provide foundation for real-time sound synthesis

### Approach Overview
- Dual Fourier series model
- Decompose acoustic signal into harmonic components
- Model resonator contribution as frequency-dependent transformation
- Extract features from recorded signals
- Reconstruct using mathematical framework for DSP implementation

---

## 2. Theory

### 2.0 | Notation

**Time Domain Variables:**
- \( t \): Time (seconds)
- \( f(t) \): Signal as a function of time
- \( E(t) \): Amplitude envelope function
- \( A_0 \): Initial amplitude of the envelope
- \( \tau \): Decay time constant (seconds)

**Frequency Domain Variables:**
- \( f_0 \): Fundamental frequency (Hz)
- \( \omega_0 = 2\pi f_0 \): Fundamental angular frequency (rad/s)
- \( n \): Harmonic index (integer: 0, 1, 2, ...)
- \( nf_0 \): Frequency of the \( n \)-th harmonic (Hz)
- \( M_n \): Magnitude of the \( n \)-th harmonic from FFT

**Fourier Series Coefficients:**
- \( A_n \): Amplitude coefficient for the \( n \)-th harmonic
- \( \phi_n \): Phase coefficient for the \( n \)-th harmonic
- \( \alpha_n \): Frequency-dependent amplitude scaling factor for the \( n \)-th harmonic
- \( \theta \): Phase shift introduced by the resonator (radians)

**Signal Processing:**
- \( F_s \): Sampling rate (Hz) - typically 48,000 Hz
- \( N \): Number of samples
- \( T \): Signal duration (seconds)
- \( \text{FFT}[\cdot] \): Fast Fourier Transform operator

**Model Parameters:**
- \( \text{num\_harmonics} \): Number of harmonics used in the model (400 in this work)
- \( \text{SASANDO\_FREQ\_MIN} = 98 \text{ Hz} \): Minimum frequency of sasando resonator range
- \( \text{SASANDO\_FREQ\_MAX} = 1047 \text{ Hz} \): Maximum frequency of sasando resonator range

### 2.0.1 | Assumptions

- **Harmonic Structure**: Signal can be accurately represented as sum of harmonics (integer multiples of fundamental)
- **Linear Resonator Model**: Resonator effect is linear transformation with frequency-dependent scaling and constant phase shift
- **Exponential Decay**: Amplitude envelope follows exponential decay model, uniform across all harmonics
- **Stationary Signal**: Signal characteristics are stationary over analysis window (except decay envelope)
- **Perfect Harmonic Alignment**: All frequency components are exact integer multiples of fundamental
- **Noise-Free Extraction**: Noise doesn't significantly affect harmonic amplitude/phase extraction
- **Resonator Frequency Response**: Approximated by step function (0.8 inside sasando range, 0.2 outside)
- **Constant Phase Shift**: Phase shift \( \theta \) is constant across all frequencies

### 2.1 | Sound Waves and Harmonic Decomposition

- Sound waves as periodic pressure variations
- Fourier analysis for frequency decomposition
- Fundamental frequency (\(f_0\)): lowest frequency, perceived pitch
- Harmonics: integer multiples (\(nf_0\))
- Inharmonic partials: non-integer multiples
- Traditional sasando: vibrating strings + lontar leaf resonator
- Resonator amplifies 98-1047 Hz range

### 2.2 | Problem Composition

- **Harmonic Components**: Primary frequency content (integer multiples) - can be represented by Fourier series
- **Inharmonic Components**: Non-harmonic partials, especially during attack phase - contribute to timbre but not captured by harmonic model

### 2.3 | Mathematical Model

- Dual Fourier series representation equation
- First sum: original signal decomposed into harmonics
- Second sum: resonator's contribution with frequency-dependent scaling
- Variables: \( \omega_0 \), \( A_n \), \( \phi_n \), \( \alpha_n \), \( \theta \)
- Resonator response function: step function (0.8 inside range, 0.2 outside)

### 2.4 | Amplitude Envelope and Decay

- Exponential decay model: \( E(t) = A_0 e^{-t/\tau} \)
- Multiply steady-state model by exponential envelope
- \( A_0 \): initial amplitude (from signal peak)
- \( \tau \): decay time constant (fitted from envelope)
- Envelope extraction using Hilbert transform

### 2.5 | A_n Extraction Formula

- Problem: Extracting \( A_n \) from recorded signal (already includes resonator effects)
- FFT magnitude \( M_n \) represents combined magnitude of both series terms
- Derivation: sum of two cosine terms with phase shift
- Result: \( A_n = \frac{M_n}{\sqrt{1 + 2\alpha_n \cos(\theta) + \alpha_n^2}} \)
- Accounts for phase relationship between terms

---

## 3. Methods

### 3.1 | Data Collection

- Three types of guitars: acoustic, classical, electric
- Three musical notes: C, D, E
- Sampling rate: 48 kHz
- Format: M4A audio files
- Duration: 7-10 seconds per recording
- Total: 9 instrument-note combinations
- Purpose: Reference signals representing acoustic characteristics

### 3.2 | Feature Extraction

- **Fundamental Frequency Estimation**: Peak magnitude in FFT spectrum (20-2000 Hz range)
- **Harmonic Detection**: 400 harmonics extracted (n = 0 to 399)
- **Amplitude and Phase Extraction**:
  - Extract FFT magnitude \( M_n \) at \( n \cdot f_0 \)
  - Extract phase \( \phi_n \) from FFT phase spectrum
  - Calculate \( A_n \) using derived formula (accounts for resonator)
- **Envelope Extraction**: Hilbert transform method, smoothed with moving average (100 samples)
- **Decay Parameter Fitting**: Exponential model fitted using linear regression on log envelope

### 3.3 | Model Implementation

- **Fourier Series Reconstruction**: Dual series model with 400 harmonics
- **Exponential Decay Envelope**: \( E(t) = A_0 e^{-t/\tau} \)
- **Resonator Effect**: Second series term with \( \alpha_n \) and \( \theta \)
- **Parameters**: \( \theta = \pi/4 \) radians, \( \alpha_n \) from resonator function

### 3.4 | Evaluation Metrics

**Time Domain:**
- Pearson Correlation Coefficient
- Root Mean Squared Error (RMSE)
- Mean Absolute Error (MAE)
- Normalized Mean Squared Error

**Frequency Domain:**
- Frequency spectrum comparison (normalized magnitudes)
- Harmonic magnitude matching

**Amplitude Ratios:**
- Max Ratio (Fourier/Original)
- RMS Ratio (Fourier/Original)

---

## 4. Results

### 4.1 | Harmonic Detection

- 400 harmonics used for all notes
- Fixed number ensures consistent analysis
- Comprehensive frequency spectrum coverage

### 4.2 | Time Domain Matching

- Waveform shape: close match, especially attack and sustain phases
- Amplitude envelope: exponential decay captures natural decay
- Phase alignment: preserved through \( \phi_n \) coefficients

### 4.3 | Frequency Domain Matching

- **Fundamental Frequency**: Accurately identified and replicated (130-330 Hz range)
- **Harmonic Magnitude**: Primary harmonics well-matched, especially fundamental and lower-order
- **Higher Frequency Content**: Some discrepancies due to:
  - Continuous spectral content (noise floor) not representable by discrete harmonics
  - Very weak harmonics below extraction threshold
  - Inharmonic partials (non-integer multiples)

### 4.4 | Quantitative Metrics

- **Time Domain**: Strong correlations, RMSE/MAE reflect amplitude matching, normalized MSE accounts for variance
- **Frequency Domain**: Spectrum correlations show overall shape replication, individual harmonic comparisons
- **Amplitude Ratios**: Max and RMS ratios near 1.0 indicate good energy matching

### 4.5 | Instrument-Specific Observations

- **Classical Guitar**: Best matching, clear harmonic structure, strong fundamentals
- **Acoustic Guitar**: More complex spectral content (especially note E), denser frequency distributions
- **Electric Guitar**: Similar to acoustic, good fundamental matching, some higher frequency discrepancies

### 4.6 | Effect of Harmonic Count

- 400 harmonics vs. initial 31-77: significant improvement
- Better higher frequency representation
- More complete spectral coverage
- Improved amplitude matching across spectrum

---

## 5. Discussion & Conclusion

### 5.1 | Model Performance

**Strengths:**
- Accurate fundamental frequency identification
- Good primary harmonic magnitude matching
- Successful decay characteristics capture
- Effective resonator effect modeling (dual-series approach)

**Limitations:**
- Cannot represent continuous spectral content (noise floor)
- Limited to harmonic components (misses inharmonic partials)
- Simplified resonator model (step function, constant phase)
- Transient effects during attack may not be fully captured

### 5.2 | Sources of Discrepancy

- **Inherent Model Limitations**: Fourier series only represents periodic harmonics; real sounds have inharmonic content
- **Feature Extraction Challenges**: Weak harmonics below threshold, noise affects accuracy, f₀ errors propagate
- **Simplified Resonator Model**: Step-function \( \alpha_n \) may not match true response; constant \( \theta \) may not capture frequency-dependent phase
- **Spectral Characteristics**: Real FFTs show leakage/broadening; model produces ideal sharp lines; noise floor not representable

### 5.3 | Parameter Sensitivity

- **Number of Harmonics**: 400 significantly improved matching; beyond 400 may have diminishing returns
- **Resonator Parameters**: Choice of \( \alpha_n \) function and \( \theta \) affects resonator effect replication
- **Decay Parameters**: Exponential model effectively captures decay; extracted \( \tau \), \( A_0 \) provide good matches

### 5.4 | Application to Electric Sasando Signal Generation

**Step 1: Parameter Extraction from Reference**
- Extract \( A_n \), \( \phi_n \), \( f_0 \), \( \tau \), \( A_0 \) from traditional sasando recordings
- These define target acoustic characteristics

**Step 2: Electric Sasando Input Processing**
- Take electric sasando signal (from pickup) as input
- Extract fundamental frequency \( f_0^{\text{elec}} \) and harmonic structure
- Provides base signal needing transformation

**Step 3: Signal Transformation**
- Electric signal \( s_{\text{elec}}(t) \) transformed using extracted parameters
- Transformation equation: combines reference parameters with electric input frequency
- Electric input provides pitch/timing; reference provides harmonic structure/phases/decay
- Creates hybrid signal: electric pitch + acoustic timbre

**Real-Time Implementation:**
- Continuously extract \( f_0^{\text{elec}} \) from electric input
- Use pre-extracted reference parameters from database
- Synthesize output using transformation equation
- Apply decay envelope based on note onset detection

### 5.5 | Applications and Future Work

**Applications:**
- Foundation for replicating traditional sasando sound in electric versions
- Adaptable to other resonator-based instruments
- Demonstrates mathematical modeling for preserving acoustic characteristics

**Future Improvements:**
- Inharmonic modeling (non-harmonic partials)
- Noise modeling (continuous spectral content)
- Adaptive resonator model (learn actual frequency response)
- Time-varying parameters (\( \alpha_n \), \( \theta \) vary with time)
- Better fundamental frequency estimation (autocorrelation, cepstral analysis)
- Spectral envelope modeling (overall spectral shape)
- Real-time optimization (low-latency processing)

### 5.6 | Conclusion

- Mathematical model successfully captures primary harmonic structure and decay
- Demonstrates feasibility of preserving acoustic sound quality in electric instruments
- Extracted parameters (\( A_n \), \( \phi_n \), \( f_0 \), \( \tau \), \( A_0 \)) form compact representation
- Can be applied to transform electric sasando signals
- Combines electric input pitch with reference timbre
- 400 harmonics ensure comprehensive frequency coverage
- Dual-series approach effectively models resonator interactions
- While perfect replication not achievable with purely harmonic model, captures essential characteristics
- Future work on inharmonic modeling and sophisticated resonator representations can improve accuracy
- Enables real-time implementation in electric sasando systems

---

## 6. References

### Signal Processing and Fourier Analysis
- Oppenheim, A. V., & Schafer, R. W. (2010). *Discrete-Time Signal Processing* (3rd ed.). Prentice Hall. Section 2.6: Sinusoidal Signals and Phasors.
- Proakis, J. G., & Manolakis, D. G. (2007). *Digital Signal Processing* (4th ed.). Prentice Hall. Chapter 2: Discrete-Time Signals and Systems.

### Audio Analysis and Synthesis
- Roads, C. (1996). *The Computer Music Tutorial*. MIT Press. Chapters on additive synthesis and spectral analysis.
- Smith, J. O. (2011). *Spectral Audio Signal Processing*. W3K Publishing.

### Musical Acoustics
- Fletcher, N. H., & Rossing, T. D. (1998). *The Physics of Musical Instruments* (2nd ed.). Springer-Verlag. Chapters on string instruments and resonators.

### Python Libraries
- McFee, B., et al. (2020). librosa: Audio and Music Analysis in Python. *Journal of Open Source Software*, 5(50), 2154.
- Harris, C. R., et al. (2020). Array programming with NumPy. *Nature*, 585, 357–362.
- Virtanen, P., et al. (2020). SciPy 1.0: fundamental algorithms for scientific computing in Python. *Nature Methods*, 17, 261–272.

---

## Appendix A | Mathematical Model Implementation

### A.1 | Core Functions

**A.1.1 | Signal Loading**
- `load_audio()`: Load audio file, return time, amplitude, sample rate
- Handles M4A, WAV, CSV formats
- Robust path resolution

**A.1.2 | Envelope Extraction**
- `extract_envelope()`: Extract amplitude envelope using Hilbert transform
- Parameters: signal, sample_rate, method ('hilbert' or 'abs'), smoothing_window
- Returns: amplitude envelope array

**A.1.3 | Fourier Series Reconstruction**
- `process_fourier_series()`: Reconstruct signal using dual Fourier series
- Parameters: signal, sample_rate, time, omega_0, num_harmonics, A_n, phi_n, alpha_n, theta, include_resonator
- Implements: \( f(t) = \Sigma A_n \cos(n\omega_0 t + \phi_n) + \Sigma \alpha_n A_n \cos(n\omega_0 t + \phi_n + \theta) \)
- Returns: reconstructed signal

**A.1.4 | A_n Extraction with Resonator Accounting**
- `extract_A_n()`: Extract base amplitude coefficient accounting for resonator
- Formula: \( A_n = M_n / \sqrt{1 + 2\alpha_n \cos(\theta) + \alpha_n^2} \)
- Parameters: M_n, alpha_n_value, theta
- Returns: A_n

**A.1.5 | Resonator Response Function**
- `alpha_n_func()`: Frequency-dependent scaling based on sasando range
- Parameters: n, estimated_f0, sasando_min, sasando_max, inside_amplification, outside_amplification
- Returns: alpha_n (0.8 inside range, 0.2 outside)

**A.1.6 | Decay Envelope Application**
- `apply_exponential_decay()`: Apply exponential decay envelope to signal
- Formula: \( E(t) = A_0 \cdot \exp(-t/\tau) \)
- Parameters: signal, time, tau, A0
- Returns: decayed signal

### A.2 | Configuration Parameters

- `NUM_HARMONICS = 400`: Number of harmonics
- `THETA = π/4`: Phase shift (45 degrees)
- `INCLUDE_RESONATOR = True`: Include resonator effect
- `SASANDO_FREQ_MIN = 98 Hz`: Minimum resonator frequency
- `SASANDO_FREQ_MAX = 1047 Hz`: Maximum resonator frequency
- `ENVELOPE_METHOD = 'hilbert'`: Envelope extraction method
- `ENVELOPE_SMOOTHING = 100`: Smoothing window size
- `APPLY_DECAY = True`: Apply exponential decay
- `USE_EXTRACTED_DECAY = True`: Use decay from signal vs. manual

### A.3 | Complete Processing Pipeline

1. Load audio file → `load_audio()`
2. Extract envelope → `extract_envelope()`
3. Estimate fundamental frequency from FFT
4. Extract A_n and φ_n for each harmonic using `extract_A_n()`
5. Reconstruct signal → `process_fourier_series()`
6. Apply decay envelope → `apply_exponential_decay()`
7. Compare with original signal

---

## Key Equations Summary

### Main Model
\[
f(t) = \sum_{n=0}^{\infty} A_n \cos\left(n\omega_0 t + \phi_n\right) + \sum_{n=0}^{\infty} \alpha_n A_n \cos\left(n\omega_0 t + \phi_n + \theta\right)
\]

### With Decay
\[
f_{\text{decay}}(t) = A_0 e^{-t/\tau} \cdot \left[ \sum_{n=0}^{\infty} A_n \cos\left(n\omega_0 t + \phi_n\right) + \sum_{n=0}^{\infty} \alpha_n A_n \cos\left(n\omega_0 t + \phi_n + \theta\right) \right]
\]

### A_n Extraction
\[
A_n = \frac{M_n}{\sqrt{1 + 2\alpha_n \cos(\theta) + \alpha_n^2}}
\]

### Resonator Response
\[
\alpha_n = \begin{cases}
0.8 & \text{if } 98 \text{ Hz} \leq n \cdot f_0 \leq 1047 \text{ Hz} \\
0.2 & \text{otherwise}
\end{cases}
\]

### Electric Sasando Transformation
\[
s_{\text{transformed}}(t) = E(t) \cdot \left[ \sum_{n=0}^{399} A_n^{\text{ref}} \cos\left(n\omega_0^{\text{elec}} t + \phi_n^{\text{ref}}\right) + \sum_{n=0}^{399} \alpha_n A_n^{\text{ref}} \cos\left(n\omega_0^{\text{elec}} t + \phi_n^{\text{ref}} + \theta\right) \right]
\]

