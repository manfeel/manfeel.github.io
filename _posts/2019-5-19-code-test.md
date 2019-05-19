---
layout: text
title: code test
---

```
diff --git a/src/flash/nor/Makefile.am b/src/flash/nor/Makefile.am
index 4c94c28c..ae2fd7f1 100644
--- a/src/flash/nor/Makefile.am
+++ b/src/flash/nor/Makefile.am
@@ -41,7 +41,6 @@ NOR_DRIVERS = \
     %D%/lpcspifi.c \
     %D%/max32xxx.c \
     %D%/mdr.c \
-    %D%/msp432.c \
     %D%/mrvlqspi.c \
     %D%/niietcm4.c \
     %D%/non_cfi.c \
@@ -80,5 +79,4 @@ NORHEADERS = \
     %D%/imp.h \
     %D%/non_cfi.h \
     %D%/ocl.h \
-    %D%/spi.h \
-    %D%/msp432.h
+    %D%/spi.h
diff --git a/src/flash/nor/drivers.c b/src/flash/nor/drivers.c
index d4628362..bb2e5840 100644
--- a/src/flash/nor/drivers.c
+++ b/src/flash/nor/drivers.c
@@ -54,7 +54,7 @@ extern const struct flash_driver lpcspifi_flash;
 extern const struct flash_driver max32xxx_flash;
 extern const struct flash_driver mdr_flash;
 extern const struct flash_driver mrvlqspi_flash;
-extern const struct flash_driver msp432_flash;
+//extern const struct flash_driver msp432_flash;
 extern const struct flash_driver niietcm4_flash;
 extern const struct flash_driver nrf5_flash;
 extern const struct flash_driver nrf51_flash;
@@ -84,8 +84,8 @@ extern const struct flash_driver xcf_flash;
 extern const struct flash_driver xmc1xxx_flash;
 extern const struct flash_driver xmc4xxx_flash;

-extern struct flash_driver plugin_flash;
-extern struct flash_driver msp432p4_flash;
+//extern struct flash_driver plugin_flash;
+//extern struct flash_driver msp432p4_flash;
 extern struct flash_driver aducm302x_flash;
 extern struct flash_driver aducm4x50_flash;

@@ -131,7 +131,7 @@ static const struct flash_driver * const flash_drivers[] = {
     &max32xxx_flash,
     &mdr_flash,
     &mrvlqspi_flash,
-    &msp432_flash,
+//    &msp432_flash,
     &niietcm4_flash,
     &nrf5_flash,
     &nrf51_flash,
@@ -160,8 +160,8 @@ static const struct flash_driver * const flash_drivers[] = {
     &xmc1xxx_flash,
     &xmc4xxx_flash,
     &w600_flash,
-    &msp432p4_flash,
-    &plugin_flash,
+//    &msp432p4_flash,
+//    &plugin_flash,
     NULL,
 };

diff --git a/src/rtos/rtos.c b/src/rtos/rtos.c
index 734a0374..87bf8ba5 100644
--- a/src/rtos/rtos.c
+++ b/src/rtos/rtos.c
@@ -36,7 +36,7 @@ extern struct rtos_type chromium_ec_rtos;
 extern struct rtos_type embKernel_rtos;
 extern struct rtos_type mqx_rtos;
 extern struct rtos_type uCOS_III_rtos;
-extern struct rtos_type multicore_rtos;
+//extern struct rtos_type multicore_rtos;
 extern struct rtos_type nuttx_rtos;
 extern struct rtos_type hwthread_rtos;

@@ -51,7 +51,7 @@ static struct rtos_type *rtos_types[] = {
     &mqx_rtos,
     &uCOS_III_rtos,
     &nuttx_rtos,
-    &multicore_rtos,
+//    &multicore_rtos,
     &hwthread_rtos,
     NULL
 };
diff --git a/src/target/aarch64.c b/src/target/aarch64.c
index 64c5a9cd..20e4f0ed 100644
--- a/src/target/aarch64.c
+++ b/src/target/aarch64.c
@@ -2055,7 +2055,7 @@ static int aarch64_read_cpu_memory(struct target *target,
         int wordCount = ((size * count) + delta + 3) / 4;
         uint8_t *pTmp = (uint8_t *)malloc(wordCount * 4);
         int r = aarch64_read_cpu_memory(target, alignedAddr, 4, wordCount, pTmp);
- if (r == ERROR_SUCCESS)
+ if (r == 0)
         {
             memcpy(buffer, pTmp + delta, size * count);
         }
```