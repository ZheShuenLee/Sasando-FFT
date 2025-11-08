# Sasando Acoustic Replication - ASCII Diagrams

## 1. The Sasando Instrument Structure

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    TRADITIONAL ACOUSTIC SASANDO                         │
└─────────────────────────────────────────────────────────────────────────┘

                    ┌─────────────────────┐
                    │   Strings (Metal)   │
                    │   ────┬───────┬───  │
                    │       │       │     │
                    │   ────┴───────┴───  │
                    │                     │
                    └──────────┬──────────┘
                               │
                               │ Vibration
                               ▼
        ┌──────────────────────────────────────┐
        │   Lontar Leaf Resonator              │
        │   (Hand-formed, dried)               │
        │                                      │
        │   ┌────────────────────────────┐    │
        │   │  Natural Frequency Filter │    │
        │   │  98 Hz ──────── 1047 Hz   │    │
        │   │  (Amplifies this range)   │    │
        │   └────────────────────────────┘    │
        │                                      │
        │   Physical Properties:               │
        │   • 70 cm bamboo tube                │
        │   • 8 cm diameter                   │
        │   • Resonant cavity                 │
        │   • Acoustic amplification          │
        └──────────────────────────────────────┘
                               │
                               │ Acoustic Sound
                               ▼
                    ┌──────────────────┐
                    │  Microphone      │
                    │  Recording       │
                    └────────┬─────────┘
                             │
                             ▼
                    acoustic_sasando.wav


┌─────────────────────────────────────────────────────────────────────────┐
│                         ELECTRIC SASANDO                               │
└─────────────────────────────────────────────────────────────────────────┘

                    ┌─────────────────────┐
                    │   Strings (Metal)   │
                    │   ────┬───────┬───  │
                    │       │       │     │
                    │   ────┴───────┴───  │
                    │                     │
                    └──────────┬──────────┘
                               │
                               │ Vibration
                               ▼
                    ┌──────────────────┐
                    │  Pickup          │
                    │  (Electromagnetic│
                    │   Transducer)    │
                    └────────┬─────────┘
                             │
                             │ Electrical Signal
                             ▼
                    ┌──────────────────┐
                    │  Amplifier/      │
                    │  Speaker         │
                    │  (No resonator)  │
                    └────────┬─────────┘
                             │
                             │ Electric Sound
                             ▼
                    ┌──────────────────┐
                    │  Microphone      │
                    │  Recording       │
                    └────────┬─────────┘
                             │
                             ▼
                    electric_sasando.wav
```

## 2. The Problem: Frequency Response Difference

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    FREQUENCY RESPONSE COMPARISON                         │
└─────────────────────────────────────────────────────────────────────────┘

    Amplitude
        │
        │  Electric Sasando (Original)
        │  ┌─────────────────────────────┐
        │  │                             │
        │  │  ────────────────           │  ← Broad frequency range
        │  │       ╱╲    ╱╲              │     (no natural filtering)
        │  │      ╱  ╲  ╱  ╲             │
        │  │     ╱    ╲╱    ╲            │
        │  │    ╱            ╲           │
        │  └───┴───────────────┴──────────┴──→ Frequency
        │     0    98        1047     20000 Hz
        │
        │  Acoustic Sasando (Target)
        │  ┌─────────────────────────────┐
        │  │                             │
        │  │      ┌───────────┐          │  ← Resonator amplifies
        │  │      │    ╱╲    │          │     98-1047 Hz range
        │  │      │   ╱  ╲   │          │
        │  │      │  ╱    ╲  │          │
        │  │      │ ╱      ╲ │          │
        │  └──────┴─┴────────┴──────────┴──→ Frequency
        │     0    98        1047     20000 Hz
        │           │         │
        │           └─────────┘
        │         Resonator Range
        │
        │  GOAL: Transform electric to match acoustic
```

## 3. Complete Code Pipeline Flow

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    SASANDO ACOUSTIC REPLICATION PIPELINE                  │
└─────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 1: DATA INPUT                                                      │
└─────────────────────────────────────────────────────────────────────────┘

    ┌──────────────────────┐         ┌──────────────────────┐
    │ acoustic_sasando.wav  │         │ electric_sasando.wav │
    │ (Reference Signal)    │         │ (Input Signal)       │
    └───────────┬───────────┘         └───────────┬───────────┘
                │                                 │
                │ load_audio_file()               │ load_audio_file()
                │                                 │
                ▼                                 ▼
    ┌──────────────────────┐         ┌──────────────────────┐
    │  Audio Array         │         │  Audio Array          │
    │  • Sample Rate       │         │  • Sample Rate        │
    │  • Time Array         │         │  • Time Array         │
    │  • Amplitude Values   │         │  • Amplitude Values   │
    └───────────┬───────────┘         └───────────┬───────────┘
                │                                 │
                │ normalize_audio()               │ normalize_audio()
                │                                 │
                ▼                                 ▼
    ┌──────────────────────┐         ┌──────────────────────┐
    │  Normalized Signals  │         │  Normalized Signals  │
    │  (Peak normalized)    │         │  (Peak normalized)   │
    └──────────────────────┘         └───────────────────────┘


┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 2: FEATURE EXTRACTION                                              │
└─────────────────────────────────────────────────────────────────────────┘

    Acoustic Signal                    Electric Signal
    ───────────────                    ───────────────
         │                                    │
         │                                    │
    ┌────┴───────────────────────────────┐   │
    │  Feature Extraction Functions      │   │
    └────┬───────────────────────────────┘   │
         │                                    │
    ┌────┼────────────────────────────────────┼────┐
    │    │                                    │    │
    │    ▼                                    ▼    │
    │ ┌──────────────┐              ┌──────────────┐│
    │ │ Time Domain │              │ Time Domain  ││
    │ │ Features:    │              │ Features:    ││
    │ │ • RMS        │              │ • RMS        ││
    │ │ • ZCR        │              │ • ZCR        ││
    │ │ • Energy     │              │ • Energy     ││
    │ └──────┬───────┘              └──────┬───────┘│
    │        │                              │        │
    │        ▼                              ▼        │
    │ ┌──────────────┐              ┌──────────────┐│
    │ │ Frequency    │              │ Frequency    ││
    │ │ Domain:      │              │ Domain:       ││
    │ │ • FFT        │              │ • FFT        ││
    │ │ • Magnitude  │              │ • Magnitude  ││
    │ │ • Centroid   │              │ • Centroid   ││
    │ │ • Rolloff    │              │ • Rolloff    ││
    │ │ • Bandwidth  │              │ • Bandwidth  ││
    │ └──────┬───────┘              └──────┬───────┘│
    │        │                              │        │
    │        ▼                              ▼        │
    │ ┌──────────────┐              ┌──────────────┐│
    │ │ Advanced:    │              │ Advanced:    ││
    │ │ • MFCC       │              │ • MFCC       ││
    │ │ • Chroma     │              │ • Chroma     ││
    │ │ • Contrast   │              │ • Contrast   ││
    │ │ • Tonnetz    │              │ • Tonnetz    ││
    │ └──────────────┘              └──────────────┘│
    └────────────────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 3: RESONATOR CHARACTERIZATION                                      │
└─────────────────────────────────────────────────────────────────────────┘

    Acoustic Signal
    ───────────────
         │
         │ analyze_resonator_response()
         │ (Focus on 98-1047 Hz range)
         │
         ▼
    ┌────────────────────────────────────┐
    │  Frequency Response Analysis       │
    │                                    │
    │  ┌──────────────────────────────┐  │
    │  │  Full Spectrum              │  │
    │  │  0 ───────────────── 20000 Hz│  │
    │  └──────────────────────────────┘  │
    │           │                         │
    │           │ Filter to Range         │
    │           ▼                         │
    │  ┌──────────────────────────────┐  │
    │  │  Resonator Range              │  │
    │  │  98 ──────────── 1047 Hz     │  │
    │  │  ┌─────────────┐             │  │
    │  │  │   ╱╲        │             │  │
    │  │  │  ╱  ╲       │             │  │
    │  │  │ ╱    ╲      │             │  │
    │  │  └─────────────┘             │  │
    │  └──────────────────────────────┘  │
    │                                    │
    │  Extract:                          │
    │  • Peak frequencies                │
    │  • Gain profile                    │
    │  • Magnitude response             │
    │  • Mean/max gain                  │
    └──────────────┬─────────────────────┘
                   │
                   ▼
    ┌────────────────────────────────────┐
    │  Acoustic Resonator Profile        │
    │  (Target Response)                 │
    └────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 4: TRANSFORMATION DESIGN                                           │
└─────────────────────────────────────────────────────────────────────────┘

    Acoustic Profile          Electric Profile
    ───────────────          ───────────────
         │                          │
         │                          │
         └──────────┬───────────────┘
                    │
                    ▼
        ┌───────────────────────────┐
        │  Compare Frequency        │
        │  Responses                │
        └───────────┬───────────────┘
                    │
        ┌───────────┼───────────────┐
        │           │               │
        ▼           ▼               ▼
    ┌─────────┐ ┌──────────┐ ┌──────────┐
    │ Bandpass│ │ EQ Curve │ │ Filter   │
    │ Filter  │ │ Design   │ │ Design   │
    └────┬────┘ └────┬─────┘ └────┬─────┘
         │           │             │
         │           │             │
    ┌────┴───────────┴─────────────┴────┐
    │  Transformation Parameters         │
    │                                    │
    │  • Filter coefficients (b, a)     │
    │  • EQ gain curve (dB)              │
    │  • Frequency mapping               │
    └────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 5: TRANSFORMATION APPLICATION                                      │
└─────────────────────────────────────────────────────────────────────────┘

    Electric Signal (Input)
    ────────────────────────
         │
         │
         ▼
    ┌────────────────────────────┐
    │  Step 1: Bandpass Filter  │
    │  ──────────────────────── │
    │  • Butterworth filter     │
    │  • 98-1047 Hz passband   │
    │  • Attenuate outside      │
    │    resonator range        │
    └────────────┬───────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │  Filtered Signal          │
    │  (Frequency limited)       │
    └────────────┬───────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │  Step 2: EQ Application   │
    │  ──────────────────────── │
    │  • FFT → Frequency Domain │
    │  • Apply gain curve       │
    │  • Match acoustic profile │
    │  • IFFT → Time Domain     │
    └────────────┬───────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │  Step 3: Normalization     │
    │  ──────────────────────── │
    │  • Peak normalize          │
    │  • Prevent clipping        │
    └────────────┬───────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │  Transformed Signal        │
    │  (Electric → Acoustic-like) │
    └────────────────────────────┘


┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 6: VALIDATION & METRICS                                            │
└─────────────────────────────────────────────────────────────────────────┘

    Acoustic (Reference)    Transformed Electric    Original Electric
    ────────────────────    ────────────────────    ─────────────────
         │                          │                        │
         │                          │                        │
         └──────────┬───────────────┴────────────────────────┘
                    │
                    ▼
        ┌────────────────────────────┐
        │  Extract Features         │
        │  (Same as Phase 2)        │
        └──────────────┬─────────────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
    ┌─────────┐  ┌──────────┐  ┌──────────┐
    │ Spectral│  │ Spectral │  │ Spectral │
    │ Features│  │ Features │  │ Features │
    └────┬────┘  └────┬─────┘  └────┬─────┘
         │            │              │
         └────────────┼──────────────┘
                      │
                      ▼
        ┌────────────────────────────┐
        │  Compute Similarity        │
        │  Metrics                   │
        │                            │
        │  • Euclidean Distance      │
        │  • Cosine Similarity       │
        │  • MFCC Distance            │
        └──────────────┬─────────────┘
                       │
                       ▼
        ┌────────────────────────────┐
        │  Compare Results           │
        │                            │
        │  Transformed vs Acoustic:  │
        │  ✓ Better match           │
        │                            │
        │  Original vs Acoustic:     │
        │  ✗ Poor match             │
        └────────────────────────────┘


┌─────────────────────────────────────────────────────────────────────────┐
│ PHASE 7: VISUALIZATION & OUTPUT                                         │
└─────────────────────────────────────────────────────────────────────────┘

    ┌────────────────────────────────────┐
    │  Generate Visualizations           │
    │                                    │
    │  1. Time Domain Waveforms         │
    │  2. Frequency Response            │
    │  3. Resonator Range Detail         │
    │  4. EQ Curve                      │
    │  5. Spectrograms (3 panels)       │
    │  6. Feature Comparisons           │
    │  7. Similarity Metrics            │
    └──────────────┬─────────────────────┘
                   │
                   ▼
    ┌────────────────────────────────────┐
    │  Save Output                       │
    │                                    │
    │  • transformed_electric_sasando  │
    │    .wav                            │
    │  • sasando_analysis_results.png    │
    │  • Summary Report (console)       │
    └────────────────────────────────────┘
```

## 4. Signal Processing Flow (Detailed)

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    SIGNAL PROCESSING DETAILS                            │
└─────────────────────────────────────────────────────────────────────────┘

ELECTRIC SIGNAL TRANSFORMATION:

    Time Domain Signal
    ──────────────────
         │
         │ [electric_audio]
         │ Array of amplitude values
         │
         ▼
    ┌────────────────────────────┐
    │  FFT (Fast Fourier Transform)│
    │  ─────────────────────────── │
    │  rfft(electric_audio, n=2048)│
    └────────────┬───────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │  Frequency Domain          │
    │  ───────────────────────── │
    │  • Frequencies: 0-22050 Hz │
    │  • Magnitude spectrum      │
    │  • Phase information       │
    └────────────┬───────────────┘
                 │
        ┌────────┼────────┐
        │        │        │
        ▼        ▼        ▼
    ┌──────┐ ┌──────┐ ┌──────┐
    │ <98  │ │98-   │ │>1047 │
    │ Hz   │ │1047  │ │ Hz   │
    │      │ │ Hz   │ │      │
    └──┬───┘ └──┬───┘ └──┬───┘
       │        │        │
       │        │        │
       ▼        ▼        ▼
    ┌──────┐ ┌──────┐ ┌──────┐
    │Atten-│ │Keep &│ │Atten-│
    │uate  │ │Boost │ │uate  │
    │      │ │      │ │      │
    └──┬───┘ └──┬───┘ └──┬───┘
       │        │        │
       └────────┼────────┘
                │
                ▼
    ┌────────────────────────────┐
    │  Apply Bandpass Filter     │
    │  ───────────────────────── │
    │  signal.filtfilt(b, a, x)  │
    │  • Forward & backward     │
    │  • Zero phase distortion   │
    └────────────┬───────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │  Filtered Signal           │
    │  (98-1047 Hz emphasized)   │
    └────────────┬───────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │  Apply EQ Curve            │
    │  ───────────────────────── │
    │  1. FFT → Freq Domain      │
    │  2. Interpolate gain curve │
    │  3. Multiply by gain       │
    │  4. IFFT → Time Domain      │
    └────────────┬───────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │  Normalize                 │
    │  ───────────────────────── │
    │  Peak normalization        │
    │  Prevent clipping          │
    └────────────┬───────────────┘
                 │
                 ▼
    ┌────────────────────────────┐
    │  Transformed Signal        │
    │  (Matches acoustic profile) │
    └────────────────────────────┘
```

## 5. Key Functions and Their Roles

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    FUNCTION ARCHITECTURE                                 │
└─────────────────────────────────────────────────────────────────────────┘

DATA LOADING:
    load_audio_file()
    ├── Reads WAV or CSV
    ├── Converts to mono if stereo
    ├── Normalizes data types
    ├── Resamples if needed
    └── Returns: audio, sr, time

    normalize_audio()
    ├── Peak normalization (default)
    └── RMS normalization (optional)


FEATURE EXTRACTION:
    extract_time_domain_features()
    ├── RMS Energy
    ├── Zero Crossing Rate
    └── Total Energy

    extract_frequency_domain_features()
    ├── FFT computation
    ├── Spectral Centroid (brightness)
    ├── Spectral Rolloff (85% energy)
    └── Spectral Bandwidth

    extract_advanced_features()
    ├── MFCC (if librosa available)
    ├── Chroma features
    ├── Spectral Contrast
    ├── Tonnetz
    └── Harmonic/Percussive separation


RESONATOR ANALYSIS:
    analyze_resonator_response()
    ├── Compute FFT
    ├── Filter to 98-1047 Hz range
    ├── Normalize magnitude
    ├── Find peaks
    └── Return: frequencies, magnitude, peaks


TRANSFORMATION:
    design_bandpass_filter()
    ├── Calculate Nyquist frequency
    ├── Normalize frequencies
    └── Design Butterworth filter

    design_eq_curve()
    ├── Normalize responses
    ├── Calculate gain in dB
    └── Smooth curve (Gaussian)

    apply_eq_curve()
    ├── FFT to frequency domain
    ├── Interpolate gain curve
    ├── Apply gain
    └── IFFT back to time domain


METRICS:
    compute_euclidean_distance()
    └── L2 norm between vectors

    compute_cosine_similarity()
    └── Dot product / (norm1 * norm2)

    compute_spectral_distance()
    ├── Interpolate to common grid
    ├── Normalize
    └── Compute both metrics

    compute_mfcc_distance()
    └── Average MFCCs over time, then distance
```

## 6. Data Flow Through the System

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    DATA FLOW DIAGRAM                                    │
└─────────────────────────────────────────────────────────────────────────┘

INPUT FILES
    │
    ├── acoustic_sasando.wav
    │   └──→ [44100 Hz, 2.0 sec, float32]
    │
    └── electric_sasando.wav
        └──→ [44100 Hz, 2.0 sec, float32]
                │
                │ PROCESSING
                ▼
    ┌───────────────────────────────────────┐
    │  Electric Signal Array               │
    │  Shape: (88200,)                     │
    │  Type: float32                        │
    │  Range: [-1.0, 1.0]                   │
    └───────────────┬───────────────────────┘
                    │
                    │ FFT (n=2048)
                    ▼
    ┌───────────────────────────────────────┐
    │  Frequency Domain                      │
    │  Frequencies: [0, 21.5, 43, ...] Hz   │
    │  Magnitude: [0.5, 0.3, 0.8, ...]      │
    │  Shape: (1025,)                       │
    └───────────────┬───────────────────────┘
                    │
                    │ Filter & EQ
                    ▼
    ┌───────────────────────────────────────┐
    │  Transformed Frequency Domain          │
    │  • Attenuated outside 98-1047 Hz      │
    │  • Boosted to match acoustic          │
    └───────────────┬───────────────────────┘
                    │
                    │ IFFT
                    ▼
    ┌───────────────────────────────────────┐
    │  Transformed Time Domain               │
    │  Shape: (88200,)                      │
    │  Type: float32                        │
    │  Range: [-1.0, 1.0]                   │
    └───────────────┬───────────────────────┘
                    │
                    │ Convert to int16
                    ▼
    ┌───────────────────────────────────────┐
    │  Output WAV File                      │
    │  transformed_electric_sasando.wav    │
    │  • 44100 Hz sample rate              │
    │  • 16-bit PCM                         │
    │  • Mono                               │
    └───────────────────────────────────────┘
```

## 7. Frequency Domain Operations

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    FREQUENCY DOMAIN TRANSFORMATIONS                     │
└─────────────────────────────────────────────────────────────────────────┘

ORIGINAL ELECTRIC SPECTRUM:
    │
    │     ╱╲        ╱╲
    │    ╱  ╲      ╱  ╲
    │   ╱    ╲    ╱    ╲
    │  ╱      ╲  ╱      ╲
    │ ╱        ╲╱        ╲
    └─┴────────┴─────────┴──────────→
     0        98       1047      20000 Hz
              │         │
              └─────────┘
            Resonator Range

STEP 1: BANDPASS FILTER
    │
    │     ╱╲        ╱╲
    │    ╱  ╲      ╱  ╲
    │   ╱    ╲    ╱    ╲
    │  ╱      ╲  ╱      ╲
    │ ╱        ╲╱        ╲
    └─┴────────┴─────────┴──────────→
     0        98       1047      20000 Hz
      ╲        │         │        ╱
       ╲       │         │       ╱
        ╲      │         │      ╱
         ╲     │         │     ╱
          ╲    │         │    ╱
           ╲   │         │   ╱
            ╲  │         │  ╱
             ╲ │         │ ╱
              ╲│         │╱
               └─────────┘
            Attenuated

STEP 2: EQ MATCHING
    │
    │      ┌───────────┐
    │     ╱│    ╱╲     │╲
    │    ╱ │   ╱  ╲    │ ╲
    │   ╱  │  ╱    ╲   │  ╲
    │  ╱   │ ╱      ╲  │   ╲
    │ ╱    │╱        ╲╱│    ╲
    └─┴────┴───────────┴──────┴──→
     0    98         1047    20000 Hz
              │         │
              └─────────┘
        Boosted to match acoustic

RESULT: MATCHES ACOUSTIC PROFILE
    │
    │      ┌───────────┐
    │     ╱│    ╱╲     │╲
    │    ╱ │   ╱  ╲    │ ╲
    │   ╱  │  ╱    ╲   │  ╲
    │  ╱   │ ╱      ╲  │   ╲
    │ ╱    │╱        ╲╱│    ╲
    └─┴────┴───────────┴──────┴──→
     0    98         1047    20000 Hz
              │         │
              └─────────┘
        Matches acoustic response
```

## 8. Complete System Overview

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    COMPLETE SYSTEM ARCHITECTURE                         │
└─────────────────────────────────────────────────────────────────────────┘

PHYSICAL WORLD                    DIGITAL PROCESSING
──────────────                    ──────────────────

┌──────────────┐                  ┌──────────────────┐
│ Acoustic     │                  │ 1. Load Audio    │
│ Sasando      │───Record───────→ │    Files         │
│              │                  └────────┬─────────┘
│ • Strings    │                          │
│ • Resonator  │                          ▼
│ • 98-1047 Hz│                  ┌──────────────────┐
└──────────────┘                  │ 2. Extract       │
                                  │    Features      │
┌──────────────┐                  └────────┬─────────┘
│ Electric     │                          │
│ Sasando      │───Record───────→         ▼
│              │                  ┌──────────────────┐
│ • Strings    │                  │ 3. Analyze      │
│ • Pickup     │                  │    Resonator     │
│ • Broad freq │                  └────────┬─────────┘
└──────────────┘                          │
                                          ▼
                                  ┌──────────────────┐
                                  │ 4. Design        │
                                  │    Transform     │
                                  └────────┬─────────┘
                                          │
                                          ▼
                                  ┌──────────────────┐
                                  │ 5. Apply         │
                                  │    Transform     │
                                  └────────┬─────────┘
                                          │
                                          ▼
                                  ┌──────────────────┐
                                  │ 6. Validate      │
                                  │    & Compare     │
                                  └────────┬─────────┘
                                          │
                                          ▼
                                  ┌──────────────────┐
                                  │ 7. Output        │
                                  │    • WAV file    │
                                  │    • Plots       │
                                  │    • Metrics     │
                                  └──────────────────┘
                                          │
                                          ▼
                                  ┌──────────────────┐
                                  │ Transformed      │
                                  │ Electric Signal  │
                                  │ (Acoustic-like)  │
                                  └──────────────────┘
```

## 9. Modern Electric Sasando - Detailed Visualization

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    MODERN ELECTRIC SASANDO                              │
│                    (Detailed Component Diagram)                         │
└─────────────────────────────────────────────────────────────────────────┘

                    FRONT VIEW (Playing Position)
                    ════════════════════════════

                         ┌─────────────────────┐
                         │   Tuning Pegs       │
                         │   ┌───┐ ┌───┐ ┌───┐ │
                         │   │ ● │ │ ● │ │ ● │ │
                         │   └───┘ └───┘ └───┘ │
                         │   ┌───┐ ┌───┐ ┌───┐ │
                         │   │ ● │ │ ● │ │ ● │ │
                         │   └───┘ └───┘ └───┘ │
                         └───────────┬───────────┘
                                     │
                    ┌────────────────┼────────────────┐
                    │                │                │
                    │  ┌─────────────┴─────────────┐  │
                    │  │   Nut (String Guide)      │  │
                    │  └─────────────┬─────────────┘  │
                    │                │                │
                    │                │                │
                    │  ┌─────────────┴─────────────┐  │
                    │  │                            │  │
                    │  │   STRINGS (Metal)          │  │
                    │  │   ──────────────────────── │  │
                    │  │   ──────────────────────── │  │
                    │  │   ──────────────────────── │  │
                    │  │   ──────────────────────── │  │
                    │  │   ──────────────────────── │  │
                    │  │   ──────────────────────── │  │
                    │  │                            │  │
                    │  │   (Vibrating when plucked) │  │
                    │  │                            │  │
                    │  └─────────────┬─────────────┘  │
                    │                │                │
                    │                │                │
                    │  ┌─────────────┴─────────────┐
                    │  │   Bridge                    │
                    │  │   ┌─────┐ ┌─────┐ ┌─────┐   │
                    │  │   │  ●  │ │  ●  │ │  ●  │   │
                    │  │   └─────┘ └─────┘ └─────┘   │
                    │  └─────────────┬───────────────┘
                    │                │                │
                    │                │                │
                    │  ┌─────────────┴─────────────┐  │
                    │  │   PICKUP                   │  │
                    │  │   ┌─────────────────────┐  │  │
                    │  │   │  Electromagnetic    │  │  │
                    │  │   │  Coil               │  │  │
                    │  │   │  ┌───────────────┐  │  │  │
                    │  │   │  │  Magnet       │  │  │  │
                    │  │   │  │  ┌─────────┐  │  │  │  │
                    │  │   │  │  │  Coil   │  │  │  │  │
                    │  │   │  │  │  Wire   │  │  │  │  │
                    │  │   │  │  └─────────┘  │  │  │  │
                    │  │   │  └───────────────┘  │  │  │
                    │  │   └─────────────────────┘  │  │
                    │  │                            │  │
                    │  │   Detects string vibration │  │
                    │  │   Converts to electrical   │  │
                    │  │   signal                    │  │
                    │  └─────────────┬───────────────┘  │
                    │                │                │
                    │                │                │
                    │  ┌─────────────┴─────────────┐  │
                    │  │   CONTROL PANEL            │  │
                    │  │   ┌─────┐ ┌─────┐ ┌─────┐ │  │
                    │  │   │Vol  │ │Tone │ │EQ   │ │  │
                    │  │   └─────┘ └─────┘ └─────┘ │  │
                    │  └─────────────┬───────────────┘  │
                    │                │                │
                    │                │                │
                    │  ┌─────────────┴─────────────┐  │
                    │  │   OUTPUT JACK             │  │
                    │  │   ┌─────────────────────┐ │  │
                    │  │   │  1/4" Phone Jack    │ │  │
                    │  │   │  (Audio Output)     │ │  │
                    │  │   └─────────────────────┘ │  │
                    │  └───────────────────────────┘  │
                    │                                  │
                    │  ┌───────────────────────────┐  │
                    │  │   BAMBOO TUBE BODY        │  │
                    │  │   (70 cm length)           │  │
                    │  │   (8 cm diameter)           │  │
                    │  │                            │  │
                    │  │   No acoustic resonator    │  │
                    │  │   (Hollow but not          │  │
                    │  │    amplifying acoustically)│  │
                    │  └───────────────────────────┘  │
                    │                                  │
                    └──────────────────────────────────┘


                    SIDE VIEW (Cross Section)
                    ════════════════════════

    ┌─────────────────────────────────────────────────────────────┐
    │                                                               │
    │  ┌─────────────────────────────────────────────────────┐   │
    │  │  String (Vibrating)                                    │   │
    │  │  ────────────────────────────────────────────────────  │   │
    │  └─────────────────────────────────────────────────────┘   │
    │           │                                                 │
    │           │ Vibration                                      │
    │           ▼                                                 │
    │  ┌─────────────────────────────────────────────────────┐   │
    │  │  Pickup                                              │   │
    │  │  ┌───────────────────────────────────────────────┐  │   │
    │  │  │  Magnetic Field                                │  │   │
    │  │  │  ┌───────────────────────────────────────────┐ │  │   │
    │  │  │  │  Coil (Wire wrapped around magnet)        │ │  │   │
    │  │  │  │  ┌───────────────────────────────────────┐  │ │  │   │
    │  │  │  │  │  Induced Current (AC Signal)        │  │ │  │   │
    │  │  │  │  └───────────────────────────────────────┘  │ │  │   │
    │  │  │  └───────────────────────────────────────────┘ │  │   │
    │  │  └───────────────────────────────────────────────┘  │   │
    │  └─────────────────────────────────────────────────────┘   │
    │           │                                                 │
    │           │ Electrical Signal                              │
    │           │ (Low voltage AC)                                │
    │           ▼                                                 │
    │  ┌─────────────────────────────────────────────────────┐   │
    │  │  Output Jack                                         │   │
    │  │  ┌───────────────────────────────────────────────┐   │   │
    │  │  │  Cable Connection                              │   │   │
    │  │  └───────────────────────────────────────────────┘   │   │
    │  └─────────────────────────────────────────────────────┘   │
    │           │                                                 │
    │           │ Signal travels through cable                   │
    │           ▼                                                 │
    │  ┌─────────────────────────────────────────────────────┐   │
    │  │  Amplifier / Audio Interface                         │   │
    │  │  ┌───────────────────────────────────────────────┐   │   │
    │  │  │  Preamp (Gain)                                │   │   │
    │  │  │  ┌──────────────────────────────────────────┐ │   │   │
    │  │  │  │  EQ / Tone Controls                      │ │   │   │
    │  │  │  └──────────────────────────────────────────┘ │   │   │
    │  │  │  ┌──────────────────────────────────────────┐ │   │   │
    │  │  │  │  Power Amplifier                         │ │   │   │
    │  │  │  └──────────────────────────────────────────┘ │   │   │
    │  │  └───────────────────────────────────────────────┘   │   │
    │  └─────────────────────────────────────────────────────┘   │
    │           │                                                 │
    │           │ Amplified Signal                                │
    │           ▼                                                 │
    │  ┌─────────────────────────────────────────────────────┐   │
    │  │  Speaker / Headphones                                │   │
    │  │  ┌───────────────────────────────────────────────┐   │   │
    │  │  │  Sound Output                                  │   │   │
    │  │  │  (Broad frequency range)                      │   │   │
    │  │  └───────────────────────────────────────────────┘   │   │
    │  └─────────────────────────────────────────────────────┘   │
    │                                                             │
    └─────────────────────────────────────────────────────────────┘


                    SIGNAL PATH DIAGRAM
                    ═══════════════════

    Physical Vibration          Electrical Signal          Sound Output
    ──────────────────          ──────────────────          ────────────

    ┌──────────────┐
    │   Player     │
    │   Plucks     │
    │   String     │
    └──────┬───────┘
           │
           │ Mechanical Energy
           ▼
    ┌──────────────┐
    │   String     │
    │   Vibrates   │
    │   ─────────  │  ← Oscillating at fundamental
    │   ─────────  │     frequency + harmonics
    │   ─────────  │
    └──────┬───────┘
           │
           │ Vibration creates
           │ changing magnetic field
           ▼
    ┌──────────────────────────────────┐
    │   PICKUP                         │
    │   ────────────────────────────── │
    │   • Permanent magnet              │
    │   • Coil of wire                  │
    │   • Faraday's Law:                │
    │     Changing magnetic field       │
    │     → Induced voltage             │
    │                                   │
    │   Output: AC voltage signal      │
    │   (millivolts, low impedance)    │
    └──────┬───────────────────────────┘
           │
           │ Low-level electrical signal
           │ (Analog, AC)
           ▼
    ┌──────────────────────────────────┐
    │   OUTPUT JACK                    │
    │   ────────────────────────────── │
    │   • 1/4" phone connector         │
    │   • Shielded cable               │
    │   • Carries signal to amplifier   │
    └──────┬───────────────────────────┘
           │
           │ Through instrument cable
           ▼
    ┌──────────────────────────────────┐
    │   AMPLIFIER / AUDIO INTERFACE     │
    │   ────────────────────────────── │
    │                                   │
    │   ┌───────────────────────────┐  │
    │   │  Preamp Stage             │  │
    │   │  • Gain control            │  │
    │   │  • Impedance matching      │  │
    │   │  • Signal amplification    │  │
    │   └───────────┬────────────────┘  │
    │               │                    │
    │               ▼                    │
    │   ┌───────────────────────────┐  │
    │   │  EQ / Tone Controls       │  │
    │   │  • Bass, Mid, Treble       │  │
    │   │  • Frequency shaping       │  │
    │   │  • No natural filtering    │  │
    │   └───────────┬────────────────┘  │
    │               │                    │
    │               ▼                    │
    │   ┌───────────────────────────┐  │
    │   │  Power Amplifier          │  │
    │   │  • High voltage output    │  │
    │   │  • Drive speaker          │  │
    │   └───────────┬────────────────┘  │
    └───────────────┼────────────────────┘
                    │
                    │ Amplified signal
                    │ (Volts, high power)
                    ▼
    ┌──────────────────────────────────┐
    │   SPEAKER / HEADPHONES            │
    │   ────────────────────────────── │
    │   • Electromagnetic driver        │
    │   • Converts electrical → sound  │
    │   • Broad frequency response      │
    │   • No natural resonance         │
    │                                   │
    │   Output: Sound waves            │
    │   (20 Hz - 20,000 Hz range)      │
    └───────────────────────────────────┘


                    COMPONENT SPECIFICATIONS
                    ════════════════════════

    ┌─────────────────────────────────────────────────────────────┐
    │  STRINGS                                                      │
    │  ──────                                                      │
    │  • Material: Metal (steel, bronze, or nickel)               │
    │  • Number: Typically 28-36 strings                          │
    │  • Tuning: Chromatic scale                                   │
    │  • Length: ~60-65 cm (vibrating length)                     │
    │  • Tension: Adjustable via tuning pegs                       │
    │  • Vibration: Fundamental + harmonics                      │
    │                                                              │
    │  Frequency Range:                                           │
    │  • Lowest: ~98 Hz (G2)                                       │
    │  • Highest: ~1047 Hz (C6)                                   │
    │  • Plus harmonics up to ~10 kHz                            │
    └─────────────────────────────────────────────────────────────┘

    ┌─────────────────────────────────────────────────────────────┐
    │  PICKUP                                                      │
    │  ─────                                                      │
    │  • Type: Electromagnetic (passive or active)              │
    │  • Principle: Faraday's Law of Induction                    │
    │  • Components:                                             │
    │    - Permanent magnet(s)                                    │
    │    - Coil of wire (thousands of turns)                     │
    │    - Pole pieces (under each string)                       │
    │  • Output:                                                  │
    │    - Voltage: 0.1-1.0 V (peak)                             │
    │    - Impedance: 5-15 kΩ                                    │
    │    - Frequency: 20 Hz - 20 kHz                             │
    │  • Characteristics:                                        │
    │    - No frequency filtering                                 │
    │    - Captures all harmonics                                 │
    │    - Linear response (within range)                        │
    └─────────────────────────────────────────────────────────────┘

    ┌─────────────────────────────────────────────────────────────┐
    │  BODY / FRAME                                                 │
    │  ───────────                                                  │
    │  • Material: Bamboo tube                                     │
    │  • Dimensions:                                               │
    │    - Length: 70 cm                                           │
    │    - Diameter: 8 cm                                          │
    │  • Function:                                                 │
    │    - Structural support                                     │
    │    - String anchor points                                    │
    │    - Pickup mounting                                        │
    │  • Note:                                                     │
    │    - Hollow but NOT acoustically resonant                   │
    │    - No natural amplification                               │
    │    - Does not filter frequencies                             │
    └─────────────────────────────────────────────────────────────┘

    ┌─────────────────────────────────────────────────────────────┐
    │  ELECTRONICS                                                  │
    │  ──────────                                                   │
    │  • Controls:                                                 │
    │    - Volume potentiometer                                    │
    │    - Tone control (optional)                                 │
    │    - EQ switches (optional)                                  │
    │  • Output:                                                   │
    │    - 1/4" phone jack (mono or stereo)                      │
    │    - Standard instrument cable                               │
    │  • Power:                                                    │
    │    - Passive: No battery needed                            │
    │    - Active: 9V battery (if active pickup)                  │
    └─────────────────────────────────────────────────────────────┘


                    COMPARISON: ACOUSTIC vs ELECTRIC
                    ════════════════════════════════

    ACOUSTIC SASANDO              ELECTRIC SASANDO
    ────────────────              ─────────────────

    ┌──────────────┐              ┌──────────────┐
    │ Strings      │              │ Strings      │
    │ ─────────    │              │ ─────────    │
    └──────┬───────┘              └──────┬───────┘
           │                              │
           │ Vibration                    │ Vibration
           ▼                              ▼
    ┌──────────────┐              ┌──────────────┐
    │ Lontar Leaf  │              │ Pickup       │
    │ Resonator    │              │ (Magnetic)   │
    │              │              │              │
    │ • Natural    │              │ • Electrical │
    │   filtering  │              │   signal     │
    │ • 98-1047 Hz │              │ • Broad freq │
    │   amplified  │              │   range      │
    │ • Acoustic   │              │ • No filter  │
    │   sound      │              │              │
    └──────┬───────┘              └──────┬───────┘
           │                              │
           │                              │ Electrical
           │                              ▼
           │                      ┌──────────────┐
           │                      │ Amplifier    │
           │                      │              │
           │                      │ • Gain       │
           │                      │ • EQ         │
           │                      │ • Power amp  │
           │                      └──────┬───────┘
           │                              │
           │                              │
           ▼                              ▼
    ┌──────────────┐              ┌──────────────┐
    │ Microphone   │              │ Speaker      │
    │ Recording    │              │ Output       │
    │              │              │              │
    │ Natural      │              │ Electronic  │
    │ sound        │              │ sound        │
    └──────────────┘              └──────────────┘

    KEY DIFFERENCES:
    ───────────────
    • Acoustic: Natural resonator filters and amplifies
    • Electric: No natural filtering, broad frequency response
    • Acoustic: Direct sound production
    • Electric: Requires amplification system
    • Acoustic: Limited to resonator range (98-1047 Hz)
    • Electric: Captures full harmonic spectrum
```

## Summary

The code works by:

1. **Loading** acoustic and electric recordings
2. **Analyzing** the acoustic's frequency response in the 98-1047 Hz range
3. **Designing** a bandpass filter and EQ curve to match the acoustic
4. **Applying** these transformations to the electric signal
5. **Validating** using similarity metrics (Euclidean distance, cosine similarity)
6. **Outputting** the transformed signal and visualizations

The instrument difference:
- **Acoustic**: Natural resonator amplifies 98-1047 Hz
- **Electric**: No resonator, broad frequency response
- **Solution**: Digital filtering and EQ to replicate the acoustic response

