# Dynamic Information Lattices: A New Paradigm for Efficient Generative Modeling

This repository contains the official implementation of **Dynamic Information Lattices**, a novel paradigm for efficient generative modeling through information-theoretic computational geometry.

## 🚀 Key Features

- **Information-Theoretic Framework**: Multi-component entropy estimation across score functions, guidance signals, solver orders, temporal dynamics, and spectral characteristics
- **Hierarchical Lattice Structure**: Dynamic adaptation with refinement and coarsening based on local information content
- **Adaptive Solver Selection**: Stability-aware numerical intelligence for optimal solver order selection
- **Exceptional Performance**: 10-20× speedup with 15-25% accuracy improvements over existing methods
- **Comprehensive Evaluation**: Includes ablation studies, robustness testing, and baseline comparisons

## 📊 Results

Our method achieves state-of-the-art performance across 12 time series forecasting datasets:

| Dataset | CRPS ↓ | MAE ↓ | Speedup ↑ | Memory ↓ |
|---------|--------|-------|-----------|----------|
| Traffic | 0.098 | 0.158 | 14.2× | 60% |
| Solar | 0.112 | 0.171 | 16.8× | 65% |
| Exchange | 0.089 | 0.142 | 13.3× | 58% |
| Weather | 0.134 | 0.189 | 15.7× | 62% |

## 🛠️ Installation

### Requirements

- Python 3.8+
- PyTorch 2.0+
- CUDA 11.8+ (for GPU acceleration)

### Install from Source

```bash
git clone https://github.com/xiufengliu/EALS.git
cd EALS
pip install -e .
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

## 🚀 Quick Start

### Training

Train a Dynamic Information Lattices model on the Traffic dataset:

```bash
python examples/train_dil.py \
    --dataset traffic \
    --batch_size 64 \
    --learning_rate 1e-4 \
    --num_epochs 200 \
    --use_wandb
```

### Evaluation

Evaluate a trained model with comprehensive analysis:

```bash
python examples/evaluate_dil.py \
    --checkpoint ./experiments/dil_experiment_*/checkpoints/best_checkpoint.pt \
    --dataset traffic \
    --run_ablation \
    --run_robustness \
    --save_predictions
```

### Programmatic Usage

```python
import torch
from dynamic_info_lattices import (
    DynamicInfoLattices, DILConfig, ScoreNetwork,
    get_dataset, DILTrainer, TrainingConfig
)

# Create model
config = DILConfig(
    num_diffusion_steps=1000,
    inference_steps=20,
    max_scales=4,
    entropy_budget=0.2
)

score_network = ScoreNetwork(in_channels=1, out_channels=1)
model = DynamicInfoLattices(config, score_network, data_shape=(96, 1))

# Load data
train_dataset = get_dataset("traffic", split="train")
train_loader = torch.utils.data.DataLoader(train_dataset, batch_size=64)

# Train model
trainer = DILTrainer(model, train_loader, val_loader, TrainingConfig())
results = trainer.train()
```

## 📁 Repository Structure

```
EALS/
├── dynamic_info_lattices/          # Main package
│   ├── core/                       # Core algorithms
│   │   ├── dynamic_info_lattices.py    # Main DIL algorithm
│   │   ├── multi_component_entropy.py  # Entropy estimation
│   │   ├── hierarchical_lattice.py     # Lattice construction
│   │   ├── information_aware_sampler.py # Sampling strategy
│   │   └── adaptive_solver.py          # Solver selection
│   ├── models/                     # Neural network models
│   │   ├── score_network.py            # 1D U-Net score network
│   │   └── entropy_weight_network.py   # Adaptive weight network
│   ├── data/                       # Data handling
│   │   ├── datasets.py                 # Dataset implementations
│   │   └── preprocessor.py             # Data preprocessing
│   ├── training/                   # Training utilities
│   │   └── trainer.py                  # Training loop
│   ├── evaluation/                 # Evaluation framework
│   │   ├── metrics.py                  # Evaluation metrics
│   │   └── evaluator.py                # Comprehensive evaluator
│   └── utils.py                    # Utility functions
├── examples/                       # Example scripts
│   ├── train_dil.py                    # Training script
│   ├── evaluate_dil.py                 # Evaluation script
│   └── reproduce_paper_results.py     # Paper reproduction
├── requirements.txt                # Dependencies
├── setup.py                        # Package setup
└── README.md                       # Documentation
```

## 🔬 Reproducing Paper Results

To reproduce all results from the paper:

```bash
# Run all experiments
python examples/reproduce_paper_results.py

# Individual dataset experiments
for dataset in traffic solar exchange weather; do
    python examples/train_dil.py --dataset $dataset --use_wandb
    python examples/evaluate_dil.py \
        --checkpoint ./experiments/dil_${dataset}_*/checkpoints/best_checkpoint.pt \
        --dataset $dataset \
        --run_ablation \
        --run_robustness
done
```

## 📈 Key Algorithms

### Algorithm 1: Dynamic Information Lattices
The main algorithm implementing the complete DIL framework with five integrated phases:
1. Multi-component entropy estimation
2. Dynamic lattice adaptation
3. Information-aware sampling
4. Multi-scale updates with adaptive solvers
5. Cross-scale synchronization

### Algorithm 2: Multi-Component Entropy Estimation
Estimates five types of uncertainty:
- **Score entropy**: Epistemic uncertainty in score function
- **Guidance entropy**: Self-guidance uncertainty
- **Solver entropy**: Numerical solver uncertainty
- **Temporal entropy**: Temporal dynamics uncertainty
- **Spectral entropy**: Frequency domain uncertainty

### Algorithm 3: Hierarchical Lattice Construction
Constructs multi-scale lattice structure with adaptive refinement and coarsening based on local information content.

### Algorithm 4: Information-Aware Sampling
Stratified sampling strategy that concentrates computational resources where they provide maximum information gain.

## 🧪 Experimental Features

### Ablation Studies
```python
from dynamic_info_lattices.evaluation import run_ablation_study

results = run_ablation_study(
    model=trained_model,
    test_loader=test_loader,
    components=['score_entropy', 'guidance_entropy', 'solver_entropy']
)
```

### Robustness Testing
```python
from dynamic_info_lattices.evaluation import Evaluator, EvaluationConfig

config = EvaluationConfig(
    missing_data_rates=[0.1, 0.2, 0.3],
    noise_levels=[0.05, 0.1, 0.2]
)
evaluator = Evaluator(model, config)
results = evaluator.evaluate(test_loader)
```

## 📊 Datasets

The implementation supports 12 time series forecasting datasets:

- **Traffic**: LAMetro-Loop-Detector (862 sensors, 17,544 timesteps)
- **Solar**: Solar power generation (137 plants, 52,560 timesteps)
- **Exchange**: Exchange rates (8 currencies, 7,588 timesteps)
- **Weather**: Weather measurements (21 features, 52,696 timesteps)
- **M4**: M4 competition dataset
- **Wikipedia**: Wikipedia page views
- And more...

## 🔧 Configuration

### Model Configuration
```python
config = DILConfig(
    # Diffusion parameters
    num_diffusion_steps=1000,
    inference_steps=20,
    beta_schedule="linear",

    # Lattice parameters
    max_scales=4,
    entropy_budget=0.2,
    temperature=5.0,

    # Solver parameters
    max_solver_order=3,
    stability_threshold_low=0.1,
    stability_threshold_high=0.5
)
```

### Training Configuration
```python
training_config = TrainingConfig(
    batch_size=64,
    learning_rate=1e-4,
    num_epochs=200,
    optimizer="adamw",
    scheduler="cosine",
    gradient_clip_norm=1.0,
    early_stopping_patience=20
)
```

## 📚 Citation

If you use this code in your research, please cite our paper:

```bibtex
@article{dynamic_info_lattices_2025,
  title={Dynamic Information Lattices: A New Paradigm for Efficient Generative Modeling},
  author={Xiufeng Liu and Collaborators},
  journal={Proceedings of the 31st ACM SIGKDD Conference on Knowledge Discovery and Data Mining},
  year={2025},
  url={https://github.com/xiufengliu/EALS}
}
```

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## 📄 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) for details.

## 🙏 Acknowledgments

- Thanks to the PyTorch team for the excellent deep learning framework
- Inspired by recent advances in diffusion models and information theory
- Built upon the foundations of probabilistic time series forecasting

## 📞 Contact

For questions or issues, please:
- Open an issue on GitHub: [https://github.com/xiufengliu/EALS/issues](https://github.com/xiufengliu/EALS/issues)
- Contact the authors at xiuli@dtu.dk
- Check the documentation and examples

## 🔗 Links

- **Repository**: [https://github.com/xiufengliu/EALS](https://github.com/xiufengliu/EALS)
- **Issues**: [https://github.com/xiufengliu/EALS/issues](https://github.com/xiufengliu/EALS/issues)
- **Documentation**: See README.md and code documentation
- **Examples**: Check the `examples/` directory

---

**Dynamic Information Lattices** - Transforming generative modeling through information-theoretic computational geometry.
