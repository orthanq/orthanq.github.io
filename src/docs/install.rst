Installation
======================================================

Latest Orthand release is downloaded from the corresponding GitHub `repository`_.

.. _repository: https://github.com/orthanq/orthanq/releases

Then Orthanq is built and the easiest way to build Orthanq is via Conda/Mamba.

First, create the environment given by the `environment.yml` in this repository::

    mamba env create -n orthanq-dev -f environment.yml

Second, activate the environment with::

    mamba activate orthanq-dev

Third, build Orthanq via::

    HDF5_DIR=$CONDA_PREFIX RUSTFLAGS='-L $CONDA_PREFIX/include' cargo build

Last, conda lib location is set via::

    export LD_LIBRARY_PATH=$CONDA_PREFIX/lib
