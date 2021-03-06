# Generated by Neurodocker version 0.5.0
# Timestamp: 2019-05-03 14:52:09 UTC
# 
# Thank you for using Neurodocker. If you discover any issues
# or ways to improve this software, please submit an issue or
# pull request on our GitHub repository:
# 
#     https://github.com/kaczmarj/neurodocker

Bootstrap: docker
From: neurodebian:stretch-non-free

%post
export ND_ENTRYPOINT="/neurodocker/startup.sh"
apt-get update -qq
apt-get install -y -q --no-install-recommends \
    apt-utils \
    bzip2 \
    ca-certificates \
    curl \
    locales \
    unzip
apt-get clean
rm -rf /var/lib/apt/lists/*
sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
dpkg-reconfigure --frontend=noninteractive locales
update-locale LANG="en_US.UTF-8"
chmod 777 /opt && chmod a+s /opt
mkdir -p /neurodocker
if [ ! -f "$ND_ENTRYPOINT" ]; then
  echo '#!/usr/bin/env bash' >> "$ND_ENTRYPOINT"
  echo 'set -e' >> "$ND_ENTRYPOINT"
  echo 'export USER="${USER:=`whoami`}"' >> "$ND_ENTRYPOINT"
  echo 'if [ -n "$1" ]; then "$@"; else /usr/bin/env bash; fi' >> "$ND_ENTRYPOINT";
fi
chmod -R 777 /neurodocker && chmod a+s /neurodocker

apt-get update -qq
apt-get install -y -q --no-install-recommends \
    bc \
    libgomp1 \
    libxmu6 \
    libxt6 \
    perl \
    tcsh
apt-get clean
rm -rf /var/lib/apt/lists/*
echo "Downloading FreeSurfer ..."
mkdir -p /opt/freesurfer-6.0.0
curl -fsSL --retry 5 ftp://surfer.nmr.mgh.harvard.edu/pub/dist/freesurfer/6.0.0/freesurfer-Linux-centos6_x86_64-stable-pub-v6.0.0.tar.gz \
| tar -xz -C /opt/freesurfer-6.0.0 --strip-components 1 \
  --exclude='freesurfer/average/mult-comp-cor' \
  --exclude='freesurfer/lib/cuda' \
  --exclude='freesurfer/lib/qt' \
  --exclude='freesurfer/subjects/V1_average' \
  --exclude='freesurfer/subjects/bert' \
  --exclude='freesurfer/subjects/cvs_avg35' \
  --exclude='freesurfer/subjects/cvs_avg35_inMNI152' \
  --exclude='freesurfer/subjects/fsaverage3' \
  --exclude='freesurfer/subjects/fsaverage4' \
  --exclude='freesurfer/subjects/fsaverage5' \
  --exclude='freesurfer/subjects/fsaverage6' \
  --exclude='freesurfer/subjects/fsaverage_sym' \
  --exclude='freesurfer/trctrain'
sed -i '$isource "/opt/freesurfer-6.0.0/SetUpFreeSurfer.sh"' "$ND_ENTRYPOINT"

apt-get update -qq
apt-get install -y -q --no-install-recommends \
    fsl \
    git \
    num-utils
apt-get clean
rm -rf /var/lib/apt/lists/*

sed -i '$isource /etc/fsl/fsl.sh' $ND_ENTRYPOINT

export PATH="/opt/miniconda-latest/bin:$PATH"
echo "Downloading Miniconda installer ..."
conda_installer="/tmp/miniconda.sh"
curl -fsSL --retry 5 -o "$conda_installer" https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash "$conda_installer" -b -p /opt/miniconda-latest
rm -f "$conda_installer"
conda update -yq -nbase conda
conda config --system --prepend channels conda-forge
conda config --system --set auto_update_conda false
conda config --system --set show_channel_urls true
sync && conda clean --all && sync
conda create -y -q --name bidsonym
conda install -y -q --name bidsonym \
    'python=3.6' \
    'traits' \
    'nipype'
sync && conda clean --all && sync
bash -c "source activate bidsonym
  pip install --no-cache-dir  \
      'nibabel' \
      'pydeface' \
      'quickshear'"
rm -rf ~/.cache/pip/*
sync
sed -i '$isource activate bidsonym' $ND_ENTRYPOINT


bash -c 'git clone https://github.com/mih/mridefacer'

bash -c 'PATH=/Users/peerherholz/abin:/usr/local/antsbin/bin:/Applications/MATLAB_R2014a.app/bin:/Applications/freesurfer/bin:/Applications/freesurfer/fsfast/bin:/Applications/freesurfer/tktools:/usr/local/fsl/bin:/Applications/freesurfer/mni/bin:/usr/local/fsl/bin:/Users/peerherholz/anaconda3/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/Applications/workbench/bin_macosx64:/home/mridefacer/'

echo '{
\n  "pkg_manager": "apt",
\n  "instructions": [
\n    [
\n      "base",
\n      "neurodebian:stretch-non-free"
\n    ],
\n    [
\n      "_header",
\n      {
\n        "version": "generic",
\n        "method": "custom"
\n      }
\n    ],
\n    [
\n      "freesurfer",
\n      {
\n        "version": "6.0.0",
\n        "min": "true"
\n      }
\n    ],
\n    [
\n      "install",
\n      [
\n        "fsl",
\n        "git",
\n        "num-utils"
\n      ]
\n    ],
\n    [
\n      "add_to_entrypoint",
\n      "source /etc/fsl/fsl.sh"
\n    ],
\n    [
\n      "env",
\n      {
\n        "FSLDIR": "/usr/share/fsl/5.0",
\n        "FSLOUTPUTTYPE": "NIFTI_GZ",
\n        "FSLMULTIFILEQUIT": "TRUE",
\n        "POSSUMDIR": "/usr/share/fsl/5.0",
\n        "LD_LIBRARY_PATH": "/usr/lib/fsl/5.0:",
\n        "FSLTCLSH": "/usr/bin/tclsh",
\n        "FSLWISH": "/usr/bin/wish",
\n        "PATH": "/usr/lib/fsl/5.0:/Users/peerherholz/abin:/usr/local/antsbin/bin:/Applications/MATLAB_R2014a.app/bin:/Applications/freesurfer/bin:/Applications/freesurfer/fsfast/bin:/Applications/freesurfer/tktools:/usr/local/fsl/bin:/Applications/freesurfer/mni/bin:/usr/local/fsl/bin:/Users/peerherholz/anaconda3/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/Applications/workbench/bin_macosx64"
\n      }
\n    ],
\n    [
\n      "miniconda",
\n      {
\n        "create_env": "bidsonym",
\n        "conda_install": [
\n          "python=3.6",
\n          "traits",
\n          "nipype"
\n        ],
\n        "pip_install": [
\n          "nibabel",
\n          "pydeface",
\n          "quickshear"
\n        ],
\n        "activate": true
\n      }
\n    ],
\n    [
\n      "run_bash",
\n      "git clone https://github.com/mih/mridefacer"
\n    ],
\n    [
\n      "env",
\n      {
\n        "MRIDEFACER_DATA_DIR": "/home/mirdefacer/data"
\n      }
\n    ],
\n    [
\n      "run_bash",
\n      "PATH=/Users/peerherholz/abin:/usr/local/antsbin/bin:/Applications/MATLAB_R2014a.app/bin:/Applications/freesurfer/bin:/Applications/freesurfer/fsfast/bin:/Applications/freesurfer/tktools:/usr/local/fsl/bin:/Applications/freesurfer/mni/bin:/usr/local/fsl/bin:/Users/peerherholz/anaconda3/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/Applications/workbench/bin_macosx64:/home/mridefacer/"
\n    ],
\n    [
\n      "copy",
\n      [
\n        "./run.py",
\n        "/home/run.py"
\n      ]
\n    ],
\n    [
\n      "copy",
\n      [
\n        "./version",
\n        "/home/version"
\n      ]
\n    ],
\n    [
\n      "copy",
\n      [
\n        "./fs_data",
\n        "/home/fs_data"
\n      ]
\n    ]
\n  ]
\n}' > /neurodocker/neurodocker_specs.json

%environment
export LANG="en_US.UTF-8"
export LC_ALL="en_US.UTF-8"
export ND_ENTRYPOINT="/neurodocker/startup.sh"
export FREESURFER_HOME="/opt/freesurfer-6.0.0"
export PATH="/opt/freesurfer-6.0.0/bin:$PATH"
export FSLDIR="/usr/share/fsl/5.0"
export FSLOUTPUTTYPE="NIFTI_GZ"
export FSLMULTIFILEQUIT="TRUE"
export POSSUMDIR="/usr/share/fsl/5.0"
export LD_LIBRARY_PATH="/usr/lib/fsl/5.0:"
export FSLTCLSH="/usr/bin/tclsh"
export FSLWISH="/usr/bin/wish"
export PATH="/usr/lib/fsl/5.0:/Users/peerherholz/abin:/usr/local/antsbin/bin:/Applications/MATLAB_R2014a.app/bin:/Applications/freesurfer/bin:/Applications/freesurfer/fsfast/bin:/Applications/freesurfer/tktools:/usr/local/fsl/bin:/Applications/freesurfer/mni/bin:/usr/local/fsl/bin:/Users/peerherholz/anaconda3/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/Applications/workbench/bin_macosx64"
export CONDA_DIR="/opt/miniconda-latest"
export PATH="/opt/miniconda-latest/bin:$PATH"
export MRIDEFACER_DATA_DIR="/home/mirdefacer/data"

%files
./run.py /home/run.py
./version /home/version
./fs_data /home/fs_data

%runscript
/neurodocker/startup.sh "$@"
