# Improving Single Noise Level Diffusion Samplers with Restricted Gaussian Oracles

ICLR 2025 Workshop DeLTa submission

----------------------

## Abstract

Practical generative modeling pipelines and diffusion Monte-Carlo schemes, which adapt diffusion models for sampling
from unnormalized log-densities, both rely on denoisers (or score estimates) at different noise scales. This complicates
the sampling process as denoising schedules require careful tuning and nested inner-MCMC loops. In this work, we propose
a single noise level sampling procedure that only requires a single low-noise denoiser. Our framework results from
improvements we bring to the multimeasurement Walk-Jump sampler of Saremi et al. 2021 by mixing in ideas from the
proximal sampler of Shen et al. 2020. Our analysis shows that annealing (or multiple noise scales) is unnecessary if one
is willing to pay an increased memory cost. We demonstrate this by proposing an *entirely log-concave* sampling
framework.

## Instructions

### Obtaining the code

Clone this repository by running:

`git clone https://github.com/LIONS-EPFL/MultiProxDiffusion.git`

### Dependencies

This repository extends the implementation of "DiGress: Discrete Denoising diffusion models for graph generation",
originally available at https://github.com/cvignac/DiGress.

To install the Python programming language and the dependent Python packages, you need to execute the following steps:

- Download anaconda/miniconda if needed

- Create a rdkit environment that directly contains rdkit:

  ```conda create -c conda-forge -n digress rdkit=2023.03.2 python=3.9```

- `conda activate digress`

- Check that this line does not return an error:

  ``` python3 -c 'from rdkit import Chem' ```

- Install graph-tool (https://graph-tool.skewed.de/):

  ```conda install -c conda-forge graph-tool=2.45```

- Check that this line does not return an error:

  ```python3 -c 'import graph_tool as gt' ```

- Install the nvcc drivers for your cuda version. For example:

  ```conda install -c "nvidia/label/cuda-11.8.0" cuda```

- Install a corresponding version of pytorch, for example:

  ```pip3 install torch==2.0.1 --index-url https://download.pytorch.org/whl/cu118```

- Install other packages using the requirement file:

  ```pip install -r requirements.txt```

- Run:

  ```pip install -e .```

- Navigate to the `src/analysis/orca` directory and compile `orca.cpp`:

  ```g++ -O2 -std=c++11 -o orca orca.cpp```

### Reproducing results

To generate molecules with our Multi-prox sampler and obtain example chains like the one displayed in our work, run the
following command from inside the `src` directory:

```CUDA_VISIBLE_DEVICES=<GPU_ID> python main.py dataset="qm9" general.name="c_gibbs_startNone_N10_M100_t0.5_0.1" model="continuous" general.test_only="<REPO_ABSPATH>/checkpoints/qm9_c.ckpt" +model.gibbs=True +model.gibbs_N=10 +model.gibbs_M=100 +model.gibbs_fixed_t_2=0.5 +model.gibbs_fixed_t_1=0.1 +model.gibbs_chain_freq=0.1 ++general.final_model_samples_to_generate=1000```

where you would replace `<GPU_ID>` with the integer ID of one of your available GPU devices and replace `<REPO_ABSPATH>`
with the absolute path of the main repo directory (where you cloned it).

The batch of full molecule chains will be saved to
`<REPO_ABSPATH>/chains/c_gibbs_startNone_N10_M100_t0.5_0.1_resume/epoch0/chains/`, while the batch of just the final
molecules will be saved to `<REPO_ABSPATH>/graphs/c_gibbs_startNone_N10_M100_t0.5_0.1_resume/epoch0_b0/`.

----------

## Authors

- Leello Tadesse Dadi, EPFL STI IEM LIONS, Lausanne, Switzerland, leello.dadi@epfl.ch
- Andrej Janchevski, EPFL STI IEM LIONS, Lausanne, Switzerland, andrej.janchevski@epfl.ch
- Volkan Cevher, EPFL STI IEM LIONS, Lausanne, Switzerland, volkan.cevher@epfl.ch
