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
| 14   | STD    | sm                                  | jackie | In-progress |                                                    |
| 15   | STD    | sm-common                           | jackie | In-progress |                                                    |
| 16   | STD    | sm-db                               | jackie | In-progress |                                                    |
| 17   | STD    | gpu-operator                        |        |             | The helm charts are outdated, should be removed.   |
| 18   | STD    | grub-efi                            |        |             |                                                    |
| 19   | STD    | grub2                               |        |             |                                                    |
| 20   | STD    | armada                              | jackie | Fxied       | Pass after 21, 22, 24 fixed                        |
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
| 31   | STD    | qemu                                |        |             |                                                    |
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
| 44   | STD    | linux                               |        |             |                                                    |
| 45   | STD    | kpatch-prebuilt                     | litao  | Checked     | Out of scope [arm64 not supported][52]             |
| 46   | STD    | pxe-network-installer               |        |             |                                                    |
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
| 66   | STD    | opae-sdk                            |        |             | Should be able to remove                           |
| 67   | STD    | pcm                                 | litao  | Checked     | Out of scope, [intel cpu only][50], [STX vRAN][51] |
| 68   | STD    | stx-vault-helm                      | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 69   | STD    | vault-helm                          | jackie | Fixed       | Pass after 22, 24 fixed                            |
| 70   | RT     | bnxt-en                             |        |             |                                                    |
| 71   | RT     | i40e                                |        |             |                                                    |
| 72   | RT     | i40e-cvl-2.54                       |        |             |                                                    |
| 73   | RT     | iavf                                |        |             |                                                    |
| 74   | RT     | iavf-cvl-2.54                       |        |             |                                                    |
| 75   | RT     | ice                                 |        |             |                                                    |
| 76   | RT     | ice-cvl-2.54                        |        |             |                                                    |
| 77   | RT     | igb-uio                             |        |             |                                                    |
| 78   | RT     | kmod-opae-fpga-driver               |        |             |                                                    |
| 79   | RT     | iqvlinux                            |        |             |                                                    |
| 80   | RT     | mlnx-ofed-kernel                    |        |             |                                                    |
| 81   | RT     | qat1.7.l                            |        |             |                                                    |
| 82   | RT     | linux-rt                            |        |             |                                                    |
| 83   | RT     | kpatch-prebuilt                     |        |             |                                                    |

## Fixes

* https://github.com/jackiehjm/stx-integ/compare/master...jackiehjm:stx-integ:jhuang0/20230208-build-arm64
* https://github.com/jackiehjm/stx-fault/compare/master...jackiehjm:stx-fault:jhuang0/20230208-build-arm64
* https://github.com/jackiehjm/stx-containers/compare/master...jackiehjm:stx-containers:jhuang0/20230208-build-arm64

[12]: https://github.com/jackiehjm/stx-containers/commit/c5363249cca07f76cdd6959d2f8471a7f4739cc9
[13]: https://github.com/jackiehjm/stx-fault/commit/8ba2dfdfd711531d0b09454c655889910d310902
[22]: https://github.com/jackiehjm/stx-integ/commit/c46be4067969e938fe572f86504e801e09c030d2
[23]: https://github.com/jackiehjm/stx-integ/commit/dd67a23edbd63375cb8c5b85690c8c866ccb1710
[24]: https://github.com/jackiehjm/stx-integ/commit/00e4140b92b06ece1c4cc4441afee9df55c2be9d
[25]: https://github.com/jackiehjm/stx-integ/commit/a27b2651d19e4e0c0862a237544b85d911b327ca
[29]: https://github.com/jackiehjm/stx-integ/commit/8fc3b3ef234bfd6b295e7ea5db8f2274917e1766

[50]: https://github.com/intel/pcm
[51]: https://docs.starlingx.io/usertasks/kubernetes/vran-tools-2c3ee49f4b0b.html
[52]: https://github.com/dynup/kpatch/#supported-architectures


## Logs

```
jackie@prj-oran-stx-deb-stx-builder-7f6d499b7b-flws9:/localdisk/loadbuild/jackie/prj-oran-stx-deb$ build-pkgs -c --parallel 10 -p metrics-server-helm,stx-metrics-server-helm,monitor-helm,monitor-helm-elastic,stx-monitor-helm,stx-nginx-ingress-controller-helm,stx-oidc-auth-helm,openstack-helm,stx-openstack-helm,stx-openstack-helm-fluxcd,platform-helm,stx-platform-helm,portieris-helm,stx-portieris-helm,stx-ptp-notification-helm,stx-snmp-helm,stx-vault-helm,vault-helm

2023-02-08 09:50:17,185 - debcontroller - INFO: Total std packages needing to be built: 18
2023-02-08 09:50:17,185 - debcontroller - INFO: -------------------------------------------
2023-02-08 09:50:17,185 - debcontroller - INFO: Total std packages reused from remote: 0
2023-02-08 09:50:17,185 - debcontroller - INFO: Total std packages needing to be built locally: 18
2023-02-08 09:50:17,185 - debcontroller - INFO: Successfully built: 18
2023-02-08 09:50:17,186 - debcontroller - INFO: metrics-server-helm
2023-02-08 09:50:17,187 - debcontroller - INFO: stx-metrics-server-helm
2023-02-08 09:50:17,188 - debcontroller - INFO: monitor-helm
2023-02-08 09:50:17,189 - debcontroller - INFO: monitor-helm-elastic
2023-02-08 09:50:17,190 - debcontroller - INFO: stx-monitor-helm
2023-02-08 09:50:17,191 - debcontroller - INFO: stx-nginx-ingress-controller-helm
2023-02-08 09:50:17,193 - debcontroller - INFO: stx-oidc-auth-helm
2023-02-08 09:50:17,194 - debcontroller - INFO: openstack-helm
2023-02-08 09:50:17,195 - debcontroller - INFO: stx-openstack-helm
2023-02-08 09:50:17,195 - debcontroller - INFO: stx-openstack-helm-fluxcd
2023-02-08 09:50:17,196 - debcontroller - INFO: platform-helm
2023-02-08 09:50:17,197 - debcontroller - INFO: stx-platform-helm
2023-02-08 09:50:17,199 - debcontroller - INFO: portieris-helm
2023-02-08 09:50:17,200 - debcontroller - INFO: stx-portieris-helm
2023-02-08 09:50:17,201 - debcontroller - INFO: stx-ptp-notification-helm
2023-02-08 09:50:17,202 - debcontroller - INFO: stx-snmp-helm
2023-02-08 09:50:17,203 - debcontroller - INFO: stx-vault-helm
2023-02-08 09:50:17,204 - debcontroller - INFO: vault-helm
2023-02-08 09:50:17,204 - debcontroller - INFO: Total rt packages needing to be built: 0
2023-02-08 09:50:17,204 - debcontroller - INFO: -------------------------------------------
2023-02-08 09:50:17,204 - debcontroller - INFO: Total rt packages reused from remote: 0
2023-02-08 09:50:17,204 - debcontroller - INFO: Total rt packages needing to be built locally: 0
2023-02-08 09:50:17,204 - debcontroller - INFO: build-pkgs done

$ build-pkgs -c --parallel 10 -p stx-sdo-helm,istio-helm,kiali-helm,stx-istio-helm,stx-kubevirt-app-helm,stx-oran-o2-helm,stx-security-profiles-operator-helm,stx-sriov-fec-operator-helm,stx-sts-silicom-helm,stx-audit-helm,stx-cert-manager-helm

2023-02-08 09:57:04,931 - debcontroller - INFO: Total std packages needing to be built: 11
2023-02-08 09:57:04,931 - debcontroller - INFO: -------------------------------------------
2023-02-08 09:57:04,931 - debcontroller - INFO: Total std packages reused from remote: 0
2023-02-08 09:57:04,931 - debcontroller - INFO: Total std packages needing to be built locally: 11
2023-02-08 09:57:04,931 - debcontroller - INFO: Successfully built: 11
2023-02-08 09:57:04,933 - debcontroller - INFO: stx-sdo-helm
2023-02-08 09:57:04,934 - debcontroller - INFO: istio-helm
2023-02-08 09:57:04,936 - debcontroller - INFO: kiali-helm
2023-02-08 09:57:04,937 - debcontroller - INFO: stx-istio-helm
2023-02-08 09:57:04,939 - debcontroller - INFO: stx-kubevirt-app-helm
2023-02-08 09:57:04,941 - debcontroller - INFO: stx-oran-o2-helm
2023-02-08 09:57:04,942 - debcontroller - INFO: stx-security-profiles-operator-helm
2023-02-08 09:57:04,944 - debcontroller - INFO: stx-sriov-fec-operator-helm
2023-02-08 09:57:04,945 - debcontroller - INFO: stx-sts-silicom-helm
2023-02-08 09:57:04,946 - debcontroller - INFO: stx-audit-helm
2023-02-08 09:57:04,948 - debcontroller - INFO: stx-cert-manager-helm
2023-02-08 09:57:04,948 - debcontroller - INFO: Total rt packages needing to be built: 0
2023-02-08 09:57:04,949 - debcontroller - INFO: -------------------------------------------
2023-02-08 09:57:04,949 - debcontroller - INFO: Total rt packages reused from remote: 0
2023-02-08 09:57:04,949 - debcontroller - INFO: Total rt packages needing to be built locally: 0
2023-02-08 09:57:04,949 - debcontroller - INFO: build-pkgs done

```
