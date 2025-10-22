# Gravitational Lensing Regimes: Suvan's Parameters

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![Astropy](https://img.shields.io/badge/Astropy-6.0%2B-yellow)](https://www.astropy.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](https://opensource.org/licenses/MIT)

## Overview

This repository implements a Monte Carlo (MC) simulation framework to derive mass-based thresholds ("Suvan's Parameters") for gravitational lensing regimes in Galactic geometry. Inspired by the Chandrasekhar limit, we classify lenses into microlensing ($M < 0.99 \, M_\odot$), weak/astrometric lensing ($0.99 \, M_\odot \leq M \leq 9.9 \times 10^5 \, M_\odot$), and strong lensing ($M > 9.9 \times 10^5 \, M_\odot$) based on the angular Einstein radius $\theta_E$.

Key features:
- **Thin-lens equation** implementation with astropy units for SI consistency.
- **1000 MC iterations** sampling distances ($D_L \sim \mathcal{N}(4,1)$ kpc, $D_S \approx 8$ kpc).
- **Verification** against 16 observed microlensing events from OGLE and MOA surveys.
- **Visualization** of $\theta_E$ vs. mass with regime shading and error bands.
- Accompanying LaTeX paper: [Suvan's Parameters](paper/suvans_parameters.tex) (compiles to PDF via `pdflatex`).

The analysis is detailed in the paper and supported by Jupyter notebooks for reproducibility.

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/gravitational-lensing-suvans-parameters.git
   cd gravitational-lensing-suvans-parameters
   ```

2. Create a virtual environment (recommended):
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

### Requirements
See [requirements.txt](requirements.txt) for full list. Core dependencies:
- `numpy>=1.24`
- `scipy>=1.10`
- `matplotlib>=3.7`
- `astropy>=6.0`
- `pandas>=2.0`

No additional setup required for data files (included in `data/`).

## Usage

### Running the Main Script
Execute the primary analysis script:
```bash
python src/lensing_analysis.py
```
- Outputs: Console prints (boundaries, summaries), plots (`figures/lensing_regimes.png`).
- Flags: `--no-obs` to skip observed data overlay; `--seed 42` for reproducibility.

### Jupyter Notebooks
For interactive exploration:
```bash
jupyter notebook notebooks/
```
- `lensing_mc_simulation.ipynb`: Full MC pipeline with plots.
- `verification_notebook.ipynb`: Loads CSVs and overlays observations.

### Key Outputs
- **Mass Boundaries** (printed):
  ```
  Strong lensing (θ_E > 1 arcsec): M > 1e+06 M_sun
  Weak lensing (1 mas < θ_E < 1 arcsec): 9.9e-01 < M < 1e+06 M_sun
  Micro lensing (θ_E < 1 mas): M < 9.9e-01 M_sun
  ```
- **Plots**: Log-log $\theta_E$ vs. mass with shaded regimes and observed points.
- **Data**: Enhanced CSV with residuals (if predictions enabled).

### Data Files
- `data/observed_microlensing_masses.csv`: 16 events (masses, $\theta_E$, distances).
- `data/observed_vs_predicted_microlensing.csv`: Pre-computed predictions.
- `data/verified_microlensing_data.csv`: Output with residuals (generated on run).

## Examples

### Quick Plot Generation
```python
from src.lensing import compute_theta_E_gal, run_mc_simulation

# Single computation
theta_E = compute_theta_E_gal(M=1 * u.Msun, D_l=4, D_s=8, D_ls=4)  # ~0.5 mas

# Run MC
log_m, median_theta, p16, p84 = run_mc_simulation(n_mc=1000)
plt.plot(log_m, np.log10(median_theta))
plt.show()
```

See `examples/quick_demo.py` for full script.

## Structure
```
gravitational-lensing-suvans-parameters/
├── src/                  # Core Python modules
│   └── lensing.py        # θ_E computation and MC
├── notebooks/            # Interactive Jupyter notebooks
├── data/                 # CSV datasets (observed events)
├── figures/              # Generated plots
├── requirements.txt      # Dependencies
├── README.md             # This file
└── LICENSE               # MIT License
```

## Contributing
1. Fork the repo and create a feature branch (`git checkout -b feat/amazing-feature`).
2. Commit changes (`git commit -m 'Add some AmazingFeature'`).
3. Push to branch (`git push origin feat/amazing-feature`).
4. Open a Pull Request.

Report issues via GitHub Issues. Suggestions for extragalactic extensions or Bayesian inference welcome!

## License
MIT License—see [LICENSE](LICENSE) for details.

## Acknowledgments
- OGLE and MOA survey teams for public data.
- Astropy collaboration for units and constants.
- References: See paper bibliography (e.g., Sahu et al. 2022, Mróz et al. 2019).

## References
- Full citations in `paper/suvans_parameters.tex`.
- Data sources: [OGLE](https://ogle.astrouw.edu.pl/), [MOA](https://www.massey.ac.nz/~iabond/moa/).

---

*Last updated: October 22, 2025*  
Questions? Open an issue or contact [your.email@example.com](mailto:your.email@example.com).
