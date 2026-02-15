# BEng Biomedical Engineering
### Object-Oriented Programming (5CCYB041) &ndash; Assessment 1


## Objective

To demonstrate proficiency in C++ by developing a program to perform a defined computational task.


# Assessment criteria

Your submission will be marked according to the following criteria:

**40%: Performance**
- Does the program compile?
- Does the program run successfully?
- Is the output correct?
- Does the program meet all requirements?
- Is the code for each individual aspect of the task correct?
- Are command-line arguments handled correctly?

**30%: Design**
- Is the code logically organised?
- Does it use appropriate C++ features?
- Does it make good use of control and data structures?
- Does it make good use of functions?
- Does the code include error checking and appropriate handling?

**30%: Maintainability**
- Is the code easily understandable?
- Is the code designed for modularity and re-use?
- Is the code organised across multiple files?
- Are functions and variables given interpretable names?
- Does the code follow established naming conventions?
- Does the code include comments where appropriate?
- Is code indentation correct throughout?




---

# Introduction 

An electrocardiogram (ECG) records the electrical activity of the heart. Each heartbeat produces a characteristic waveform with the following components (see figure below): 
- the P-wave (atrial depolarisation), 
- the QRS complex (ventricular depolarisation)
- the T-wave (ventricular repolarisation) 
. The R-peak is the tallest point of the QRS complex. 

Heart Rate Variability (HRV) is a key clinical metric used in wearable health devices to assess the autonomic nervous system. HRV is derived by analysing the time intervals between consecutive R-peaks (in milliseconds), known as RR intervals. 

![QRS complex](QRS.png)


# Instructions 

Your task in this assessment is to write a C++ program to read an ECG signal from a file, detect the R-peaks of the QRS complex, compute R-R intervals, determine Heart Rate Variability (HRV) metrics, and write a formatted analysis report to an output file.

Your program should perform the following main steps (these are described in more detail below):
1. Load the ECG data from a text file specified as a command-line argument.
2. Detect R-peaks (QRS detection) in the ECG signal using a threshold-based peak detection algorithm. 
3. Compute R-R intervals from the detected R-peaks, by calculating the time difference (in milliseconds) and instantaneous heart rate (in BPM) for each consecutive pair of peaks.
4. Compute HRV metrics from the R-R intervals, including: Mean R-R interval, SDNN (standard deviation of R-R intervals), RMSSD (root mean square of successive differences), and mean/min/max heart rate.
5. Classify the stress level based on the computed RMSSD value using clinical reference thresholds.
6. Write a summary of the analysis to an output text file (specified as a command-line argument), listing all detected peaks, R-R intervals, and the HRV summary.

# Program requirements

## Command-line interface 

Your program should accept at least 2 arguments: the filename containing the ECG recording, and the filename to use for the output file for the report of the analysis. 

If a third argument is provided, this should be interpreted as a voltage threshold $V_{\textrm{threshold}}$ in mV for the peak detection, which should be set to 0.5mV by default.

If a fourth argument is provided, this should be interpreted as the minimum time $t_{\textrm{min}}$ (in seconds) between peaks, which should be set to 0.3s by default.


## Loading the input data 

You are provided with two data files (`ecg_data1.txt` and `ecg_data2.txt`) containing synthetic ECG signals. Each line in these files contains two values: the time in seconds and the voltage in mV of the sampled signal. Your program should be capable of loading any file provided in this format.

## Detecting the R-peaks

Your program should detect peaks by scanning through the ECG signal, identifying a sample at index $i$ as an R-peak if all three of the following conditions are met:
- It is a local maximum: $V[i]>V[i-1]$ and $V[i]>V[i+1]$ 
- Its voltage exceeds the threshold: $V[i] > V_{\textrm{threshold}}$
- Sufficient time has elapsed since the last detected peak (refractory period): $t[i]-t[\textrm{last peak}] >= t_{\textrm{min}}$


## Computing the R-R intervals

Once R-peaks have been detected, your program should compute the R-R intervals in milliseconds by calculating the time difference between each consecutive pair of peaks (remember to take the sampling time interval into account). 

**TBD: do we need this?**: For each R-R interval, also compute the instantaneous heart rate in beats per minute (BPM), which is computed as $\textrm{BPM}[i] = 60,000 / \textrm{RR}[i]$ (where $\textrm{RR}[i]$ is the $i$th  R-R interval in milliseconds).


## Computing the HRV metrics 

From all the R-R intervals, your program should compute the following clinical HRV metrics: 
- The arithmetic mean of all R-R interval durations (in ms)
- The RMSSD (Root Mean Square of Successive Differences), computed from the successive differences between adjacent R-R intervals $t_{\textrm{RR}}$ as:

  ![$$ \textrm{RMSSD} = \sqrt{ \frac{1}{N-1} \sum_i^{N-1} \left( \textrm{RR}[i] - \textrm{RR}[i-1] \right)^2 } $$](eqn.png)

## Classifying the stress level 

Based on simplified clinical reference ranges, your program should use the RMSSD value to classify the stress level as follows:

| RMSSD Range | Classification | Interpretation | 
|---|---|:---|
| > 50 ms | Low Stress | Good parasympathetic tone | 
| 20 – 50 ms | Moderate Stress | Normal range |
| < 20 ms | High Stress | Low HRV — consider follow-up |
 
## Writing the result to file

Your program should write its final report to the text file specified as the second argument on the command-line. This should include the name of the file containing the EGC signal, the mean of the R-R intervals, the RMSSD and the stress level, in a format similar to:
```
data file: ecg_data1.txt
mean RR interval: 1.2
RMSSD: 1.123
stress level: low
```

