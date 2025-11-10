======================================================================================================================================================
SUMMARY: Fourier Model Matching Results
======================================================================================================================================================
Instrument           Note     Fundamental (Hz)   Max Ratio    RMS Ratio    Decay τ (s)    
------------------------------------------------------------------------------------------------------------------------------------------------------
acoustic guitar      C        262.63             0.04         0.34         ∞              
acoustic guitar      D        145.90             0.08         0.39         ∞              
acoustic guitar      E        163.96             0.01         0.04         ∞              
classical guitar     C        131.14             0.01         0.06         ∞              
classical guitar     D        293.85             0.01         0.04         3.110          
classical guitar     E        164.75             0.01         0.06         10.546         
electric guitar      C        129.89             0.01         0.08         ∞              
electric guitar      D        146.26             0.03         0.32         ∞              
electric guitar      E        329.29             0.01         0.16         ∞              
======================================================================================================================================================

======================================================================================================================================================
DISTRIBUTION COMPARISON METRICS
======================================================================================================================================================

Amplitude vs. Time Distribution Comparison:
Instrument           Note     Correlation  RMSE         MAE          Norm. MSE   
------------------------------------------------------------------------------------------------------------------------------------------------------
acoustic guitar      C        0.2741       0.007498     0.004926     0.929029    
acoustic guitar      D        0.3936       0.009126     0.005475     0.845082    
acoustic guitar      E        0.4748       0.009140     0.005008     0.961715    
classical guitar     C        0.4378       0.039996     0.019873     0.947888    
classical guitar     D        0.4579       0.041512     0.014708     0.967067    
classical guitar     E        0.4759       0.041276     0.017455     0.947047    
electric guitar      C        0.4628       0.008346     0.004537     0.933177    
electric guitar      D        0.3558       0.004679     0.002489     0.874548    
electric guitar      E        0.1674       0.004121     0.001468     0.971988    

Amplitude vs. Frequency Distribution Comparison:
Instrument           Note     Correlation  RMSE         MAE          Norm. MSE   
------------------------------------------------------------------------------------------------------------------------------------------------------
acoustic guitar      C        0.2921       0.007762     0.000609     0.920363    
acoustic guitar      D        0.4193       0.005212     0.000388     0.828763    
acoustic guitar      E        0.5057       0.004015     0.000288     0.748092    
classical guitar     C        0.4658       0.005019     0.000225     0.784627    
classical guitar     D        0.7419       0.005327     0.000179     0.579146    
classical guitar     E        0.6044       0.004257     0.000167     0.657324    
electric guitar      C        0.4926       0.003780     0.000221     0.759944    
electric guitar      D        0.3818       0.006651     0.001021     0.874860    
electric guitar      E        0.1792       0.013293     0.001784     0.985631    
======================================================================================================================================================

Metric Explanations:
  Correlation: Pearson correlation coefficient (1.0 = perfect match, 0.0 = no correlation)
  RMSE: Root Mean Squared Error (lower is better, 0 = perfect match)
  MAE: Mean Absolute Error (lower is better, 0 = perfect match)
  Norm. MSE: Normalized Mean Squared Error (lower is better, 0 = perfect match)

Note: Max Ratio and RMS Ratio show how well the Fourier model matches the original.
  Ratio ≈ 1.0 means good match
  Ratio < 1.0 means model is quieter
  Ratio > 1.0 means model is louder

Decay τ (tau): Time constant for exponential decay. Smaller values = faster decay.
  Using decay parameters extracted from signal envelope.
  Manual decay version also generated: τ=1.0s for comparison.