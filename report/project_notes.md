# Project notes

## Objective

This project explores whether a small parameterised quantum circuit can be integrated with a pretrained convolutional feature extractor for chest X-ray classification. It is an experimental implementation and ablation study, not a clinical model or a demonstration of quantum advantage.

## Model family

All variants use a pretrained ResNet18 with its final fully connected layer replaced by an identity mapping. The resulting 512 features feed a classical branch and, for hybrid models, a quantum branch implemented with Qiskit Machine Learning.

The four-qubit circuit uses a two-repetition `ZZFeatureMap`, a two-repetition `RealAmplitudes` ansatz, and Pauli-Z expectation values. `EstimatorQNN` exposes the circuit to PyTorch through `TorchConnector`.

## Rationale for 32 classical + 4 quantum features

The 32+4 design was introduced to address two weaknesses of the earlier 512+1 model: the single QNN output had little numerical presence beside 512 classical features, and it did not expose all four measured qubit expectations. Producing 32 classical and 4 quantum outputs also gives the final classifier the same 36-dimensional input as a classical control.

This is a useful ablation choice, but equal output dimensionality does not prove that a performance difference comes from quantum correlations. The models still differ in parameterisation, nonlinear operations, optimisation dynamics, stochastic training, and checkpoint selection.

## Evaluation limitations

- The original training functions pass `test_loader` into the epoch loop and select the best checkpoint using test accuracy.
- The original train, validation, and test directories are merged, balanced, and randomly split 80/20 before training.
- Package versions, seeds, and patient-level identifiers were not recorded consistently.
- Several notebooks reuse generic checkpoint filenames; this can overwrite results between variants.
- The 32+4 3+15 notebook has incomplete stored final metrics, and some report tables disagree with notebook outputs.
- Experiments use statevector simulation rather than quantum hardware.

Consequently, all repository metrics are labelled historical development results. A fair comparison requires patient-disjoint train/validation/test partitions, validation-selected checkpoints, a single final test evaluation, fixed environments, and repeated seeds.

## Report source

The supplied Word reports were used to reconstruct the architecture and experimental rationale. They are not included in the public package because they contain placeholders, inconsistent tables, and stronger causal language than the code supports. The repository README and these notes provide the cleaned, code-grounded account.
