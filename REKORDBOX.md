# Pioneer Rekordbox & DJ Software Guide

This edition of Zotify includes tailored features and optimizations for **Pioneer Rekordbox**, **Engine DJ**, **Traktor**, **Serato**, and club-standard CDJ/XDJ hardware.

---

## 🎧 Key Features for DJs

- **Lossless FLAC Downloads (`--codec flac`)**  
  High-fidelity FLAC audio output without lossy compression or bitrate degradation. Perfect for large sound systems and club setups.
  
- **Rekordbox-Ready Metadata & Tags**  
  Accurate Vorbis Comment tags automatically written to every audio file:
  - **Title** (`TITLE`) & **Artist** (`ARTIST`)
  - **Album** (`ALBUM`) & **Album Artist** (`ALBUMARTIST`)
  - **Track Number** & **Disc Number** (`TRACKNUMBER`, `DISCNUMBER`, `TRACKTOTAL`)
  - **Release Year** (`DATE`)
  - **Genre** (`GENRE`)
  - **ISRC Code** (`ISRC`) — enables Pioneer CDJ/Rekordbox duplicate detection and track history.
  
- **High-Resolution Embedded Album Art**  
  Album art is embedded directly into the FLAC picture metadata block. Artwork displays automatically on **CDJ-3000**, **CDJ-2000NXS2**, **Opus Quad**, **XDJ-XZ**, **XDJ-RX3**, and inside the Rekordbox library browser.

- **1-Click Crate Import via M3U8 Playlists**  
  Zotify exports UTF-8 `.m3u8` playlist files with relative paths (`--export-m3u8 True`). Drag and drop the `.m3u8` directly into Rekordbox to instantly recreate your Spotify playlists as DJ Crates with the original track order preserved.

- **Smart Metadata Caching & Network Reconnection**  
  Caches metadata locally on disk to prevent rate limits and automatically reconnects if Wi-Fi drops while downloading on tour or backstage.

- **Windows UTF-8 / Unicode Safety**  
  Special characters, Cyrillic, Japanese kanji, and accented artist/title names are handled cleanly without encoding errors or corrupted file names.

---

## ⚙️ Recommended `config.json` for Rekordbox

To set up your Zotify installation for DJ workflows, configure your `config.json` (located in `C:\Users\<USERNAME>\AppData\Roaming\Zotify\config.json` on Windows):

```json
{
  "DOWNLOAD_FORMAT": "flac",
  "DOWNLOAD_QUALITY": "very_high",
  "EXPORT_M3U8": true,
  "M3U8_REL_PATHS": true,
  "OUTPUT_PLAYLIST_EXT": "{playlist}/{playlist_num} - {artist} - {name}",
  "OUTPUT_ALBUM": "{album_artist}/{album}/{track_number} - {artist} - {name}",
  "OUTPUT_SINGLE": "Singles/{artist} - {name}",
  "MD_DISC_TRACK_TOTALS": true,
  "MD_SAVE_GENRES": true,
  "BULK_WAIT_TIME": 1.0,
  "RETRY_ATTEMPTS": 3,
  "RETRY_DELAY": 5.0
}
```

---

## 🚀 Step-by-Step: Importing into Pioneer Rekordbox

### 1. Download your Spotify Playlist / Crate
Run Zotify with playlist URL and M3U8 export:
```bash
zotify -e True <SPOTIFY_PLAYLIST_URL>
```
*Tip: If you've set `"EXPORT_M3U8": true` in your config, simply run:*
```bash
zotify <SPOTIFY_PLAYLIST_URL>
```

### 2. Locate Your Files
Zotify will download your tracks into your music directory:
```
~/Music/Zotify Music/
└── My DJ Set/
    ├── My DJ Set.m3u8
    ├── 01 - Artist A - Track Name.flac
    ├── 02 - Artist B - Track Name.flac
    └── ...
```

### 3. Import into Rekordbox
1. Open **Pioneer Rekordbox** (Export Mode or Performance Mode).
2. In the left navigation panel, expand the **Playlists** section.
3. Either:
   - **Drag & drop** the generated `My DJ Set.m3u8` file directly into the **Playlists** section; **OR**
   - Right-click **Playlists** -> choose **Import Playlist** -> select the `.m3u8` file.
4. Rekordbox will immediately populate the playlist with all tracks in exact order, with complete tags and embedded cover artwork.
5. Rekordbox will automatically analyze BPM, Key, and Beatgrid.

---

## 🎛️ Hardware Compatibility Notes

| Hardware | FLAC Support | Recommended Format |
| :--- | :--- | :--- |
| **Pioneer CDJ-3000 / CDJ-2000NXS2** | Native (16/24-bit, 44.1/48/96kHz) | FLAC |
| **Pioneer Opus Quad / XDJ-XZ / XDJ-RX3** | Native | FLAC |
| **Pioneer XDJ-1000MK2 / XDJ-700** | Native (MK2) / AAC/MP3 (700) | FLAC (MK2), MP3/AAC (700) |
| **Older CDJs (CDJ-2000 original / CDJ-900 / CDJ-850)** | Not Supported | Use `--codec mp3` or `--codec aac` |

*Note for older CDJ hardware: If your club or venue uses legacy CDJs that do not read FLAC files, you can download in high-bitrate MP3 or AAC using:*
```bash
zotify --codec mp3 -b 320k <URL>
```

---

## 💡 Pro Tips for DJs

1. **Keep Original Order:**  
   Using `{playlist_num}` in `OUTPUT_PLAYLIST_EXT` guarantees the tracks stay in the intended track order even when browsing raw USB directories.
2. **Harmonic Mixing:**  
   Rekordbox will analyze the key and BPM as soon as tracks are imported. Use Rekordbox's traffic light feature to find matching keys for smooth transitions.
3. **Rekordbox Export to USB:**  
   Once imported into Rekordbox, select the playlist, right-click, and choose **Export Playlist to Device** to transfer to your FAT32 or exFAT USB flash drive ready for club CDJs.
