# Steps to manage multiple CUDA environments

This gist contains all the steps required to:
- Install multiple CUDA versions (e.g., `CUDA 11.3` and `CUDA 11.8`).
- Manage multiple CUDA environments on Ubuntu using the utility called [environment modules](https://modules.readthedocs.io/en/latest/).
- Use this approach to avoid CUDA environment conflicts.

> Environment Modules is a package that provides for the dynamic modification of a user's environment via modulefiles. You can find more on it at https://modules.readthedocs.io/en/latest/

## 1. Install the Compatible NVIDIA Drivers (if required)

- Add PPA GPU Drivers Repository to the System
    ```bash
    sudo add-apt-repository ppa:graphics-drivers/ppa
    ```
- Check GPU and available drives
    ```bash
    ubuntu-devices drivers
    ```

- Install the compatible driver
    ```bash
    # in my case it is nvidia-driver-530
    sudo apt install nvidia-driver-530
    ```

- Check the installed NVIDIA driver
    ```bash
    nvidia-detector 
    ```

> **Note**:
> - You can also auto-install the compatible driver using `sudo ubuntu-drivers autoinstall`.
> - Additionally, you can also install NVIDIA drivers using the **Software & Updates** Ubuntu app. Just go to the **Additional Drivers** tab, choose a driver, and click **Apply Changes**.
  

## 2. Install `CUDA 11.3` and `CUDA 11.8`

- Go to the [https://developer.nvidia.com/cuda-toolkit-archive](https://developer.nvidia.com/cuda-toolkit-archive) and select `CUDA Toolkit 11.3` from the available options. 
- Choose your OS, architecture, distribution, version, and installer type. For example, in my case:

    Option  | value
    | :---:|:---:|
    | OS  | Linux |
    | Architecture  | x86_64 |
    | Distribution  | Linux |
    | Version  | 20.04 |
    | Installer type  | deb(local) |

- Follow the provided installation instructions by copying and pasting the commands into your terminal. This will install `CUDA 11.3`. Use the following commands:
    ```bash
    wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
    sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
    wget https://developer.download.nvidia.com/compute/cuda/11.3.0/local_installers/cuda-repo-ubuntu2004-11-3-local_11.3.0-465.19.01-1_amd64.deb
    sudo dpkg -i cuda-repo-ubuntu2004-11-3-local_11.3.0-465.19.01-1_amd64.deb
    sudo apt-key add /var/cuda-repo-ubuntu2004-11-3-local/7fa2af80.pub
    sudo apt-get update
    sudo apt-get -y install cuda
    ```
- Similarly, install `CUDA 11.8` using the following commands:
    ```bash
    wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
    sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
    wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-ubuntu2004-11-8-local_11.8.0-520.61.05-1_amd64.deb
    sudo dpkg -i cuda-repo-ubuntu2004-11-8-local_11.8.0-520.61.05-1_amd64.deb
    sudo cp /var/cuda-repo-ubuntu2004-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
    sudo apt-get update
    sudo apt-get -y install cuda
    ```
- Make sure to copy and execute the commands above in your terminal to install `CUDA 11.3` and `CUDA 11.8` on your system.

## 3. Install `cuDNN` library

- Go to https://developer.nvidia.com/cudnn and download the `cuDNN` library for `CUDA 11.x`. Note that you might need to create a developer's account first.

- Untar the downloaded file using the following command:
    ```bash
    tar -xvf cudnn-linux-x86_64-8.9.2.26_cuda11-archive.tar.xz
    ```

- Copy the `cuDNN` files to the `CUDA` toolkit files:
    ```bash
    # for CUDA 11.3
    sudo cp include/cudnn*.h /usr/local/cuda-11.3/include
    sudo cp lib64/libcudnn* /usr/local/cuda-11.3/lib64

    # for CUDA 11.8
    sudo cp include/cudnn*.h /usr/local/cuda-11.8/include
    sudo cp lib64/libcudnn* /usr/local/cuda-11.8/lib64
    ```

- Make the files executable:
    ```bash
    sudo chmod a+r /usr/local/cuda-11.3/include/cudnn*.h /usr/local/cuda-11.3/lib64/libcudnn*
    sudo chmod a+r /usr/local/cuda-11.8/include/cudnn*.h /usr/local/cuda-11.8/lib64/libcudnn*
    ```

> **Note**:
> Strictly speaking, you are done with the CUDA setup. You can use it by adding the CUDA bin and library path to the PATH and LD_LIBRARY_PATH environment variables. For example, you can set up CUDA 11.8 by adding the following lines in the `~/.bashrc`:
> ```bash
> PATH=/usr/local/cuda-11.3/bin:$PATH
> LD_LIBRARY_PATH=/usr/local/cuda-11.8/extras/CUPTI/lib64:$LD_LIBRARY_PATH
> LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH
> ```
> Similarly, you can set up CUDA 11.3. However, manually changing the paths every time can be cumbersome!

## 4. Manage multipe CUDA versions using `environment modules`

### a) Install the environment modules utility:
- Run the following commands:
    ```bash
        sudo apt-get update
        sudo apt-get install environment-modules
    ```
- Check the installation:
    ```bash
    # Check the installation by running
    module list
    ```

 > You should see a list of default installed modules like git and maybe their versions displayed when you run the command `module list`. This confirms that the environment modules utility has been successfully installed on your system.

### b) Create modulefiles for CUDA distributions
  
> **Note**: You might need root permissions to create directories and files. Use sudo in that case.

- Create a directory `/usr/share/modules/modulefiles/cuda` to hold modulefiles for cuda distributions
    ```bash
    sudo mkdir -p /usr/share/modules/modulefiles/cuda
    ```

- Create a modulefile `/usr/share/modules/modulefiles/cuda/11.3` for `CUDA 11.3` and add the following lines:
    ```bash
    #%Module1.0
    ##
    ## cuda 11.3 modulefile
    ##

    proc ModulesHelp { } {
        global version
        
        puts stderr "\tSets up environment for CUDA $version\n"
    }

    module-whatis "sets up environment for CUDA 11.8"

    if { [ is-loaded cuda/11.8 ] } {
    module unload cuda/11.8
    }

    set version 11.3
    set root /usr/local/cuda-11.3
    setenv CUDA_HOME	$root

    prepend-path PATH $root/bin
    prepend-path LD_LIBRARY_PATH $root/extras/CUPTI/lib64
    prepend-path LD_LIBRARY_PATH $root/lib64
    conflict cuda
    ```

- Similarly, create a modulefile `/usr/share/modules/modulefiles/cuda/11.3` for `CUDA 11.3` and add the following lines:
    ```bash
    #%Module1.0
    ##
    ## cuda 11.8 modulefile
    ##

    proc ModulesHelp { } {
        global version
        
        puts stderr "\tSets up environment for CUDA $version\n"
    }

    module-whatis "sets up environment for CUDA 11.8"

    if { [ is-loaded cuda/11.3 ] } {
    module unload cuda/11.3
    }

    set version 11.8
    set root /usr/local/cuda-11.8
    setenv CUDA_HOME	$root

    prepend-path PATH $root/bin
    prepend-path LD_LIBRARY_PATH $root/extras/CUPTI/lib64
    prepend-path LD_LIBRARY_PATH $root/lib64
    conflict cuda
    ```

### c) Make `CUDA 11.8` the default cuda version
- Create a file `/usr/share/modules/modulefiles/cuda.version` to make `CUDA 11.8` the default cuda module:
    ```bash
    #%Module
    set ModulesVersion 11.8
    ```

> **Note**: make sure to reload your terminal.
## 5. Changing and Viewing the CUDA Module
- To change and view the loaded CUDA module, you can use the following commands:
    ```bash
    # Check the currently loaded module
    module list
    # Check the available modules
    module avail
    
    # Load a specific cuda version
    module load cuda/11.3
    # Unload the currently loaded CUDA module
    module unload cuda
    # Load CUDA 11.8
    module load cuda/11.8
    
    # verify the paths of the loaded CUDA
    nvcc --version # should give the loaded CUDA version
    echo $CUDA_HOME
    echo $PATH
    echo $LD_LIBRARY_PATH
    ```
> **Note**: You can add additional `CUDA` versions or other packages by creating corresponding modulefiles and following the steps outlined in this gist.