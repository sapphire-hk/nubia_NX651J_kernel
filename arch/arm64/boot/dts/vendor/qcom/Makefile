# NX651J: 按官方规格编译 DT
#
# 结构与官方 boot.img / dtbo.img 完全一致：
#   base    : 4 个 SoC（lagoon / lito / lito-v2 / orchid）
#   overlay : 18 个 board 变体（lagoon 6 + lito 12，其中 orchid 2 个）
#
# 本机实际使用：lito-v2.dtb + lito-v2-mtp-overlay.dtbo
# （adb 实测 model "Lito v2 MTP", board-id <8 0>, msm-id <400/440 0x20000>）
# 其余变体保留，以保证 dtbo 表结构与官方规格一致。
#
# DTC_FLAGS 说明:
#   -@  base dtb 必须带 __symbols__，overlay 才能通过 __fixups__ 解析 phandle
#   -f  lagoon 的 dts 存在 duplicate_property_names（qcom,mdss-dsi-bl-max-level），
#       官方用旧版 dtc 或内部已修，开源包里直接编会中断整个 dtbs。
#       加 -f 强制输出，仅影响 lagoon 变体（本机不使用）。

DTC_FLAGS += -@ -f

# ---- SoC base ----
dtb-y += lagoon.dtb lito.dtb lito-v2.dtb orchid.dtb

# ---- lagoon overlays (6) ----
dtbo-y += \
	lagoon-rumi-overlay.dtbo \
	lagoon-mtp-overlay.dtbo \
	lagoon-mtp-usbc-overlay.dtbo \
	lagoon-cdp-overlay.dtbo \
	lagoon-atp-overlay.dtbo \
	lagoon-qrd-overlay.dtbo

lagoon-rumi-overlay.dtbo-base      := lagoon.dtb
lagoon-mtp-overlay.dtbo-base       := lagoon.dtb
lagoon-mtp-usbc-overlay.dtbo-base  := lagoon.dtb
lagoon-cdp-overlay.dtbo-base       := lagoon.dtb
lagoon-atp-overlay.dtbo-base       := lagoon.dtb
lagoon-qrd-overlay.dtbo-base       := lagoon.dtb

# ---- lito overlays (10) ----
dtbo-y += \
	lito-rumi-overlay.dtbo \
	lito-mtp-overlay.dtbo \
	lito-v2-mtp-overlay.dtbo \
	lito-cdp-overlay.dtbo \
	lito-v2-cdp-overlay.dtbo \
	lito-atp-overlay.dtbo \
	lito-v2-atp-overlay.dtbo \
	lito-v2-atp-lcd-overlay.dtbo \
	lito-qrd-overlay.dtbo \
	lito-v2-qrd-overlay.dtbo

lito-rumi-overlay.dtbo-base        := lito.dtb lito-v2.dtb
lito-mtp-overlay.dtbo-base         := lito.dtb
lito-v2-mtp-overlay.dtbo-base      := lito-v2.dtb
lito-cdp-overlay.dtbo-base         := lito.dtb
lito-v2-cdp-overlay.dtbo-base      := lito-v2.dtb
lito-atp-overlay.dtbo-base         := lito.dtb
lito-v2-atp-overlay.dtbo-base      := lito-v2.dtb
lito-v2-atp-lcd-overlay.dtbo-base  := lito-v2.dtb
lito-qrd-overlay.dtbo-base         := lito.dtb
lito-v2-qrd-overlay.dtbo-base      := lito-v2.dtb

# ---- orchid overlays (2) ----
dtbo-y += \
	orchid-mtp-overlay.dtbo \
	orchid-cdp-overlay.dtbo

orchid-mtp-overlay.dtbo-base := orchid.dtb
orchid-cdp-overlay.dtbo-base := orchid.dtb

always		:= $(dtb-y)
subdir-y	:= $(dts-dirs)
clean-files	:= *.dtb *.dtbo
