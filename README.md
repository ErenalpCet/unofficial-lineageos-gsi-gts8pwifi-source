# Unofficial LineageOS 23.2 GSI for Samsung Galaxy Tab S8+ (`gts8pwifi`)

[![Android 16](https://img.shields.io/badge/Android-16-3DDC84?logo=android&logoColor=white)](https://www.android.com)
[![LineageOS](https://img.shields.io/badge/LineageOS-23.2-167C80?logo=lineageos&logoColor=white)](https://lineageos.org)
[![Device](https://img.shields.io/badge/Device-gts8pwifi-blue)](#)

This repository contains the overlays, custom modifications, removal scripts, and prebuilt configurations used to build an **Unofficial LineageOS 23.2 (Android 16) GSI** tailored specifically for the **Samsung Galaxy Tab S8+ (`gts8pwifi`)**, built on top of [MisterZtr's treble manifest](https://github.com/MisterZtr/treble_manifest).

---

## 📖 About LineageOS

LineageOS is a free, community-built, aftermarket firmware distribution of Android 16, designed to increase performance and reliability over stock Android for your device.

LineageOS is based on the Android Open Source Project (AOSP) with extra contributions from many people within the Android community. It can be used without any need to have Google applications installed. Linked below is a package that has come from another Android project that restores the Google parts. LineageOS does still include various hardware-specific code, which is also slowly being open-sourced anyway.

---

## ⚠️ Disclaimer

> **Your warranty is now void.**
>
> We are not responsible for bricked devices, dead SD cards, thermonuclear war, or you getting fired because the alarm app failed. Please do some research if you have any concerns about features included in this ROM before flashing it!
>
> **YOU** are choosing to make these modifications, and if you point the finger at us for messing up your device, we will laugh at you.

---

## 📊 Feature Status

| Feature | State | Notes |
| :--- | :--- | :--- |
| **Boot** | 🟢 Supported | |
| **Wi-Fi** | 🟢 Supported | |
| **Bluetooth** | 🟢 Supported | Audio supported |
| **Audio Playback** | 🟢 Supported | |
| **Video Playback** | 🟢 Supported | |
| **Sensors** | 🟢 Supported | |
| **GNSS (GPS)** | 🟢 Supported | |
| **Camera** | 🟢 Supported | Photo & Video |
| **Touch Screen** | 🟢 Supported | |
| **S-Pen** | 🟢 Supported | S-Pen button clicks supported |
| **Autobrightness** | 🟢 Supported | |
| **Double Tap to Wake (DT2W)** | 🟢 Supported | |
| **Vibration** | 🟢 Supported | |
| **USB** | 🟢 Supported | |
| **SELinux** | 🟢 Supported | Enforcing |
| **Hardware Encryption** | 🟢 Supported | |
| **Fingerprint Sensor** | 🔴 NOT WORKING | HAL/Calibration issues (removed from build) |
| **Official Book Cover Keyboard & Trackpad** | 🟡 Not tested yet | |
| **Casting** | 🟡 Not tested yet | |
| **Video over USB-C** | 🟡 Not tested yet | |
| **OTG over USB-C** | 🟡 Not tested yet | |

---

## 🛠️ Notes About the Build & Included Apps

* **Tablet Properties:** Hardcoded system properties for tablet configuration so you don't have to deal with missing cellular interfaces or modem errors.
* **Dialer & Messaging Removed:** Removed unused `Dialer` and `Messaging` applications since there is no modem on the Wi-Fi variant.
* **Lock Screen Safety:** The power menu does not show up on the lock screen when pressed via the physical button to avoid accidents and unwanted reboots.
* **Fingerprint Removal:** Fingerprint functionality was removed from the build tree due to HAL and calibration complexities on `gts8pwifi`.
* **Included Prebuilts:**
  * **F-Droid & Aurora Store:** Alternative app store support out of the box.
  * **Fossify Notes & Fossify Paint:** Note-taking and drawing tools suitable for S-Pen usage.
  * **Button Mapper:** Included to remap the S-Pen button (by default, pressing the button outputs PROG_RED).
  * **Pride App:** Included in the build (source code available on GitHub).
* **Maybe More?**

---

## 🛠️ How to Build from Source

### Prerequisites
* Java 17 set as default (e.g., `archlinux-java` on Arch Linux or `update-java-alternatives` on Ubuntu).
* Git, Repo tool, and standard Android compilation tools installed.

---

### Step 1: Create Directories & Initialize Repo

```bash
mkdir LineageOS
cd LineageOS

# Initialize local repository
repo init -u [https://github.com/LineageOS/android.git](https://github.com/LineageOS/android.git) -b lineage-23.2 --git-lfs

```

---

### Step 2: Clone Treble Manifest & Sync Source

```bash
# Clone the Treble manifest for GSI dependencies
git clone [https://github.com/MisterZtr/treble_manifest.git](https://github.com/MisterZtr/treble_manifest.git) .repo/local_manifests -b lineage-23.2

# Sync the source code
repo sync --force-sync --optimized-fetch --no-tags --no-clone-bundle --prune -j4

```

---

### Step 3: Apply Treble Patches & GTS8PWIFI Customizations

```bash
# 1. Apply base Treble GSI patches
bash LineageOS_gsi/patches/apply-patches.sh .

# 2. Clone this overlay & customization repository
git clone [https://github.com/ErenalpCet/unofficial-lineageos-gsi-gts8pwifi-source.git](https://github.com/ErenalpCet/unofficial-lineageos-gsi-gts8pwifi-source.git) ~/gsi_changes

# 3. Copy gts8pwifi overlays, prebuilts, and keymappings to source tree
cp -r ~/gsi_changes/source_files/* .

# 4. Remove stock telephony/fingerprint components
bash ~/gsi_changes/apply_deletions.sh ~/gsi_changes/deleted_files.txt

```

---

### Step 4: Setup Ccache Environment

Add or export the following variables to speed up rebuilds:

```bash
export USE_CCACHE=1
export CCACHE_COMPRESS=1
export CCACHE_MAXSIZE=50G

# Apply to current session
ccache -M 50G -F 0

```

---

### Step 5: Build Android System Image

Source the environment setup script and build your target variant:

```bash
. build/envsetup.sh

```

Choose your desired variant target and build:

* **GAPPS version with ext4:**
```bash
breakfast lineage_arm64_bgN4-bp4a-userdebug
make systemimage -j$(nproc --all)

```


* **GAPPS version with erofs:**
```bash
breakfast lineage_arm64_bgNE-bp4a-userdebug
make systemimage -j$(nproc --all)

```


* **VANILLA version with ext4:**
```bash
breakfast lineage_arm64_bvN4-bp4a-userdebug
make systemimage -j$(nproc --all)

```


* **VANILLA version with erofs:**
```bash
breakfast lineage_arm64_bvNE-bp4a-userdebug
make systemimage -j$(nproc --all)

```



---

## 📥 Flashing Instructions

1. Download the latest release `.7z` / `.img` file.
2. Reboot to `fastbootd` *(Refer to [XDA fastbootd guide](https://xdaforums.com/t/patch-modify-stock-recovery-with-fastbootd-only-dynamic-samsung-devices-twrp-alternative.4643956/))*.
3. Factory reset and wipe cache partitions through recovery.
4. Flash the GSI system image:
```bash
fastboot flash system system.img

```


5. Reboot back to LineageOS! :3

---

## 🤝 Credits & Acknowledgments

These people have helped this project in some way or another, so they should be the ones who receive all the credit:

* **LineageOS Team**
* **MisterZtr**
* **Phhusson**
* **AndyYan**
* **Ponces**
* **Peter Cai**
* **Iceows**
* **ChonDoit**
* **Nazim N**
* **Ahnet**
* **mytja**
* **cawilliamson**
* **Doze-off**
* **ErenalpCet** (Custom `gts8pwifi` overlays & device customizations)
