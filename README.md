# Calcium Imaging

This analysis code accompanies the manuscript Wenninger et al. "Human Neuron and Mouse Models Reveal Synaptic Imbalance in Kabuki Syndrome" (submitted for publication).

It detects and quantifies spiking fluorescence transients for upward-reporting (GCaMP, iGluSnFR) or downward-reporting (iGABASnFR) sensors, correcting for photobleaching throughout the experiment.

## Pipeline for Replication
- Record cells for 2 min at a frame rate of 10 Hz using a 20x objective in Mono 16-bit with a region of interest of 1192x1192 pixels. Ensure all images use the same exposure time.
- Extract fluorescence traces per-neuron cell body in MATLAB using the analysis tools of [Sun & Südhof (2020)](https://pmc.ncbi.nlm.nih.gov/articles/PMC7853247/). The output of this analysis is a .xlsx workbook that serves as the input for our scripts.
- Run the notebook `upward_transients.ipynb` or `downward_transients.ipynb` depending on the reporting direction. The top lines allow the user to specify the name of the .xlsx file to be analyzed, the video duration in sections (120 for our experiments), and whether to generate a plot showing the photobleaching-corrected traces after the analysis.

## System Requirements and Installation
Python 3 is required. The software has been tested in Python 3.13 with package versions provided in `requirements.txt`.

To use the software (est. 2 minutes):
- Clone the repository: `git clone https://github.com/cmackenzie29/calcium-imaging.git`
- Create a new environment with the required packages: `pip install -r requirements.txt`
- Launch Jupyter: `jupyter lab`

## Demo (est. 1 minute)
- Copy .xlsx files into `data` folder. A `sample.xlsx` file is provided.
- Open `upward_transients.ipynb` in Jupyter.
- Change the three user-defined parameters if necessary. Run the cell.
- Expected output: Below the cell, output values are printed to the console and a plot displays the normalized traces. If `trace_outputs = True`, the normalized trace values are also written to `sample_normalized.xlsx` in the `data` folder.

## Methodology
*(For upward-reporting traces)*
- The baseline trace intensity is corrected for photobleaching by subtracting the 3 second rolling minimum, keeping the absolute minimum of the corrected trace the same as that of the original trace.
- The signal is then normalized by computing the relative change $\frac{F_i - F_0}{F_0}$, where $F_0$ is the absolute minimum signal.
- Spikes are detected using a dual-theshold state algorithm. A spike is triggered by the normalized transient crossing above the level of the mean plus one standard deviation, and a new spike cannot be triggered until the normalized transient crosses below the mean.
- The spike amplitude is recorded as the maximum normalized signal within the detected spiking event.

The script returns the spike frequency and mean spike amplitude for each individual neuron trace, as well as for the trace that results from averaging the unnormalized signals across all neurons (synchronous spiking). It also computes the mean Fisher-transformed Pearson correlations between the unnormalized signals across all active neurons, as a measure of synchronicity.

## Citation
Wenninger, A., Knopp, J., McAdams, N., et al. (2026). Human Neuron and Mouse Models Reveal Synaptic Imbalance
in Kabuki Syndrome. Submitted for publication.

**bibtex**
```bibtex
@article{wenninger2026,
  title		= {Human Neuron and Mouse Models Reveal Synaptic Imbalance in Kabuki Syndrome},
  author	= {Anna Wenninger and James Knopp and Neil McAdams and Toshihiro Nomura and Joaqu{\'i}n Campos and Steven Frye and Alexander Weaver and Sean Miller and Ava Whitlark and Jack Swantkowski and Alexandra Noble and Caroline Lavender and Sandy Nam and Cameron MacKenzie and Isabella Wiebelt-Smith and Charles Sander and {\'A}ine Fitzpatrick and Jessica Stewart and Katharyn Hutson and Jenny Bergqvist-Patzke and Sheri Sanders and Kasturi Haldar and Claudio Acuna and Anis Contractor and Christopher Patzke},
  journal	= {Submitted for publication},
  year		= {2026}
}
```
