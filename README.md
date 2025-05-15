# Luckfox-Lyra-Plus
rkdeveloptool db results/rk3506_spl_loader_v1.04.110.bin
rkdeveloptool wl 0x40 idbLoader.img
rkdeveloptool wl 0x4000 uboot.img
rkdeveloptool wl 0x8000 boot.img
rkdeveloptool wl 0x40000 rootfs.img


./tools/mkimage  -n rk3399 -T rksd -d /path/to/rkbin/bin/rk33/rk3399_ddr_800MHz_v1.20.bin idbloader.img
cat /path/to/rkbin/bin/rk33/rk3399_miniloader_v1.19.bin >> idbloader.img


u-boot-rockchip/tools/mkimage -n rk3506 -T rkspi -d rkbin/bin/rk35/rk3506_ddr_750MHz_v1.05.bin idbloader.img
