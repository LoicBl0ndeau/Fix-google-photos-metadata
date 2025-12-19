# Google Photos EXIF Metadata Fixer (PowerShell)

This PowerShell script restores **EXIF metadata** (date taken and GPS location) to photos and videos exported from **Google Photos**.

When you export your data from Google Photos (via Google Takeout), metadata is often stored in separate `.json` sidecar files instead of being embedded directly in the media files. This script automatically matches media files with their corresponding JSON metadata and uses **ExifTool** to write the correct EXIF information back into the files.

---

## ✨ Features

- 📂 Recursively scans a Google Photos export folder
- 🔎 Automatically matches media files to their JSON metadata (even with renamed files or `(1)`, `(2)` duplicates)
- 🕒 Restores:
  - `DateTimeOriginal`
  - `CreateDate`
- 🌍 Restores GPS data:
  - Latitude / Longitude
  - Altitude
- 🧾 Generates a reusable `fix_exif_metadata.json`
- 🔁 Can reuse an existing metadata file without rescanning
- 🧹 Optional cleanup of Google Photos `.json` sidecar files

---

## 📦 Requirements

- **Windows**
- **PowerShell 5.1+** or **PowerShell Core**
- **ExifTool** installed and available in `PATH`

Download ExifTool:  
https://exiftool.org/

Verify installation:
```powershell
exiftool -ver
````

---

## 📁 Expected Folder Structure

This script expects the standard Google Photos Takeout structure, where media files and their metadata JSON files are in the same folders:

```text
Google Photos/
├── 2019/
│   ├── IMG_1234.jpg
│   ├── IMG_1234.jpg.json
│   ├── VID_5678.mp4
│   └── VID_5678.mp4.json
└── 2020/
    └── ...
```

---

## 🚀 Usage

1. **Clone or download** this repository
2. Open **PowerShell**
3. Run the script:

```powershell
.\fix-google-photos-exif.ps1
```

4. Enter the path to your **unzipped Google Photos export folder**
5. Follow the interactive prompts:

   * Reuse an existing `fix_exif_metadata.json` (if present)
   * Apply EXIF metadata to files
   * Optionally delete `.json` sidecar files after processing

---

## 🧾 Output

* A file named **`fix_exif_metadata.json`** is generated in the root export folder
* This file contains all EXIF corrections and can be reused later with ExifTool:

```powershell
exiftool -r -m -j=fix_exif_metadata.json -overwrite_original <photos_folder>
```

---

## ⚠️ Notes & Limitations

* Files without matching JSON metadata will be reported
* If multiple JSON files match a single media file, the script will warn and skip it
* EXIF GPS data is written in standard EXIF format (N/S/E/W handling included)
* Original files are modified **in place** (no backup copies are created)

---

## 🛡️ Safety Tips

* **Make a backup** of your Google Photos export before running the script
* Test on a small folder first
* Review `fix_exif_metadata.json` if you want full control before applying changes

---

## 🙌 Credits

* Google Photos Takeout format
* Phil Harvey’s **ExifTool**

---

If this script helped you fix your photo library, a ⭐ on GitHub is always appreciated!
