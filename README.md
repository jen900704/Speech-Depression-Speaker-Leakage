# Who is Speaking or Who is Depressed? A Controlled Study of Speaker Leakage in Speech-Based Depression Detection

This repository provides research code for the experimental pipeline used in the paper:

**"Who is Speaking or Who is Depressed? A Controlled Study of Speaker Leakage in Speech-Based Depression Detection."**

The project studies whether speech-based depression detection models learn depression-related acoustic cues or instead rely on speaker identity information. We use a size-matched speaker-overlap controlled split on the DAIC-WOZ dataset to compare two settings:

- **Training Set A / Speaker-Independent Setting:** no speaker overlap between training and test sets.
- **Training Set B / Speaker-Overlapped Setting:** partial speaker overlap between training and test sets while keeping the training size and test set fixed.

The repository includes code for generating the controlled data splits, training baseline and DANN-enhanced models, and evaluating both depression classification performance and speaker-identification accuracy.

Domain-Adversarial Neural Network (DANN) models are included as an adversarial stress test for reducing speaker-identity information. The paper does not claim that DANN fully solves speaker leakage; instead, the results show that speaker identity remains difficult to suppress and that strict speaker-independent evaluation is necessary for reliable clinical assessment.

## Dataset Access

This repository does **not** redistribute the DAIC-WOZ dataset, audio files, transcripts, or restricted metadata. Users must obtain DAIC-WOZ through the official data-access procedure and update local paths before running the scripts.

Because the code depends on local dataset organization, computing environment, random seed, and preprocessing details, exact numerical results may vary slightly across environments.

## Repository Structure

### 1. Data Preparation

- `experiment_generator.py`  
  Generates the size-matched controlled data splits used in the experiments, including the speaker-independent setting and the speaker-overlapped setting.

### 2. Wav2Vec Linear-Probing Baselines

- `C_run_linear_probing_A.py`  
  Extracts Wav2Vec2-based features and trains a linear classifier under the speaker-independent setting.

- `C_run_linear_probing_B.py`  
  Runs the corresponding linear-probing experiment under the speaker-overlapped setting.

### 3. DANN-Enhanced Models

- `run_dann_A_v2.py`  
  Applies the DANN-enhanced model under the speaker-independent setting.

- `run_dann_B_v2.py`  
  Applies the DANN-enhanced model under the speaker-overlapped setting.

### 4. Full Fine-Tuning Baselines

- `replicate_huang_scenario_A_v2.py`  
  Runs a full fine-tuning baseline under the speaker-independent setting.

- `replicate_huang_scenario_B_v2.py`  
  Runs a full fine-tuning baseline under the speaker-overlapped setting.

### 5. Speaker-Identity Evaluation

- `final_unify_probe.py`  
  Evaluates speaker-identification accuracy across feature spaces to quantify the extent to which speaker identity is retained.

## Quick Start

### 1. Install Dependencies

The main dependencies include:

```bash
pip install torch torchaudio transformers scikit-learn pandas numpy tqdm
```

Additional packages may be required depending on the specific experiment and feature-extraction setting.

### 2. Prepare the Dataset

Obtain DAIC-WOZ through the official access procedure. After downloading and preprocessing the dataset, update the local dataset paths in the scripts.

For example, replace local paths such as:

```python
AUDIO_ROOT = "/path/to/your/daic_woz_audio"
```

with the correct path on your machine.

### 3. Generate Data Splits

```bash
python experiment_generator.py
```

### 4. Run Baseline Experiments

Speaker-independent setting:

```bash
python C_run_linear_probing_A.py
```

Speaker-overlapped setting:

```bash
python C_run_linear_probing_B.py
```

### 5. Run DANN Experiments

Speaker-independent setting:

```bash
python run_dann_A_v2.py
```

Speaker-overlapped setting:

```bash
python run_dann_B_v2.py
```

### 6. Evaluate Speaker Identity Leakage

```bash
python final_unify_probe.py
```

## Notes

This repository is provided as research code to support transparency and reproducibility. It is not intended as a polished software package or clinical deployment system.

Please make sure that no restricted DAIC-WOZ data are committed to this repository. Only code and non-restricted configuration files should be shared publicly.

## Citation

If you use this code, please cite the corresponding paper:

```bibtex
@misc{yeh2026speakingdepressedcontrolledstudy,
      title={Who is Speaking or Who is Depressed? A Controlled Study of Speaker Leakage in Speech-Based Depression Detection}, 
      author={Hsiang-Chen Yeh and Luqi Sun and Aurosweta Mahapatra and Shreeram Suresh Chandra and Emily Mower Provost and Berrak Sisman},
      year={2026},
      eprint={2604.14354},
      archivePrefix={arXiv},
      primaryClass={eess.AS},
      url={https://arxiv.org/abs/2604.14354}, 
}
```
