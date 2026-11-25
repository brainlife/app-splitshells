<h1 align="center">Split Shells</h1>

<p align="center">
  A single-shell DWI (one b-value shell plus the b0 volumes, with matching bvecs and bvals) extracted from multi-shell diffusion data.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/bl.app.17"><img src="https://img.shields.io/badge/Brainlife-bl.app.17-blue.svg" alt="Run on brainlife.io"></a>
  <a href="https://github.com/brainlife/app-splitshells/blob/master/LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey.svg" alt="MIT license"></a>
</p>

> **This branch, `1.2`, is the brainlife.io App Split Shells** ([doi:10.25663/bl.app.17](https://doi.org/10.25663/bl.app.17)).

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

> **Extracts a single b-value shell and the b0 volumes from multi-shell DWI, rounding b-values to a chosen step, using MATLAB and vistasoft.**

Rounds b-values to the nearest bvals_round, treats b-values at or below b0_max as b0, and keeps the volumes, bvecs and bvals whose rounded b-value equals the chosen shell or 0. Use it before methods that need single-shell data (such as some tensor or CSD fits).

| | |
|---|---|
| **Inputs** | `neuro/dwi` |
| **Outputs** | `neuro/dwi` |
| **Container** | `brainlife/mcr:neurodebian1604-r2017a` |
| **Resources** | 1 CPUs · 29 GB memory · 0.33 h walltime |
| **Run it** | [doi.org/10.25663/bl.app.17](https://doi.org/10.25663/bl.app.17) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `dwi` | `neuro/dwi` (!single_shell) | yes | Multi-shell dwi data. Must contain volumes at the requested shell b-value after rounding. |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `output` | `neuro/dwi` | DWI with only the chosen shell and the b0 volumes, with matching bvecs and rounded bvals. |

<details>
<summary><b>Parameters</b> (3)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `bvals_round` | number | `100` | bvals will be rounded up to nearest bvals with this value. For example, 100 means 2401-2499 will be rounded to 2500. |
| `shell` | integer | `1000` | bval corresponding to the shell you want to extract. |
| `b0_max` | number | `200` | Any bvals equal or less than this value will be considered b0. |

</details>

<!-- brainlife-app:end -->

## Running the App

### On brainlife.io

Open the App at [doi.org/10.25663/bl.app.17](https://doi.org/10.25663/bl.app.17), choose a multi-shell `neuro/dwi` dataset, set the parameters and press **Execute**. The single-shell DWI is archived as a new `neuro/dwi` dataset tagged `single_shell`. Use it before methods that need single-shell data, such as some tensor or CSD fits.

A DWI is made of many volumes with different gradient directions (bvecs) and strengths (bvals), and the bvals of one shell are often similar but not identical. The App therefore rounds the bvals before selecting volumes:

- `bvals_round`: round each b-value to the nearest multiple of this value (default 100; for example, 1980 becomes 2000).
- `b0_max`: treat b-values at or below this value as b0 (default 200).
- `shell`: the rounded b-value of the shell to keep (default 1000).

### Locally

You need [Singularity/Apptainer](https://apptainer.org/). The App runs compiled MATLAB code (vistasoft and jsonlab) in the `brainlife/mcr:neurodebian1604-r2017a` container.

1. Clone this repository and check out the App branch: `git checkout 1.2`.
2. Create `config.json` with the paths to your inputs and the parameters (see `config.json.sample`):

   ```json
   {
       "shell": 2000,
       "b0_max": 200,
       "bvals_round": 100,
       "bvals": "input/dwi/dwi.bvals",
       "bvecs": "input/dwi/dwi.bvecs",
       "dwi": "input/dwi/dwi.nii.gz"
   }
   ```

3. Run the App:

   ```bash
   ./main
   ```

### Sample data

Download a sample DWI from brainlife.io with the [brainlife CLI](https://github.com/brainlife/cli):

```bash
npm install -g brainlife
bl login
mkdir input
bl dataset download 5a050a00eec2b300611abff3 && mv 5a050a00eec2b300611abff3 input/dwi
```

## Outputs

Written to the working directory:

```text
dwi.nii.gz   volumes of the chosen shell and the b0 volumes
dwi.bvecs    matching gradient directions
dwi.bvals    matching b-values, rounded
```

## Troubleshooting

- **`output missing`**: `dwi.nii.gz` was not written, for example because no volumes matched the shell. Check `shell` against the rounded bvals of the input.

## Citing

> [!TIP]
> If you use this App in your work, please cite the App, the software it builds on and brainlife.io (below). GitHub's **Cite this repository** button gives the App citation in APA and BibTeX.

- Split Shells. brainlife.io App. [doi:10.25663/bl.app.17](https://doi.org/10.25663/bl.app.17)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. Sci Data 6, 69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)

## Authors

- Lindsey Kitchell
- Soichi Hayashi
- Franco Pestilli (project director)

**Contributors:** Giulia Bertò

## Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io is publicly funded, and acknowledging that support helps keep it running. This App was developed with support from Wellcome, the U.S. National Institutes of Health (including the BRAIN Initiative BRAIN CONNECTS program: The Axonal Projectome EXchange, U24NS140384, and the Center for Mesoscale Connectomics, UM1NS132207) and the U.S. National Science Foundation.</sub>

## License

MIT.
