#
# arch/arm64/boot/Makefile
#
# This file is included by the global makefile so that you can add your own
# architecture-specific flags and dependencies.
#
# This file is subject to the terms and conditions of the GNU General Public
# License.  See the file "COPYING" in the main directory of this archive
# for more details.
#
# Copyright (C) 2012, ARM Ltd.
# Author: Will Deacon <will.deacon@arm.com>
#
# Based on the ia64 boot/Makefile.
#

include $(srctree)/arch/arm64/boot/dts/Makefile

OBJCOPYFLAGS_Image :=-O binary -R .note -R .note.gnu.build-id -R .comment -S

targets := Image Image.bz2 Image.gz Image.lz4 Image.lzma Image.lzo dtbo.img

DTB_NAMES := $(subst $\",,$(CONFIG_BUILD_ARM64_APPENDED_DTB_IMAGE_NAMES))
ifneq ($(DTB_NAMES),)
DTB_LIST := $(addsuffix .dtb,$(DTB_NAMES))
else
DTB_LIST := $(dtb-y)
endif
DTB_OBJS := $(shell find $(obj)/dts/ -name \*.dtb)

# Add RTIC DTB to the DTB list if RTIC MPGen is enabled
# Note, we keep this for compatibility with
# BUILD_ARM64_APPENDED_DTB_IMAGE targets.
# The rtic_mp.dts would be generated with the vmlinux if
# MPGen enabled (RTIC_MPGEN defined).
ifdef RTIC_MPGEN
DTB_OBJS += rtic_mp.dtb
endif

rtic_mp.dtb: vmlinux FORCE
	$(DTC) -O dtb -o rtic_mp.dtb -b 0 $(DTC_FLAGS) rtic_mp.dts

$(obj)/Image: vmlinux FORCE
	$(call if_changed,objcopy)

$(obj)/Image.bz2: $(obj)/Image FORCE
	$(call if_changed,bzip2)

$(obj)/Image-dtb-hdr: $(obj)/Image FORCE
	echo -n 'UNCOMPRESSED_IMG' > $@ && \
	$(call size_append, $(filter-out FORCE,$^)) >> $@

$(obj)/Image-dtb: $(obj)/Image-dtb-hdr $(obj)/Image $(DTB_OBJS) FORCE
	$(call if_changed,cat)

$(obj)/Image.gz: $(obj)/Image FORCE
	$(call if_changed,gzip)

$(obj)/Image.lz4: $(obj)/Image FORCE
	$(call if_changed,lz4)

$(obj)/Image.lzma: $(obj)/Image FORCE
	$(call if_changed,lzma)

$(obj)/Image.lzo: $(obj)/Image FORCE
	$(call if_changed,lzo)

$(obj)/Image.gz-dtb: $(obj)/Image.gz $(DTB_OBJS) FORCE
	$(call if_changed,cat)

# dtbo.img：条目顺序必须与官方 dtbo.img 完全一致。
# bootloader 用 androidboot.dtbo_idx 硬索引（本机 idx=14 -> lito-v2-mtp-overlay.dtbo），
# 顺序错一位就会加载到别的机型 overlay，表现为黑屏/卡第一屏。
DTBO_IMG_ORDER := \
	lagoon-mtp-overlay.dtbo \
	lito-v2-qrd-overlay.dtbo \
	lito-v2-cdp-overlay.dtbo \
	lagoon-rumi-overlay.dtbo \
	lito-atp-overlay.dtbo \
	lagoon-qrd-overlay.dtbo \
	lito-v2-atp-lcd-overlay.dtbo \
	lagoon-atp-overlay.dtbo \
	lagoon-mtp-usbc-overlay.dtbo \
	lagoon-cdp-overlay.dtbo \
	lito-mtp-overlay.dtbo \
	lito-cdp-overlay.dtbo \
	lito-qrd-overlay.dtbo \
	lito-rumi-overlay.dtbo \
	lito-v2-mtp-overlay.dtbo \
	lito-v2-atp-overlay.dtbo \
	orchid-cdp-overlay.dtbo \
	orchid-mtp-overlay.dtbo

DTBO_OBJS := $(addprefix $(obj)/dts/vendor/qcom/,$(DTBO_IMG_ORDER))

$(obj)/dtbo.img: $(DTBO_OBJS) FORCE
	$(call if_changed,mkdtimg)

install:
	$(CONFIG_SHELL) $(srctree)/$(src)/install.sh $(KERNELRELEASE) \
	$(obj)/Image System.map "$(INSTALL_PATH)"

zinstall:
	$(CONFIG_SHELL) $(srctree)/$(src)/install.sh $(KERNELRELEASE) \
	$(obj)/Image.gz System.map "$(INSTALL_PATH)"
