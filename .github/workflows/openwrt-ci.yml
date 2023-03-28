# 
# <https://github.com/KFERMercer/OpenWrt-CI>
#
# Copyright (C) 2019 P3TERX
#
# Copyright (C) 2020 KFERMercer
#
name: OpenWrt-CI

on:
  push:
    branches:
      - master
  # schedule:
  #   - cron: 0 20 * * *
  release:
    types: [published]

permissions:
  contents: read

jobs:

  build_openwrt:

    name: Build OpenWrt Firmware

    runs-on: ubuntu-latest

    steps:
      - name: Space cleanup and Initialization environment
        env:
          DEBIAN_FRONTEND: noninteractive
        run: |
          docker rmi `docker images -q`
          sudo -E rm -rf /usr/share/dotnet /etc/mysql /etc/php /etc/apt/sources.list.d /usr/local/lib/android
          sudo -E apt-mark hold grub-efi-amd64-signed
          sudo -E apt update
          sudo -E apt -y purge azure-cli* docker* ghc* zulu* llvm* firefox google* dotnet* powershell* openjdk* mysql* php* mongodb* dotnet* moby* snap*
          sudo -E apt -y full-upgrade
          sudo -E apt -y install ack antlr3 aria2 asciidoc autoconf automake autopoint binutils bison build-essential bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python3 python3-pip libpython3-dev qemu-utils rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
          sudo -E systemctl daemon-reload
          sudo -E apt -y autoremove --purge
          sudo -E apt clean
          sudo -E timedatectl set-timezone "Asia/Shanghai"

      - name: Checkout OpenWrt
        uses: actions/checkout@v3

      - name: Update feeds
        run: |
          sed -i 's/#src-git helloworld/src-git helloworld/g' ./feeds.conf.default
          ./scripts/feeds update -a
          ./scripts/feeds install -a

      - name: Generate configuration file
        run: |
          rm -f ./.config*
          touch ./.config

          #
          # 在 cat >> .config <<EOF 到 EOF 之间粘贴你的编译配置, 需注意缩进关系
          # 例如:
          cat >> .config <<EOF
          CONFIG_TARGET_ipq40xx=y
          CONFIG_TARGET_ipq40xx_DEVICE_p2w_r619ac-128m=y
          CONFIG_TARGET_BOARD="ipq40xx"
          CONFIG_ARIA2_BITTORRENT=y
          CONFIG_ARIA2_NOXML=y
          CONFIG_ARIA2_OPENSSL=y
          CONFIG_ARIA2_WEBSOCKET=y
          CONFIG_BATMAN_ADV_BATMAN_V=y
          CONFIG_BATMAN_ADV_BLA=y
          CONFIG_BATMAN_ADV_DAT=y
          CONFIG_BATMAN_ADV_MCAST=y
          CONFIG_DOCKER_CGROUP_OPTIONS=y
          CONFIG_DOCKER_NET_MACVLAN=y
          CONFIG_DOCKER_STO_EXT4=y
          CONFIG_KERNEL_CGROUP_DEVICE=y
          CONFIG_KERNEL_CGROUP_FREEZER=y
          CONFIG_KERNEL_CGROUP_NET_PRIO=y
          CONFIG_KERNEL_EXT4_FS_POSIX_ACL=y
          CONFIG_KERNEL_EXT4_FS_SECURITY=y
          CONFIG_KERNEL_FS_POSIX_ACL=y
          CONFIG_KERNEL_NET_CLS_CGROUP=y
          CONFIG_KSMBD_SMB_INSECURE_SERVER=y
          CONFIG_LIBSODIUM_MINIMAL=y
          CONFIG_LXC_SECCOMP=y
          CONFIG_MBEDTLS_AES_C=y
          CONFIG_MBEDTLS_CMAC_C=y
          CONFIG_MBEDTLS_DES_C=y
          CONFIG_MBEDTLS_ECP_DP_CURVE25519_ENABLED=y
          CONFIG_MBEDTLS_ECP_DP_SECP256K1_ENABLED=y
          CONFIG_MBEDTLS_ECP_DP_SECP256R1_ENABLED=y
          CONFIG_MBEDTLS_ECP_DP_SECP384R1_ENABLED=y
          CONFIG_MBEDTLS_ENTROPY_FORCE_SHA256=y
          CONFIG_MBEDTLS_GCM_C=y
          CONFIG_MBEDTLS_KEY_EXCHANGE_ECDHE_ECDSA_ENABLED=y
          CONFIG_MBEDTLS_KEY_EXCHANGE_ECDHE_PSK_ENABLED=y
          CONFIG_MBEDTLS_KEY_EXCHANGE_ECDHE_RSA_ENABLED=y
          CONFIG_MBEDTLS_KEY_EXCHANGE_PSK_ENABLED=y
          CONFIG_MBEDTLS_NIST_KW_C=y
          CONFIG_MBEDTLS_RSA_NO_CRT=y
          CONFIG_NFS_KERNEL_SERVER_V4=y
          CONFIG_NGINX_HEADERS_MORE=y
          CONFIG_NGINX_HTTP_ACCESS=y
          CONFIG_NGINX_HTTP_AUTH_BASIC=y
          CONFIG_NGINX_HTTP_AUTOINDEX=y
          CONFIG_NGINX_HTTP_BROWSER=y
          CONFIG_NGINX_HTTP_CACHE=y
          CONFIG_NGINX_HTTP_CHARSET=y
          CONFIG_NGINX_HTTP_EMPTY_GIF=y
          CONFIG_NGINX_HTTP_FASTCGI=y
          CONFIG_NGINX_HTTP_GEO=y
          CONFIG_NGINX_HTTP_GZIP=y
          CONFIG_NGINX_HTTP_GZIP_STATIC=y
          CONFIG_NGINX_HTTP_LIMIT_CONN=y
          CONFIG_NGINX_HTTP_LIMIT_REQ=y
          CONFIG_NGINX_HTTP_MAP=y
          CONFIG_NGINX_HTTP_MEMCACHED=y
          CONFIG_NGINX_HTTP_PROXY=y
          CONFIG_NGINX_HTTP_REFERER=y
          CONFIG_NGINX_HTTP_REWRITE=y
          CONFIG_NGINX_HTTP_SCGI=y
          CONFIG_NGINX_HTTP_SPLIT_CLIENTS=y
          CONFIG_NGINX_HTTP_SSI=y
          CONFIG_NGINX_HTTP_UPSTREAM_HASH=y
          CONFIG_NGINX_HTTP_UPSTREAM_IP_HASH=y
          CONFIG_NGINX_HTTP_UPSTREAM_KEEPALIVE=y
          CONFIG_NGINX_HTTP_UPSTREAM_LEAST_CONN=y
          CONFIG_NGINX_HTTP_USERID=y
          CONFIG_NGINX_HTTP_UWSGI=y
          CONFIG_NGINX_HTTP_V2=y
          CONFIG_NGINX_NAXSI=y
          CONFIG_NGINX_PCRE=y
          CONFIG_NGINX_UBUS=y
          CONFIG_NODEJS_ICU_SMALL=y
          CONFIG_OCSERV_HTTP_PARSER=y
          CONFIG_OCSERV_PROTOBUF=y
          CONFIG_PACKAGE_6in4=y
          CONFIG_PACKAGE_SAMBA_MAX_DEBUG_LEVEL=-1
          CONFIG_PACKAGE_UnblockNeteaseMusic=y
          CONFIG_PACKAGE_adblock=y
          CONFIG_PACKAGE_ahcpd=y
          CONFIG_PACKAGE_aliyundrive-fuse=y
          CONFIG_PACKAGE_aliyundrive-webdav=y
          CONFIG_PACKAGE_alsa-lib=y
          CONFIG_PACKAGE_alsa-ucm-conf=y
          CONFIG_PACKAGE_alsa-utils=y
          CONFIG_PACKAGE_amule=y
          CONFIG_PACKAGE_aria2=y
          CONFIG_PACKAGE_ariang=y
          CONFIG_PACKAGE_attendedsysupgrade-common=y
          CONFIG_PACKAGE_attr=m
          CONFIG_PACKAGE_baidupcs-web=y
          CONFIG_PACKAGE_batctl-default=y
          CONFIG_PACKAGE_bcp38=y
          CONFIG_PACKAGE_bird1-ipv4=y
          CONFIG_PACKAGE_bird1-ipv4-uci=y
          CONFIG_PACKAGE_bird1-ipv6=y
          CONFIG_PACKAGE_bird1-ipv6-uci=y
          CONFIG_PACKAGE_blkid=y
          CONFIG_PACKAGE_boost=y
          CONFIG_PACKAGE_boost-program_options=y
          CONFIG_PACKAGE_boost-system=y
          CONFIG_PACKAGE_brook=y
          CONFIG_PACKAGE_btrfs-progs=y
          CONFIG_PACKAGE_certtool=y
          CONFIG_PACKAGE_cgi-io=y
          CONFIG_PACKAGE_cgroupfs-mount=y
          CONFIG_PACKAGE_chinadns-ng=y
          CONFIG_PACKAGE_collectd=y
          CONFIG_PACKAGE_collectd-mod-cpu=y
          CONFIG_PACKAGE_collectd-mod-interface=y
          CONFIG_PACKAGE_collectd-mod-iwinfo=y
          CONFIG_PACKAGE_collectd-mod-load=y
          CONFIG_PACKAGE_collectd-mod-memory=y
          CONFIG_PACKAGE_collectd-mod-network=y
          CONFIG_PACKAGE_collectd-mod-rrdtool=y
          CONFIG_PACKAGE_confuse=y
          CONFIG_PACKAGE_containerd=y
          CONFIG_PACKAGE_coreutils=y
          CONFIG_PACKAGE_coreutils-base64=y
          CONFIG_PACKAGE_coreutils-nohup=y
          CONFIG_PACKAGE_coreutils-sort=y
          CONFIG_PACKAGE_cshark=y
          CONFIG_PACKAGE_dawn=y
          CONFIG_PACKAGE_dns2socks=y
          CONFIG_PACKAGE_dns2tcp=y
          CONFIG_PACKAGE_docker=y
          CONFIG_PACKAGE_dockerd=y
          CONFIG_PACKAGE_dump1090=y
          CONFIG_PACKAGE_dynapoint=y
          CONFIG_PACKAGE_e2fsprogs=y
          CONFIG_PACKAGE_e2guardian=y
          CONFIG_PACKAGE_fdisk=y
          CONFIG_PACKAGE_fdk-aac=y
          CONFIG_PACKAGE_flock=y
          CONFIG_PACKAGE_frpc=y
          CONFIG_PACKAGE_fuse-utils=y
          CONFIG_PACKAGE_getopt=y
          CONFIG_PACKAGE_gowebdav=y
          CONFIG_PACKAGE_haproxy=y
          CONFIG_PACKAGE_hd-idle=y
          CONFIG_PACKAGE_ip6tables=y
          CONFIG_PACKAGE_ipt2socks=y
          CONFIG_PACKAGE_iptables-mod-conntrack-extra=y
          CONFIG_PACKAGE_iptables-mod-ipopt=y
          CONFIG_PACKAGE_iptables-mod-iprange=y
          CONFIG_PACKAGE_iptables-mod-nat-extra=y
          CONFIG_PACKAGE_iptables-mod-socket=y
          CONFIG_PACKAGE_iputils-arping=y
          CONFIG_PACKAGE_ipv6helper=y
          CONFIG_PACKAGE_jq=y
          CONFIG_PACKAGE_kmod-batman-adv=y
          CONFIG_PACKAGE_kmod-br-netfilter=y
          CONFIG_PACKAGE_kmod-crypto-cts=y
          CONFIG_PACKAGE_kmod-crypto-lib-chacha20=y
          CONFIG_PACKAGE_kmod-crypto-lib-chacha20poly1305=y
          CONFIG_PACKAGE_kmod-crypto-lib-curve25519=y
          CONFIG_PACKAGE_kmod-crypto-lib-poly1305=y
          CONFIG_PACKAGE_kmod-crypto-md4=y
          CONFIG_PACKAGE_kmod-crypto-sha512=y
          CONFIG_PACKAGE_kmod-dax=y
          CONFIG_PACKAGE_kmod-dm=y
          CONFIG_PACKAGE_kmod-dnsresolver=y
          CONFIG_PACKAGE_kmod-dummy=y
          CONFIG_PACKAGE_kmod-fs-btrfs=y
          CONFIG_PACKAGE_kmod-fs-cifs=y
          CONFIG_PACKAGE_kmod-fs-exportfs=y
          CONFIG_PACKAGE_kmod-fs-ksmbd=y
          CONFIG_PACKAGE_kmod-fs-nfs=y
          CONFIG_PACKAGE_kmod-fs-nfs-common=y
          CONFIG_PACKAGE_kmod-fs-nfs-common-rpcsec=y
          CONFIG_PACKAGE_kmod-fs-nfs-v4=y
          CONFIG_PACKAGE_kmod-fs-nfsd=y
          CONFIG_PACKAGE_kmod-fuse=y
          CONFIG_PACKAGE_kmod-ifb=y
          CONFIG_PACKAGE_kmod-ikconfig=y
          CONFIG_PACKAGE_kmod-input-core=y
          CONFIG_PACKAGE_kmod-ip6tables=y
          CONFIG_PACKAGE_kmod-ipt-conntrack-extra=y
          CONFIG_PACKAGE_kmod-ipt-ipopt=y
          CONFIG_PACKAGE_kmod-ipt-iprange=y
          CONFIG_PACKAGE_kmod-ipt-nat-extra=y
          CONFIG_PACKAGE_kmod-ipt-nat6=y
          CONFIG_PACKAGE_kmod-ipt-socket=y
          CONFIG_PACKAGE_kmod-iptunnel=y
          CONFIG_PACKAGE_kmod-keys-encrypted=y
          CONFIG_PACKAGE_kmod-keys-trusted=y
          CONFIG_PACKAGE_kmod-l2tp=y
          CONFIG_PACKAGE_kmod-lib-crc32c=y
          CONFIG_PACKAGE_kmod-lib-raid6=y
          CONFIG_PACKAGE_kmod-lib-xor=y
          CONFIG_PACKAGE_kmod-lib-zstd=y
          CONFIG_PACKAGE_kmod-nf-ipt6=y
          CONFIG_PACKAGE_kmod-nf-ipvs=y
          CONFIG_PACKAGE_kmod-nf-log6=y
          CONFIG_PACKAGE_kmod-nf-nat6=y
          CONFIG_PACKAGE_kmod-nf-reject6=y
          CONFIG_PACKAGE_kmod-nf-socket=y
          CONFIG_PACKAGE_kmod-oid-registry=y
          CONFIG_PACKAGE_kmod-pppol2tp=y
          CONFIG_PACKAGE_kmod-random-core=y
          CONFIG_PACKAGE_kmod-sched=y
          CONFIG_PACKAGE_kmod-sched-connmark=y
          CONFIG_PACKAGE_kmod-sched-core=y
          CONFIG_PACKAGE_kmod-siit=y
          CONFIG_PACKAGE_kmod-sit=y
          CONFIG_PACKAGE_kmod-sound-core=y
          CONFIG_PACKAGE_kmod-tpm=y
          CONFIG_PACKAGE_kmod-udptunnel4=y
          CONFIG_PACKAGE_kmod-udptunnel6=y
          CONFIG_PACKAGE_kmod-usb-printer=y
          CONFIG_PACKAGE_kmod-veth=y
          CONFIG_PACKAGE_kmod-wireguard=y
          CONFIG_PACKAGE_ksmbd-server=y
          CONFIG_PACKAGE_lame-lib=y
          CONFIG_PACKAGE_libaio=y
          CONFIG_PACKAGE_libbfd=y
          CONFIG_PACKAGE_libbz2=y
          CONFIG_PACKAGE_libcares=y
          CONFIG_PACKAGE_libcomerr=y
          CONFIG_PACKAGE_libconfig=y
          CONFIG_PACKAGE_libcryptopp=y
          CONFIG_PACKAGE_libdevmapper=y
          CONFIG_PACKAGE_libedit=y
          CONFIG_PACKAGE_libev=y
          CONFIG_PACKAGE_libevent2=y
          CONFIG_PACKAGE_libexif=y
          CONFIG_PACKAGE_libext2fs=y
          CONFIG_PACKAGE_libfdisk=y
          CONFIG_PACKAGE_libffmpeg-full=y
          CONFIG_PACKAGE_libflac=y
          CONFIG_PACKAGE_libfreetype=y
          CONFIG_PACKAGE_libfuse=y
          CONFIG_PACKAGE_libgcrypt=y
          CONFIG_PACKAGE_libgd-full=y
          CONFIG_PACKAGE_libgdbm=y
          CONFIG_PACKAGE_libgpg-error=y
          CONFIG_PACKAGE_libhttp-parser=y
          CONFIG_PACKAGE_libid3tag=y
          CONFIG_PACKAGE_libjpeg-turbo=y
          CONFIG_PACKAGE_libkeyutils=y
          CONFIG_PACKAGE_libltdl=y
          CONFIG_PACKAGE_liblua5.3=y
          CONFIG_PACKAGE_liblxc=y
          CONFIG_PACKAGE_liblzo=y
          CONFIG_PACKAGE_libmbedtls=y
          CONFIG_PACKAGE_libmount=y
          CONFIG_PACKAGE_libnetwork=y
          CONFIG_PACKAGE_libnl-core=y
          CONFIG_PACKAGE_libnl-genl=y
          CONFIG_PACKAGE_libogg=y
          CONFIG_PACKAGE_libopus=y
          CONFIG_PACKAGE_libpagekite=y
          CONFIG_PACKAGE_libparted=y
          CONFIG_PACKAGE_libpcap=y
          CONFIG_PACKAGE_libpcre2=y
          CONFIG_PACKAGE_libplist=y
          CONFIG_PACKAGE_libpng=y
          CONFIG_PACKAGE_libprotobuf-c=y
          CONFIG_PACKAGE_librrd1=y
          CONFIG_PACKAGE_librtlsdr=y
          CONFIG_PACKAGE_libseccomp=y
          CONFIG_PACKAGE_libsmartcols=y
          CONFIG_PACKAGE_libsodium=y
          CONFIG_PACKAGE_libsoxr=y
          CONFIG_PACKAGE_libsqlite3=y
          CONFIG_PACKAGE_libss=y
          CONFIG_PACKAGE_libtasn1=m
          CONFIG_PACKAGE_libtiff=y
          CONFIG_PACKAGE_libubox-lua=y
          CONFIG_PACKAGE_libudns=y
          CONFIG_PACKAGE_libunbound=y
          CONFIG_PACKAGE_libunbound_ipset=y
          CONFIG_PACKAGE_libunbound_libevent=y
          CONFIG_PACKAGE_libunbound_libpthread=y
          CONFIG_PACKAGE_libunistring=y
          CONFIG_PACKAGE_libupnp=y
          CONFIG_PACKAGE_liburing=m
          CONFIG_PACKAGE_libusb-1.0=y
          CONFIG_PACKAGE_libustream-mbedtls=y
          CONFIG_PACKAGE_libuv=y
          CONFIG_PACKAGE_libvorbis=y
          CONFIG_PACKAGE_libwebp=y
          CONFIG_PACKAGE_libwebsockets-full=y
          CONFIG_PACKAGE_libwrap=y
          CONFIG_PACKAGE_libwxbase=y
          CONFIG_PACKAGE_libxml2=y
          CONFIG_PACKAGE_libzip-gnutls=y
          CONFIG_PACKAGE_lsblk=y
          CONFIG_PACKAGE_lua-md5=y
          CONFIG_PACKAGE_luci-app-adblock=y
          CONFIG_PACKAGE_luci-app-advanced-reboot=y
          CONFIG_PACKAGE_luci-app-ahcp=y
          CONFIG_PACKAGE_luci-app-airplay2=y
          CONFIG_PACKAGE_luci-app-aliyundrive-fuse=y
          CONFIG_PACKAGE_luci-app-aliyundrive-webdav=y
          CONFIG_PACKAGE_luci-app-amule=y
          CONFIG_PACKAGE_luci-app-argon-config=y
          CONFIG_PACKAGE_luci-app-aria2=y
          CONFIG_PACKAGE_luci-app-asterisk=y
          CONFIG_PACKAGE_luci-app-attendedsysupgrade=y
          CONFIG_PACKAGE_luci-app-bcp38=y
          CONFIG_PACKAGE_luci-app-bird1-ipv4=y
          CONFIG_PACKAGE_luci-app-bird1-ipv6=y
          CONFIG_PACKAGE_luci-app-cifs-mount=y
          CONFIG_PACKAGE_luci-app-cifsd=y
          CONFIG_PACKAGE_luci-app-commands=y
          CONFIG_PACKAGE_luci-app-cshark=y
          CONFIG_PACKAGE_luci-app-dawn=y
          CONFIG_PACKAGE_luci-app-design-config=y
          CONFIG_PACKAGE_luci-app-diag-core=y
          CONFIG_PACKAGE_luci-app-diskman=y
          CONFIG_PACKAGE_luci-app-diskman_INCLUDE_btrfs_progs=y
          CONFIG_PACKAGE_luci-app-diskman_INCLUDE_lsblk=y
          CONFIG_PACKAGE_luci-app-docker=y
          CONFIG_PACKAGE_luci-app-dockerman=y
          CONFIG_PACKAGE_luci-app-dump1090=y
          CONFIG_PACKAGE_luci-app-dynapoint=y
          CONFIG_PACKAGE_luci-app-e2guardian=y
          CONFIG_PACKAGE_luci-app-easymesh=y
          CONFIG_PACKAGE_luci-app-frpc=y
          CONFIG_PACKAGE_luci-app-guest-wifi=y
          CONFIG_PACKAGE_luci-app-haproxy-tcp=y
          CONFIG_PACKAGE_luci-app-hd-idle=y
          CONFIG_PACKAGE_luci-app-hnet=y
          CONFIG_PACKAGE_luci-app-ipsec-server=y
          CONFIG_PACKAGE_luci-app-kodexplorer=y
          CONFIG_PACKAGE_luci-app-lxc=y
          CONFIG_PACKAGE_luci-app-minidlna=y
          CONFIG_PACKAGE_luci-app-music-remote-center=y
          CONFIG_PACKAGE_luci-app-mwan3=y
          CONFIG_PACKAGE_luci-app-mwan3helper=y
          CONFIG_PACKAGE_luci-app-nfs=y
          CONFIG_PACKAGE_luci-app-nps=y
          CONFIG_PACKAGE_luci-app-ntpc=y
          CONFIG_PACKAGE_luci-app-ocserv=y
          CONFIG_PACKAGE_luci-app-omcproxy=y
          CONFIG_PACKAGE_luci-app-openvpn=y
          CONFIG_PACKAGE_luci-app-p910nd=y
          CONFIG_PACKAGE_luci-app-pagekitec=y
          CONFIG_PACKAGE_luci-app-passwall=y
          CONFIG_PACKAGE_luci-app-passwall_INCLUDE_Brook=y
          CONFIG_PACKAGE_luci-app-passwall_INCLUDE_ChinaDNS_NG=y
          CONFIG_PACKAGE_luci-app-passwall_Iptables_Transparent_Proxy=y
          CONFIG_PACKAGE_luci-app-pgyvpn=y
          CONFIG_PACKAGE_luci-app-phtunnel=y
          CONFIG_PACKAGE_luci-app-qbittorrent=y
          CONFIG_PACKAGE_luci-app-qos=y
          CONFIG_PACKAGE_luci-app-ramfree=y
          CONFIG_PACKAGE_luci-app-samba=y
          CONFIG_PACKAGE_luci-app-serverchan=y
          CONFIG_PACKAGE_luci-app-shadowsocks-libev=y
          CONFIG_PACKAGE_luci-app-siitwizard=y
          CONFIG_PACKAGE_luci-app-simple-adblock=y
          CONFIG_PACKAGE_luci-app-socat=y
          CONFIG_PACKAGE_luci-app-splash=y
          CONFIG_PACKAGE_luci-app-statistics=y
          CONFIG_PACKAGE_luci-app-syncdial=y
          CONFIG_PACKAGE_luci-app-transmission=y
          CONFIG_PACKAGE_luci-app-travelmate=y
          CONFIG_PACKAGE_luci-app-ttyd=y
          CONFIG_PACKAGE_luci-app-udpxy=y
          CONFIG_PACKAGE_luci-app-unbound=y
          CONFIG_PACKAGE_luci-app-usb-printer=y
          CONFIG_PACKAGE_luci-app-uugamebooster=y
          CONFIG_PACKAGE_luci-app-verysync=y
          CONFIG_PACKAGE_luci-app-vnstat=y
          CONFIG_PACKAGE_luci-app-vpnbypass=y
          CONFIG_PACKAGE_luci-app-watchcat=y
          CONFIG_PACKAGE_luci-app-webdav=y
          CONFIG_PACKAGE_luci-app-wifischedule=y
          CONFIG_PACKAGE_luci-app-wireguard=y
          CONFIG_PACKAGE_luci-app-wrtbwmon=y
          CONFIG_PACKAGE_luci-app-xlnetacc=y
          CONFIG_PACKAGE_luci-compat=y
          CONFIG_PACKAGE_luci-i18n-adblock-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-advanced-reboot-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-ahcp-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-airplay2-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-aliyundrive-fuse-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-aliyundrive-webdav-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-amule-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-argon-config-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-aria2-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-asterisk-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-bcp38-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-cifs-mount-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-cifsd-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-commands-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-dawn-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-design-config-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-diag-core-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-diskman-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-docker-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-dockerman-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-easymesh-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-frpc-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-guest-wifi-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-haproxy-tcp-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-hd-idle-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-ipsec-server-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-kodexplorer-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-minidlna-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-music-remote-center-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-mwan3-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-mwan3helper-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-nfs-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-nps-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-ntpc-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-omcproxy-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-openvpn-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-p910nd-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-passwall-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-phtunnel-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-qbittorrent-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-qos-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-ramfree-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-samba-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-socat-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-splash-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-statistics-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-transmission-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-ttyd-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-udpxy-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-usb-printer-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-uugamebooster-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-verysync-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-vnstat-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-vpnbypass-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-watchcat-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-wifischedule-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-wireguard-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-wrtbwmon-zh-cn=y
          CONFIG_PACKAGE_luci-i18n-xlnetacc-zh-cn=y
          CONFIG_PACKAGE_luci-lib-docker=y
          CONFIG_PACKAGE_luci-lib-json=y
          CONFIG_PACKAGE_luci-proto-ipv6=y
          CONFIG_PACKAGE_luci-proto-wireguard=y
          CONFIG_PACKAGE_luci-theme-argon=y
          CONFIG_PACKAGE_luci-theme-argon-mod=y
          CONFIG_PACKAGE_luci-theme-design=y
          CONFIG_PACKAGE_luci-theme-infinityfreedom=y
          CONFIG_PACKAGE_luci-theme-material=y
          CONFIG_PACKAGE_luci-theme-netgear=y
          CONFIG_PACKAGE_lxc=y
          CONFIG_PACKAGE_lxc-attach=y
          CONFIG_PACKAGE_lxc-common=y
          CONFIG_PACKAGE_lxc-configs=y
          CONFIG_PACKAGE_lxc-console=y
          CONFIG_PACKAGE_lxc-create=y
          CONFIG_PACKAGE_lxc-hooks=y
          CONFIG_PACKAGE_lxc-templates=y
          CONFIG_PACKAGE_microsocks=y
          CONFIG_PACKAGE_minidlna=y
          CONFIG_PACKAGE_mount-utils=y
          CONFIG_PACKAGE_mwan3=y
          CONFIG_PACKAGE_mxml=y
          CONFIG_PACKAGE_nfs-kernel-server=y
          CONFIG_PACKAGE_nfs-kernel-server-utils=y
          CONFIG_PACKAGE_nfs-utils=y
          CONFIG_PACKAGE_nfs-utils-libs=y
          CONFIG_PACKAGE_nginx-ssl=y
          CONFIG_PACKAGE_nginx-ssl-util=y
          CONFIG_PACKAGE_nginx-util=y
          CONFIG_PACKAGE_node=y
          CONFIG_PACKAGE_npc=y
          CONFIG_PACKAGE_ntpclient=y
          CONFIG_PACKAGE_ocserv=y
          CONFIG_PACKAGE_odhcp6c=y
          CONFIG_PACKAGE_odhcp6c_ext_cer_id=0
          CONFIG_PACKAGE_odhcpd-ipv6only=y
          CONFIG_PACKAGE_odhcpd_ipv6only_ext_cer_id=0
          CONFIG_PACKAGE_omcproxy=y
          CONFIG_PACKAGE_oniguruma=y
          CONFIG_PACKAGE_owntone=y
          CONFIG_PACKAGE_p910nd=y
          CONFIG_PACKAGE_pagekitec=y
          CONFIG_PACKAGE_parted=y
          CONFIG_PACKAGE_pgyvpn=y
          CONFIG_PACKAGE_php8=y
          CONFIG_PACKAGE_php8-cgi=y
          CONFIG_PACKAGE_php8-fastcgi=y
          CONFIG_PACKAGE_php8-fpm=y
          CONFIG_PACKAGE_php8-mod-curl=y
          CONFIG_PACKAGE_php8-mod-dom=y
          CONFIG_PACKAGE_php8-mod-gd=y
          CONFIG_PACKAGE_php8-mod-iconv=y
          CONFIG_PACKAGE_php8-mod-mbstring=y
          CONFIG_PACKAGE_php8-mod-mysqlnd=y
          CONFIG_PACKAGE_php8-mod-opcache=y
          CONFIG_PACKAGE_php8-mod-pdo=y
          CONFIG_PACKAGE_php8-mod-pdo-mysql=y
          CONFIG_PACKAGE_php8-mod-pdo-sqlite=y
          CONFIG_PACKAGE_php8-mod-session=y
          CONFIG_PACKAGE_php8-mod-sqlite3=y
          CONFIG_PACKAGE_php8-mod-xml=y
          CONFIG_PACKAGE_php8-mod-xmlreader=y
          CONFIG_PACKAGE_php8-mod-xmlwriter=y
          CONFIG_PACKAGE_php8-mod-zip=y
          CONFIG_PACKAGE_phtunnel=y
          CONFIG_PACKAGE_ppp-mod-pppol2tp=y
          CONFIG_PACKAGE_qBittorrent-static=y
          CONFIG_PACKAGE_qos-scripts=y
          CONFIG_PACKAGE_resolveip=y
          CONFIG_PACKAGE_rpcbind=y
          CONFIG_PACKAGE_rpcd-mod-lxc=y
          CONFIG_PACKAGE_rpcd-mod-rpcsys=y
          CONFIG_PACKAGE_rrdtool1=y
          CONFIG_PACKAGE_runc=y
          CONFIG_PACKAGE_samba36-server=y
          CONFIG_PACKAGE_samba4-libs=m
          CONFIG_PACKAGE_shadowsocks-libev-ss-local=y
          CONFIG_PACKAGE_shadowsocks-libev-ss-redir=y
          CONFIG_PACKAGE_shadowsocks-libev-ss-server=y
          CONFIG_PACKAGE_shadowsocksr-libev-ssr-local=y
          CONFIG_PACKAGE_shadowsocksr-libev-ssr-redir=y
          CONFIG_PACKAGE_shairport-sync-openssl=y
          CONFIG_PACKAGE_simple-adblock=y
          CONFIG_PACKAGE_simple-obfs=y
          CONFIG_PACKAGE_smartmontools=y
          CONFIG_PACKAGE_socat=y
          CONFIG_PACKAGE_sqlite3-cli=y
          CONFIG_PACKAGE_strongswan-mod-openssl=y
          CONFIG_PACKAGE_tc-tiny=y
          CONFIG_PACKAGE_tcping=y
          CONFIG_PACKAGE_tini=y
          CONFIG_PACKAGE_transmission-daemon-openssl=y
          CONFIG_PACKAGE_transmission-web-control=y
          CONFIG_PACKAGE_travelmate=y
          CONFIG_PACKAGE_trojan-plus=y
          CONFIG_PACKAGE_ttyd=y
          CONFIG_PACKAGE_udpxy=y
          CONFIG_PACKAGE_umdns=y
          CONFIG_PACKAGE_unbound-daemon=y
          CONFIG_PACKAGE_unzip=y
          CONFIG_PACKAGE_uugamebooster=y
          CONFIG_PACKAGE_v2ray-core=y
          CONFIG_PACKAGE_v2ray-plugin=y
          CONFIG_PACKAGE_verysync=y
          CONFIG_PACKAGE_vnstat=y
          CONFIG_PACKAGE_vnstati=y
          CONFIG_PACKAGE_vpnbypass=y
          CONFIG_PACKAGE_watchcat=y
          CONFIG_PACKAGE_wifischedule=y
          CONFIG_PACKAGE_wireguard-tools=y
          CONFIG_PACKAGE_xl2tpd=y
          CONFIG_PACKAGE_xray-core=y
          CONFIG_PACKAGE_zoneinfo-asia=y
          CONFIG_PACKAGE_zoneinfo-core=y
          CONFIG_PARTED_READLINE=y
          CONFIG_PCRE2_JIT_ENABLED=y
          CONFIG_PHP8_LIBXML=y
          CONFIG_PHP8_SYSTEMTZDATA=y
          CONFIG_RPCBIND_LIBWRAP=y
          CONFIG_RPCBIND_RMTCALLS=y
          CONFIG_SQLITE3_COLUMN_METADATA=y
          CONFIG_SQLITE3_DYNAMIC_EXTENSIONS=y
          CONFIG_SQLITE3_FTS3=y
          CONFIG_SQLITE3_FTS4=y
          CONFIG_SQLITE3_FTS5=y
          CONFIG_SQLITE3_JSON1=y
          CONFIG_SQLITE3_LIBEDIT=y
          CONFIG_SQLITE3_RTREE=y
          CONFIG_boost-compile-visibility-hidden=y
          CONFIG_boost-runtime-shared=y
          CONFIG_boost-static-and-shared-libs=y
          CONFIG_boost-variant-release=y
          # CONFIG_PACKAGE_kmod-crypto-kpp is not set

          EOF

          #
          # ===============================================================
          #

          sed -i 's/^[ \t]*//g' ./.config
          make defconfig

      - name: Download packages
        run: make download -j16

      - name: Compile firmware
        run: |
          make -j$(nproc) || make -j1 V=s
          echo "======================="
          echo "Space usage:"
          echo "======================="
          df -h
          echo "======================="
          du -h --max-depth=1 ./ --exclude=build_dir --exclude=bin
          du -h --max-depth=1 ./build_dir
          du -h --max-depth=1 ./bin

      - name: Prepare artifact
        run: |
          mkdir -p ./artifact/package
          mkdir -p ./artifact/buildinfo
          rm -rf $(find ./bin/targets/ -type d -name "packages")
          cp -rf $(find ./bin/packages/ -type f -name "*.ipk") ./artifact/package/
          cp -rf $(find ./bin/targets/ -type f -name "*.buildinfo" -o -name "*.manifest") ./artifact/buildinfo/

      - name: Upload buildinfo
        uses: actions/upload-artifact@v3
        with:
          name: OpenWrt_buildinfo
          path: ./artifact/buildinfo/

      - name: Upload package
        uses: actions/upload-artifact@v3
        with:
          name: OpenWrt_package
          path: ./artifact/package/

      - name: Upload firmware
        uses: actions/upload-artifact@v3
        with:
          name: OpenWrt_firmware
          path: ./bin/targets/
