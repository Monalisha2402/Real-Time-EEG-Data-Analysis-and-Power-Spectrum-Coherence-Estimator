# Real-Time-EEG-Data-Analysis-and-Power-Spectrum-Coherence-Estimator
## Project Overview

This project encompasses my scientific initiation venture into digital signal processing. The focus of this endeavor is on Brain-Computer Interface (BCI) development using the Ultracortex "Mark IV" EEG system from OpenBCI. The primary aim is to propose a non-parametric analysis technique for stress task data, employing Welch's power spectrum and coherence estimator.

The BCI is composed by acquiring, filtering, plotting data and interacting with user through Montreal stress task.

## BCI
### Acquisition and filtering 
The BCI is designed to interface with the Ultracortex Mark IV EEG hardware through the OpenBCI library for Python. The program initializes according to OpenBCI specifications, utilizing a queue mechanism to receive data at the designated sampling frequency of 250Hz. 

A band-pass Hamming linear phase Finite Impulse Response (FIR) filter with a size of 256 is employed to process the incoming data. Filtered samples are accumulated using a specialized list structure, facilitating efficient data management.

#### Raw Data
Filtered data is also stored in .txt files, each containing 9 columns and X rows, where X = t x 250, and t represents the recording time. The first 8 columns correspond to the 8 EEG channels (electrodes) pinned to the Ultracortex, and the last column represents recording time. 

- CH0 : FP2
- CH1 : FPz
- CH2 : F8
- CH3 : Cz
- CH4 : F4
- CH5 : P3
- CH6 : T6
- CH7 : T4
  
Raw data during both normal activity and stress-inducing tasks, such as the Stroop and Montreal tasks is taken

### Plotting Data
Real-time data visualization is achieved through a pyqtgraph-based interface. The plot data thread continuously rotates the filtered sample deque, plotting the data as it is filtered. A synchronization mechanism ensures alignment between the plot thread and the data acquisition and filtering thread.

### Montreal stress task
A dedicated thread handles the Montreal stress task display, which is based on previous research (Dedovic et al., 2005).  The Montreal stress task interface is developed using the tkinter library.

### Stroop task
Additionally, a Stroop task application (Stroop, 1935) is integrated into the project. This task is implemented in the stroopstress_class.py file and can be seamlessly switched with the Montreal task. Appropriate code modifications are required to enable the Stroop task.

This one is the part of main code for Motreal task (montrealstress.py must be in the same folder as main code) :

```
from montrealstress import Question
# Montreal
levelqueue=Queue()
question  = Question(level=1, timer = 20,levelqueue = levelqueue)
# Montreal thread
procs.append(Thread(name="test", target=question.loop, args=()))
```

For Stroop, these previous code lines can be erased and the following should be added : 

```
from stroopstress_class import Stroop
# Stroop
stroop  = Stroop()
# Stroop thread 
procs.append(Thread(name="test", target=stroop.loop, args=()))
```

## Simulations
The simulations folder contains files designed for observing the behavior of processing tools intended for analyzing EEG raw data in the frequency domain. These simulations cover various scenarios, including comparing Welch's method with the classic periodogram, coherence dynamics analysis, and the effects of random inserted peaks on frequency domains.

## Data analysis
1. Recorded EEG data is segmented into 50-second windows, and the power spectrum for each channel is estimated using Welch's method. 
2. Average power bands are computed for each channel window and presented in radar plots. Coherence estimation involves the use of Welch's periodogram for power and crossed spectra. 
3. Coherence is calculated as a frequency-dependent function, and matrices are generated to assess coherence in different frequency bands during stressor tasks. 
4. The analysis process is automated through the coerEEG.py script, which generates ratio matrices for further assessment. 
5. These ratio matrices are visually analyzed using corrEEG.py.
