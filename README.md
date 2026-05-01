# QCHFT: Quantum Cross-Hybrid Fine-Tuning for LLMs

This repository hosts the official implementation of **QCHFT (Quantum Cross-Hybrid Fine-Tuning for LLMs)**.

## Paper Status

📌 **Manuscript Status:** *Accepted*  
📌 **Target Journal:** *IEEE Transactions on Quantum Engineering (IEEE TQE)*  
📌 **Code Release:** The **full training/evaluation code and reproduction scripts** will be released **after the paper is officially published**.  

## 🚀 Usage

### Basic Configuration

````python
# Default Hyperparameters for IEEE TQE QCHFT experiments
SEED          = 1755
NUM_EPOCHS    = 1
BATCH_SIZE    = 1
LEARNING_RATE = 5e-4
LORA_R        = 2      # Rank (measurement count) of the quantum adapter
NUM_QLAYERS   = 4      # Number of layers in the PQC
````

### Core Implementation Logic

**1. Automatic Qubit Scaling**

In the QCHFT framework, the number of qubits ($n$) is not a fixed hyperparameter.  
It is automatically determined by the maximum dimensionality between the input and output features $max(d_{in}, d_{out}$):
- For Amplitude-based PQCs (MPS, APC, HPC), the qubit count is calculated as $n = \lceil \log_2(\max(d_{in}, d_{out})) \rceil$.  
- This logic ensures the feature vectors can be perfectly mapped into the quantum Hilbert space without capacity bottlenecks during state preparation or output projection.

**2. Rank-Controlled Quantum Measurement**

The rank parameter ($r$) dictates the dimension of the quantum features by controlling the number of measurement outcomes. 
Crucially, the rank must not exceed the total number of qubits ($r \le n$).

- For a given `rank=r`, the model performs measurements (typically Pauli-Z expectation values) on the last $r$ qubits of the circuit.
- For example, if `rank=2`, qubits $n-2$ and $n-1$ are measured. 

**3. AQB Measurement Logic**

The AQB (LinearA-Quantum-LinearB) adapter family follows a distinct measurement scheme independent of the $r$ parameter:

- AQB measurement is structurally independent of the variable rank value.
- The number of measurement outcomes is always equal to the total qubit count ($n$).

---

## 📊 Datasets & Results

The datasets and compiled results are available on [IEEE DataPort](https://ieee-dataport.org/documents/qchft-quantum-cross-hybrid-fine-tuning-llms-0).

- **Datasets:** Includes IoT Resource Allocation, SST-2, and HellaSwag.
- **Backbones:** Extensive experiments cover Mamba2-130M, GPT-Neo-125M, Pythia-160M, Llama-3.2-1B, and Qwen-3.5-2B.

---

## Updates

- **[2025-12-13]** Manuscript submitted to IEEE TQE; review in progress.
- **[2026-04-30]** Manuscript accepted.
- **[TBD]** Code release upon publication.

## Citation

If you find this work helpful, please cite our paper (BibTeX will be provided after publication).

```bibtex
@article{qchft_tqe,
  title   = {QCHFT: Quantum Cross-Hybrid Fine-Tuning for LLMs},
  author  = {Yuanjie Li, Kyungmin Lim, Jinsuk Baek, Minho Jo*},
  journal = {IEEE Transactions on Quantum Engineering},
  year    = {TBD},
  note    = {Accepted}
}
