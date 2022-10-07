## Efficient Entity Embedding Construction from Type Knowledge for BERT
Pytorch Implementation of AACL Findings 2022 paper ["Efficient Entity Embedding
Construction from Type Knowledge for BERT"](to_add)

## Download entity types
We extracted entity types through instanceof relation from Wikidata and it can be downloaded from [here](https://drive.google.com/file/d/17ClfmuM65U_rRG4OHx34TMXvE0EEyiki/view?usp=sharing).
After downloading, unzip it and move to `./resources/`:
```
gzip -d  wikidata_entity_types.tsv.gz
mv wikidata_entity_types.tsv ./resources/
```

Each line in `wikidata_entity_types.tsv` is as follows:
```
entity_id[TAB]entity_label[TAB]entity_wikipedia_title[TAB]entity_ids_connected_by_instanceof
```

## Entity Linking Experiment
Our code for entity linking experiment is adapted from [E-BERT](https://github.com/NPoe/ebert).
To set the environment for E-BERT, use the following command:

```
cd entity_linking
conda env create -f environment.yaml
conda activate e-bert
```

Then download wikipedia2vec (used only to get normalized wikipedia titles):
```
wget https://www.cis.uni-muenchen.de/~poerner/blobs/e-bert/wikipedia2vec-base-cased
mv wikipedia2vec-base-cased resources/wikipedia2vec
```

Finally, install [KnowBert](https://github.com/allenai/kb) for candidate entity generation.
```
cd code/kb
pip install -r requirements.txt
python -c "import nltk; nltk.download('wordnet')"
python -m spacy download en_core_web_sm
pip install --editable .
```

Use following command to train model on AIDA dataset and the score on test dataset will be reported.

```
python -u ./run_aida.py \
    --model_dir "./model_dirs/model" \
    --lr 3e-5 \
    --epochs 4 \
    --train_file ../data/AIDA/aida_train.txt \
    --dev_file ../data/AIDA/aida_dev.txt \
    --test_file ../data/AIDA/aida_test.txt \
    --wikidata_entity_types_path ../../resources/wikidata_entity_types.tsv
```
