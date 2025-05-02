###Installing VirDetLoc on HPC with older libc

setup mamba
setup env with python 3.9
```
mamba install pytorch=1.10.2 torchvision=0.11.3 cudatoolkit=10.2 -c pytorch

python -m pip install detectron2 -f https://dl.fbaipublicfiles.com/detectron2/wheels/cu102/torch1.10/index.html

```

