# CCN January 2023 Workshop

Materials for the workshop on CaImAn and Mesmerize for Calcium & Voltage Imaging. This repo currently includes installation instructions, we will add more materials as the workshop approaches.

This workshop will cover the following topics:

1. [`fastplotlib`](https://github.com/kushalkolar/fastplotlib) for very fast interactive visualization of very large datasets in notebooks.
2. Preprocessing of calcium imaging data
3. Various [`CaImAn`](https://github.com/flatironinstitute/CaImAn) algorithms including:
    - NoRMCorre motion correction
    - CNMF(E) - Constrained Non-negative Matrix Factorization for source separation and extraction
    - OnACID and FIOLA for online calcium imaging analysis
    - VolPy for voltage imaging analysis
4. [`mesmerize-core`](https://github.com/nel-lab/mesmerize-core) for parameter optimization and data organization with CaImAn.
5. Postprocessing

# Demos

Fork and clone this repo to follow along with the demos.

1. Fork this repo to your GitHub:

![image](https://user-images.githubusercontent.com/9403332/215364128-7b5575a0-d72e-4b03-9c4a-3051b4ea5930.png)

2. Clone the fork to your local:

Run this in your terminal, anaconda promp or mambaforge prompt:

![image](https://user-images.githubusercontent.com/9403332/215364183-efa6748c-b0b1-4e6e-bfaf-e5ae477a56a7.png)

3. To run the demos launch `jupyter lab` from within your environment. 

4. Download the demo movie from the following link and place it within your `caiman_data/example_movies` dir.

https://caiman.flatironinstitute.org/~neuro/caiman_downloadables/Sue_2x_3000_40_-46.tif

# Installation instructions

**If you are starting from scratch, start here. If you just need to update, then see the [Update instructions](https://github.com/EricThomson/CCN_caiman_mesmerize_workshop_2023#update-instructions) below.**

We recommend installing the `mesmerize-core` conda package which gives you everything that you need, including `caiman`. `fastplotlib` is installed separately but that is relatively trivial.

1. Install Anaconda if you already don't have it. [Click here for official instructions on the Anaconda website](https://docs.anaconda.com/anaconda/install/index.html). A few important notes:
    - On Windows and Mac if you use the graphical installer make sure you choose to install **"Just Me"** and not a system install.
    - On Linux you can probably accept the default `PREFIX` to install to your user home directory, do not perform a system install.

**All the following commands are to be entered into the Anaconda prompt on Window, or in the terminal on Linux and Mac.**

2. Install `mamba` into your base environment. Skip this step if you have `mamba`. This step may take 10 minutes and display several messages like "Solving environment: failed with..." but it should eventually install `mamba`.

```bash
conda install -c conda-forge mamba

# if conda is behaving slow, this command can sometimes help
conda clean -a
```

  - **Important note:** In general, sometimes conda or mamba will get stuck at a step, such as creating an environment or installing a package. Pressing `Enter` on your keyboard can sometimes help it continue when it hangs like this.

3. To create a new environment and install `mesmerize-core` into it do this:

```bash
mamba create -n mescore -c conda-forge mesmerize-core
```

 `caiman` is a dependency of `mesmerize-core` so it will automatically grab `caiman` too

 If you already have an environment with `caiman`:

```bash
mamba install -n name-of-env-with-caiman mesmerize-core
```

4. Activate environment. You can only use `mesmerize-core` in the environment that it's installed into.

```bash
mamba activate mescore
```

5. Install `caimanmanager`

```bash
caimanmanager.py install
```

  - The `caimanmanager.py` step may cause issues, especially on Windows. Assuming your anaconda is in your user directory a workaround is to call it using the full path:

```bash
python C:\Users\your-username\anaconda3\envs\your-env-name\bin\caimanmanager.py install
```

  - If you continue to have issues with this step, please post an issue on the [`CaImAn` github](https://github.com/flatironinstitute/CaImAn) or post in the `#caiman` channel on slack.

6. Run `ipython` and verify that `caiman` and `mesmerize_core` are installed (note to run ipython just enter `ipython` in your anaconda prompt):

```python
# run in ipython
import caiman
import mesmerize_core
print(caiman.__version__)  # should be 1.9.13, anything over 1.9.10 is mostly fine for the workshop but we recommend 1.9.13
print(mesmerize_core.__version__)  # should be 0.1.0, make sure it's not the 0.1.0.b1 beta version
```

7. Install `fastplotlib` for visualization into the same environment (run this in the anaconda prompt, not ipython)

```bash
pip install fastplotlib
```

(The previous instructions said to install it directly from github but we were able to make a pip-installable release since `pygfx` recently updated their release, and `pygfx` is the main dependency for `fastplotlib`)

 If you don't have git installed you will need to install that first in the environment:

```bash
conda install git
```

8. If you have C compilers installed on your system we recommend `pip install simplejpeg`. This is usually easier on Linux & Mac than on Windows. If you cannot install `simplejpeg` don't worry, it will just make `fastplotlib` slightly less fast.

9. Install Vulkan drivers for your GPU (this includes GPUs that are commonly integrated within CPUs).
    - Windows: Vulkan drivers should be installed by default so you shouldn't have to do anything, if you can run the first few cells of the `fastplotlib` `simple.ipynb` example you're good to go!
    - Mac uses Metal instead of Vulkan, which should also be installed by default so you don't have to do anything!
    - Linux: This is the one time where you need to do more work on Linux :D. See the [`fastplotlib` repo](https://github.com/kushalkolar/fastplotlib#linux) for instructions.

10. Finally we recommend trying to run the simplest demo notebook for each library:
    - `fastplotlib`: https://github.com/kushalkolar/fastplotlib/blob/master/examples/simple.ipynb
    - `caiman`: https://github.com/flatironinstitute/CaImAn/blob/master/demos/notebooks/demo_pipeline.ipynb
    - `mesmerize-core`: https://github.com/nel-lab/mesmerize-core/blob/master/notebooks/mcorr_cnmf.ipynb

     You can either clone each repo to try out the demo notebooks or download the repo as a zip file:

      - fastplotlib: https://github.com/kushalkolar/fastplotlib/archive/refs/heads/master.zip
      - caiman: https://github.com/flatironinstitute/CaImAn/archive/refs/heads/master.zip
      - mesmerize-core: https://github.com/nel-lab/mesmerize-core/archive/refs/heads/master.zip

# Update instructions

Note when new releases are available for mesmerize-core, run the following from within your virtual environment to update: `mamba update -c conda-forge mesmerize-core`. To update fastplotlib, use the following command (from within your activate virtual environment): `pip install --upgrade fastplotlib` 
