# User Guide

------
**Attention**:
     When you download this project on github, there are three model files in the '_model_' directory named with model.ckpt\*. 
     The files are too large to download to be included in the zipped file. You need to download the models file mannually, then put them in the 'model' directory.

-------

1. Environment requirement and installation:
    You need to install **python2.7**, **tensorflow (>1.15)** and **SASTBX** kit.
    1) Recommend you use Anaconda to manage your environment. Just download Anaconda (python2.7 version) on https://www.anaconda.com, then follow the installation and user guide which suit for your platform. (https://docs.anaconda.com/anaconda/install/linux/)

    2) After you installed anaconda successfully, use conda to install tensorflow. 
    If you have GPU devices, you can install tensorfloe-gpu version:

        conda install tensorflow-gpu 

    (you can also specify version like this: conda install tensorflow-gpu=1.15.0, which is the version I used.)
    Or just install CPU version:

        conda install tensorflow=1.15.0

    3) The next step is to install SASTBX, which is available from this link:

    http://liulab.csrc.ac.cn/dokuwiki/doku.php?id=sastbx

    Follow the instruction to install the command-line version.
    
    The SASTBX should be installed in the **same** the python environment of tensorflow.
    
    After installation, you can type:
    ```shell
        sastbx.python
    ```
    Then, type:
    ```python
        import tensorflow as tf
        tf.__version__
    ```
    If it prints the tensorflow version information, the installation is successful.

The supports to Python 3.0 and Tensorflow 2.x will be provided in future releases (see Notes at the end). For the moment, tensorflow 2.0 or above can be used by the following (added to the current release)
```python
import tensorflow.compat.v1 as tf
tf.disable_v2_behavior()
```

2. Description of files:
    Files in folder model are the trained autoencoder model, which is used in the reconstruction project.
    main.py is the only file you need to use when you run the project.
    SASDA25.* are samples which can use to run a demo.
    Other py files are library-files, which should be to placed in the same folder with **main.py**.

3. How to run 
    An 'example' script is prepared in the example folder, try 'run.sh' in command terminal.
    Once the installation is completely, use the following command to do a reconstruction:

```shell
    sastbx.python main.py --model_path $model_path --iq_path $iq_path --output_folder $output_path --rmax $known_rmax --target_pdb $targetpdb_path

    --model_path: the path of well-trained tensorflow's model files.
    --iq_path: file of experimental saxs data, DAT* or GNOM.
    --output_folder: output path of results.
    --rmax: size(Å) of the reconstruction object if you know.
    --target_pdb: the target pdb structure if you want to compare with results.
    --rmax_start: minimum of rmax (default=10)
    --rmax_end: maximum of rmax (default=300)

    --iq_path and --output_folder are necessary. if the --iq_path you give is a GNOM type(eg.example/SASDA25.out) file, --rmax is not needed. This project will extract the rmax from your input file. If the --iq_path you give is just a DAT* file (two or three columns of data), you can specify a certain value of --rmax. If not, this project will find a well-matched size during reconstruction. You also can give a range of rmax by specify --rmax_start and --rmax_end.
    --target_pdb is another additional parameter, help you know the relationship (correlation coefficient) between the reconstruction results and the structure you give.
```

4. Reconstruction results
    When the reconstruction is finished, you will get the following files:
    ```
    out.ccp4: ccp4 format reconstruction result.
    out_za.pdb: pdb format reconstruction  result.
    Log.txt: Record the time spent in each part.
    cc_mat.txt: if you give --target_pdb, this file record the correlation coefficient of each iteration's top 20 samples' results.
    final_saxs.txt: Record the fitting saxs data of last generation.
    score_mat.txt: Record top 20 samples with high score on every generation.
    cc_mat.txt: Record top 20 samples' correlation coefficient with target structure in every generation.
    ```
    
5. Notes
    The SASTBX package is used to provide two functions: (1) computation of model saxs profile from voxel objects; (2) superpose the reconstructed models to reference model to quantify the consistency. Both functions can be replaced by other options. For example, the SAXS profiles can be computed using Crysol, AquaSAXS, PepsiSAXS or DENSS; model comparison can be done with chimera program, or the program in ATSAS. The codes in map2iq.py needs to be modified to reflect the change of these functions.
    
 6. Training of the auto-encoder can be accomplished using two methods:
     * two-stage training (as described in the paper) 
     * single-stage training
     The source codes are available in **train_net** folder for both methods (see README inside).
