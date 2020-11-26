# stabletune
## Environment

DockerFile: `dancingsoul/pytorch:1.6.0-cuda10.1-cudnn7-devel-apex`

Install the fine-tuning code: `pip install ./`

## Data & Model Preparation

### XTREME Datasets  

To download XTREME data, first create a download folder with mkdir -p download in the root of this project. 
You then need to manually download `panx_dataset` (for NER) [here][2], (note that it will download as `AmazonPhotos.zip`) to the download directory.
Finally, run the following command to download the remaining datasets:

`bash scripts/download_data.sh`

The code of downloading dataset from XTREME is from [xtreme offical repo][1], 
note we keep the labels in test set for easier evaluation.

To prevent accidental evaluation on the test sets while running experiments, use code in from [xtreme offical repo][1], which removes labels of the test data during pre-processing and changes the order of the test sentences for cross-lingual sentence retrieval. 
Replace `csv.writer(fout, delimiter='\t')` with `csv.writer(fout, delimiter='\t', quoting=csv.QUOTE_NONE, quotechar='')` in utils_process.py if using XTREME official repo.
### Translations

XTREME provides translations for SQuAD v1.1 (only train and dev), MLQA, PAWS-X, TyDiQA-GoldP, XNLI, and XQuAD,
which can be downloaded from [here][3]. Manully move the `xtreme_translations` folder to the root of this project. 

The target language translations for panx and udpos are obtained with Google Translate, since they are not provided.
Our processed version can be downloaded from [here][4]. Manully merge it with the previous `xtreme_translations` folder.

### Bi-lingual dictionaries

We obtain the bi-lingual dictionaries from [MUSE][6] repo.
For convenience, download them from [here][7] and move it to the download directory, i.e. ./download/dicts.

### Models
Currently only XLM-Roberta is supported. We utilize the [huggingface][5] version, which can be downloaded with `bash download_model.sh`.



## Fine-tuning Command

Our experiments is conducted on 32GB V100. 
Reduce `per_gpu_train_batch_size` while increase `gradient_accumulation_steps` or use multi-gpu if needed.

The complete stabletune is consisted of a two-stage training process.

### Translate-Train-All Setting

`bash ./scripts/train.sh translate-train-all [task] [model] [stage] [gpu] [data_dir] [output_dir]`

For instance, run the following command to run a full stabletune process on XNLI with xlm-roberta-base model:

`bash ./scripts/train.sh translate-train-all xnli xlm-roberta-base 1`

`bash ./scripts/train.sh translate-train-all xnli xlm-roberta-base 2`

### Cross-Lingual Transfer Setting

`bash ./scripts/train.sh cross-lingual-transfer [task] [model] [stage] [gpu] [data_dir] [output_dir]`

For instance, run the following command to run a full stabletune process on XNLI with xlm-roberta-base model:

`bash ./scripts/train.sh cross-lingual-transfer xnli xlm-roberta-base 1`

`bash ./scripts/train.sh cross-lingual-transfer xnli xlm-roberta-base 2`

# Paper

[1]: https://github.com/google-research/xtreme
[2]: https://www.amazon.com/clouddrive/share/d3KGCRCIYwhKJF0H3eWA26hjg2ZCRhjpEQtDL70FSBN?_encoding=UTF8&%2AVersion%2A=1&%2Aentries%2A=0&mgh=1
[3]: https://console.cloud.google.com/storage/browser/xtreme_translations
[4]: https://drive.google.com/drive/folders/1Rdbc0Us_4I5MpRCwLASxBwqSW8_dlF87?usp=sharing
[5]: https://github.com/huggingface/transformers/
[6]: https://github.com/facebookresearch/MUSE
[7]: https://drive.google.com/drive/folders/1k9rQinwUXicglA5oyzo9xtgqiuUVDkjT?usp=sharing