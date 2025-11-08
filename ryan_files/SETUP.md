# Sasando Acoustic Replication - Setup Guide

## Overview
This project implements a comprehensive pipeline to replicate the authentic acoustic sound of the traditional sasando using an electric version. The pipeline analyzes the frequency response of the acoustic resonator (98-1047 Hz) and applies transformations to match it.

## Virtual Environment Setup

A virtual environment has been created at `venv/` with all necessary dependencies installed.

### Activating the Virtual Environment

```bash
source venv/bin/activate
```

### Installing Additional Dependencies

If you need to reinstall dependencies:

```bash
pip install -r requirements.txt
```

## Jupyter Kernel

A Jupyter kernel named **"Python (Sasando FFT)"** has been created and installed. 

### Using the Kernel in Jupyter

1. Open the notebook `Sasando_Acoustic_Replication.ipynb`
2. In Jupyter, go to **Kernel** → **Change Kernel** → **Python (Sasando FFT)**
3. The kernel will use the virtual environment with all required packages

### Verifying the Kernel

To see all available kernels:
```bash
jupyter kernelspec list
```

You should see `sasando-fft` in the list.

## Using the Notebook

### Prerequisites

1. **Audio Files**: You'll need:
   - `acoustic_sasando.wav` - Recording of acoustic sasando
   - `electric_sasando.wav` - Recording of electric sasando

   OR use CSV files with time, amplitude format

2. **Update File Paths**: In Phase 2 of the notebook, update:
   ```python
   ACOUSTIC_FILE = 'path/to/acoustic_sasando.wav'
   ELECTRIC_FILE = 'path/to/electric_sasando.wav'
   ```

### Running the Notebook

1. Open `Sasando_Acoustic_Replication.ipynb` in Jupyter
2. Select the **"Python (Sasando FFT)"** kernel
3. Run cells sequentially from top to bottom
4. The notebook will:
   - Load and preprocess audio files
   - Extract features (time, frequency, advanced)
   - Characterize the resonator response
   - Design and apply transformations
   - Compute similarity metrics
   - Generate visualizations
   - Save the transformed audio

### Output Files

- `transformed_electric_sasando.wav` - The transformed electric signal
- `sasando_analysis_results.png` - Comprehensive visualization of results

## Features Implemented

### Phase 1: Setup
- Library imports with fallbacks
- Configuration parameters

### Phase 2: Data Loading
- Support for WAV and CSV files
- Audio normalization
- Resampling capabilities

### Phase 3: Feature Extraction
- Time domain: RMS, ZCR, Energy
- Frequency domain: FFT, Spectral Centroid, Rolloff, Bandwidth
- Advanced: MFCC, Chroma, Spectral Contrast, Tonnetz (if librosa available)

### Phase 4: Resonator Characterization
- Frequency response analysis (98-1047 Hz)
- Peak detection
- Gain profile extraction

### Phase 5: Transformation Design
- Bandpass filter design
- EQ curve matching
- Frequency response matching

### Phase 6: Transformation Application
- Bandpass filtering
- EQ application
- Output normalization

### Phase 7: Similarity Metrics
- Euclidean distance
- Cosine similarity
- MFCC distance

### Phase 8: Visualization
- Time domain waveforms
- Frequency response comparison
- Spectrograms
- Feature comparisons
- Metric visualizations

## Dependencies

- **numpy** - Numerical computing
- **scipy** - Scientific computing (signal processing, FFT)
- **matplotlib** - Visualization
- **librosa** - Advanced audio analysis (optional but recommended)
- **soundfile** - Audio I/O (optional, falls back to scipy)
- **jupyter** - Notebook environment
- **ipykernel** - Jupyter kernel

## Troubleshooting

### Kernel Not Found
If the kernel doesn't appear:
```bash
source venv/bin/activate
python -m ipykernel install --user --name=sasando-fft --display-name="Python (Sasando FFT)"
```

### Missing Libraries
If you get import errors:
```bash
source venv/bin/activate
pip install -r requirements.txt
```

### Audio File Issues
- Ensure files are in WAV format (or CSV with time, amplitude columns)
- Check file paths are correct
- The notebook will use synthetic data for demonstration if files aren't found

## Notes

- The notebook gracefully handles missing optional libraries (librosa, soundfile)
- Synthetic data is generated if audio files aren't found (for testing)
- All transformations are designed to match the 98-1047 Hz resonator range
- Metrics compare transformed electric signal to acoustic reference

