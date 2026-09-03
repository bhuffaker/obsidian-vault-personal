#hardware computer pc 

# Current Hardware Inventory

**System:** CyberPowerPC prebuilt · Base configuration $2,835 · As-configured $3,374 **Documented:** 2026-08-27**Source:** OEM build sheet, verified against physical inspection and HWiNFO64 measurements

---

## Summary

| Component   | Spec                                             |
| ----------- | ------------------------------------------------ |
| CPU         | Intel Core i9-10900K                             |
| Motherboard | ASUS ProArt Z490-Creator 10G                     |
| GPU         | NVIDIA RTX 3080 10 GB GDDR6X (MSI)               |
| RAM         | 64 GB DDR4-3200 (4×16 GB)                        |
| Boot drive  | 1 TB WD Blue SN550 NVMe                          |
| Secondary   | 3 TB SATA III HDD                                |
| External    | 4 TB ADATA HD830 USB                             |
| PSU         | Enermax Revolution D.F. 850 W (ERF850AWT)        |
| Cooling     | DeepCool Castle 360EX ARGB 360 mm AIO            |
| Case        | Phanteks Evolv X ATX Mid-Tower (Special Edition) |
| OS          | Windows 11                                       |

---

## CPU

**Intel Core i9-10900K**

| Architecture    | Comet Lake (14 nm)       |
| --------------- | ------------------------ |
| Cores / Threads | 10 / 20                  |
| Base clock      | 3.70 GHz                 |
| Turbo           | 5.20 GHz                 |
| Cache           | 20 MB                    |
| Socket          | LGA1200                  |
| TDP (PL1)       | 125 W                    |
| PL2             | 250 W                    |
| PCIe            | 16 lanes, **Gen 3 only** |
| Overclocking    | None applied             |
  
**Measured:** 245 W package power max, 88 °C max under OCCT Power test.

**Notes:**

- PL2 confirmed unrestricted — the board is not enforcing a 125 W PL1
- 88 °C at 245 W is warmer than expected for a 360 mm AIO; likely dried thermal paste after ~6 years
- LGA1200 is a dead-end platform — the only upgrade is the i9-11900K, which has fewer cores

---

## Motherboard

**ASUS ProArt Z490-Creator 10G** · ATX · Z490 chipset

### PCIe slot layout

|Slot|Source|Mode|Occupied by|
|---|---|---|---|
|PCIEX16_1|CPU|x16 (→ x8 if slot 2 populated)|RTX 3080|
|PCIEX16_2|CPU|x8|_empty_|
|PCIEX16_3|Chipset|x4|ASUS Hyper 10G LAN card|
|PCIEX1_1|Chipset|x1|Killer AX1650 Wi-Fi|
|PCIEX1_2|Chipset|x1|_empty_|

Supported configurations: `x16` · `x8/x8` · `x8/x4+x4`

### Storage interfaces

| |Detail|
|---|---|
|SATA|6 × SATA III|
|M.2_1|Occupied — WD Blue SN550|
|M.2_2|**Free** — defaults to PCIe 3.0 **x2**|

### ⚠️ Known conflicts

- **Thunderbolt 3 is disabled.** The chipset x4 lanes switch from TB3 to PCIEX16_3 when that slot is populated. The 10G LAN card occupies it, so both TB3 ports have no output.
- **M.2_2 shares bandwidth with SATA6G_5 and SATA6G_6.** Setting M.2_2 to x4 disables those ports. ⚠️ _Unverified: which SATA port the 3 TB HDD uses._
- **M.2_1 in SATA mode disables SATA6G_2.** Not applicable — SN550 is NVMe.

**Everything is PCIe Gen 3.** Comet Lake provides no Gen 4 support regardless of what a device is rated for.

---

## Graphics

**NVIDIA GeForce RTX 3080 10 GB** (MSI) · VR Ready · Single card


|Architecture|Ampere, GA102|
|---|---|
|VRAM|10 GB GDDR6X|
|Bus width|320-bit|
|Bandwidth|~760 GB/s|
|TGP|320 W|
|Power connectors|**2 × 8-pin** (verified by inspection)|
|Form factor|~2.5 slot|

**Measured:** 319 W max under load.

**Notes:**

- ⚠️ **10 GB, not 12 GB** — the 12 GB variant is a different SKU with a 384-bit bus
- No FP8 support, no DLSS Frame Generation (Ada/Blackwell exclusive)
- 10 GB is the system's weakest forward-looking spec for 1440p gaming

---

## Memory

**64 GB DDR4-3200** · 4 × 16 GB · Dual channel

Native Comet Lake support is DDR4-2933; 3200 runs via XMP. No upgrade needed for any planned workload.

---

## Storage

|Device|Capacity|Interface|Role|
|---|---|---|---|
|WD Blue SN550|1 TB|M.2 NVMe Gen3, DRAM-less|Boot, Windows, games|
|Generic HDD|3 TB|SATA III|Bulk storage|
|ADATA HD830|4 TB|USB, 2.5" mechanical|External / backup|

**Notes:**

- SN550 is DRAM-less with HMB; ~2,400 MB/s sequential
- ⚠️ 3 TB HDD's SATA port number **not yet verified** — needed before enabling M.2_2 at x4
- HD830 is a spinning drive over USB (~110–140 MB/s, low IOPS) — unsuitable for games, VMs, or active model storage
- ⚠️ Free space on C: **not yet measured**

---

## Power Supply

**Enermax Revolution D.F. 850 W** · Model ERF850AWT

| | |
|---|---|
|Capacity|850 W|
|Efficiency|80 Plus Gold|
|Modularity|Fully modular|
|Fan|Twister bearing|
|Rail topology|**Multi-rail** (up to 70.83 A / ~850 W total on +12 V)|
|Standard|ATX 2.x — **no power-excursion rating**|
|Released|~2019|
|Warranty|5 years — **expired**|

### Measured load

| |Watts|
|---|---|
|CPU package (max)|245|
|GPU (max)|319|
|AIO, 6 fans, 10G NIC, Wi-Fi, board, 4 DIMMs, 2 drives|~115|
|**System peak**|**~679 W (80% load)**|

**⚠️ This is the build's limiting component.** Adding a second GPU exceeds safe capacity even with aggressive power capping. Three compounding issues: multi-rail per-rail OCP limits, no ATX 3.0 excursion headroom for GDDR6X transients, and ~7 years of capacitor aging.

---

## Cooling

**DeepCool Castle 360EX ARGB** · 360 mm AIO · Copper cold plate · 3 × 120 mm fans Plus default Phanteks case fans.

⚠️ **Radiator mounting position not verified.** If front-mounted as intake, the GPU breathes radiator exhaust. Top-mounting is preferred when adding a second card.

---

## Case

**Phanteks Evolv X ATX Mid-Tower** (Special Edition) · Anodized aluminum · Tempered glass swing panel · USB-C front I/O

|||
|---|---|
|Expansion slots|7 + 2 vertical|
|Vertical GPU mount|Supported (riser cable required, not included)|
|PSU|Bottom chamber, shrouded|

**Notes:**

- Known for restricted airflow — solid front panel with side intake slits
- ⚠️ **Slot spacing between PCIEX16_1 and PCIEX16_2 not yet measured** — determines maximum second-GPU thickness
- Visible dust accumulation on GPU backplate and shroud

---

## Networking

| | | 
|---|---|
|Onboard|Gigabit LAN|
|Add-in|ASUS Hyper 10G LAN card (PCIEX16_3, x4)|
|Wireless|Killer Wi-Fi 6 AX1650, 2×2, BT 5.0, PCIe adapter + dual antennas|

---

## Operating System

**Windows 10 Home 64-bit**

⚠️ **Action required.** Windows 10 lost security support in October 2025, and Nvidia's Game Ready driver support for RTX GPUs on Windows 10 ends around October 2026.

Upgrade path is free and supported: the 10th-gen CPU clears the Windows 11 requirement, and Z490 provides TPM 2.0 via **Intel PTT** (enable in BIOS → Advanced → PCH-FW Configuration).

---

## Peripherals & extras

CyberPowerPC multimedia USB keyboard · CyberPowerPC 4000 DPI optical mouse with weight system · Onboard 7.1 HD audio · No monitor, headset, speakers, or mousepad included

---

## Warranty

| | |
|---|---|
|Parts|1 year (expired)|
|Service plan|3 years, labor + lifetime technical support|

---

## Open items

|#|Item|Why it matters|
|---|---|---|
|1|Free space on C:|Determines whether an SSD purchase is needed at all|
|2|3 TB HDD's SATA port|Ports 5/6 conflict with M.2_2 at x4|
|3|Slot spacing, PCIEX16_1 → _2|Determines max second-GPU thickness|
|4|AIO radiator orientation|Front-intake preheats the GPUs|
|5|Clearance below slot 2 to PSU shroud|Second-GPU length and sag support|

---

## System strengths and constraints

**Strengths**

- 64 GB RAM — already at target for any planned workload
- Three full-length PCIe slots with an x8/x8 CPU configuration available
- Free M.2_2 slot
- Capable case with vertical mounting as a fallback
- 10 GbE networking

**Constraints**

- PSU capacity — the hard blocker for any second GPU
- 10 GB VRAM on the render card
- PCIe Gen 3 throughout
- LGA1200 dead-end platform
- Restricted case airflow
- Windows 10 driver deadline
- 1 TB boot drive
- Thunderbolt 3 unavailable