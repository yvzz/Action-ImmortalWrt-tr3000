# Config 差异分析

对比文件: `128m.config`, `128muboot.config`, `128muboot0.config`, `128muboot11.config`, `256m.config`

共发现 **291** 处差异。

> 图例: `Y` = 该配置项存在，`-` = 不存在

## 一、关键差异总结

| 维度 | 128m / 128muboot0 / 256m (组A) | 128muboot / 128muboot11 (组B) |
|---|---|---|
| **设备型号** | `cudy_tr3000-v1` (128m) / `cudy_tr3000-v1-256mb` (256m) | `cudy_tr3000-v1-ubootmod` |
| **TR3000-v1** | Y (128m/uboot0) / N (256m) | N |
| **TR3000-v1-256mb** | N (128m/uboot0) / Y (256m) | N |
| **TR3000-v1-ubootmod** | N | Y |
| **UBOOT固件包** | N | `trusted-firmware-a` + `u-boot` |
| **USB支持** | N | Y (kmod-usb2, usbutils, rndis) |
| **分区工具** | N | Y (blkid, blockdev, fdisk) |
| **Samba4** | N | Y (server + libs + wsdd2 + luci) |
| **vsftpd** | N | Y (FTP服务 + luci) |
| **OpenClash** | Y | N (B组 is not set) |
| **UPnP** | Y | N |
| **zoneinfo (全部时区)** | N | Y |
| **SNMP** | N (组A用的是 -nossl 变体 Y) | Y (nossl) |
| **Zabbix** | Y (组A) | N |
| **mtwifi-wapp** | N | Y |
| **rtp2httpd** | N | Y |
| **radicale** | Y (v2, 组A) | Y (v3, 组B) |

### 1. 设备目标 (`CONFIG_TARGET_PROFILE`)

| 文件 | 目标设备 |
|---|---|
| `128m.config` | `DEVICE_cudy_tr3000-v1` |
| `128muboot.config` | `DEVICE_cudy_tr3000-v1-ubootmod` |
| `128muboot0.config` | `DEVICE_cudy_tr3000-v1-ubootmod` |
| `128muboot11.config` | `DEVICE_cudy_tr3000-v1-ubootmod` |
| `256m.config` | `DEVICE_cudy_tr3000-v1-256mb` |

### 2. 分组规律

- **组A** (`128m`, `128muboot0`, `256m`): 基本一致，仅目标设备不同
  - `128m.config` → tr3000-v1 (128M闪存)
  - `128muboot0.config` → tr3000-v1-ubootmod (128M闪存)
  - `256m.config` → tr3000-v1-256mb (256M闪存)
- **组B** (`128muboot`, `128muboot11`): 完全一致，都编译 ubootmod 版，额外带 USB/Samba/FTP/zoneinfo

### 3. 128muboot0 vs 128muboot/128muboot11 差异

同为 ubootmod，但 `128muboot0` 更像组A（精简），不带 USB/Samba/FTP 等，而 `128muboot` 和 `128muboot11` 是带全套外设的"豪华版"。

## 二、完整差异表

| 配置项 | 128m | 128muboot | 128muboot0 | 128muboot11 | 256m |
|---|---|---|---|---|---|
| `# CONFIG_DBUS_VERBOSE is not set` | - | Y | - | Y | - |
| `# CONFIG_GNUTLS_CRYPTODEV is not set` | - | Y | - | Y | - |
| `# CONFIG_GNUTLS_EXT_LIBTASN1 is not set` | - | Y | - | Y | - |
| `# CONFIG_GNUTLS_PKCS11 is not set` | - | Y | - | Y | - |
| `# CONFIG_GNUTLS_SRP is not set` | - | Y | - | Y | - |
| `# CONFIG_GNUTLS_TPM is not set` | - | Y | - | Y | - |
| `# CONFIG_LIBCURL_HTTP3 is not set` | - | Y | - | Y | - |
| `# CONFIG_LIBCURL_NGHTTP3 is not set` | Y | - | Y | - | Y |
| `# CONFIG_LIBCURL_NGTCP2 is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_attr is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_avahi-dbus-daemon is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_conntrack is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_dbus is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_kmod-bonding is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_kmod-netlink-diag is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libattr is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libavahi-client is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libavahi-dbus-support is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libdaemon is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libdbus is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libexpat is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libgnutls is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libnetfilter-cthelper is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libnetfilter-cttimeout is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libnetfilter-queue is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libnetsnmp is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libnetsnmp-nossl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_libnetsnmp-ssl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_libpopt is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libtasn1 is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_libtirpc is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_liburing is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-app-openclash is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-app-radicale2 is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-app-rtp2httpd is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-app-samba4 is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-app-upnp is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-app-vsftpd is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-de is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-es is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-fr is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-it is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-ja is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-ko is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-nl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-pl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-ru is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-tr is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-uk is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-aurora-config-zh-tw is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-ar is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-bg is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-bn is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-ca is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-cs is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-da is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-de is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-el is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-es is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-fa is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-fi is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-fr is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-ga is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-he is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-hi is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-hu is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-it is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-ja is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-ko is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-lt is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-mr is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-ms is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-nl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-no is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-pl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-pt is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-pt-br is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-ro is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-ru is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-sk is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-sv is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-tr is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-uk is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-vi is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-yua is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-samba4-zh-tw is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_luci-i18n-upnp-ar is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-bg is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-bn is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-ca is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-cs is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-da is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-de is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-el is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-es is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-fi is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-fr is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-ga is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-he is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-hi is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-hu is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-it is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-ja is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-ko is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-lt is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-mr is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-ms is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-nl is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-no is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-pl is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-pt is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-pt-br is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-ro is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-ru is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-sk is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-sv is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-tr is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-uk is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-vi is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-yua is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_luci-i18n-upnp-zh-tw is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_mtwifi-wapp is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_pnpids is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_python3-libpass is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_python3-parsley is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_python3-passlib is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_python3-pika is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_radicale is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_radicale2 is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_radicale2-examples is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_radicale3 is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_rpcd-mod-rad2-enc is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_rtp2httpd is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_samba4-libs is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_samba4-server is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_snmp-utils is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_snmp-utils-nossl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_snmp-utils-ssl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_snmpd is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_snmpd-nossl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_snmpd-ssl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_snmptrapd is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_snmptrapd-nossl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_snmptrapd-ssl is not set` | - | Y | - | Y | - |
| `# CONFIG_PACKAGE_vsftpd is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_wsdd2 is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-agentd is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-agentd-gnutls is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-agentd-openssl is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-extra-network is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-extra-wifi is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-get is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-get-gnutls is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-get-openssl is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-proxy is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-proxy-gnutls is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-proxy-openssl is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-sender is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-sender-gnutls is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-sender-openssl is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-server is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-server-frontend is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-server-gnutls is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zabbix-server-openssl is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zoneinfo-africa is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zoneinfo-all is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zoneinfo-america is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zoneinfo-asia is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zoneinfo-atlantic is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zoneinfo-australia-nz is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zoneinfo-core is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zoneinfo-europe is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zoneinfo-indian is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zoneinfo-pacific is not set` | Y | - | Y | - | Y |
| `# CONFIG_PACKAGE_zoneinfo-poles is not set` | Y | - | Y | - | Y |
| `# CONFIG_SAMBA4_SERVER_AD_DC is not set` | - | Y | - | Y | - |
| `# CONFIG_SAMBA4_SERVER_QUOTAS is not set` | - | Y | - | Y | - |
| `# CONFIG_SAMBA4_SERVER_VFSX is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_airpi_ap3000m is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_bt_r320 is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_bt_rb300 is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_a10 is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_rax3000m-emmc is not set` | Y | - | Y | - | Y |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_rax3000m-emmc-mtk is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_rax3000m-nand-mtk is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_rax3000m-stock is not set` | Y | - | Y | - | Y |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_tr3000-v1 is not set` | - | Y | Y | Y | Y |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_tr3000-v1-256mb is not set` | Y | Y | Y | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_tr3000-v1-ubootmod is not set` | Y | - | - | - | Y |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_wbr3000uax-v1 is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_wbr3000uax-v1-ubootmod is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_netis_nx32u is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_philips_hy3000 is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_ew-6000gx-pro is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_ew-6000gx-pro-expand is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_ew-6000gx-pro-stock is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_ew-6000gx-pro-ubootmod is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_rg-x60-new is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_rg-x60-new-expand is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_rg-x60-new-stock is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_rg-x60-new-ubootmod is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_rg-x60-pro-107m is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_sn_r1 is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_superelectron_zn-m5-stock is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_supergateway_s20l is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_supergateway_s20m is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_supergateway_s20p is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_wma301 is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_wma301-stock is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_wma301-ubootmod is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_wma301_2.1 is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_zbtlink_zbt-z8103ax-c is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_zbtlink_zbt-z8103ax-c-nmbm is not set` | - | Y | - | Y | - |
| `# CONFIG_TARGET_mediatek_filogic_DEVICE_zbtlink_zbt-z8103ax-c-ubootmod is not set` | - | Y | - | Y | - |
| `# CONFIG_ZABBIX_ENABLE_ZABBIX is not set` | - | Y | - | Y | - |
| `# CONFIG_ZABBIX_MYSQL is not set` | Y | - | Y | - | Y |
| `# CONFIG_ZABBIX_SQLITE is not set` | Y | - | Y | - | Y |
| `CONFIG_DEFAULT_blkid=y` | - | Y | - | Y | - |
| `CONFIG_DEFAULT_blockdev=y` | - | Y | - | Y | - |
| `CONFIG_DEFAULT_fdisk=y` | - | Y | - | Y | - |
| `CONFIG_DEFAULT_kmod-dummy=y` | - | Y | - | Y | - |
| `CONFIG_DEFAULT_kmod-nls-cp437=y` | - | Y | - | Y | - |
| `CONFIG_DEFAULT_kmod-nls-iso8859-1=y` | - | Y | - | Y | - |
| `CONFIG_DEFAULT_kmod-usb-net-rndis=y` | - | Y | - | Y | - |
| `CONFIG_DEFAULT_kmod-usb2=y` | - | Y | - | Y | - |
| `CONFIG_DEFAULT_usbutils=y` | - | Y | - | Y | - |
| `CONFIG_GNUTLS_ALPN=y` | - | Y | - | Y | - |
| `CONFIG_GNUTLS_ANON=y` | - | Y | - | Y | - |
| `CONFIG_GNUTLS_DTLS_SRTP=y` | - | Y | - | Y | - |
| `CONFIG_GNUTLS_HEARTBEAT=y` | - | Y | - | Y | - |
| `CONFIG_GNUTLS_OCSP=y` | - | Y | - | Y | - |
| `CONFIG_GNUTLS_PSK=y` | - | Y | - | Y | - |
| `CONFIG_LIBCURL_HTTP2=y` | - | Y | - | Y | - |
| `CONFIG_LIBCURL_HTTP_AUTH=y` | - | Y | - | Y | - |
| `CONFIG_LIBCURL_NGHTTP2=y` | Y | - | Y | - | Y |
| `CONFIG_OVERRIDE_PKGS="sendat sms-tool"` | Y | - | Y | - | Y |
| `CONFIG_PACKAGE_attr=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_avahi-dbus-daemon=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_conntrack=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_dbus=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_kmod-bonding=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_kmod-netlink-diag=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libattr=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libavahi-client=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libavahi-dbus-support=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libdaemon=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libdbus=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libexpat=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libgnutls=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libnetfilter-cthelper=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libnetfilter-cttimeout=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libnetfilter-queue=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libpopt=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libtasn1=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_libtirpc=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_liburing=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_luci-app-openclash=y` | Y | - | Y | - | Y |
| `CONFIG_PACKAGE_luci-app-samba4=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_luci-app-upnp=y` | Y | - | Y | - | Y |
| `CONFIG_PACKAGE_luci-app-vsftpd=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_luci-i18n-samba4-zh-cn=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_luci-i18n-upnp-zh-cn=y` | Y | - | Y | - | Y |
| `CONFIG_PACKAGE_luci-i18n-vsftpd-zh-cn=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_samba4-libs=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_samba4-server=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_trusted-firmware-a-mt7981-cudy-tr3000-v1=y` | - | Y | Y | Y | - |
| `CONFIG_PACKAGE_u-boot-mt7981_cudy_tr3000-v1=y` | - | Y | Y | Y | - |
| `CONFIG_PACKAGE_vsftpd=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_wsdd2=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_zoneinfo-africa=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_zoneinfo-all=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_zoneinfo-america=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_zoneinfo-asia=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_zoneinfo-atlantic=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_zoneinfo-australia-nz=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_zoneinfo-core=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_zoneinfo-europe=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_zoneinfo-indian=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_zoneinfo-pacific=y` | - | Y | - | Y | - |
| `CONFIG_PACKAGE_zoneinfo-poles=y` | - | Y | - | Y | - |
| `CONFIG_SAMBA4_SERVER_AVAHI=y` | - | Y | - | Y | - |
| `CONFIG_SAMBA4_SERVER_NETBIOS=y` | - | Y | - | Y | - |
| `CONFIG_SAMBA4_SERVER_VFS=y` | - | Y | - | Y | - |
| `CONFIG_SAMBA4_SERVER_WSDD2=y` | - | Y | - | Y | - |
| `CONFIG_TARGET_PROFILE="DEVICE_cudy_tr3000-v1"` | Y | - | - | - | - |
| `CONFIG_TARGET_PROFILE="DEVICE_cudy_tr3000-v1-256mb"` | - | - | - | - | Y |
| `CONFIG_TARGET_PROFILE="DEVICE_cudy_tr3000-v1-ubootmod"` | - | Y | Y | Y | - |
| `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_tr3000-v1-256mb=y` | - | - | - | - | Y |
| `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_tr3000-v1-ubootmod=y` | - | Y | Y | Y | - |
| `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_tr3000-v1=y` | Y | - | - | - | - |
| `CONFIG_ZABBIX_POSTGRESQL=y` | Y | - | Y | - | Y |
