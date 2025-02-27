# Framing Detection
This repository contains the code and data for the paper "Conflicts, Villains, Resolutions:
Towards models of Narrative Media Framing" by Lea Frermann, Jiatong Li, Shima Khanehzar and Gosia Mikolajczak in ACL 2023. Here is the [Paper Link](https://aclanthology.org/2023.acl-long.486/).

For question about code and experiments, please contact Jiatong Li at [jiatong.li@connect.polyu.hk](mailto:jiatong.li@connect.polyu.hk)<br>
For all other questions contact Lea Frermann at [lea.frermann@unimelb.edu.au](mailto:lea.frermann@unimelb.edu.au)

## The Narrative Frames Corpus
The Narrative Frames Corpus can be found under [./data/](./data/). It includes all articles, metadata, and aggregated as well as dis-aggregated annotations of narrative frames, and entity roles.

## Code

### Requirements
* torch == 1.13.1
* argparse == 1.1
* numpy == 1.21.5
* tqdm == 4.64.1
* transformers == 4.28.0
* nltk == 3.7

<!-- Other versions might also work, but the default settings are recommended if you encounter any version inconsistency problems. -->

### Data splits for model training

There are also two processed dataset in the current repo.
* [`data_splits_raw`](./experiments/data_splits_raw)

Structure:
```
- Fold_(1-5)
  - dev.txt
  - test.txt
  - train.txt
```

Format:
```
ID  Source  Bias  Time  Full_News_Content  AR  HI  CO  MO  EC
```
* [`data_splits_sentence_tokenized`](./experiments/data_splits_sentence_tokenized)

Structure:
```
- Fold_(1-5)
  - LABEL
    - dev.txt
    - test.txt
    - train.txt
```

Format:
```
ID  Source  Bias  Time  S1  S2  S3  S4  S5  Remaining_Sentences_RankedByRelateness  Full_News_Content  LABEL
```

### Usage
#### baselines 
##### [KNN](./experiments/naive_baselines.ipynb)
KNN implementation on this problem. For usage, see more instructions in the jupyter notebook.

##### [BERT & Longformer](./experiments/training_baseline.py)
```
cd experiments
python ./training_baseline.py

Framing Detection in Climate Change (BASE)

optional arguments:
  -h, --help            show this help message and exit
  --random_seed RANDOM_SEED
                        Random Seed for the program
  --device DEVICE       Selecting running device (default:cuda:0)
  --lr LR               learning rate (default: 2e-6)
  --lm LM               pre-trained language model
  --model MODEL         model structure to use
  --dataset DATASET     dataset folder path
  --specified_label SPECIFIED_LABEL
                        label for training
  --fine_tuning         fine tune the weights of bert
  --dataset_balancing   Balance the label distribution in the dataset
  --max_len MAX_LEN     max length the input can take (default: 256)
  --fold FOLD           We do 5-fold validation, select fold number here (range: 1~5)
  --ckp_path CKP_PATH   further pretrained model path
  --batch_size BATCH_SIZE
                        batch size for training (default: 16)
  --epochs EPOCHS       number of training epochs (default: 20)
  --log_dir LOG_DIR     tensorboard log directory
  --checkpoint_dir CHECKPOINT_DIR
                        directory to save checkpoint
```
##### [RBF](./experiments/training_rbf.py)
```
cd experiments
python ./training_rbf.py

Framing Detection in Climate Change (RBF)

optional arguments:
  -h, --help            show this help message and exit
  --random_seed RANDOM_SEED
                        Random Seed for the program
  --device DEVICE       Selecting running device (default:cuda:0)
  --dataset DATASET     dataset folder path
  --lr LR               learning rate (default: 2e-6)
  --lm LM               pre-trained language model
  --max_len MAX_LEN     max length the input can take (default: 256)
  --fold FOLD           We do 5-fold validation, select fold number here (range: 1~5)
  --n_passages N_PASSAGES
                        How many channels to select (range: 1~5), RBF-C <=> n_passages=5, RBF-C -a <=> n_passages=4, RBF-C -a-t <=> n_passages=3
  --batch_size BATCH_SIZE
                        batch size for training (default: 8)
  --epochs EPOCHS       number of training epochs (default: 20)
  --specified_label SPECIFIED_LABEL
                        label for training
  --fusion FUSION       Fusion Strategy
  --fine_tuning         fine tune the weights of PLM
  --dataset_balancing   Balance the label distribution in the dataset
  --log_dir LOG_DIR     tensorboard log directory
  --checkpoint_dir CHECKPOINT_DIR
                        directory to save checkpoint
```
##### [unsupervisedRBF](./experiments/unsupervisedRBF.ipynb)
Unsupervised version of RBF. For usage, see more instructions in the jupyter notebook.

##### [Snippext](https://github.com/rit-git/Snippext_public)
Please refer to the original directory of Snippext. We do not re-upload their codes here. It is easy to transfer their codes to our task. And we are willing to provide help if you encounter any problems.

#### run batch training
`Sample batch training codes. Remember to edit the codes (change the pre-defined paths to your customized paths) first before running batch training`
```
python ./experiments/scripts/run_training.py
```
`Training on the cloud clusters (Please make sure the environment has been set up.). Customized paths are also suggested.`

```
sbatch ./experiments/scripts/run_training.slurm
```

## Citation
#### Short
```
@inproceedings{narrative_framing_acl2023,
  title={Conflicts, Villains, Resolutions: Towards models of Narrative Media Framing},
  author={Lea Frermann and Jiatong Li and Shima Khanehzar and Gosia Mikolajczak},
  booktitle={Proceedings of The 61st Annual Meeting of the Association for Computational Linguistics},
  year={2023}
}
```
#### Official Full
```
@inproceedings{frermann-etal-2023-conflicts,
    title = "Conflicts, Villains, Resolutions: Towards models of Narrative Media Framing",
    author = "Frermann, Lea  and
      Li, Jiatong  and
      Khanehzar, Shima  and
      Mikolajczak, Gosia",
    booktitle = "Proceedings of the 61st Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers)",
    month = jul,
    year = "2023",
    address = "Toronto, Canada",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2023.acl-long.486",
    pages = "8712--8732",
    abstract = "Despite increasing interest in the automatic detection of media frames in NLP, the problem is typically simplified as single-label classification and adopts a topic-like view on frames, evading modelling the broader document-level narrative. In this work, we revisit a widely used conceptualization of framing from the communication sciences which explicitly captures elements of narratives, including conflict and its resolution, and integrate it with the narrative framing of key entities in the story as heroes, victims or villains. We adapt an effective annotation paradigm that breaks a complex annotation task into a series of simpler binary questions, and present an annotated data set of English news articles, and a case study on the framing of climate change in articles from news outlets across the political spectrum. Finally, we explore automatic multi-label prediction of our frames with supervised and semi-supervised approaches, and present a novel retrieval-based method which is both effective and transparent in its predictions. We conclude with a discussion of opportunities and challenges for future work on document-level models of narrative framing.",
}
```
