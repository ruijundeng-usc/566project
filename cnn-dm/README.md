# CNN-DM

1. FairSeq: https://github.com/pytorch/fairseq/tree/main/examples/bart

2. CNN-DM Dataset: https://github.com/artmatsak/cnn-dailymail

3. Rouge Score: https://github.com/pltrdy/files2rouge

4. StanfordNLP lib (BPE for Rouge Score): https://stanfordnlp.github.io/CoreNLP/

# Note

1. create interactive job (while in discovery node): salloc --time=2:00:00 --cpus-per-task=8 --mem=16GB --account=xiangren_818 --partition=gpu --gres=gpu:v100:2

2. compute node has no internet access, but you can create python env and pip install in discovery node. You will also have to predownload the pretrained model in discovery node.

3. During my script running on interactive mode, I also have to download (wget) 2 files of line 15,16 in (https://github.com/pytorch/fairseq/blob/main/fairseq/data/encoders/gpt2_bpe.py) and change them to path to my local repository.
