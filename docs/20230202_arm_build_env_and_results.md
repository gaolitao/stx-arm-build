## Setup Debian build env on ARM server and the build results

[TOC]

### Server Info

* IP: 147.11.94.20
* user/pass: jackie/Li69nux*

To create new users for other desingers:
```
sudo su
adduser <username>
usermod -aG sudo <username>
```

### Setup build env

* Referenc doc: https://wiki.openstack.org/wiki/StarlingX/DebianBuildEnvironment

#### 1. Docker

* Docker is already installed
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
mkdir -p ~/stx-arm-build-2023/ws-stx-arm64-20230202
cd ~/stx-arm-build-2023/ws-stx-arm64-20230202

cat << EOF >> env.prj-stx-deb-$USER

export WORKSPACE=`pwd`
export STX_BUILD_HOME=$WORKSPACE
export PROJECT=prj-stx-deb-$USER
export STX_MIRROR_DIR=$WORKSPACE/mirrors
export STX_REPO_ROOT=$WORKSPACE/src
export STX_REPO_ROOT_SUBDIR="localdisk/designer/$USER/$PROJECT"

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
https://github.com/starlingx/tools/compare/master...jackiehjm:stx-tools:jhuang0/20230202-build-arm64?expand=1

```
cd $STX_REPO_ROOT/stx-tools
git fetch https://github.com/jackiehjm/stx-tools jhuang0/20230202-build-arm64
git merge FETCH_HEAD
```

* Fixes and workdournad for build-tools
https://github.com/starlingx/root/compare/master...jackiehjm:stx-cgcs-root:jhuang0/20230202-build-arm64?expand=1

```
cd $STX_REPO_ROOT/cgcs-root
git fetch https://github.com/jackiehjm/stx-cgcs-root jhuang0/20230202-build-arm64
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
2023-02-02 20:44:58,081 - debcontroller - INFO: Total std packages needing to be built: 298
2023-02-02 20:44:58,081 - debcontroller - INFO: -------------------------------------------
2023-02-02 20:44:58,081 - debcontroller - INFO: Total std packages reused from remote: 0
2023-02-02 20:44:58,082 - debcontroller - INFO: Total std packages needing to be built locally: 298
2023-02-02 20:44:58,082 - debcontroller - INFO: Successfully built: 229

2023-02-02 20:44:58,551 - debcontroller - ERROR: Failed to build: 69
2023-02-02 20:44:58,552 - debcontroller - ERROR: stx-sdo-helm
2023-02-02 20:44:58,553 - debcontroller - ERROR: istio-helm
2023-02-02 20:44:58,555 - debcontroller - ERROR: kiali-helm
2023-02-02 20:44:58,556 - debcontroller - ERROR: stx-istio-helm
2023-02-02 20:44:58,557 - debcontroller - ERROR: stx-kubevirt-app-helm
2023-02-02 20:44:58,558 - debcontroller - ERROR: stx-oran-o2-helm
2023-02-02 20:44:58,559 - debcontroller - ERROR: stx-security-profiles-operator-helm
2023-02-02 20:44:58,560 - debcontroller - ERROR: stx-sriov-fec-operator-helm
2023-02-02 20:44:58,561 - debcontroller - ERROR: stx-sts-silicom-helm
2023-02-02 20:44:58,562 - debcontroller - ERROR: stx-audit-helm
2023-02-02 20:44:58,564 - debcontroller - ERROR: stx-cert-manager-helm
2023-02-02 20:44:58,567 - debcontroller - ERROR: registry-token-server
2023-02-02 20:44:58,567 - debcontroller - ERROR: fm-mgr
2023-02-02 20:44:58,568 - debcontroller - ERROR: sm
2023-02-02 20:44:58,569 - debcontroller - ERROR: sm-common
2023-02-02 20:44:58,570 - debcontroller - ERROR: sm-db
2023-02-02 20:44:58,571 - debcontroller - ERROR: gpu-operator
2023-02-02 20:44:58,573 - debcontroller - ERROR: grub-efi
2023-02-02 20:44:58,574 - debcontroller - ERROR: grub2
2023-02-02 20:44:58,575 - debcontroller - ERROR: armada
2023-02-02 20:44:58,576 - debcontroller - ERROR: armada-helm-toolkit
2023-02-02 20:44:58,578 - debcontroller - ERROR: chartmuseum
2023-02-02 20:44:58,579 - debcontroller - ERROR: crictl
2023-02-02 20:44:58,581 - debcontroller - ERROR: helm
2023-02-02 20:44:58,582 - debcontroller - ERROR: kubernetes-1.21.8
2023-02-02 20:44:58,583 - debcontroller - ERROR: kubernetes-1.22.5
2023-02-02 20:44:58,584 - debcontroller - ERROR: kubernetes-1.23.1
2023-02-02 20:44:58,585 - debcontroller - ERROR: kubernetes-1.24.4
2023-02-02 20:44:58,586 - debcontroller - ERROR: kubectl-cert-manager
2023-02-02 20:44:58,587 - debcontroller - ERROR: kpatch
2023-02-02 20:44:58,588 - debcontroller - ERROR: qemu
2023-02-02 20:44:58,589 - debcontroller - ERROR: bnxt-en
2023-02-02 20:44:58,590 - debcontroller - ERROR: i40e
2023-02-02 20:44:58,591 - debcontroller - ERROR: i40e-cvl-2.54
2023-02-02 20:44:58,592 - debcontroller - ERROR: iavf
2023-02-02 20:44:58,593 - debcontroller - ERROR: iavf-cvl-2.54
2023-02-02 20:44:58,594 - debcontroller - ERROR: ice
2023-02-02 20:44:58,595 - debcontroller - ERROR: ice-cvl-2.54
2023-02-02 20:44:58,596 - debcontroller - ERROR: igb-uio
2023-02-02 20:44:58,597 - debcontroller - ERROR: kmod-opae-fpga-driver
2023-02-02 20:44:58,598 - debcontroller - ERROR: iqvlinux
2023-02-02 20:44:58,599 - debcontroller - ERROR: mlnx-ofed-kernel
2023-02-02 20:44:58,600 - debcontroller - ERROR: qat1.7.l
2023-02-02 20:44:58,602 - debcontroller - ERROR: linux
2023-02-02 20:44:58,602 - debcontroller - ERROR: kpatch-prebuilt
2023-02-02 20:44:58,603 - debcontroller - ERROR: pxe-network-installer
2023-02-02 20:44:58,604 - debcontroller - ERROR: mtce
2023-02-02 20:44:58,604 - debcontroller - ERROR: mtce-common
2023-02-02 20:44:58,605 - debcontroller - ERROR: metrics-server-helm
2023-02-02 20:44:58,606 - debcontroller - ERROR: stx-metrics-server-helm
2023-02-02 20:44:58,607 - debcontroller - ERROR: monitor-helm
2023-02-02 20:44:58,608 - debcontroller - ERROR: monitor-helm-elastic
2023-02-02 20:44:58,609 - debcontroller - ERROR: stx-monitor-helm
2023-02-02 20:44:58,610 - debcontroller - ERROR: stx-nginx-ingress-controller-helm
2023-02-02 20:44:58,611 - debcontroller - ERROR: stx-oidc-auth-helm
2023-02-02 20:44:58,612 - debcontroller - ERROR: openstack-helm
2023-02-02 20:44:58,614 - debcontroller - ERROR: openstack-helm-infra
2023-02-02 20:44:58,614 - debcontroller - ERROR: stx-openstack-helm
2023-02-02 20:44:58,615 - debcontroller - ERROR: stx-openstack-helm-fluxcd
2023-02-02 20:44:58,616 - debcontroller - ERROR: platform-helm
2023-02-02 20:44:58,617 - debcontroller - ERROR: stx-platform-helm
2023-02-02 20:44:58,618 - debcontroller - ERROR: portieris-helm
2023-02-02 20:44:58,619 - debcontroller - ERROR: stx-portieris-helm
2023-02-02 20:44:58,620 - debcontroller - ERROR: stx-ptp-notification-helm
2023-02-02 20:44:58,621 - debcontroller - ERROR: stx-snmp-helm
2023-02-02 20:44:58,622 - debcontroller - ERROR: opae-sdk
2023-02-02 20:44:58,623 - debcontroller - ERROR: pcm
2023-02-02 20:44:58,623 - debcontroller - ERROR: stx-vault-helm
2023-02-02 20:44:58,625 - debcontroller - ERROR: vault-helm

2023-02-02 20:44:58,697 - debcontroller - INFO: For the failure reason, you can check with:
2023-02-02 20:44:58,697 - debcontroller - INFO: 'cat /localdisk/builder.log | grep ERROR' or
2023-02-02 20:44:58,697 - debcontroller - INFO: 'cat ${MY_WORKSPACE}/<std or rt>/<Failed package>/*.build'
2023-02-02 20:44:58,697 - debcontroller - INFO: Total rt packages needing to be built: 14
2023-02-02 20:44:58,697 - debcontroller - INFO: -------------------------------------------
2023-02-02 20:44:58,698 - debcontroller - INFO: Total rt packages reused from remote: 0
2023-02-02 20:44:58,698 - debcontroller - INFO: Total rt packages needing to be built locally: 14
2023-02-02 20:44:58,698 - debcontroller - ERROR: Failed to build: 14
2023-02-02 20:44:58,699 - debcontroller - ERROR: bnxt-en
2023-02-02 20:44:58,700 - debcontroller - ERROR: i40e
2023-02-02 20:44:58,701 - debcontroller - ERROR: i40e-cvl-2.54
2023-02-02 20:44:58,702 - debcontroller - ERROR: iavf
2023-02-02 20:44:58,703 - debcontroller - ERROR: iavf-cvl-2.54
2023-02-02 20:44:58,704 - debcontroller - ERROR: ice
2023-02-02 20:44:58,705 - debcontroller - ERROR: ice-cvl-2.54
2023-02-02 20:44:58,706 - debcontroller - ERROR: igb-uio
2023-02-02 20:44:58,707 - debcontroller - ERROR: kmod-opae-fpga-driver
2023-02-02 20:44:58,708 - debcontroller - ERROR: iqvlinux
2023-02-02 20:44:58,709 - debcontroller - ERROR: mlnx-ofed-kernel
2023-02-02 20:44:58,710 - debcontroller - ERROR: qat1.7.l
2023-02-02 20:44:58,711 - debcontroller - ERROR: linux-rt
2023-02-02 20:44:58,712 - debcontroller - ERROR: kpatch-prebuilt
2023-02-02 20:44:58,712 - debcontroller - INFO: List of failed packages:
2023-02-02 20:44:58,713 - debcontroller - ERROR: bnxt-en
2023-02-02 20:44:58,714 - debcontroller - ERROR: i40e
2023-02-02 20:44:58,715 - debcontroller - ERROR: i40e-cvl-2.54
2023-02-02 20:44:58,716 - debcontroller - ERROR: iavf
2023-02-02 20:44:58,717 - debcontroller - ERROR: iavf-cvl-2.54
2023-02-02 20:44:58,718 - debcontroller - ERROR: ice
2023-02-02 20:44:58,719 - debcontroller - ERROR: ice-cvl-2.54
2023-02-02 20:44:58,720 - debcontroller - ERROR: igb-uio
2023-02-02 20:44:58,721 - debcontroller - ERROR: kmod-opae-fpga-driver
2023-02-02 20:44:58,722 - debcontroller - ERROR: iqvlinux
2023-02-02 20:44:58,723 - debcontroller - ERROR: mlnx-ofed-kernel
2023-02-02 20:44:58,724 - debcontroller - ERROR: qat1.7.l
2023-02-02 20:44:58,725 - debcontroller - ERROR: linux-rt
2023-02-02 20:44:58,726 - debcontroller - ERROR: kpatch-prebuilt
2023-02-02 20:44:58,726 - debcontroller - INFO: For the failure reason, you can check with:
2023-02-02 20:44:58,726 - debcontroller - INFO: 'cat /localdisk/builder.log | grep ERROR' or
2023-02-02 20:44:58,726 - debcontroller - INFO: 'cat ${MY_WORKSPACE}/<std or rt>/<Failed package>/*.build'
2023-02-02 20:44:58,726 - debcontroller - INFO: build-pkgs done

```
