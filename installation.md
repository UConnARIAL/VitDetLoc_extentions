###Installing VitDetLoc on HPC with older libc

setup mamba
setup env with python 3.9
```
mamba install pytorch=1.10.2 torchvision=0.11.3 cudatoolkit=10.2 -c pytorch

python -m pip install detectron2 -f https://dl.fbaipublicfiles.com/detectron2/wheels/cu102/torch1.10/index.html

```
module purge
module load gcc/11.2.0
module load python3/3.9.7
module load cuda/11.3

FOR LS6
conda create --name vit_sandbox python=3.10
conda activate vit_sandbox
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
module load gcc/9.4.0
pip install --use-pep517 --no-build-isolation git+https://github.com/facebookresearch/detectron2.git
