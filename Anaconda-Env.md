conda create --name dsp python=3.6

conda remove --name dsp --all

source activate dsp

source deactivate

conda env list

conda env export > environment.yaml

conda env create -f environment.yaml

