# Anaconda-Env

conda create --name dsp python=3.6

conda remove --name dsp --all

source activate dsp

source deactivate

conda env list

conda env export &gt; environment.yaml

conda env create -f environment.yaml

source activate myenv  
conda install pip  
conda install ipykernel \# or pip install ipykernel  
source activate myenv  
python -m ipykernel install --user --name myenv --display-name "Python \(myenv\)"

