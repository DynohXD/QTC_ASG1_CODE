# QTC_ASG1_CODE
Implementation of Quantum Fourier Transform (QFT)

Quantum Fourier Transform (QFT) - Dynoh
Theoretical Foundation
https://medium.com/@marcell.ujlaki/exploring-quantum-computing-demystifying-quantum-fourier-transformations-unveiling-the-math-with-5d74f3f8025f 
The Quantum Fourier Transform (QFT), discovered by Don Coppersmith in his June 1994 paper titled "An Approximate Fourier Transform Useful in Quantum Factoring” is a fundamental algorithm in quantum computing. It serves as a quantum analog of the Discrete Fourier Transform (DFT), performing a similar mathematical operation but on qubits instead of classical data. While DFT converts a sequence of complex numbers from the time domain to the frequency domain, QFT transforms quantum states into phase-encoded superpositions, enabling powerful computational capabilities through quantum parallelism, such as in quantum phase estimation where it is used for precision measurements in quantum chemistry and material science. QFT plays a pivotal role in Shor’s Algorithm, where it is used for period finding, a crucial step in the integer factorization process (Wikipedia contributors, 2025). By efficiently identifying the periodicity of functions related to modular exponentiation, the QFT enables Shor’s Algorithm to factor large numbers exponentially faster than classical algorithms, posing a significant challenge to traditional cryptographic systems like Rivest–Shamir–Adleman (RSA), a widely used encryption algorithm that secures data by relying on the computational difficulty of factoring large prime numbers. (Quantum Cryptography - Shor'S Algorithm Explained, 2022)
 
Onto the Algorithms Themselves, below is an example of the DFT Formula:
 
Figure 1, Source: (Explaining DFT formula, n.d.)
Explanation of DFT Formula Components
In this formula, X(k) represents the k-th frequency component of the transformed signal, where X represents the transformed version of the input signal x(n) in the signal domain. It indicates how much of the frequency corresponding to the index k is present in the original signal, and the result is a complex number that provides both amplitude and phase information. The summation symbol ∑ indicates that the equation sums over all N discrete time points in the input signal x(n), with the value of n ranging from 0 to N - 1.

The term x(n) refers to the input signal in the time domain at the sample point n. This represents the actual values of the signal being transformed, such as sound amplitudes or pixel intensity within an image. The complex exponential e-j2πkn/N , also known as the twiddle factor, performs the frequency analysis by introducing a rotation in the complex plane to decompose the signal into its frequency components (Hillier et al., 2015/2015). Here, j is the imaginary unit, 2π converts the angle to radians, k is the index of the frequency component, is the time sample index, and N is the total number of samples ensuring correct frequency scaling. 
* Twiddle factors are complex number constants used when recursively combining results from smaller discrete Fourier Transforms

This formula projects the time-domain signal onto a set of complex exponential functions, essentially sine and cosine waves. The result X(k) tells you how much of the -th frequency is present in the original signal x(n).
Now that a basis of understanding has been made with DFT, the QFT formula can be seen as follows: 
 
Figure 2, Source: (Wikipedia contributors, 2025)

Detailed QFT Circuit Formula
 
Figure 2, Source: (Wikipedia contributors, 2025)

Explanation of QFT Formula Components
Figure 1 shows that the QFT transforms the basis state ∣j>, into an equal superposition of all basis states ∣k>, each weighted by a phase factor e2πi(jk/N) where i is the imaginary unit. N = 2n represents the dimension of the quantum system, corresponding to n qubits, for example:
•	For 1 qubit, there are N = 21 = 2 possible basis states (∣0⟩ and ∣1⟩).
•	For n qubits, there are N = 2n possible basis states (ranging from ∣0⟩ to ∣2n - 1⟩).

Figure 2 shows the QFT formula broken down into step-by-step transformations that occur on each qubit in the quantum circuit. The steps are as follows:
•	Hadamard Gates, represented by |0> + e2πi(phase) |1> creates superpositions for each qubit for parallel computation in quantum systems.
•	Controlled-phase shift gates represented as e2πi(phase) is the phase factor that introduces phase relationships between qubits. The phase factors encode the frequency information and ensure the qubits interfere in a way that reveals the periodicity of the original input.
The tensor product plays a crucial role in combining these individual qubit transformations into the overall quantum state where the first term represents the transformation of the last qubit with a phase dependent on the least significant bit and the second term incorporates the influence of the second-to-last qubit and the last qubit. This process continues until the first term, where the phase depends on all qubits.

The QFT circuit design consists of three main components that work together to perform the transformation efficiently on quantum states. First, Hadamard Gates is applied to create uniform superpositions of qubit states, laying the foundation for parallel computation. Next, Controlled Phase Shift Gates introduce precise phase relationships between qubits, encoding the necessary frequency information into the quantum state. Finally, Swap Gates are used to reverse the bit order at the end of the circuit, as the QFT naturally outputs result in a reversed binary format. This sequence of operations allows the QFT to be implemented with remarkable efficiency, contributing to its exponential speedup over classical Fourier Transform methods.
A simple 3-qubit QFT circuit can be illustrated as shown:
|q0> --- H ----●---------●---------X
         |     |         |
|q1> --- H ----●---------X
         |
|q2> --- H ----X
 
Quantum vs Classical
Features	DFT	QFT
Time complexity	O(N^2)	O(log^2 N)
Energy Consumption	Consume more energy as data size increases due to hardware limitations and cooling.	Lower energy is used to do the same computation as classical, however, energy incurred through cooling requirements
Scalability	Inefficient for larger N values	Exponentially efficient with N
(Wikipedia contributors, 2025), (Musk, 2020)
* O(N^2) means that as N grows, the computational resources required is the square of N
* O(log^2 N) means that even as N grows exponentially, the computational resources required only grow logarithmically.

	DFT	QFT
Use Cases	•	Signal-Related Cryptographic Protocol

•	Audio and File Compression (JPEG & MP3)

•	Spectral Analysis in physics and data science (e.g.  studying seismic waves in earthquake research, studying neural activity patterns with Electroencephalogram or EEG)

	•	Break RSA Encryption in Shor’s Algorithm

•	Speed up unstructured searches in Grover’s Algorithm

•	Quantum phase estimation for quantum chemistry simulation and material science

(Discrete Fourier Transform: Meaning & Applications, n.d.)
 
Implementation Challenges
One of the primary challenges in implementing QFT is dealing with the hardware limitations. Gate errors and decoherence are major issues because QFT circuits rely heavily on precise control of qubits. However, today’s quantum computers have short coherence times, meaning qubits lose their quantum state quickly due to environmental interactions. This causes errors before computations can be completed accurately. Additionally, limited qubit connectivity poses another challenge. The controlled-phase gates used in QFT require qubits to interact with each other, but many quantum devices have restricted connectivity, making multi-qubit operations difficult to perform efficiently. (Mohammadbagherpoor et al., 2019)

Another significant hurdle in QFT implementation is dealing with noise. The controlled-phase shifts are highly sensitive to environmental disturbances, leading to phase distortions that reduce the accuracy of the final output. Although Quantum Error Correction techniques exist to mitigate these issues, they require significant overhead in terms of additional qubits and complex error-correcting protocols. This makes practical QFT implementation challenging on current Noisy Intermediate-Scale Quantum (NISQ) devices. (Mohammadbagherpoor et al., 2019)

Scalability is a key concern when implementing QFT on current quantum systems. QFT circuits tend to scale poorly in the Noisy Intermediate-Scale Quantum (NISQ) area due to various hardware constraints and error rates. While QFT theoretically offers an exponential speedup over classical Fourier transforms, it requires a logarithmically increasing number of qubits as the problem size grows. This means that QFT can only outperform the classical Fast Fourier Transform (currently the most efficient algorithm for computing DFT) method at very large input sizes that are currently beyond the capability of any existing quantum hardware. As a result, the full potential of QFT remains untapped until a more robust and scalable quantum system is developed. (Mohammadbagherpoor et al., 2019)
QIskit Simulation
 
Code source: medium.com
The image represents QFT being applied to a 4-qubit quantum circuit, the circuit includes:
•	X gates (Blue Box) initialize |1100⟩ (binary for 12).
•	H gates (Red Box) introduce superposition.
•	Controlled Phase gates (Blue lines with 0 ends), for the QFT transformation.
•	Final swap gates (Blue Lines with X ends) ensure the correct bit order.

What does it do:
For the Initial State Preparation, the given number, which in this case is 12, is converted into its binary representation, 1100. The corresponding qubits are then set to |1⟩ using X-gates on qubits 2 and 3.
Moving on to applying the QFT function, QFT rotations are performed by applying Hadamard gates to create superpositions. Controlled phase rotations are then applied to introduce quantum interference.
Lastly, in swapping the qubits, the register order is reversed using swap gates to match the classical Fourier Transform.



Bloch Sphere Representations
Each Bloch sphere shows the state of an individual qubit. The QFT transforms basis states into superpositions with complex amplitudes, which are represented as different orientations on the Bloch spheres.
•	Qubits 0 and 1: Angles suggest superposition states with different phases.
•	Qubits 2 and 3: Indicate the influence of initial X-gates and phase shifts.

Quantum State Representation
The final quantum state is written as a linear combination of basis states:
 This formula describes how QFT has transformed the initial state |1100⟩ into a state with distributed amplitudes and phases.

IBM Quantum Backend Simulation
 
IBM Simulation Circuit
IBM's quantum backend does not explicitly support Hadamard gates, so I had to transpile the circuit. The transpiling process decomposes the H gate into a sequence of supported gates:
 
This ensures compatibility with IBM's hardware, which primarily supports rotation and basic Pauli gates rather than Hadamard directly, the results can be seen in the IBM Simulation Circuit above.
Functionality demonstration

How Shor’s Algorithm Breaks Encryption:
Online security systems like RSA (Rivest–Shamir–Adleman) encryption, keep data safe by using huge numbers that are the product of two prime numbers (e.g. 5141 = 53 × 97). The idea is that multiplying two numbers is easy, but reverse engineering the equation and figuring out the original prime numbers from their product is extremely difficult, at least for classical computers.
Shor’s Algorithm efficiently finds these prime numbers much faster using quantum computing. Instead of checking every possible factor one by one like a normal computer, it uses quantum formulas like QFT to find repeating patterns in numbers, which leads directly to the factors.
Since RSA encryption relies on the difficulty of factoring large numbers, Shor’s Algorithm could break encryption by quickly finding these hidden prime numbers.

Shor’s Algorithm contains 3 parts:
1)	Classical Preprocessing (Order Finding)
o	Choose 𝑎
o	Check Greatest Common Divisor
o	Define the periodic function
2)	Quantum Phase Estimation using QFT
o	Find period 𝑟 of the periodic function
3)	Classical Post-Processing 
o	Compute the Greatest Common Divisor from 𝑟 to get factors








Sample Output
 

