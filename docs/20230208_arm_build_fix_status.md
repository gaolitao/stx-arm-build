# StarlingX ARM build fix and status

## Failed packages and fix status

| #    | STD/RT | Pkg name                            | Owner  | Status      | Comment                                            |
| ---- | ------ | ----------------------------------- | ------ | ----------- | -------------------------------------------------- |
| 1    | STD    | stx-sdo-helm                        | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 2    | STD    | istio-helm                          | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 3    | STD    | kiali-helm                          | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 4    | STD    | stx-istio-helm                      | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 5    | STD    | stx-kubevirt-app-helm               | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 6    | STD    | stx-oran-o2-helm                    | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 7    | STD    | stx-security-profiles-operator-helm | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 8    | STD    | stx-sriov-fec-operator-helm         | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 9    | STD    | stx-sts-silicom-helm                | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 10   | STD    | stx-audit-helm                      | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 11   | STD    | stx-cert-manager-helm               | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 12   | STD    | registry-token-server               | jackie | [Fixed][12] |                                                    |
| 13   | STD    | fm-mgr                              | jackie | [Fixed][13] |                                                    |
| 14   | STD    | sm                                  | jackie | Fixed       | Pass after 15 fixed                                |
| 15   | STD    | sm-common                           | jackie | [Fixed][15] |                                                    |
| 16   | STD    | sm-db                               | jackie | Fixed       | Pass after 15 fixed                                |
| 17   | STD    | gpu-operator                        | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 18   | STD    | grub-efi                            | jackie | [Fixed][19] |                                                    |
| 19   | STD    | grub2                               | jackie | [Fixed][19] |                                                    |
| 20   | STD    | armada                              | jackie | Fixed       | Pass after 21, 22, 24 fixed                        |
| 21   | STD    | armada-helm-toolkit                 | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 22   | STD    | chartmuseum                         | jackie | [Fixed][22] | Ver upgraded 0.12.0 -> 0.13.0, not sure the impact |
| 23   | STD    | crictl                              | jackie | [Fixed][23] |                                                    |
| 24   | STD    | helm                                | jackie | [Fixed][24] |                                                    |
| 25   | STD    | kubernetes-1.21.8                   | jackie | [Fixed][25] |                                                    |
| 26   | STD    | kubernetes-1.22.5                   | jackie | [Fixed][25] |                                                    |
| 27   | STD    | kubernetes-1.23.1                   | jackie | [Fixed][25] |                                                    |
| 28   | STD    | kubernetes-1.24.4                   | jackie | [Fixed][25] |                                                    |
| 29   | STD    | kubectl-cert-manager                | jackie | [Fixed][29] |                                                    |
| 30   | STD    | kpatch                              | litao  | Checked     | Out of scope [arm64 not supported][52]             |
| 31   | STD    | qemu                                | jackie |             | Should be able to remove                           |
| 32   | STD    | bnxt-en                             |        |             |                                                    |
| 33   | STD    | i40e                                |        |             |                                                    |
| 34   | STD    | i40e-cvl-2.54                       |        |             |                                                    |
| 35   | STD    | iavf                                |        |             |                                                    |
| 36   | STD    | iavf-cvl-2.54                       |        |             |                                                    |
| 37   | STD    | ice                                 |        |             |                                                    |
| 38   | STD    | ice-cvl-2.54                        |        |             |                                                    |
| 39   | STD    | igb-uio                             |        |             |                                                    |
| 40   | STD    | kmod-opae-fpga-driver               |        |             | Should be able to remove                           |
| 41   | STD    | iqvlinux                            |        |             |                                                    |
| 42   | STD    | mlnx-ofed-kernel                    |        |             |                                                    |
| 43   | STD    | qat1.7.l                            |        |             | Should be able to remove                           |
| 44   | STD    | linux                               | jackie | [Fixed][44] |                                                    |
| 45   | STD    | kpatch-prebuilt                     | litao  | Checked     | Out of scope [arm64 not supported][52]             |
| 46   | STD    | pxe-network-installer               | jackie | [Fixed][46] |                                                    |
| 47   | STD    | mtce                                | jackie | Fixed       | Pass after 13 fixed                                |
| 48   | STD    | mtce-common                         | jackie | Fixed       | Pass after 13 fixed                                |
| 49   | STD    | metrics-server-helm                 | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 50   | STD    | stx-metrics-server-helm             | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 51   | STD    | monitor-helm                        | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 52   | STD    | monitor-helm-elastic                | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 53   | STD    | stx-monitor-helm                    | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 54   | STD    | stx-nginx-ingress-controller-helm   | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 55   | STD    | stx-oidc-auth-helm                  | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 56   | STD    | openstack-helm                      | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 57   | STD    | openstack-helm-infra                | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 58   | STD    | stx-openstack-helm                  | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 59   | STD    | stx-openstack-helm-fluxcd           | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 60   | STD    | platform-helm                       | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 61   | STD    | stx-platform-helm                   | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 62   | STD    | portieris-helm                      | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 63   | STD    | stx-portieris-helm                  | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 64   | STD    | stx-ptp-notification-helm           | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 65   | STD    | stx-snmp-helm                       | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 66   | STD    | opae-sdk                            | jackie | Removed     | Should be able to remove                           |
| 67   | STD    | pcm                                 | litao  | Removed     | Out of scope, [intel cpu only][50], [STX vRAN][51] |
| 68   | STD    | stx-vault-helm                      | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 69   | STD    | vault-helm                          | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 70   | RT     | bnxt-en                             | jackie | Removed     |                                                    |
| 71   | RT     | i40e                                | jackie | Removed     |                                                    |
| 72   | RT     | i40e-cvl-2.54                       | jackie | Removed     |                                                    |
| 73   | RT     | iavf                                | jackie | Removed     |                                                    |
| 74   | RT     | iavf-cvl-2.54                       | jackie | Removed     |                                                    |
| 75   | RT     | ice                                 | jackie | Removed     |                                                    |
| 76   | RT     | ice-cvl-2.54                        | jackie | Removed     |                                                    |
| 77   | RT     | igb-uio                             | jackie | Removed     |                                                    |
| 78   | RT     | kmod-opae-fpga-driver               | jackie | Removed     |                                                    |
| 79   | RT     | iqvlinux                            | jackie | Removed     |                                                    |
| 80   | RT     | mlnx-ofed-kernel                    | jackie | Removed     |                                                    |
| 81   | RT     | qat1.7.l                            | jackie | Removed     |                                                    |
| 82   | RT     | linux-rt                            | jackie | Removed     |                                                    |
| 83   | RT     | kpatch-prebuilt                     | jackie | Removed     |                                                    |

## Fixes

* https://github.com/jackiehjm/stx-cgcs-root/compare/master...jackiehjm:stx-cgcs-root:jhuang0/20230202-build-arm64
* https://github.com/jackiehjm/stx-tools/compare/master...jackiehjm:stx-tools:jhuang0/20230202-build-arm64
* https://github.com/jackiehjm/stx-integ/compare/master...jackiehjm:stx-integ:jhuang0/20230208-build-arm64
* https://github.com/jackiehjm/stx-fault/compare/master...jackiehjm:stx-fault:jhuang0/20230208-build-arm64
* https://github.com/jackiehjm/stx-containers/compare/master...jackiehjm:stx-containers:jhuang0/20230208-build-arm64
* https://github.com/jackiehjm/stx-ha/compare/master...jackiehjm:stx-ha:jhuang0/20230208-build-arm64
* https://github.com/jackiehjm/stx-kernel/compare/master...jackiehjm:stx-kernel:jhuang0/20230208-build-arm64
* https://github.com/jackiehjm/stx-metal/compare/master-20230214...jackiehjm:stx-metal:jhuang0/20230214-build-arm64

[12]: https://github.com/jackiehjm/stx-containers/commit/c5363249cca07f76cdd6959d2f8471a7f4739cc9
[15]: https://github.com/jackiehjm/stx-ha/compare/master...jackiehjm:stx-ha:jhuang0/20230208-build-arm64
[13]: https://github.com/jackiehjm/stx-fault/commit/8ba2dfdfd711531d0b09454c655889910d310902
[19]: https://github.com/jackiehjm/stx-integ/commit/ba578c673cdd7679050fe64cf8031b746fb4502a
[22]: https://github.com/jackiehjm/stx-integ/commit/c46be4067969e938fe572f86504e801e09c030d2
[23]: https://github.com/jackiehjm/stx-integ/commit/dd67a23edbd63375cb8c5b85690c8c866ccb1710
[24]: https://github.com/jackiehjm/stx-integ/commit/00e4140b92b06ece1c4cc4441afee9df55c2be9d
[25]: https://github.com/jackiehjm/stx-integ/commit/a27b2651d19e4e0c0862a237544b85d911b327ca
[29]: https://github.com/jackiehjm/stx-integ/commit/8fc3b3ef234bfd6b295e7ea5db8f2274917e1766
[44]: https://github.com/jackiehjm/stx-kernel/commit/c408447cb9206e12343bc98746cc3999b278c130
[46]: https://github.com/jackiehjm/stx-metal/commit/8fd372b2c6b771b9f1f34f7e12cd435110cfbfbc

[50]: https://github.com/intel/pcm
[51]: https://docs.starlingx.io/usertasks/kubernetes/vran-tools-2c3ee49f4b0b.html
[52]: https://github.com/dynup/kpatch/#supported-architectures

### workarounds for build-image

* https://github.com/jackiehjm/stx-cgcs-root/compare/master-20230213...jackiehjm:stx-cgcs-root:jhuang0/20230213-build-arm64
* https://github.com/jackiehjm/stx-tools/compare/master-20230202...jackiehjm:stx-tools:jhuang0/20230213-build-arm64
* https://github.com/jackiehjm/stx-kernel/compare/master...jackiehjm:stx-kernel:jhuang0/20230213-build-arm64-temp
* https://github.com/jackiehjm/stx-integ/compare/master...jackiehjm:stx-integ:jhuang0/20230213-build-arm64-temp
* https://github.com/jackiehjm/stx-metal/compare/master-20230214...jackiehjm:stx-metal:jhuang0/20230214-build-arm64


## Test Results

### Results for 2023-02-09

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

### Results for 2023-02-02

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

```
