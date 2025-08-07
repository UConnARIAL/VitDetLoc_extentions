###Installing VitDetLoc on FRONTERA with older libc and other potential incompatibility issues
Frontera has cuda 11.3 but there is no driver that can be installed via cuda so pip is the only solution

```
# Download Miniconda3 installer for Linux x86_64
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
chmod +x miniconda.sh

# Install to a writable directory
./miniconda.sh -b -p $SCRATCH/miniconda3

# Initialize conda
source $SCRATCH/miniconda3/etc/profile.d/conda.sh
conda init bash
source ~/.bashrc

# make sure it is using the installed miniconda version
conda info | grep 'base environment'
#Check output
#base environment : /scratch/<XXX>/<XXX>/miniconda3  (writable)

module load cuda/11.3
module load gcc/9.1.0

# Create the following updated env to create new env
channels:
  - conda-forge
  - pytorch
  - defaults
dependencies:
  - python=3.10
  - pip
  - rasterio
  - opencv
  - torchgeo
  - albumentations
  - kornia
  - scikit-learn
  - seaborn
  - pip:
      - lightning
      - haversine
      - frozendict

conda env create -f env2.yml --prefix $scratch/CONDA_ENV/vitdetloc-env

# activate env
conda activate /$scratch/CONDA_ENV/vitdetloc-env

#NOT going to work: conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia
pip install torch==1.12.1+cu113 torchvision==0.13.1+cu113 torchaudio==0.12.1 --extra-index-url https://download.pytorch.org/whl/cu113

# Check:
python -c "import torch; print(torch.cuda.is_available()); print(torch.version.cuda)"
python -c "import torch; print(torch.cuda.get_device_name(0))"

check If there are issues with cuda liraries it could be PATH issue
echo $LD_LIBRARY_PATH

Should see:
/usr/local/cuda-11.3/lib64
/usr/lib/x86_64-linux-gnu

Check if it is pointing to stubs
ls -l $(find /opt/apps/cuda/11.3/ -name libcuda.so 2>/dev/null)

Not the real driver 
/opt/apps/cuda/11.3/targets/x86_64-linux/lib/stubs/libcuda.so

Check where PyTorch is looking for libcuda.so
ldd $(python -c "import torch; print(torch.__file__)")

If you see PROBLEM
/usr/local/cuda-11.3/stubs

Solution
export LD_LIBRARY_PATH=/usr/lib64:$LD_LIBRARY_PATH

Check again
ldd $(python -c "import torch; print(torch._C.__file__)") | grep libcuda

Output should be (OR Similar)
libcuda.so.1 => /usr/lib64/libcuda.so.1 (0x00007f1234567000)

# Check C++ aka gcc compiler version
which gcc
gcc --version
# Use gcc > 9 by module load after checking with module spider or eq.
# Detectron2 cannot be built with old < 9 gcc compilers in the HPC by default.
module load gcc/9.4.0
pip install --use-pep517 --no-build-isolation git+https://github.com/facebookresearch/detectron2.git

With above you may need to have the following before loafing the env on the SLRUM scrips 

module load gcc/11.3
module load gcc/9.1.3
export LD_LIBRARY_PATH=/usr/lib64:$LD_LIBRARY_PATH




#####################################################################################################


###Installing VitDetLoc on HPC with older libc and other potential incompatibility issues

```
# Download Miniconda3 installer for Linux x86_64
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
chmod +x miniconda.sh

# Install to a writable directory
./miniconda.sh -b -p $SCRATCH/miniconda3

# Initialize conda
source $SCRATCH/miniconda3/etc/profile.d/conda.sh
conda init bash
source ~/.bashrc

# make sure it is using the installed miniconda version
conda info | grep 'base environment'
#Check output
#base environment : /scratch/<XXX>/<XXX>/miniconda3  (writable)

# Create the following updated env to create new env
channels:
  - conda-forge
  - pytorch
  - defaults
dependencies:
  - python=3.10
  - pip
  - rasterio
  - opencv
  - torchgeo
  - albumentations
  - kornia
  - scikit-learn
  - seaborn
  - pip:
      - lightning
      - haversine
      - frozendict

conda env create -f env2.yml --prefix $scratch/CONDA_ENV/vitdetloc-env

# activate env
conda activate /$scratch/CONDA_ENV/vitdetloc-env

#install torch.... may want change the cuda based on the hpc env
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia 

# Check C++ aka gcc compiler version
which gcc
gcc --version
# Use gcc > 9 by module load after checking with module spider or eq.
# Detectron2 cannot be built with old < 9 gcc compilers in the HPC by default.
module load gcc/9.4.0
pip install --use-pep517 --no-build-isolation git+https://github.com/facebookresearch/detectron2.git


##############################################################################################################
--------------------------------------------------------------------------------------------------------------
#setup mamba
#setup env with python 3.9
mamba install pytorch=1.10.2 torchvision=0.11.3 cudatoolkit=10.2 -c pytorch

python -m pip install detectron2 -f https://dl.fbaipublicfiles.com/detectron2/wheels/cu102/torch1.10/index.html

module purge
module load gcc/11.2.0
module load python3/3.9.7
module load cuda/11.3

FOR LS6
cd $SCRATCH

# Download Miniconda3 installer for Linux x86_64
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
chmod +x miniconda.sh

# Install to a writable directory
./miniconda.sh -b -p $SCRATCH/miniconda3

# Initialize conda
source $SCRATCH/miniconda3/etc/profile.d/conda.sh
conda init bash
source ~/.bashrc

conda create --name vit_sandbox python=3.10
conda activate vit_sandbox
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia

which gcc
gcc --version
# Use gcc > 9 by module load after checking with module spider or eq.
# Detectron2 cannot be built with old < 9 gcc compilers in the HPC by default.
module load gcc/9.4.0
pip install --use-pep517 --no-build-isolation git+https://github.com/facebookresearch/detectron2.git

conda env update --file environment.yml

05/23/2025 Frontera

setup mamba
setup env with python 3.9.2

mamba install pytorch=1.10.2 torchvision=0.11.3 cudatoolkit=10.2 -c pytorch

# update env file to remove name anding pip for dependency.
conda env update --file=environment.yml
pip install --use-pep517 --no-build-isolation git+https://github.com/facebookresearch/detectron2.git

```





```
