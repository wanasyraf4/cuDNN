![image](https://github.com/wanasyraf4/cuDNN/assets/107595740/a8d00f6d-0e53-4c85-af44-4cd74f3406a2)

# cuDNN setup for WSL and ubuntu
first for cuDNN, the step u provided is ok, but here my updated one
(note that this is done on ubuntu 22.04 LTS)

## A. Downloading cuDNN for Linux
In order to download cuDNN, ensure you are registered for the NVIDIA Developer Program.
1. Go to: NVIDIA cuDNN home page.
2. Click Download.
3. Complete the short survey and click Submit.
4. Accept the Terms and Conditions. A list of available download versions of cuDNN displays.
Select the cuDNN version that you want to install. A list of available resources displays.

## B. extract the zip
1. Navigate to your directory containing the cuDNN tar file (usually on download)
2. run on terminal: 
> tar -xvf cudnn-linux-x86_64-8.x.x.x_cudaX.Y-archive.tar.xz

(note that you must replace X.Y and v8.x.x.x with your specific CUDA and cuDNN versions and package date. In my case, im using cuda 11.8 and cudnn 8.2.9 so it become:

> tar -xvf cudnn-linux-x86_64-8.2.9.x_cuda11.8-archive.tar.xz

## C. see cuda path, cuz sometimes some peps hab more than one cuda compiler
1. in terminal type 
> cd /usr/local/
2. type 
> ls
3. see if cuda only, use: cd /usr/local/cuda or if cuda 11.x use : cd /usr/local/cuda11.x
4. go back to extracted file place and copy the required file (note that example show below is for cuda folder, if cuda-11x, rewrite as /local/cuda-11x
> sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda/include 
> sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda/lib64 
> sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*

## D. update keyring
1. The new GPG public key for the CUDA repository is 3bf863cc. This must be enrolled on the system, either using the cuda-keyring package or manually; the apt-key command is deprecated and not recommended.
2. install new cuda-keyring package

> wget https://developer.download.nvidia.com/compute/cuda/repos/$distro/$arch/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb

where $distro/$arch should be replaced by one of the following:

i. ubuntu1604/x86_64

ii. ubuntu1804/cross-linux-sbsa

iii. ubuntu1804/ppc64el

iv. ubuntu1804/sbsa

v. ubuntu1804/x86_64

vi. ubuntu2004/cross-linux-sbsa

vii. ubuntu2004/sbsa

viii. ubuntu2004/x86_64

ix. ubuntu2204/sbsa

x. ubuntu2204/x86_64

3. install keyring
> sudo dpkg -i cuda-keyring_1.0-1_all.deb

4. delete old keyring with
sudo apt-key del 7fa2af80

5. update apt repo cache
sudo apt-get update

## E. Installing cuDNN library and cuDNN samples:
1. run this on terminal (one by one to prevent error, seriously)
> sudo apt-get install libcudnn8=${cudnn_version}-1+${cuda_version}
> sudo apt-get install libcudnn8-dev=${cudnn_version}-1+${cuda_version}
> sudo apt-get install libcudnn8-samples=${cudnn_version}-1+${cuda_version}

Where:
${cudnn_version} is 8.9.2.* (not 8.9.2 only, dont forget the *)
${cuda_version} is cuda12.1 or cuda11.8 (must write cuda12.1, not left with 12.1 or 11.8)

2. install the libcudnn-dev 
> sudo apt-get install libcudnn8-dev

3. install the libfreeimage3 for testing mnistCUDNN sample
> sudo apt-get install libfreeimage3 libfreeimage-dev

4. to verify install, lets cipy cuDNN samples to writable path
> cp -r /usr/src/cudnn_samples_v8/ $HOME

5. go to writable path
> cd  $HOME/cudnn_samples_v8/mnistCUDNN

6. Compile the mnistCUDNN sample
> make clean && make

7. Run the mnistCUDNN sample
./mnistCUDNN

the test will be run and if successful, it will output: 

> Test passed!

