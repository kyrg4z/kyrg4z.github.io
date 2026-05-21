---
title: Fourier Transform
draft: "false"
---


![[fourier.png]]
### 1\. The Time Domain (The Input)

To create our example, we combined three different sine waves:

- **10 Hz**: A slow, low-frequency wave.
- **15 Hz**: A slightly faster wave.
- **50 Hz**: A high-frequency "jitter."

When added together, they create the chaotic blue line in the top graph. Looking at that raw wave with the naked eye, it is nearly impossible to tell exactly which frequencies are hidden inside.

### 2\. The Frequency Domain (The Result)

By applying the **Fast Fourier Transform (FFT)**, we "solve" the signal. The bottom graph shows the magnitude of each frequency.

- Notice the sharp peaks at **10**, **15**, and **50**.
- The height of the peak represents the strength (amplitude) of that specific frequency in the original signal.

---

### How it Works Mathematically

The Fourier Transform 
$$
F(ω)
$$
 of a continuous function 
$$
f(t)
$$
 is defined by the following integral:

$$
F(ω)=∫−∞∞​f(t)e−iωtdt
$$

In practical computing (like the graph above), we use the **Discrete Fourier Transform (DFT)**. It correlates the input signal against a series of sine waves of different frequencies. If the signal "matches" a specific frequency (like 50 Hz), the math produces a high value at 그 point, creating the spikes you see in the frequency plot.

### Why is this useful?

- **Audio Engineering:** Removing background hum (like a 60 Hz buzz) from a recording.
- **Telecommunications:** Decoding different data streams sent over the same radio frequency.
- **Medical Imaging:** Processing MRI signals to create a visual image of the body.


---
## Step 1: Defining the Input Signal

First, we define our combined signal as a function of time 
$$
t
$$
. For the example provided, the signal 
$$
x(t)
$$
 is the sum of three sine waves:

$$
x(t)=sin(2π⋅10t)+sin(2π⋅15t)+sin(2π⋅50t)
$$

To process this on a computer, we **sample** it. Instead of a continuous line, we take a list of 
$$
N
$$
 specific data points (amplitudes) at fixed intervals.

### Step 2: The Discrete Fourier Transform (DFT)

The computer uses the **Discrete Fourier Transform (DFT)** formula to "probe" the signal for specific frequencies. For every possible frequency 
$$
k
$$
, it calculates a value 
$$
X[k]
$$
:

$$
X[k]=n=0∑N−1​x[n]⋅e−iN2π​kn
$$

**What this actually means:**

- $$
	x[n]
	$$
	 is your raw signal data point.
- $$
	e−iN2π​kn
	$$
	 represents a "test" frequency (a complex sinusoid).
- The formula essentially performs a **dot product** between your signal and a perfect wave of frequency 
	$$
	k
	$$
	.

### Step 3: Finding the Resonance (The "Calculation")

Imagine the Fourier Transform as a series of tests:

1. **Test 1 Hz:** The math multiplies your signal by a 1 Hz wave. Because your signal (10, 15, 50 Hz) doesn't "match" 1 Hz, the peaks and valleys cancel each other out in the summation. The result is near **zero**.
2. **Test 10 Hz:** Now, the math multiplies your signal by a 10 Hz wave. Because your signal *contains* a 10 Hz component, they align perfectly. The summation grows very large because the waves reinforce each other.
3. **Result:** This creates a **peak** at the 10 Hz index.

### Step 4: Converting Complex Numbers to Magnitude

The result 
$$
X[k]
$$
 from the formula above is a **complex number** (containing both a real and an imaginary part). To get the peaks you saw on the graph, we calculate the **Magnitude**:

$$
Magnitude=∣X[k]∣=Re(X[k])2+Im(X[k])2​
$$

Finally, we normalize the value by dividing by the number of samples (
$$
N
$$
) to get the actual amplitude of the wave.

---

### Summary of the "Solve"

| Step  | Action          | Result                                                                                                 |
| ----- | --------------- | ------------------------------------------------------------------------------------------------------ |
| **1** | **Sampling**    | Converts the wave into a list of numbers.                                                              |
| **2** | **Correlation** | Multiplies the list by "test" frequencies (1 Hz, 2 Hz, 3 Hz...).                                       |
| **3** | **Summation**   | Frequencies that don't exist in the signal sum to ~0; frequencies that do exist sum to a large number. |
| **4** | **Mapping**     | The index of the large sum tells us exactly what the frequency is (e.g., the 10th index = 10 Hz).      |