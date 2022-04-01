# CNN-DM

1. FairSeq: https://github.com/pytorch/fairseq/tree/main/examples/bart

2. CNN-DM Dataset: https://github.com/artmatsak/cnn-dailymail

3. Rouge Score: https://github.com/pltrdy/files2rouge

4. StanfordNLP lib (BPE for Rouge Score): https://stanfordnlp.github.io/CoreNLP/

# Note

1. create interactive job (while in discovery node): salloc --time=2:00:00 --cpus-per-task=8 --mem=16GB --account=xiangren_818 --partition=gpu --gres=gpu:v100:2

2. compute node has no internet access, but you can create python env and pip install in discovery node. You will also have to predownload the pretrained model in discovery node.

3. During my script running on interactive mode, I also have to download (wget) 2 files of line 15,16 in (https://github.com/pytorch/fairseq/blob/main/fairseq/data/encoders/gpt2_bpe.py) and change them to path to my local repository.

4. finetuning:
MAX_EPOCH=3
TOTAL_NUM_UPDATES=20000
WARMUP_UPDATES=500
LR=3e-05
MAX_TOKENS=2048
UPDATE_FREQ=4
BART_PATH=/home1/ruijunde/566project/bart.large/model.pt

CUDA_VISIBLE_DEVICES=0 fairseq-train cnn_dm_test-bin \
    --restore-file $BART_PATH \
    --max-tokens $MAX_TOKENS \
    --task translation \
    --source-lang source --target-lang target \
    --truncate-source \
    --layernorm-embedding \
    --share-all-embeddings \
    --share-decoder-input-output-embed \
    --reset-optimizer --reset-dataloader --reset-meters \
    --required-batch-size-multiple 1 \
    --arch bart_large \
    --criterion label_smoothed_cross_entropy \
    --max-epoch $MAX_EPOCH \
    --label-smoothing 0.1 \
    --dropout 0.1 --attention-dropout 0.1 \
    --weight-decay 0.01 --optimizer adam --adam-betas "(0.9, 0.999)" --adam-eps 1e-08 \
    --clip-norm 0.1 \
    --lr-scheduler polynomial_decay --lr $LR --total-num-update $TOTAL_NUM_UPDATES --warmup-updates $WARMUP_UPDATES \
    --fp16 --update-freq $UPDATE_FREQ \
    --skip-invalid-size-inputs-valid-test \
    --find-unused-parameters;
