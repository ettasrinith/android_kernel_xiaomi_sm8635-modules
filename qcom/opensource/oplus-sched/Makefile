# SPDX-License-Identifier: GPL-2.0-only
#
# OPlus sched/frame/vip module stack for peridot (SM8635, ACK android14-6.1).
# Sources: hoshikv/oplus_drivers_sm8635 (GPL-2.0), already adapted for the
# Xiaomi GKI tree (OTS_IDX=5, slot-occupancy self-check at load time).
#
# Built as ONE kbuild unit so the cross-module symbol references
# (sched_assist <-> schedtune, eas_opt, frame_boost, vip_binder) resolve in a
# single modpost pass - no KBUILD_EXTRA_SYMBOLS chaining needed.
#
# Feature defines are the union set: every object must see the SAME
# struct oplus_task_struct layout, so the CONFIG_* set is identical for all
# modules (kept in sync with hoshikv's build.sh FB_SCHED_FEAT).
#
# Load order on device (dependency chain, see init.oplusmods.sh):
#   oplus_bsp_schedtune.ko
#   oplus_bsp_sched_assist.ko
#   oplus_bsp_eas_opt.ko
#   oplus_bsp_frame_boost.ko
#   oplus_vip_binder.ko

LINUXINCLUDE += -I$(srctree)/

ccflags-y += -I$(src)
ccflags-y += -DCONFIG_OPLUS_SYSTEM_KERNEL_QCOM=1
ccflags-y += -DCONFIG_OPLUS_SCHED_TUNE=1
ccflags-y += -DCONFIG_OPLUS_FEATURE_SCHED_ASSIST=1
ccflags-y += -DCONFIG_OPLUS_FEATURE_SCHED_DDL=1
ccflags-y += -DCONFIG_OPLUS_SCHED_GROUP_OPT=1
ccflags-y += -DCONFIG_OPLUS_CPU_AUDIO_PERF=1
ccflags-y += -DCONFIG_OPLUS_FEATURE_LOADBALANCE=1
ccflags-y += -DCONFIG_OPLUS_FEATURE_PIPELINE=1
ccflags-y += -DCONFIG_BLOCKIO_UX_OPT=1
ccflags-y += -DCONFIG_OPLUS_FEATURE_FRAME_BOOST=1
ccflags-y += -DCONFIG_OPLUS_FEATURE_EAS_OPT=1
ccflags-y += -DCONFIG_OPLUS_FEATURE_VT_CAP=1
ccflags-y += -DCONFIG_OPLUS_CPUFREQ_IOWAIT_PROTECT=1

# schedtune: boost-group data layer (schedtune_task_boost, stune_util, ...).
# Inert until sched_assist drives it via the cgroup/task_group kabi slot.
obj-m += oplus_bsp_schedtune.o
oplus_bsp_schedtune-y := sched_tune/tune.o sched_tune/license.o

# sched_assist: the vendor-hook glue. Allocates struct oplus_task_struct per
# task (android_oem_data1 slot 5, with a slot-occupancy self-check that
# aborts the load instead of panicking), registers the fair.c/core.c
# scheduler hooks for ux/top-app boost, and exposes /proc/oplus_scheduler.
obj-m += oplus_bsp_sched_assist.o
oplus_bsp_sched_assist-y := sched_assist/sched_assist.o sched_assist/sa_common.o sched_assist/sa_sysfs.o sched_assist/sa_exec.o sched_assist/sa_fair.o sched_assist/sa_jankinfo.o sched_assist/sa_oemdata.o sched_assist/sa_priority.o sched_assist/sa_ddl.o sched_assist/sa_group.o sched_assist/sa_audio.o sched_assist/sa_balance.o sched_assist/sa_pipeline.o sched_assist/sa_blockio.o sched_assist/sa_hmbird.o

# eas_opt: EAS vtime compensation (android_rvh_place_entity) and cpu capacity
# control via /proc/eas_opt. Inert until /proc/eas_opt/eas_opt_enable is set.
obj-m += oplus_bsp_eas_opt.o
oplus_bsp_eas_opt-y := eas_opt/eas_opt.o eas_opt/oplus_cap.o eas_opt/oplus_iowait.o
