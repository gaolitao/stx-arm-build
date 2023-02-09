## Setup Debian build env on ARM server and the build results

[TOC]

### Create user for the build

To create new users:
```
sudo su
adduser <username>
usermod -aG sudo <username>
```

### Setup build env

* Referenc doc: https://wiki.openstack.org/wiki/StarlingX/DebianBuildEnvironment

#### 1. Install Docker

* Install Docker Engine:
  * ref: https://docs.docker.com/engine/install/debian/

```
sudo apt-get remove docker docker.io containerd runc

sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo docker run hello-world
```

* Add user to docker group
```
sudo usermod -aG docker $(id -un) && newgrp docker
```

#### 2. Install repo, helm and minikube

```
export PATH=~/bin:$PATH
mkdir ~/bin; cd ~/bin

# Install repo
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod +x repo

# Install minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-arm64
chmod +x minikube-linux-arm64
ln -s minikube-linux-arm64 minikube

# Install helm
curl -LO https://get.helm.sh/helm-v3.6.2-linux-arm64.tar.gz
tar xvf helm-v3.6.2-linux-arm64.tar.gz
mv linux-arm64/helm ~/bin
rm -rf helm-v3.6.2-linux-arm64.tar.gz linux-arm64
```

#### 3. Export environment variables
```
mkdir -p ~/stx-arm-build-2023/ws-stx-arm64-20230209
cd ~/stx-arm-build-2023/ws-stx-arm64-20230209

WORKSPACE=`pwd`

cat << EOF >> env.prj-stx-deb-$USER

export WORKSPACE=$WORKSPACE
export STX_BUILD_HOME=$WORKSPACE
export PROJECT=prj-stx-deb-$USER
export STX_MIRROR_DIR=$WORKSPACE/mirrors
export STX_REPO_ROOT=$WORKSPACE/src
export STX_REPO_ROOT_SUBDIR="localdisk/designer/$USER/prj-stx-deb-$USER"

export USER_NAME=$USER
export USER_EMAIL=$USER@windriver.com

# MINIKUBE
export STX_PLATFORM="minikube"
export STX_MINIKUBENAME="minikube-$USER"
export MINIKUBE_HOME=$WORKSPACE/minikube_home

# Manifest/Repo Options:
export STX_MANIFEST_URL="https://opendev.org/starlingx/manifest"
export STX_MANIFEST_BRANCH="master"
export STX_MANIFEST="default.xml"

EOF

source env.prj-stx-deb-$USER
```

#### 4. Initialize repo

```
mkdir -p $STX_REPO_ROOT_SUBDIR
ln -s $STX_REPO_ROOT_SUBDIR $STX_REPO_ROOT

mkdir -p $STX_MIRROR_DIR
mkdir -p $MINIKUBE_HOME

cd $STX_REPO_ROOT

git config --global user.name "Firstname Lastname"
git config --global user.email "Your email"
git config --global core.editor vi

# download and sync the repos
repo init -u ${STX_MANIFEST_URL} -b ${STX_MANIFEST_BRANCH} -m ${STX_MANIFEST}
repo sync
```

#### 5. Apply fixes and workarounds

* Fixes and workarounds for downloader
https://github.com/jackiehjm/stx-tools/compare/master...jackiehjm:stx-tools:jhuang0/20230202-build-arm64

```
cd $STX_REPO_ROOT/stx-tools
git checkout master
git fetch https://github.com/jackiehjm/stx-tools jhuang0/20230202-build-arm64
git merge FETCH_HEAD
```

* Fixes and workdournad for build-tools
https://github.com/jackiehjm/stx-cgcs-root/compare/master...jackiehjm:stx-cgcs-root:jhuang0/20230202-build-arm64

```
cd $STX_REPO_ROOT/cgcs-root
git checkout master
git fetch https://github.com/jackiehjm/stx-cgcs-root jhuang0/20230202-build-arm64
git merge FETCH_HEAD
```

* Fixes for packages:
  * https://github.com/jackiehjm/stx-integ/compare/master...jackiehjm:stx-integ:jhuang0/20230208-build-arm64
  * https://github.com/jackiehjm/stx-fault/compare/master...jackiehjm:stx-fault:jhuang0/20230208-build-arm64
  * https://github.com/jackiehjm/stx-containers/compare/master...jackiehjm:stx-containers:jhuang0/20230208-build-arm64
  
```
cd $STX_REPO_ROOT/cgcs-root/stx/integ
git checkout master
git fetch https://github.com/jackiehjm/stx-integ jhuang0/20230208-build-arm64
git merge FETCH_HEAD

cd $STX_REPO_ROOT/cgcs-root/stx/fault
git checkout master
git fetch https://github.com/jackiehjm/stx-fault jhuang0/20230208-build-arm64
git merge FETCH_HEAD

cd $STX_REPO_ROOT/cgcs-root/stx/containers
git checkout master
git fetch https://github.com/jackiehjm/stx-containers jhuang0/20230208-build-arm64
git merge FETCH_HEAD

cd $STX_REPO_ROOT/cgcs-root/stx/ha
git checkout master
git fetch https://github.com/jackiehjm/stx-ha jhuang0/20230208-build-arm64
git merge FETCH_HEAD

```

#### 6. Init and setup STX

```
# Init stx tool
cd $STX_REPO_ROOT/stx-tools
source import-stx

# Update stx config
# Align the builder container to use your user/UID
stx config --add builder.myuname $(id -un)
stx config --add builder.uid $(id -u)

# Embedded in ~/localrc of the build container
stx config --add project.gituser ${USER}
stx config --add project.gitemail ${USER_EMAIL}

# This will be included in the name of your build container and the basename for $MY_REPO_ROOT_DIR  
stx config --add project.name ${PROJECT}
 
stx config --show
```

#### 7. Create and start build containers

* Note: 
  * stx-builder, stx-pkgbuilder, and stx-aptly will be rebuild for arm64
  * stx-lat-tool (x86-64) will download from dockerhub for now
  * TODO: re-build the lat-sdk and stx-lat-tool container for arm64

```
./stx-init-env --rebuild=builder,pkgbuilder,aptly
```

#### 8. Enter Pods and do the workarounds

* workaround: enter stx-pkgbuilder and do the following changes

```
stx shell --container pkgbuilder

# Inside the stx-pkgbuilder
sed -i '/mirror.starlingx.cengn.ca/ d' /etc/sbuild/sbuild.conf
```

#### 9. Execute these mandatory steps inside the stx-builder pod:

```
sudo apt-get update
git config --global user.name "Jackie Huang"
git config --global user.email "jackie.huang@windriver.com"
```

#### 10. Download 3rd-party tar & deb files

```
downloader -b -s
```

#### 11. Build packages

```
# workaround: disable snapshot inside the stx-builder pod
unset DEBIAN_SNAPSHOT
unset DEBIAN_SECURITY_SNAPSHOT

build-pkgs -a --parallel 10
```

### Build packages results

```
2023-02-09 13:26:35,566 - debcontroller - INFO: Total std packages needing to be built: 299
2023-02-09 13:26:35,567 - debcontroller - INFO: -------------------------------------------
2023-02-09 13:26:35,567 - debcontroller - INFO: Total std packages reused from remote: 0
2023-02-09 13:26:35,567 - debcontroller - INFO: Total std packages needing to be built locally: 299
2023-02-09 13:26:35,567 - debcontroller - INFO: Successfully built: 278

2023-02-09 13:26:36,126 - debcontroller - ERROR: Failed to build: 21
2023-02-09 13:26:36,128 - debcontroller - ERROR: grub-efi
2023-02-09 13:26:36,130 - debcontroller - ERROR: grub2
2023-02-09 13:26:36,131 - debcontroller - ERROR: kpatch
2023-02-09 13:26:36,131 - debcontroller - ERROR: qemu
2023-02-09 13:26:36,132 - debcontroller - ERROR: bnxt-en
2023-02-09 13:26:36,133 - debcontroller - ERROR: i40e
2023-02-09 13:26:36,134 - debcontroller - ERROR: i40e-cvl-2.54
2023-02-09 13:26:36,135 - debcontroller - ERROR: iavf
2023-02-09 13:26:36,136 - debcontroller - ERROR: iavf-cvl-2.54
2023-02-09 13:26:36,138 - debcontroller - ERROR: ice
2023-02-09 13:26:36,139 - debcontroller - ERROR: ice-cvl-2.54
2023-02-09 13:26:36,140 - debcontroller - ERROR: igb-uio
2023-02-09 13:26:36,141 - debcontroller - ERROR: kmod-opae-fpga-driver
2023-02-09 13:26:36,142 - debcontroller - ERROR: iqvlinux
2023-02-09 13:26:36,143 - debcontroller - ERROR: mlnx-ofed-kernel
2023-02-09 13:26:36,144 - debcontroller - ERROR: qat1.7.l
2023-02-09 13:26:36,145 - debcontroller - ERROR: linux
2023-02-09 13:26:36,146 - debcontroller - ERROR: kpatch-prebuilt
2023-02-09 13:26:36,147 - debcontroller - ERROR: pxe-network-installer
2023-02-09 13:26:36,148 - debcontroller - ERROR: opae-sdk
2023-02-09 13:26:36,148 - debcontroller - ERROR: pcm

2023-02-09 13:26:36,170 - debcontroller - INFO: Total rt packages reused from remote: 0
2023-02-09 13:26:36,171 - debcontroller - INFO: Total rt packages needing to be built locally: 14
2023-02-09 13:26:36,171 - debcontroller - INFO: Successfully built in pkgbuilder: 14

2023-02-09 13:26:36,200 - debcontroller - INFO: List of failed packages:
2023-02-09 13:26:36,201 - debcontroller - ERROR: bnxt-en
2023-02-09 13:26:36,202 - debcontroller - ERROR: i40e
2023-02-09 13:26:36,203 - debcontroller - ERROR: i40e-cvl-2.54
2023-02-09 13:26:36,204 - debcontroller - ERROR: iavf
2023-02-09 13:26:36,205 - debcontroller - ERROR: iavf-cvl-2.54
2023-02-09 13:26:36,207 - debcontroller - ERROR: ice
2023-02-09 13:26:36,208 - debcontroller - ERROR: ice-cvl-2.54
2023-02-09 13:26:36,209 - debcontroller - ERROR: igb-uio
2023-02-09 13:26:36,210 - debcontroller - ERROR: kmod-opae-fpga-driver
2023-02-09 13:26:36,211 - debcontroller - ERROR: iqvlinux
2023-02-09 13:26:36,212 - debcontroller - ERROR: mlnx-ofed-kernel
2023-02-09 13:26:36,213 - debcontroller - ERROR: qat1.7.l
2023-02-09 13:26:36,214 - debcontroller - ERROR: linux-rt
2023-02-09 13:26:36,215 - debcontroller - ERROR: kpatch-prebuilt

```
