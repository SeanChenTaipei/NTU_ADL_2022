# ADL HW2

## Environment
```shell
## If you want to run in a virtual env
conda create --name adl-hw2 python=3.9
conda activate adl-hw2
pip install -r requirements.txt
```

## Download pretrained model weights
```
bash ./download.sh 
```
Pretrained model weights will be downloaded and unzip into 2 folder.<br>
Checkpoint folders `mc_best_ckpt` and `qa_best_ckpt` correspondes to context selection model and question answering model, respectively.


## Data Preprocessing

```shell
## MC SWAG dataset gen
python preprocess_mc.py --train_file ./data/train.json --validation_file ./data/valid.json --context_file ./data/context.json --test_file ./data/test.json --output_dir ./mc_data

## QA SQuaD dataset gen
python preprocess_qa.py --train_file ./data/train.json --validation_file ./data/valid.json --context_file ./data/context.json --output_dir ./qa_data
```
## Training
- Multiple Choice Model
```shell
python run_swag_no_trainer.py -h # check out the arguments
python run_swag_no_trainer.py --train_file <path/to/train> \
      --validation_file <path/to/valid> --max_length 512 \
      --per_device_train_batch_size 32 --per_device_train_batch_size 2 --lr_scheduler_type cosine\
      --model_name_or_path hfl/chinese-bert-wwm-ext --tokenizer_name bert-base-chinese \
      --with_tracking --output_dir <output/dir>

```
- Question Answering Model
```shell
python run_qa_no_trainer.py -h # check out the arguments

python run_qa_no_trainer.py --train_file <path/to/train> --validation_file <path/to/valid> \
                            --test_file <path/to/test> --max_seq_length 512 \
                            --model_name_or_path hfl/chinese-roberta-wwm-ext-large --per_device_train_batch_size 2 \
                            --per_device_eval_bastch_size 2 --gradient_accumulation_steps 32 \
                            --learning_rate 3e-5 --num_train_epochs 5 --num_warmup_steps 0 \
                            --with_tracking --output_dir <output/dir> --checkpointing_step epoch \
```

## Generate prediction
```shell
bash ./run.sh /path/to/context.json /path/to/test.json  /path/to/pred/prediction.csv
```
