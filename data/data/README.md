# Dataset

This repository does not redistribute the CSE-CIC-IDS2018 dataset.

The experiments use the following six daily domains:

* 02-14-2018
* 02-15-2018
* 02-16-2018
* 02-22-2018
* 02-28-2018
* 03-01-2018

## Official Dataset Source

Canadian Institute for Cybersecurity (CIC), University of New Brunswick (UNB):

https://www.unb.ca/cic/datasets/ids-2018.html

Users should obtain the dataset from the official source and comply with its stated terms and citation requirements.

## Data Used in the Study

The experiments reported in the associated manuscript use six daily domains from CSE-CIC-IDS2018:

1. 02-14-2018
2. 02-15-2018
3. 02-16-2018
4. 02-22-2018
5. 02-28-2018
6. 03-01-2018

Each day is treated as an independent source-day domain for the source-domain training experiments.

## Data Placement

The raw CSE-CIC-IDS2018 dataset is not included in this repository.

Users should download the required daily data from the official UNB/CIC source and provide the files as input to the reproduction workflow.

For local execution, place the downloaded CSV files in the input-data directory specified by the reproduction notebook or experiment configuration.

For Kaggle execution, attach the required CSE-CIC-IDS2018 dataset as an input dataset and update the input path in the notebook if necessary.

## Data Processing

The reproduction workflow performs the following main processing steps:

1. Loads the daily network-flow CSV files.
2. Identifies the timestamp and label fields.
3. Converts applicable feature columns to numeric values.
4. Handles missing and infinite values.
5. Performs feature preprocessing.
6. Applies source-domain scaling using the training data.
7. Constructs temporal sequences containing 32 events.
8. Uses the resulting sequences for Transformer-based classification and cross-day evaluation.

The detailed implementation and experimental configuration are provided in the repository's reproduction materials.

## Dataset Citation

The CSE-CIC-IDS2018 dataset should be cited according to the requirements specified by the Canadian Institute for Cybersecurity / University of New Brunswick.

Official dataset page:

https://www.unb.ca/cic/datasets/ids-2018.html
