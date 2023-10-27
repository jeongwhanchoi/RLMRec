# RLMRec: Representation Learning with Large Language Models for Recommendation

 This is the PyTorch implementation by <a href='https://github.com/Re-bin'>@Re-bin</a> for RLMRec model proposed in this [paper](https://arxiv.org/abs/2310.15950):

 >**Representation Learning with Large Language Models for Recommendation**  
 >Xubin Ren, Wei Wei, Lianghao Xia, Lixin Su, Suqi Cheng, Junfeng Wang, Dawei Yin, Chao Huang*


\* denotes corresponding author
<p align="center">
<img src="RLMRec.png" alt="RLMRec" />
</p>

In this paper, we propose a model-agnostic framework RLMRec that aims to enhance existing recommenders with LLM-empowered representation learning. It proposes a recommendation paradigm that integrates representation learning with LLMs to capture intricate semantic aspects of user behaviors and preferences. RLMRec incorporates auxiliary textual signals, develops a user/item profiling paradigm empowered by LLMs, and aligns the semantic space of LLMs with the representation space of collaborative relational signals through a cross-view alignment framework.

## Environment

Run the following commands to create a conda environment:

```bash
conda create -y -n rlmrec python=3.9
conda activate rlmrec
pip install torch==1.13.1+cu116 torchvision==0.14.1+cu116 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu116
pip install torch-scatter -f https://data.pyg.org/whl/torch-1.13.1+cu117.html
pip install torch-sparse -f https://data.pyg.org/whl/torch-1.13.1+cu117.html
pip install pyyaml tqdm
```

😉 The codes are developed based on the [SSLRec](https://github.com/HKUDS/SSLRec) framework.

##  Dataset

We utilized three public datasets to evaluate RLMRec:  *Amazon-book, Yelp,* and *Steam*. 

First of all, please unzip the data by running following commands.
 ```
 cd data/
 cat data.tar.gz0* > data.tar.gz
 tar zxvf data.tar.gz
 ```

Each dataset consists of a training set, a validation set, and a test set. During the training process, we utilize the validation set to determine when to stop the training in order to prevent overfitting.
```
- amazon(yelp/steam)
|--- trn_mat.pkl
|--- val_mat.pkl
|--- tst_mat.pkl
|--- usr_prf.pkl
|--- itm_prf.pkl
|--- usr_emb_np.pkl
|--- itm_emb_np.pkl
```
### User/Item Profile
- Both user and item profiles are generated from **Large Language Models** from raw text data.
- The `user profile` (saved in `usr_prf.pkl`) encapsulates the particular types of items that users are inclined to favor. 
- The `item profile` (saved in `itm_prf.pkl`) articulates the specific types of users that the item is apt to attract. 

😊 You can run the code `python data/read_profile.py` as an example to read the profiles as follows.
```
$ python data/read_profile.py
User 626's Profile:

PROFILE: Based on the user's purchased items and reviews, it is difficult to determine a clear pattern of book preferences. However, the user seems to enjoy mystery novels with unique characters and a touch of humor. The user also seems to appreciate books with strong character development and fast-paced, thrilling plotlines.

REASONING: Although there is a lack of specific details, the purchased items and reviews suggest that the user enjoys mystery novels with vibrant characters and comedic elements, as seen in 'Alison Wonderland' and 'Whole Lotta Trouble.' Furthermore, the review for 'Innocence: A Novel' suggests an appreciation for fast-paced, thrilling plots with dark themes. Additionally, the review for the book with no title highlights an appreciation for storytelling excellence and beautifully written dialogue.
```

### Semantic Embedding
The encoded semantic embeddings from the user/item profiles are stored in `usr_emb_np.pkl` and `itm_emb_np.pkl`.

🤗 Welcome to use our processed data to improve your research!

## Examples to run the codes

The command to evaluate the backebone models and RLMRec is as follows. 

  - Backbone 

    ```python encoder/train_encoder.py --model {model_name} --dataset {dataset} --cuda 0```   

  - RLMRec-Con (Constrastive Alignment):

    ```python encoder/train_encoder.py --model {model_name}_plus --dataset {dataset} --cuda 0```

  - RLMRec-Gen (Generative Alignment):

    ```python encoder/train_encoder.py --model {model_name}_gene --dataset {dataset} --cuda 0```

Supported models/datasets:

* model_name:  `gccf`, `lightgcn`, `sgl`, `simgcl`, `dccf`, `autocf`
* dataset: `amazon`, `yelp`, `steam`

Hypeparameters:

* The hyperparameters of each model are stored in `encoder/config/modelconf` (obtained by grid-search).

 **For advanced usage of arguments, run the code with --help argument.**

**Thanks for your interest in our work.**
