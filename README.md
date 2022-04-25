# Molecular Dynamics web scrapper

## Setup your environment

Clone the repository:

```bash
git clone https://github.com/MDverse/mdws.git
```

Move to the new directory:

```bash
cd mdws
```

Install [miniconda](https://docs.conda.io/en/latest/miniconda.html).

Install [mamba](https://github.com/mamba-org/mamba):

```bash
conda install mamba -n base -c conda-forge
```

Create the `mdws` conda environment:
```
mamba env create -f binder/environment.yml
```

Load the `mdws` conda environment:
```
conda activate mdws
```

Note: you can also update the conda environment with:

```bash
mamba env update -f binder/environment.yml
```

To deactivate an active environment, use

```
conda deactivate
```

## Add API keys

### Zenodo

Create a token here: <https://zenodo.org/account/settings/applications/tokens/new/>  
and store it in the file `.env`:
```
ZENODO_TOKEN=YOUR-ZENODO-TOKEN
```
This file is ignored by git.

### FigShare

No token or API key needed.


## Scrap Zenodo

Scrap Zenodo for MD-related datasets and files:

```bash
python scrap_zenodo.py query.yml
```

Scrap Zenodo with a small query, for development or demo purpose:

```bash
python scrap_zenodo.py query_dev.yml
```

The scraping takes some time. A mechanism has been set up to avoid overloading the Zenodo API. Be patient.

Eventually, the scraper will produce two files: `zenodo_datasets.tsv` and `zenodo_files.tsv` :sparkles: 


## Scrap FigShare

Scrap FigShare for MD-related datasets and files:

```bash
python scrap_figshare.py query.yml
```

Scrap FigShare with a small query, for development or demo purpose:

```bash
python scrap_figshare.py query_dev.yml
```

The scraping takes some time (complete query: 20 min-120 min). Be patient.

Eventually, the scraper will produce two files: `figshare_datasets.tsv` and `figshare_files.tsv` :sparkles: 


## Analyse data

Run all Jupyter notebooks in batch mode:
```
jupyter nbconvert --to html  --execute --allow-errors --output-dir results notebooks/analyze_zenodo.ipynb
cp notebooks/*.{csv,png} results/
```


## Download files

To download Gromacs mdp and gro files from Zenodo, one can use the command line:

```bash
python scripts/download_files.py -i data/zenodo_files.tsv -o data/downloads/ -t mdp -t gro
```

This step will take a couple of hours to complete. Depending on the stability of your internet connection and the availability of the data repository servers, the download might fail with an error messages similar to

> requests.exceptions.HTTPError: 429 Client Error: TOO MANY REQUESTS

or 

> requests.exceptions.ConnectionError: HTTPSConnectionPool(host='zenodo.org', port=443)

Re-rerun the previous command to resume the download. Files already retrieved will not be downloaded again.

