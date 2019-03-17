# morph2vec

This repo is a modification of [prop2vec](https://github.com/oavraham1/prop2vec) code 
with some additional features on top of it.

#### Current Implementation:
* fastText to get word-vectors
* preprocessing step to modify the `.conllu` file to fit it to the fastText model

#### Modifications and additional features
* added support for WLTMN input
    * W: wordform
    * L: lemma
    * T: morphological tags
    * M: morphemes
    * N: n-grams
* morphemes are extracted with [word2morph](https://github.com/MartinXPN/word2morph)
which takes longer time because a neural network is used to extract
morphemes from a given lemma
* n-grams are added during the preprocessing step (not fasttext default one)

## Instructions
* To prepare the data for training a fastText model:
```commandline
python -m morph2vec.data.preprocess --input_path ${input_file} --output_path ${corpus_processed} # --word2morphemes_model_path w2m/model.hdf5 --word2morphemes_processor_path w2m/processor.pkl
```

* To train a fastText model:
```commandline
python -m morph2vec.train 
        train_unsupervised --input ru_syntagrus-ud-train.conllu_processed --model skipgram --props w+l+t+m+n --lr 0.025 --dim 200 --ws 2 --epoch 5 --minCount 5 --minCountLabel 0 --minn 3 --maxn 6 --neg 5 --wordNgrams 1 --loss ns --bucket 2000000 --thread 1 --lrUpdateRate 100 --t 1e-3 --label __label__ --verbose 2 --pretrainedVectors ""
        save_model --path ru_syntagrus_w+l+t+m+n.bin
```
