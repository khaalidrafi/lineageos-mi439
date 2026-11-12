# lineageos-mi439

LineageOS build for **Xiaomi Mi439 — variant3 = Redmi 8A** (models M1908C3KE / KG / KH / KI).

Guide: https://wiki.lineageos.org/devices/Mi439/build/variant3/

Device facts (from the official wiki spec): Qualcomm SDM439 (Snapdragon 439),
kernel `android_kernel_xiaomi_msm8937` 4.19, device tree
`android_device_xiaomi_Mi439` + shared `android_device_xiaomi_mithorium-common`,
current branch **lineage-23.2** (supported here: 19.1, 20, 21, 22.2, 23.0, 23.2).

## How to build

Run the `los-mi439-build` workflow (**Actions → los-mi439-build → Run workflow**).
Inputs: `runner` (runner label), `branch` (default lineage-23.2), `device`
(default Mi439). The workflow follows the wiki guide exactly:
`repo init → local_manifests → repo sync → breakfast → blobs → brunch → artifacts`
(`lineage-<ver>-UNOFFICIAL-Mi439.zip` + `recovery.img`, 7-day artifact retention).

## Hardware reality check

| branch | wiki storage | wiki RAM |
| --- | --- | --- |
| lineage-21+ | 400 GB | 64 GB |
| lineage-18.1 | 300 GB | 32 GB |
| ≤ lineage-17.1 | 200 GB | 16 GB |

GitHub free `ubuntu-latest` runners provide ~14–22 GB disk and 16 GB RAM, so a
free-runner attempt fails at the **preflight** step with the exact numbers and
an annotated reason — it does not die an hour into `repo sync`. To complete a
real build, re-run the workflow with `runner` pointed at a machine that meets
the table above (self-hosted runner or a paid larger runner); the workflow
needs no changes.

## Proprietary blobs

variant3's `before_install` is `needs_specific_android_fw`. In CI there is no
device attached, so blobs come from either:

1. a runner with a Redmi 8A attached — `./extract-files.py` inside
   `device/xiaomi/Mi439` (wiki step 4), or
2. a **verified** blob-source manifest committed to `local_manifests/`.

The classic CI mirror (TheMuppets/proprietary_vendor_xiaomi) was taken down by
DMCA (github/dmca 2019-08-09). No unverified mirror is hardcoded here on
purpose — verify a source first, then drop its manifest XML into
`local_manifests/` and re-run the workflow.
