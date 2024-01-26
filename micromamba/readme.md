* Micromamba container

Example micromamba containers for creating Python environments

In these recipes, we are pulling the Micromamba container from DockerHub, installing the needed tools in the post section, and executing the `micromamba run ...` command whenever the container is executed. Now we build the container, as:
```
module load apptainer
unset APPTAINER_BINDPATH
apptainer build mymamba.sif Singularity
```
Notice we also made the container sif file into executable, so that we can run the container directly, following the command we want to run from the container:
```
$ ./mymamba.sif bwa 
Program: bwa (alignment via Burrows-Wheeler transformation)
Version: 0.7.17-r1188
...
```
Python environment packages can be also specified by file `environment.yml` , e.g. 
```
channels:
  - defaults
  - conda-forge
dependencies:
  - matplotlib
  - python=3.9
  - pip
```
In that case, we can modify the micromamba install command as:
```
micromamba create --yes --name base --file environment.yml
```

For the GPU container, which as an example installs PyTorch environment, one has to use the `--nv` flag during the build to ensure that the mamba package manager picks up the GPU/CUDA dependencies and installs the GPU version of PyTorch.
```
 module load apptainer
 unset APPTAINER_BINDPATH
 apptainer build --nv mymamba_gpu.sif Singularity.gpu
```
To test that the GPU version if PyTorch is installed:
```
module load apptainer
setenv APPTAINER_NV true
./mymamba_gpu2.sif /opt/conda/bin/python -c "import torch; print(torch.cuda.is_available())"
True
```

