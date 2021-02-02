# conda recipe for SBOL2

Instructions for building conda recipe for [pySBOL2](https://github.com/SynBioDex/pySBOL2).

## Prerequisite

The conda package manager should have been installed. For fresh instructions, see the [conda user guide](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html).

## Building the sbol2 conda package -- the step by step way

1. Set up conda environment for building the package
```shell
# From the recipe sub folder
conda env create --file recipe/conda_build_env.yaml -n conda_build_env
```

2. Build
```shell
# Few variables for convenience
CONDA_BLD_PATH=/tmp/conda-bld
PLATFORM=$(conda info | grep platform | sed 's/.*platform : //')
PACKAGE=sbol2

# From the recipe sub folder
conda activate conda_build_env
conda build recipe --output-folder ${CONDA_BLD_PATH}
```

3. Convert for other platform
```shell
conda convert \
    --platform osx-64 \
    --platform linux-64 \
    --platform win-64 \
    --output-dir ${CONDA_BLD_PATH} \
	${CONDA_BLD_PATH}/${PLATFORM}/${PACKAGE}-*.tar.bz2
```

4. Publish
```shell
source .secrets
anaconda \
    --token ${ANACONDA_TOKEN} \
    upload \
    --user ${ANACONDA_USER} \
    --label main \
    ${CONDA_BLD_PATH}/*/${PACKAGE}-*.tar.bz2
```

## What to do on next package release?

When ever a new version is released on pypi, here how to proceed:
- Update `{% set version = "1.2" %}` to the correct number in `recipe/meta.yaml`.
- Repeat steps 1-3 from above.

## Miscellaneous

Content generated with the help of [Grayskull](https://github.com/conda-incubator/grayskull).

