# RSA CRYPTOSYSTEM: IMPLEMENTATION AND SIDE-CHANNEL ANALYSIS

AUTHOR: MathisN3x
DATE: 2026
LICENSE: Educational use only. Not for production.


TABLE OF CONTENTS

1. Overview
2. Mathematical Foundations
3. Notebook 1: Classical RSA Implementation
4. Notebook 2: Side-Channel Attack Using Deep Learning
5. Comparison of Both Approaches
6. Requirements and Installation
7. Usage Instructions
8. Limitations
9. References


## 1. OVERVIEW

This repository contains two Jupyter notebooks that explore the RSA cryptosystem
from two different perspectives:

Notebook 1 (RSA_test.ipynb):
- Implements RSA encryption and decryption from scratch
- Focuses on correct mathematical implementation
- Suitable for learning how RSA works internally

Notebook 2 (rsa.ipynb):
- Simulates a side-channel attack on RSA
- Uses deep learning to recover private key bits
- Demonstrates physical implementation vulnerabilities

## 2. MATHEMATICAL FOUNDATIONS

### 2.1 Key Generation

Let p and q be two distinct prime numbers.

Compute the modulus N:
$`\ N = p \times q `$

Compute Euler's totient function phi(N):
$`\ \varphi(N) = (p - 1)  \times (q - 1)`$

Select a public exponent e such that:
$`\ \gcd(e, \varphi(N)) = 1`$

Compute the private exponent d as the modular inverse of $`\ e `$ modulo $`\ \varphi(N)`$:
$`\ d = e^{-1} \mod \varphi(N)`$

This satisfies:
$`\ e \times d ≡ 1 \mod \varphi(N)`$

### 2.2 Encryption

For a plaintext message represented as an integer $`\ m (0 ≤ m < N) `$:
$`\ c = m^e \mod N`$

### 2.3 Decryption

For a ciphertext integer c:
$`\ m = c^d \mod N`$

### 2.4 Correctness Proof (Euler's Theorem)

If gcd(m, N) = 1, then:
$`\ m^{(\varphi(N))} ≡ 1 \mod N `$

Therefore:
$`\ m^{(e \times d)} = m^{(k.\varphi(N) + 1)} = (m^{(phi(N))})^k \times m ≡ 1^k \times m = m \mod N`$


## 3. NOTEBOOK 1: CLASSICAL RSA IMPLEMENTATION (RSA_test.ipynb)

### 3.1 Description

This notebook implements RSA with explicit, readable Python code. Each character
of a message is converted to its ASCII integer value, encrypted using the public
key, and later decrypted using the private key.

### 3.2 Functions
```
Function Name: generate_primes()
Description: Selects two distinct random primes from a generated list
Input: None
Output: N (modulus), phi (Euler's totient)
```
```
Function Name: convert_msg(msg)
Description: Converts a string to a list of ASCII integers
Input: msg (string)
Output: converted_msg (list of integers)
```
```
Function Name: crypt_msg(converted_msg)
Description: Encrypts each integer using the public key
Input: converted_msg (list of integers)
Output: crypted_msg (list of encrypted integers)
Formula: c = (m^e) mod N
```
```
Function Name: decrypt_msg(crypted_msg)
Description: Decrypts each integer using the private key
Input: crypted_msg (list of encrypted integers)
Output: decrypted_msg (list of integers), final_msg (string)
Formula: m = (c^d) mod N
```
### 3.3 Example Code
```
msg = "Salut a tous"
msg = msg.lower()
converted_msg = convert_msg(msg)
crypted_msg = crypt_msg(converted_msg)
decrypted_msg, final_msg = decrypt_msg(crypted_msg)
print(final_msg)
```
### .4 Example Output

Input: "Salut a tous"
Output after encryption: [list of integers]
Output after decryption: "salut a tous"

### 3.5 Implementation Details

- Prime generation uses sympy.primerange() with a random limit between 50 and 100
- Public exponent e is the smallest integer > 1 where $`\gcd(e, \varphi) = 1`$
- Private exponent d is found by iterating until ```(e * i) % phi == 1```
- Modular exponentiation uses the ** operator (not constant-time)

## 4. NOTEBOOK 2: SIDE-CHANNEL ATTACK (rsa.ipynb)


### 4.1 Description

This notebook simulates a side-channel attack that exploits power consumption
variations during RSA's modular exponentiation. The attack targets the
square-and-multiply algorithm, where the pattern of operations reveals the bits
of the private exponent.

### 4.2 The Square-and-Multiply Algorithm

The algorithm computes base^exponent mod n by iterating through bits of the
exponent from most significant to least significant:

Pseudo-code:
```
function power_mod(base, exponent, n):
    res = 1
    base = base mod n
    for each bit in binary representation of exponent (from MSB to LSB):
        # Square step - always performed
        res = (res * res) mod n
        
        # Multiply step - performed only if current bit = 1
        if bit == '1':
            res = (res * base) mod n
    return res
```
### 4.3 Power Consumption Pattern

For each bit of the exponent:
```
If bit = 0:
    - Operation 1: Square (power consumption level: 1.0)
    - Total operations: 1

If bit = 1:
    - Operation 1: Square (power consumption level: 1.0)
    - Operation 2: Multiply (power consumption level: 1.5)
    - Total operations: 2
```
### 4.4 Simulated Power Trace Generation
```
Function: generate_dataset(num_samples=1000)

For bit = 0:
    signal = [1.0] + [0.1] * 5
    signal = signal + GaussianNoise(mean=0, std=0.08)

For bit = 1:
    signal = [1.0, 1.5] + [0.1] * 4
    signal = signal + GaussianNoise(mean=0, std=0.08)

Output shape: (num_samples, 6, 1) - 6 time steps, 1 feature channel
```
### 4.5 Deep Learning Model Architecture

Model type: 1D Convolutional Neural Network (CNN)

Layer structure:
-------------------------------------------------------------------------------
Layer (type)               Output Shape              Parameters
-------------------------------------------------------------------------------
Input                      (None, 6, 1)              0
Conv1D(filters=8, kernel_size=3, activation='relu')  (None, 4, 8)              32
Flatten()                  (None, 32)                0
Dense(units=8, activation='relu')                    (None, 8)                 264
Dense(units=1, activation='sigmoid')                 (None, 1)                 9
-------------------------------------------------------------------------------
Total parameters: 305
Trainable parameters: 305

### 4.6 Loss Function

Binary cross-entropy:

$`\ L(y, y_{hat}) = -[y. log(y_{hat}) + (1 - y) . log(1 - y_{hat})]`$

where:
- $`\ y `$ is the true label (0 or 1)
- $`\ y_{hat} `$ is the predicted probability that the label is 1

### 4.7 Optimizer

Adam (Adaptive Moment Estimation)

Update rule:
$`\ m_t = \beta_1 \times m_{t-1} + (1 - \beta_1) \times g_t`$  
$`\ v_t = \beta_2 \times v_{t-1} + (1 - \beta_2) \times g_t^2`$  
$`\ m_{hat_t} = m_t / (1 - \beta_1^t)`$  
$`\ v_{hat_t} = v_t / (1 - \beta_2^t)`$  
$`\ \theta_{t+1} = \theta_t - \alpha \times m_{hat_t} / (sqrt(v_{hat_t}) + \epsilon)`$  

Parameters used in implementation:

- $`\ \alpha (learning rate) = 0.001 (default)`$  
- $`\ \beta_1 = 0.9  (default)`$  
- $`\ \beta_2 = 0.999 (default)`$  
- $`\ \epsilon = 1e-7 (default)`$  

### 4.8 Attack Workflow

**Step 1: Data Generation**
- Generate 3000 training samples (power traces with known bits)
- Generate 500 test samples

**Step 2: Model Training**
- Epochs: 10
- Batch size: 32
- Training time: approximately 2-5 seconds

**Step 3: Evaluation**
- Calculate accuracy on test set
- Expected accuracy: 100% for simulated data

**Step 4: Exploitation**
- Execute power_mod() with a known exponent (e.g., 0b101)
- Extract power traces for individual bits
- Pad traces to length 6 with rest values (0.1)
- Feed each trace to the trained model
- Model outputs probability that bit = 1
- Decision threshold: 0.5

### 4.9 Mathematical Formulation of the Attack

Let $`\ T(b) `$ be the power trace for exponent bit b in {0, 1}.

The classifier learns a function $`\ f: R^6 → [0, 1] `$ such that:

$`\ f(T) ≈ P(b = 1 | T)`$

The decision rule is:

$`\ b_hat = 1 if f(T) > 0.5 else 0`$

The attack succeeds when b_hat = b for a sufficient number of bits to
reconstruct the entire private exponent.

### 4.10 Expected Results

Test accuracy: 100.00%

Example prediction:
- First bit (actual = 1): model predicts 1
- Second bit (actual = 0): model predicts 0

## 5. COMPARISON OF BOTH APPROACHES


Aspect                        RSA_test.ipynb              rsa.ipynb
--------------------------------------------------------------------------------
Primary focus                 Correct RSA implementation  Physical attack on RSA
Methodology                   Number theory               Deep learning
Threat model                  Mathematical (none)         Side-channel
Output                        Encrypted/decrypted message Recovered exponent bits
Prerequisites                 Basic modular arithmetic    Basic ML knowledge
Security assumption           Key is mathematically secure Implementation leaks
Use of padding                None                        Not applicable
Real-time execution           Instant                     Training required
Code complexity               Low                         Medium

## 6. REQUIREMENTS AND INSTALLATION


### 6.1 System Requirements

- Operating System: Linux, macOS, or Windows
- Python version: 3.8 or higher
- RAM: 4 GB minimum (8 GB recommended)
- Disk space: 500 MB

### 6.2 Python Packages

Common packages (both notebooks):
- numpy (version 1.21.0 or higher)

Notebook 1 specific:
- sympy (version 1.9 or higher)

Notebook 2 specific:
- tensorflow (version 2.10.0 or higher)

### 6.3 Installation Commands

Create a virtual environment (recommended):

**On Linux/macOS:**
```
python3 -m venv rsa_env
source rsa_env/bin/activate
```
**On Windows:**
```
python -m venv rsa_env
rsa_env\Scripts\activate
```
**Install packages:**
```
pip install numpy sympy
pip install tensorflow
```
**Verify installation:**
```
python -c "import numpy, sympy; print('OK')"
python -c "import tensorflow; print(tensorflow.__version__)"
```

## 7. USAGE INSTRUCTIONS


### 7.1 Notebook 1 (Classical RSA)

**Step 1:** Launch Jupyter Notebook
```jupyter notebook RSA_test.ipynb```

**Step 2:** Run all cells sequentially
```Kernel -> Restart & Run All```

**Step 3:** Modify the message
Change the variable 'msg' in the appropriate cell

**Step 4:** Observe the output
The notebook will display:
- Generated primes and modulus
- Public key (N, e)
- Private key (N, d)
- Encrypted message (list of integers)
- Decrypted message (string)

### 7.2 Notebook 2 (Side-Channel Attack)

**Step 1:** Launch Jupyter Notebook
```jupyter notebook rsa.ipynb```

**Step 2:** Run all cells sequentially
```Kernel -> Restart & Run All```

**Step 3:** Observe the training process
The notebook will display:
- Training progress (10 epochs)
- Test accuracy (should be near 100%)
- Example predictions for exponent bits

**Step 4:** Modify parameters (optional)
You can change:
- num_samples in generate_dataset()
- epochs in model.fit()
- The exponent value in power_mod()


## 8. LIMITATIONS


### 8.1 General Limitations

- Educational use only. Not for production or real security applications.
- No cryptographic padding (OAEP or PKCS#1) is implemented.
- Small key sizes (primes < 100) for demonstration purposes.
- ASCII encoding only. Unicode characters may cause errors.

### 8.2 Notebook 1 Specific Limitations

- The public exponent e is always the smallest possible value.
- No constant-time operations (vulnerable to timing attacks).
- Prime generation is not cryptographically secure.
- Messages longer than N are not handled.

### 8.3 Notebook 2 Specific Limitations

- Power traces are synthetic, not measured from real hardware.
- Noise model is simplified (i.i.d. Gaussian).
- No electromagnetic or cache-timing side-channels simulated.
- Attack assumes perfect alignment of traces.
- Real-world attacks require preprocessing (alignment, filtering).

### 8.4 Security Disclaimer

DO NOT USE THIS CODE TO PROTECT SENSITIVE DATA. The implementations are
intentionally simplified for educational clarity and contain multiple
vulnerabilities that would be catastrophic in production.

## 9. REFERENCES

[1] Rivest, R. L., Shamir, A., & Adleman, L. (1978). A method for obtaining
    digital signatures and public-key cryptosystems. Communications of the ACM,
    21(2), 120-126.

[2] Kocher, P. C. (1996). Timing attacks on implementations of Diffie-Hellman,
    RSA, DSS, and other systems. Advances in Cryptology - CRYPTO '96, 104-113.

[3] Mangard, S., Oswald, E., & Popp, T. (2007). Power analysis attacks:
    Revealing the secrets of smart cards. Springer Science & Business Media.

[4] Menezes, A. J., Van Oorschot, P. C., & Vanstone, S. A. (2018). Handbook of
    applied cryptography. CRC press.

[5] Goodfellow, I., Bengio, Y., & Courville, A. (2016). Deep learning.
    MIT Press.
