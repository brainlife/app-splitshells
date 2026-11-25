[![Abcdspec-compliant](https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg)](https://github.com/brain-life/abcd-spec)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.1-blue.svg)](https://doi.org/10.25663/bl.app.17)

# app-splitshells

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

**Extracts a single b-value shell and the b0 volumes from multi-shell DWI, rounding b-values to a chosen step, using MATLAB and vistasoft.**

Rounds b-values to the nearest bvals_round, treats b-values at or below b0_max as b0, and keeps the volumes, bvecs and bvals whose rounded b-value equals the chosen shell or 0. Use it before methods that need single-shell data (such as some tensor or CSD fits).

Run it on brainlife.io: [https://doi.org/10.25663/bl.app.17](https://doi.org/10.25663/bl.app.17)

| Input | Datatype | Required | Description |
|---|---|---|---|
| `dwi` | `neuro/dwi` (!single_shell) | yes | Multi-shell dwi data. Must contain volumes at the requested shell b-value after rounding. |

| Output | Datatype | Description |
|---|---|---|
| `output` | `neuro/dwi` | DWI with only the chosen shell and the b0 volumes, with matching bvecs and rounded bvals. |

| Parameter | Type | Default | Description |
|---|---|---|---|
| `bvals_round` | number | `100` | bvals will be rounded up to nearest bvals with this value. For example, 100 means 2401-2499 will be rounded to 2500. |
| `shell` | integer | `1000` | bval corresponding to the shell you want to extract. |
| `b0_max` | number | `200` | Any bvals equal or less than this value will be considered b0. |

Runtime: container `brainlife/mcr:neurodebian1604-r2017a`; 1 CPUs; 29 GB memory; 0.33 h walltime.

Please cite:
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. Sci Data 6, 69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

<!-- brainlife-app:end -->

dwi images contsists of several diffusion weight images (volumes) with different gradient directions (bvecs) and strenghth (bvals). Some processing can only run on specific bvals (aka "single shell"). This service will look for volumes with specific bvals and pull those images to create a "single shell" image. bvecs are often similar but not exactly the same. This App also allows you to specify range of bvals to be rounded to the nearest 100s (like 1980 to 2000, for example).

### Authors
- Lindsey Kitchell (kitchell@indiana.edu)
- Soichi Hayashi (hayashis@iu.edu)

### Project directors
- Franco Pestilli (franpest@indiana.edu)

### Funding 
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)

## Running the App 

### On Brainlife.io

You can submit this App online at [https://doi.org/10.25663/bl.app.17](https://doi.org/10.25663/bl.app.17) via the "Execute" tab.

### Running Locally (on your machine)

1. git clone this repo.
2. Inside the cloned directory, create `config.json` with something like the following content with paths to your input files.

```json
{
        "shell": 2500,
        "b0_max": 200,
        "bvals_round": 100,
        "bvals": "input/dwi/dwi.bvals",
        "bvecs": "input/dwi/dwi.bvecs",
        "dwi": "input/dwi/dwi.nii.gz"
}
```

3. Launch the App by executing `main`

```bash
./main
```

### Sample Datasets

If you don't have your own input file, you can download sample datasets from Brainlife.io, or you can use [Brainlife CLI](https://github.com/brain-life/cli).

```
npm install -g brainlife
bl login
mkdir input
bl dataset download 5a050a00eec2b300611abff3 && mv 5a050a00eec2b300611abff3 input/dwi
```

## Output

This App will generate `dwi.nii.gz`, `dwi.bvecs`,  anb `dwi.bvals` on the current working directory. 

### Dependencies

This App only requires [singularity](https://www.sylabs.io/singularity/) to run. If you don't have singularity, you will need to install following dependencies.  

  - Matlab: https://www.mathworks.com/products/matlab.html
  - VISTASOFT: https://github.com/vistalab/vistasoft/
  - jsonlab: https://www.mathworks.com/matlabcentral/fileexchange/33381-jsonlab-a-toolbox-to-encode-decode-json-files?w.mathworks.com
  
  
