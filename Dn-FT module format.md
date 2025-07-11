# Dn-FamiTracker module file (.dnm) format

- Not yet complete!
- Last updated 2025-07-09
- version 4.50

---

## About

- This text aims to document the binary module file format as of Dn-FamiTracker v0.5.1.1.
- Each section is declared sequentially as it appears in the file.
- Unless specified otherwise, all data is stored in **little-endian** format.
- Strings are stored in **ASCII** encoding unless specified otherwise.
- The term "Song" and "Track" may be used interchangeably due to them being switched around in the source and in earlier documentation.

## How to read this

- This document aims to cover all known versions of the module format of FamiTracker, 0CC-FamiTracker, and Dn-FamiTracker.
- As such, the information will be very dense and wordy. In order to parse this, you have to keep two things in mind:

### Program version

- Keep in mind the program version for analysis.
- For example, I want to review the module format of FamiTracker v0.4.0 stable.

### Block version

- After choosing the relevant program version, use the version chart to identify the block versions used.
- Now, when reading a block format, choose the corresponding row to the version you have in mind.
- For example, I need to analyze the `INSTRUMENTS` block. Since I have chosen FamiTracker v0.4.0 for my analysis, Its block version is 5.

### Module version

- Sometimes, data formats change depending on module version. For instance, the row index in the [Patterns block](Dn-FT%20module%20format.md#Row%20Data) has a different data type depending on the module version or the block version.
- The module version doesn't necessarily change in sync with the program version.
- For example, the module version of FamiTracker v0.4.0 is `0x0430`.

### All together now

- With these in mind, one can now read the data format of any data block.
- Just pay attention to the ***Present in block version*** column if it applies to your block version!

## Version Chart

### FamiTracker

| **_Version_**        | **_Module Version_** | **_PARAMS_** | **_INFO_** | **_TUNING_** | **_HEADER_** | **_INSTRUMENTS_** | **_SEQUENCES_** | **_FRAMES_** | **_PATTERNS_** | **_TRACK_** | **_DPCM SAMPLES_** | **_SEQUENCES_VRC6_** | **_SEQUENCES_N163_** | **_COMMENTS_** | **_SEQUENCES_S5B_** |
| -------------------- | -------------------- | ------------ | ---------- | ------------ | ------------ | ----------------- | --------------- | ------------ | -------------- | ----------- | ------------------ | -------------------- | -------------------- | -------------- | ------------------- |
| **0.2.2 stable**     | 2.00 (0x0200)        | 1            | 1          | -            | -            | 1                 | 1               | 1            | 1              | -           | 1                  | -                    | -                    | -              | -                   |
| **0.2.4 stable**     | 2.01 (0x0201)        | 1            | 1          | -            | 1            | 1                 | 1               | 1            | 1              | -           | 1                  | -                    | -                    | -              | -                   |
| **0.2.5 stable**     | 2.03 (0x0203)        | 1            | 1          | -            | 1            | 1                 | 2               | 1            | 1              | -           | 1                  | -                    | -                    | -              | -                   |
| **0.2.6 stable**     | 2.03 (0x0203)        | 2            | 1          | -            | 2            | 2                 | 2               | 2            | 2              | -           | 1                  | -                    | -                    | -              | -                   |
| **0.2.7 stable**     | 3.00 (0x0300)        | 2            | 1          | -            | 2            | 2                 | 3               | 3            | 3              | -           | 1                  | -                    | -                    | -              | -                   |
| **0.2.8 beta 5**     | 3.00 (0x0300)        | 2            | 1          | -            | 3            | 2                 | 3               | 3            | 3              | -           | 1                  | 1                    | -                    | -              | -                   |
| **0.2.9 stable**     | 3.00 (0x0300)        | 2            | 1          | -            | 3            | 2                 | 3               | 3            | 3              | -           | 1                  | 1                    | -                    | -              | -                   |
| **0.3.0 stable**     | 3.00 (0x0300)        | 2            | 1          | -            | 3            | 2                 | 3               | 3            | 3              | -           | 1                  | 1                    | -                    | -              | -                   |
| **0.3.0 re-release** | 4.10 (0x0410)        | 2            | 1          | -            | 3            | 2                 | 4               | 3            | 3              | -           | 1                  | 1                    | -                    | -              | -                   |
| **0.3.5 beta 0**     | 4.20 (0x0420)        | 3            | 1          | -            | 3            | 2                 | 4               | 3            | 3              | -           | 1                  | 1                    | -                    | -              | -                   |
| **0.3.5 stable**     | 4.20 (0x0420)        | 3            | 1          | -            | 3            | 2                 | 5               | 3            | 3              | -           | 1                  | 5                    | -                    | -              | -                   |
| **0.3.6 beta 1**     | 4.20 (0x0420)        | 3            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | -                    | -              | -                   |
| **0.3.6 beta 2**     | 4.20 (0x0420)        | 3            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | -                    | -              | -                   |
| **0.3.6 beta 3**     | 4.20 (0x0420)        | 3            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | -                    | -              | -                   |
| **0.3.6 beta 4**     | 4.20 (0x0420)        | 4            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | -                    | -              | -                   |
| **0.3.6 stable**     | 4.20 (0x0420)        | 4            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | -                    | -              | -                   |
| **0.3.7 beta 0**     | 4.20 (0x0420)        | 4            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | -                    | -              | -                   |
| **0.3.7 stable**     | 4.20 (0x0420)        | 4            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | -                    | -              | -                   |
| **0.3.8 beta 0**     | 4.20 (0x0420)        | 4            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | -                    | -              | -                   |
| **0.3.8 beta 1**     | 4.20 (0x0420)        | 5            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | 1                    | -              | -                   |
| **0.3.8 beta 2**     | 4.20 (0x0420)        | 5            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | 1                    | -              | -                   |
| **0.3.8 beta 3**     | 4.20 (0x0420)        | 5            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | 1                    | -              | -                   |
| **0.3.8 beta 4**     | 4.20 (0x0420)        | 5            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | 1                    | -              | -                   |
| **0.3.8 beta 5**     | 4.20 (0x0420)        | 5            | 1          | -            | 3            | 2                 | 6               | 3            | 4              | -           | 1                  | 6                    | 1                    | -              | -                   |
| **0.4.0 stable**     | 4.30 (0x0430)        | 6            | 1          | -            | 3            | 5                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | -              | -                   |
| **0.4.1 stable**     | 4.30 (0x0430)        | 6            | 1          | -            | 3            | 5                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | -              | -                   |
| **0.4.2 beta 1**     | 4.40 (0x0440)        | 6            | 1          | -            | 3            | 6                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | 1              | -                   |
| **0.4.2 stable**     | 4.40 (0x0440)        | 6            | 1          | -            | 3            | 6                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | 1              | -                   |
| **0.4.3 beta 1**     | 4.40 (0x0440)        | 6            | 1          | -            | 3            | 6                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | 1              | -                   |
| **0.4.3 beta 2**     | 4.40 (0x0440)        | 6            | 1          | -            | 3            | 6                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | 1              | -                   |
| **0.4.3 stable**     | 4.40 (0x0440)        | 6            | 1          | -            | 3            | 6                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | 1              | -                   |
| **0.4.4 beta 1**     | 4.40 (0x0440)        | 6            | 1          | -            | 3            | 6                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | 1              | -                   |
| **0.4.4 stable**     | 4.40 (0x0440)        | 6            | 1          | -            | 3            | 6                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | 1              | -                   |
| **0.4.5 beta 1**     | 4.40 (0x0440)        | 6            | 1          | -            | 3            | 6                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | 1              | -                   |
| **0.4.5 stable**     | 4.40 (0x0440)        | 6            | 1          | -            | 3            | 6                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | 1              | -                   |
| **0.4.6 stable**     | 4.40 (0x0440)        | 6            | 1          | -            | 3            | 6                 | 6               | 3            | 5              | -           | 1                  | 6                    | 1                    | 1              | -                   |
| **0.5.0 beta 1**     | 4.50 (0x0450)        | 9            | 1          | -            | 4            | 8                 | 6               | 3            | 6              | -           | 1                  | 6                    | 1                    | 1              | 1                   |
| **0.5.0 beta 2**     | 4.50 (0x0450)        | 9            | 1          | -            | 4            | 8                 | 6               | 3            | 6              | -           | 1                  | 6                    | 1                    | 1              | 1                   |
| **0.5.0 beta 3**     | 4.50 (0x0450)        | 9            | 1          | -            | 4            | 8                 | 6               | 3            | 6              | -           | 1                  | 6                    | 1                    | 1              | 1                   |
| **0.5.0 beta 4**     | 4.50 (0x0450)        | 9            | 1          | -            | 4            | 8                 | 6               | 3            | 6              | -           | 1                  | 6                    | 1                    | 1              | 1                   |
| **0.5.0 beta 5**     | 4.50 (0x0450)        | 9            | 1          | -            | 4            | 8                 | 6               | 3            | 6              | -           | 1                  | 6                    | 1                    | 1              | 1                   |
| **0.5.0 beta 10**    | 5.00 (0x0500)        | 10           | 2          | 1            | -            | 9                 | 7               | -            | -              | 1           | 1                  | 7                    | 1                    | 1              | 1                   |

### Notes

- Information is based on [http://famitracker.com/wiki/index.php?title=FamiTracker_module](https://web.archive.org/web/20201124070633/http://famitracker.com/wiki/index.php?title=FamiTracker_module) 
- Additional data for 0.4.6 to 0.5.0b10 is based on source code analysis and module binary analysis.
- Added in FamiTracker 0.5.0 beta 10, this replaces the `HEADER`, `FRAMES`, and `PATTERNS` block.

## Format overview

### Parameters

- `m_iExpansionChip`
- `m_iChannelsAvailable`
- `m_iMachine`
- `m_iEngineSpeed`
- `m_iVibratoStyle`
- `m_vHighlight`
- `m_iNamcoChannels`
- `m_iSpeedSplitPoint`
- `m_iDetuneSemitone`
- `m_iDetuneCent`

### Song Info

- `m_strName`
- `m_strArtist`
- `m_strCopyright`

### Tuning

- `m_iDetuneSemitone`
- `m_iDetuneCent`

### Instruments

- Instrument index
- Instrument count
- pInstrument
	- Type
	- CInstrumentManager
	- Instrument name length
	- Name

### Sequences

- 2A03 sequence count
	- Sequence index
	- Sequence type
	- Sequence item count
	- Sequence loop point
	- Sequence release point
	- Settings
	- `SequenceData[Sequence item count]`

### Frames

- for each `m_iTrackCount`:
	- Track frame count
		- `m_iChannelsAvailable`
			- Pattern index
	- Track default speed
	- Track default tempo
	- Track default row count (`PatternLength`)

### Patterns

- for each `m_iTrackCount`:
	- Pattern track index
	- Pattern channel index
	- Pattern index
	- Pattern data count
		- Row index
			- `stChanNote` Note, consisting of:
				- Note value
				- Octave value
				- Instrument index
				- Channel volume
			- for each effect columns
				- Effect index
				- Effect Param

### Track (FamiTracker 0.5.0 beta 10)

- TODO: provide overview

### DSamples

- for each DPCM sample count
	- DPCM sample index
	- DPCM sample name length
	- DPCM sample name
	- DPCM sample size
	- DPCM sample bytes

### Header

- for each `m_iTrackCount`
	- `m_pTracks[i]`
- for each `m_iChannelsAvailable`:
	- Channel type index (unused)
	- for each `m_iTrackCount`:
	- Effect column count
- for each `m_iTrackCount`:
	- `m_vHighlight.First`
	- `m_vHighlight.Second`

### Comments

- `m_bDisplayComment`
- `m_strComment`

### Sequences VRC6

- for each VRC6 sequence count
	- Sequence index
	- Sequence type
	- Sequence item count
	- Sequence loop point
	- Sequence release point
	- Settings
	- `SequenceData[Sequence item count]`

### Sequences N163

- for each N163 sequence count
	- Sequence index
	- Sequence type
	- Sequence item count
	- Sequence loop point
	- Sequence release point
	- Settings
	- `SequenceData[Sequence item count]`

### Sequences S5B

- for each 5B sequence count
	- Sequence index
	- Sequence type
	- Sequence item count
	- Sequence loop point
	- Sequence release point
	- Settings
	- `SequenceData[Sequence item count]`

### Detune Tables

- for each Detune table count
	- for each Detune table index
		- for each Detune table note count
			- Detune table note index

### Grooves

- for each Groove count
	- Groove index
	- Groove size
	- Groove item
- for each Use-groove flag count
	- Use-groove flag

### Bookmarks

- for each Bookmark Count
	- `CBookmark`, which includes
		- Bookmark track index
		- Bookmark frame index
		- Bookmark row index
		- `m_vHighlight.First`
		- `m_vHighlight.Second`
		- `m_bPersist`
		- `m_sName`

### Params Extra

- `m_bLinearPitch`
- Global semitone tuning
- Global cent tuning

### JSON

- (optional JSON data, which includes:)
	- `APU1_OFFSET`
	- `APU2_OFFSET`
	- `VRC6_OFFSET`
	- `VRC7_OFFSET`
	- `FDS_OFFSET`
	- `MMC5_OFFSET`
	- `N163_OFFSET`
	- `S5B_OFFSET`
	- `USE_SURVEY_MIX`

### ParamsEmu

- `m_bUseExternalOPLLChips`
- for each patches:
	- `m_iOPLLPatchBytes[8]`
	- `m_strOPLLPatchNames`

## File Header

| Data type    | Unit size (bytes) | Repeat | Object/relevant variable in code      | Description       | Valid values                                  | Notes                                                                                                     |
| ------------ | ----------------- | ------ | ------------------------------------- | ----------------- | --------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| char[256]    | 256               |        | `FILE_HEADER_ID`, `FILE_HEADER_ID_DN` | Identifier string | `FamiTracker Module`, `Dn-FamiTracker Module` | In Dn-FT v0.5.0.0, this was changed to `Dn-FamiTracker Module` to avoid collision with FT 0.5.0b modules. |
| unsigned int | 4                 |        | `m_iFileVersion`                      | Module version    |                                               | Version number is formatted as BCD (ex. 4.50)                                                             |

### Notes

- Information is based on `DocumentFile.cpp CDocumentFile::BeginDocument()` and `FamiTrackerDoc.cpp CFamiTrackerDoc::SaveDocument()`
- Modules saved in Dn-FamiTracker v0.5.0.0 and later may save modules with `FILE_HEADER_ID_DN` identifiers, to prevent older FamiTracker versions from reading incompatible data such as `FILE_BLOCK_JSON` and `FILE_BLOCK_PARAMS_EMU`.

## Block header

Each block has a 24-byte header consisting of a block ID, block version, and block size.

| Data type    | Unit size (bytes) | Repeat | Object/relevant variable in code | Description                               | Valid range      | Notes                                                                                    |
| ------------ | ----------------- | ------ | -------------------------------- | ----------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------- |
| char[16]     | 16                |        | `m_cBlockID`                     | Identifier string                         | `char` data      | Length is zero-padded to 16 bytes                                                        |
| unsigned int | 4                 |        | `m_iBlockVersion`                | Block version                             | 0x0000 to 0xFFFF | Block version number is ANDed with 0xFFFF, but is still written as 32-bit signed integer |
| unsigned int | 4                 |        | `m_iBlockPointer`                | Block size, not counting the block header |                  |                                                                                          |

## Internal blocks

### Parameters block

Block ID: `PARAMS`

| _Data type_  | _Unit size (bytes)_ | _Repeat_ | _Object/relevant variable in code_ | _Description_                                          | _Valid range_                                      | _Notes_                                                                                          | _Present in block version_ |
| ------------ | ------------------- | -------- | ---------------------------------- | ------------------------------------------------------ | -------------------------------------------------- | ------------------------------------------------------------------------------------------------ | -------------------------- |
| unsigned int | 4                   |          | `m_iSongSpeed`                     | Song speed of track 0                                  |                                                    | Moved to `FRAMES` block. Off-by-one in module version 2.00 when less than speed-split point (20) | 1                          |
| char         | 1                   |          | `m_iExpansionChip`                 | Expansion audio bitmask                                |                                                    | Same format as expansion audio bitmask found in the NSF format                                   | 2+                         |
| unsigned int | 4                   |          | `m_iChannelsAvailable`             | Number of channels added                               | 1 to `MAX_CHANNELS`                                | Includes the 5 channels from 2A03.                                                               | 1+                         |
| unsigned int | 4                   |          | `m_iMachine`                       | NTSC or PAL                                            | `machine_t::NTSC`, `machine_t::PAL`                |                                                                                                  | 1+                         |
| unsigned int | 4                   |          | `m_iPlaybackRateType`              | Playback rate type, 0 = default, 1 = custom, 2 = video | 0 to 2                                             |                                                                                                  | 7+                         |
| unsigned int | 4                   |          | `m_iPlaybackRate`                  | NSF playback rate, in microseconds                     | 0x0000 to 0xFFFF                                   |                                                                                                  | 7+                         |
| unsigned int | 4                   |          | `m_iEngineSpeed`                   | Engine refresh rate, in Hz                             |                                                    |                                                                                                  | 1-6                        |
| unsigned int | 4                   |          | `m_iVibratoStyle`                  | 0 = old style, 1 = new style                           | `vibrato_t::VIBRATO_OLD`, `vibrato_t::VIBRATO_NEW` | Stored as `vibrato_t` in the tracker.                                                            | 3+                         |
| unsigned int | 4                   |          | `SweepReset`                       | Hardware sweep pitch reset                             |                                                    | Not implemented? Vanilla 0.5b stuff. Probably boolean type.                                      | 7+                         |
| int          | 4                   |          | `m_vHighlight.First`               | 1st row highlight                                      |                                                    |                                                                                                  | 3-6                        |
| int          | 4                   |          | `m_vHighlight.Second`              | 2nd row highlight                                      |                                                    |                                                                                                  | 3-6                        |
| unsigned int | 4                   |          | `m_iNamcoChannels`                 | Number of N163 channels used                           | 1 to 8                                             | Only accessed when N163 is enabled. Initialized to 4 when unused in version 10?                  | 5+                         |
| unsigned int | 4                   |          | `m_iSpeedSplitPoint`               | Fxx speed/tempo split-point                            |                                                    | Default is `0x20`. Set to `0x15` in versions 5 and lower.                                        | 6+                         |
| char         | 1                   |          | `m_iDetuneSemitone`                | Semitone detuning, range from -12 to 12                | 0                                                  | Moved to Tuning block in FT 050B10.                                                              | 8-9                        |
| char         | 1                   |          | `m_iDetuneCent`                    | Cent detuning, range from -100 to 100                  | 0                                                  | Moved to Tuning block in FT 050B10.                                                              | 8-9                        |

#### Notes

- Information is based on `CFamiTrackerDoc::WriteBlock_Parameters()`
- Global semitone and cent detuning equivalent in Extra Parameters block.

### Song Info block

Block ID: `INFO`

| _Data type_ | _Unit size (bytes)_                     | _Repeat_ | _Object/relevant variable in code_ | _Description_    | _Valid range_ | _Notes_                                        | _Present in block version_ |
| ----------- | --------------------------------------- | -------- | ---------------------------------- | ---------------- | ------------- | ---------------------------------------------- | -------------------------- |
| char[32]    | 32                                      |          | `m_strName`                        | Module name      |               | Length is zero-padded to 32 bytes              | 1                          |
| char[32]    | 32                                      |          | `m_strArtist`                      | Module artist    |               | Length is zero-padded to 32 bytes              | 1                          |
| char[32]    | 32                                      |          | `m_strCopyright`                   | Module copyright |               | Length is zero-padded to 32 bytes              | 1                          |
| char[]      | Length of zero-terminated char[] string |          | `m_strName`                        | Module name      |               | Length of string must not exceed 32 characters | 2                          |
| char[]      | Length of zero-terminated char[] string |          | `m_strArtist`                      | Module artist    |               | Length of string must not exceed 32 characters | 2                          |
| char[]      | Length of zero-terminated char[] string |          | `m_strCopyright`                   | Module copyright |               | Length of string must not exceed 32 characters | 2                          |

#### Notes

- Information is based on `CFamiTrackerDoc::WriteBlock_SongInfo()` and module binary analysis
- in FamiTracker 0.5.0 beta 10, this was changed to be zero-terminated strings.

### Tuning block

Block ID: `TUNING`

| _Data type_ | _Unit size (bytes)_ | _Repeat_ | _Object/relevant variable in code_ | _Description_          | _Valid range_ | _Notes_ | _Present in block version_ |
| ----------- | ------------------- | -------- | ---------------------------------- | ---------------------- | ------------- | ------- | -------------------------- |
| char        | 1                   |          | `m_iDetuneSemitone`                | Global semitone tuning | -12 to 12     |         | 1                          |
| char        | 1                   |          | `m_iDetuneCent`                    | Global cent tuning     | -100 to 100   |         | 1                          |

#### Notes

- Information is based on `CFamiTrackerDoc::WriteBlock_Tuning()` and module binary analysis
- Added in FamiTracker 0.5.0 beta 10
- Global semitone and cent detuning equivalent in Extra Parameters block.

### Header block

Block ID: `HEADER`

| _Data type_ | _Unit size (bytes)_                     | _Repeat_               | _Object/relevant variable in code_ | _Description_                                                     | _Valid range_                    | _Notes_                                                                      | _Present in block version_ |
| ----------- | --------------------------------------- | ---------------------- | ---------------------------------- | ----------------------------------------------------------------- | -------------------------------- | ---------------------------------------------------------------------------- | -------------------------- |
| char        | 1                                       |                        | (unused)                           | Channel type index                                                | 0 to `chan_id_t::CHANNELS - 1`   | Unused. Version 1 only has one track, so it initializes `m_iTrackCount = 1`  | 1                          |
| char        | 1                                       |                        | `m_iTrackCount`                    | Number of tracks added.                                           | 0 to `MAX_TRACKS - 1`            | Stored as `m_iTrackCount` - 1                                                | 2-4                        |
| char[]      | Length of zero-terminated char[] string | `m_iTrackCount`        | `m_pTracks[]->m_sTrackName`        | Names of each track                                               |                                  | Each track name is zero terminated, and therefore delineated by a byte of 0. | 3-4                        |
| char        | 1                                       | `m_iChannelsAvailable` | `m_iChannelTypes[]`                | Channel type index                                                | 0 to `chan_id_t::CHANNELS - 1`   | See Channel ID table.                                                        | 1-4                        |
| char[]      | `m_iTrackCount`                         | ^                      | `m_pTracks[]->m_iEffectColumns[]`  | Number of additional effect columns on a given channel, per track | 0 to `MAX_EFFECT_COLUMNS - 1`    | On version 1, this was restricted to the first track.                        | 1-4                        |
| int         | 4                                       | `m_iTrackCount`        | `m_vHighlight.First`               | 1st row highlight                                                 | 0 to 255 (cast to unsigned char) | FT 050b1. Stores per-track row highlights.                                   | 4                          |
| int         | 4                                       | ^                      | `m_vHighlight.Second`              | 2nd row highlight                                                 | 0 to 255 (cast to unsigned char) | FT 050B1. Stores per-track row highlights.                                   | 4                          |

#### Channel ID

| Chip | Identifiers                    |
| ---- | ------------------------------ |
| 2A03 | 00, 01, 02, 03, 04             |
| VRC6 | 05, 06, 07                     |
| MMC5 | 08, 09, 0A                     |
| N163 | 0B, 0C, 0D, 0E, 0F, 10, 11, 12 |
| FDS  | 13                             |
| VRC7 | 14, 15, 16, 17, 18, 19         |
| S5B  | 1A, 1B, 1C                     |

#### Notes

- Information is based on `CFamiTrackerDoc::WriteBlock_Header()`
- Replaced by `TRACK` block in FamiTracker 0.5.0 beta 10

### Track block

Block ID: `TRACK`

| _Data type_  | _Unit size (bytes)_                     | _Repeat_        | _Object/relevant variable in code_ | _Description_                                                 | _Valid range_                 | _Notes_                        | _Present in block version_ |
| ------------ | --------------------------------------- | --------------- | ---------------------------------- | ------------------------------------------------------------- | ----------------------------- | ------------------------------ | -------------------------- |
| char         | 1                                       |                 |                                    | data: track name                                              | 0x00                          | Track name data indicator      | 1                          |
| char[]       | Length of zero-terminated char[] string |                 | `m_pTracks[]->m_sTrackName`        | Track name                                                    |                               | Track name is zero terminated. | 1                          |
| char         | 1                                       |                 |                                    | data: track settings                                          | 0x01                          | Track setting data indicator   | 1                          |
| unsigned int | 4                                       |                 |                                    | Track default row count                                       | 1 to `MAX_PATTERN_LENGTH`     |                                | 1                          |
| unsigned int | 4                                       |                 |                                    | Track default speed                                           | 0 to `MAX_TEMPO`              |                                | 1                          |
| unsigned int | 4                                       |                 |                                    | Track default tempo                                           | 0 to `MAX_TEMPO`              |                                | 1                          |
| char         | 1                                       |                 |                                    | data: track row highlight                                     | 0x02                          | Row highlight data indicator   | 1                          |
| char         | 1                                       |                 | `m_vHighlight.First`               | 1st row highlight                                             |                               | Stores track row highlights.   | 1                          |
| char         | 1                                       |                 | `m_vHighlight.Second`              | 2nd row highlight                                             |                               | Stores track row highlights.   | 1                          |
| char         | 1                                       |                 |                                    | data: track effect columns                                    | 0x03                          | Effect column data indicator   | 1                          |
| char[]       | `m_iTrackCount`                         |                 | `m_pTracks[]->m_iEffectColumns[]`  | Number of additional effect columns on a channel, per channel | 0 to `MAX_EFFECT_COLUMNS - 1` |                                | 1                          |
| char         | 1                                       |                 |                                    | data: track frame data                                        | 0x04                          | Denotes frames                 | 1                          |
| unsigned int | 4                                       |                 | `m_iFrameCount`                    | Track frame count                                             | 1 to `MAX_FRAMES`             |                                | 1                          |
| char[]       | `m_iChannelsAvailable`                  | `m_iFrameCount` |                                    | Pattern indices in a given frame                              | 0 to `MAX_PATTERN - 1`        |                                | 1                          |
| char         | 1                                       |                 |                                    | data: track pattern data                                      | 0x05                          | Pattern data indicator         | 1                          |
| unsigned int | 4                                       |                 |                                    | Pattern size                                                  |                               |                                | 1                          |
| char         | 1                                       | Pattern count   |                                    | Pattern channel index                                         |                               |                                | 1                          |
| char         | 1                                       | ^               |                                    | Pattern index                                                 |                               |                                | 1                          |
| uint16       | 2                                       | ^               |                                    | Pattern data count                                            | 0 to `MAX_PATTERN_LENGTH`     |                                | 1                          |
| char[]       |                                         | ^               |                                    | Row data                                                      |                               | See row data format.           | 1                          |

#### Row data format

| _Data type_ | _Unit size (bytes)_ | _Repeat_           | _Object/relevant variable in code_ | _Description_             | _Valid range_                | _Notes_                                                           | _Present in block version_ |
| ----------- | ------------------- | ------------------ | ---------------------------------- | ------------------------- | ---------------------------- | ----------------------------------------------------------------- | -------------------------- |
| char        | 1                   | Pattern data count |                                    | Row index                 |                              |                                                                   | 1                          |
| char        | 1                   | ^                  |                                    | Pending effect bitflag    |                              | `4321 xIVN` = Effect on col 1/2/3/4, Instrument, Volume, and Note | 1                          |
| char        | 1                   | ^                  |                                    | Note index                | 0 to `NOTE_COUNT`            | Only present when corresponding bitflag is set                    | 1                          |
| char        | 1                   | ^                  |                                    | Instrument index          | 0 to `MAX_INSTRUMENTS`       | Only present when corresponding bitflag is set                    | 1                          |
| char        | 1                   | ^                  |                                    | Volume index              | 0 to `MAX_VOLUME`            | Only present when corresponding bitflag is set                    | 1                          |
| char        | 1                   | ^                  |                                    | Column 1 effect index     | `EF_SPEED` to `EF_COUNT - 1` | Only present when corresponding bitflag is set                    | 1                          |
| char        | 1                   | ^                  |                                    | Column 1 effect parameter |                              | Only present when corresponding bitflag is set                    | 1                          |
| char        | 1                   | ^                  |                                    | Column 2 effect index     | `EF_SPEED` to `EF_COUNT - 1` | Only present when corresponding bitflag is set                    | 1                          |
| char        | 1                   | ^                  |                                    | Column 2 effect parameter |                              | Only present when corresponding bitflag is set                    | 1                          |
| char        | 1                   | ^                  |                                    | Column 3 effect index     | `EF_SPEED` to `EF_COUNT - 1` | Only present when corresponding bitflag is set                    | 1                          |
| char        | 1                   | ^                  |                                    | Column 3 effect parameter |                              | Only present when corresponding bitflag is set                    | 1                          |
| char        | 1                   | ^                  |                                    | Column 4 effect index     | `EF_SPEED` to `EF_COUNT - 1` | Only present when corresponding bitflag is set                    | 1                          |
| char        | 1                   | ^                  |                                    | Column 4 effect parameter |                              | Only present when corresponding bitflag is set                    | 1                          |

#### Notes

- Information is based on module binary analysis
- Added in FamiTracker 0.5.0 beta 10, this replaces the `HEADER`, `FRAMES`, and `PATTERNS` block.
- More research is needed.

### Instruments block

Block ID: `INSTRUMENTS`

| _Data type_ | _Unit size (bytes)_        | _Repeat_       | _Object/relevant variable in code_                               | _Description_          | _Valid range_              | _Notes_                                                        | _Present in block version_ |
| ----------- | -------------------------- | -------------- | ---------------------------------------------------------------- | ---------------------- | -------------------------- | -------------------------------------------------------------- | -------------------------- |
| int         | 4                          |                | `m_pInstrumentManager->GetInstrumentCount()`                     | Instrument count       | 0 to `MAX_INSTRUMENTS`     | Count of existing instruments                                  | 1+                         |
| int         | 4                          | Per instrument | Instrument index                                                 | Instrument index       | 0 to `MAX_INSTRUMENTS - 1` |                                                                | 1+                         |
| char        | 1                          | ^              | `m_pInstrumentManager->GetInstrument()->m_iType`                 | Instrument type        | `enum inst_type_t`         | See table.                                                     | 1+                         |
| CInstrument | size of CInstrument object | ^              | `m_pInstrumentManager->GetInstrument()`                          | Instrument definition  |                            | See [Dn-FT instrument format](Dn-FT%20instrument%20format.md). | 1+                         |
| int         | 4                          | ^              | `strlen()` of `m_pInstrumentManager->GetInstrument()->GetName()` | Instrument name length | 0 to `INST_NAME_MAX`       |                                                                | 1+                         |
| char[]      | Instrument name length     | ^              | `m_pInstrumentManager->GetInstrument()->GetName()`               | Instrument name        |                            |                                                                | 1+                         |

#### Notes

- See [Dn-FT instrument format](Dn-FT%20instrument%20format.md) for instrument file implementation. 
- This format is shared between 2A03, VRC6, N163, and S5B instruments.
- In 0CC-FamiTracker and its forks, this format was converted into a subclass of `CInstrument`, `CSeqInstrument` to deduplicate code.
- If FDS is used then version must be at least 4 or recent files won't load
- This block is only written if any instruments exist
- v6 adds DPCM delta settings.
- TODO: investigate additional changes in v8 and v9.

#### Instrument types

| *Chip* | *Index* |
| ---- | ----- |
| 2A03 | 0x01  |
| VRC6 | 0x02  |
| VRC7 | 0x03  |
| FDS  | 0x04  |
| N163 | 0x05  |
| S5B  | 0x06  |

##### Base instrument format

| _Data type_ | _Unit size (bytes)_ | _Repeat_        | _Object/relevant variable in code_ | _Description_                                     | _Valid range_     | _Notes_                                                                              | _Present in block version_ |
| ----------- | ------------------- | --------------- | ---------------------------------- | ------------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------ | -------------------------- |
| int         | 4                   |                 | `SEQ_COUNT`                        | Sequence count                                    | `enum sequence_t` | Number of defined sequence types.  <br>Volume, Arpeggio, Pitch, Hi-Pitch, Duty cycle | 1+                         |
| char        | 1                   | Per `SEQ_COUNT` | `CSeqInstrument::m_iSeqEnable[]`   | 0 = sequence is disabled, 1 = sequence is enabled | `enum sequence_t` | If sequence is disabled, sequence index reference is still stored                    | 1+                         |
| char        | 1                   | ^               | `CSeqInstrument::m_iSeqIndex[]`    | Sequence index                                    | `enum sequence_t` | Reference to sequence data in `SEQUENCES` block.                                     | 1+                         |

###### Notes

- Information is based on `CSeqInstrument::Store()`

##### 2A03 instruments

| _Data type_    | _Unit size (bytes)_    | _Repeat_                     | _Object/relevant variable in code_              | _Description_                                                                        | _Valid range_         | _Notes_                                                      | _Present in block version_ |
| -------------- | ---------------------- | ---------------------------- | ----------------------------------------------- | ------------------------------------------------------------------------------------ | --------------------- | ------------------------------------------------------------ | -------------------------- |
| CSeqInstrument | size of CSeqInstrument |                              |                                                 | See [Base instrument format](Dn-FT%20module%20format.md#Base%20instrument%20format). |                       |                                                              | 1+                         |
| int            | 4                      |                              | `CInstrument2A03::GetSampleCount()`             | DPCM sample assignment count                                                         | 0 to `MAX_DSAMPLES`   |                                                              | 7+                         |
| char           | 1                      | DPCM sample assignment count | `Note`                                          | DPCM sample assignment note index                                                    | 0 to `NOTE_COUNT - 1` | Written only when a sample exists at that note.              | 7+                         |
| char           | 1                      | ^                            | `CInstrument2A03::m_cSamples[Octave][Note]`     | DPCM sample assignment index                                                         | 0 to `MAX_DSAMPLES`   | Written only when a sample exists at that note in FT 050b1+. | 1+                         |
| char           | 1                      | ^                            | `CInstrument2A03::m_cSamplePitch[Octave][Note]` | DPCM sample pitch                                                                    | 0x0 to 0xF            | Written only when a sample exists at that note in FT 050b1+. | 1+                         |
| char           | 1                      | ^                            | `CInstrument2A03::m_cSampleDelta[Octave][Note]` | DPCM delta offset of a given note                                                    |                       | Written only when a sample exists at that note in FT 050b1+. | 6+                         |
###### Notes

- Information is based on `CInstrument2A03::Store()`
- Only 72 notes are defined in version 1. Version 2+ has all 96 notes defined.
- In FamiTracker 0.5.0 beta, the DPCM format has been changed to only count notes with DPCM assignments.

##### VRC6 instruments

- Same format as [Base instrument format](Dn-FT%20module%20format.md#Base%20instrument%20format).
- Fifth sequence type is named `Pulse Width`.

###### Notes

- Information is based on `CSeqInstrument::Store()`
- Similar to 2A03 instruments but with no special considerations for DPCM

##### VRC7 instruments

| _Data type_ | _Unit size (bytes)_ | _Repeat_ | _Object/relevant variable in code_ | _Description_         | _Valid range_ | _Notes_                                  | _Present in block version_ |
| ----------- | ------------------- | -------- | ---------------------------------- | --------------------- | ------------- | ---------------------------------------- | -------------------------- |
| int         | 4                   |          | `CInstrumentVRC7::m_iPatch`        | VRC7 patch number     |               | Hardware patch number of the instrument. | 2+                         |
| char[8]     | 8                   |          | `CInstrumentVRC7::m_iRegs[]`       | Custom patch settings |               | Patch settings of hardware patch 0.      | 2+                         |

###### Notes

- Information is based on `CInstrumentVRC7::Store()`

##### FDS instruments

| _Data type_     | _Unit size (bytes)_ | _Repeat_                         | _Object/relevant variable in code_   | _Description_               | _Valid range_       | _Notes_ | _Present in block version_ |
| --------------- | ------------------- | -------------------------------- | ------------------------------------ | --------------------------- | ------------------- | ------- | -------------------------- |
| char[WAVE_SIZE] | WAVE_SIZE           |                                  | `CInstrumentFDS::m_iSamples[]`       | Wave data                   |                     |         | 3+                         |
| char[MOD_SIZE]  | MOD_SIZE            |                                  | `CInstrumentFDS::m_iModulation[]`    | Modulation table            |                     |         | 3+                         |
| int             | 4                   |                                  | `CInstrumentFDS::m_iModulationSpeed` | Instrument modulation rate  |                     |         | 3+                         |
| int             | 4                   |                                  | `CInstrumentFDS::m_iModulationDepth` | Instrument modulation depth |                     |         | 3+                         |
| int             | 4                   |                                  | `CInstrumentFDS::m_iModulationDelay` | Instrument modulation delay |                     |         | 3+                         |
| char            | 1                   | `CInstrumentFDS::SEQUENCE_COUNT` | `CSequence::m_iItemCount`            | Sequence item count         | 0 to 255            |         | 3+                         |
| int             | 4                   | ^                                | `CSequence::m_iLoopPoint`            | Sequence loop point         | -1 to`SeqCount - 1` |         | 3+                         |
| int             | 4                   | ^                                | `CSequence::m_iReleasePoint`         | Sequence release point      | -1 to`SeqCount - 1` |         | 4+                         |
| int             | 4                   | ^                                | `CSequence::m_iSetting`              | Sequence setting type       | `seq_setting_t`     |         | 4+                         |
| char[]          | Sequence item count | ^                                | `CSequence::m_cValues[Index]`        | Sequence data               |                     |         | 3+                         |

###### Notes

- Information is based on `CInstrumentFDS::Store()`, `CInstrumentFDS::StoreSequence()`
- Unlike other chips, FDS instruments do not store sequences inside their own dedicated block yet
- FDS instruments stores its own sequences via `CInstrumentFDS`, child class of `CSeqInstrument`
	- only stores 3 types, Volume, Arpeggio, and Pitch.
	- version 2 apparently does not have Pitch sequences
- In version 3, volume range was 0-15. Later versions have volume ranges from 0-31.
- In version 2, FDS sequences were stored incorrectly?

##### N163 instruments

| _Data type_       | _Unit size (bytes)_    | _Repeat_       | _Object/relevant variable in code_          | _Description_                                                                        | _Valid range_            | _Notes_                                                                           | _Present in block version_ |
| ----------------- | ---------------------- | -------------- | ------------------------------------------- | ------------------------------------------------------------------------------------ | ------------------------ | --------------------------------------------------------------------------------- | -------------------------- |
| CSeqInstrument    | size of CSeqInstrument |                |                                             | See [Base instrument format](Dn-FT%20module%20format.md#Base%20instrument%20format). |                          |                                                                                   | 1+                         |
| int               | 4                      |                | `m_iWaveSize`                               | N163 wave size                                                                       | 4 to `MAX_WAVE_SIZE`     | In FT 0.5.0 beta, `m_iWaveSize` is determined by `m_iWaveCount / remaining_bytes` | 2-6                        |
| int               | 4                      |                | `m_iWavePos`                                | N163 wave position                                                                   | 0 to `MAX_WAVE_SIZE - 1` | Ignored if `m_bAutoWavePos` is enabled                                            | 2-6                        |
| int               | 4                      |                | `m_bAutoWavePos`                            | Automatic wave data RAM allocation                                                   | 0, nonzero               |                                                                                   | 8+                         |
| int               | 4                      |                | `m_iWaveCount`                              | N163 wave count                                                                      | 1 to `MAX_WAVE_COUNT`    |                                                                                   | 2+                         |
| char[m_iWaveSize] | `m_iWaveSize`          | `m_iWaveCount` | `m_iSamples[MAX_WAVE_COUNT][MAX_WAVE_SIZE]` | N163 wave sample                                                                     | 0 to 0xF                 |                                                                                   | 2+                         |

###### Notes

- Information is based on `CInstrumentN163::Store()` and instrument file binary analysis
- Automatic wave data RAM allocation feature is from FT 0.5.0 beta.
- Fifth sequence type is named `Wave Index`.

##### S5B instruments

- Same format as [Base instrument format](Dn-FT%20module%20format.md#Base%20instrument%20format).
- Fifth sequence type is named `Noise / Mode`.

###### Notes

- Information is based on `CSeqInstrument::Store()`

### Sequences block

Block ID: `SEQUENCES`

| _Data type_  | _Unit size (bytes)_     | _Repeat_                                     | _Object/relevant variable in code_ | _Description_                | _Valid range_                                                            | _Notes_                                                                                | _Present in block version_ |
| ------------ | ----------------------- | -------------------------------------------- | ---------------------------------- | ---------------------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- | -------------------------- |
| unsigned int | 4                       |                                              | `Count`                            | 2A03 sequence count          | 0 to `MAX_SEQUENCES * SEQ_COUNT`                                         | Only used sequences are counted                                                        | 1+                         |
| unsigned int | 4                       | Per 2A03 sequence count                      | `Index`                            | Sequence index               | 0 to `MAX_SEQUENCES - 1`                                                 |                                                                                        | 1+                         |
| unsigned int | 4                       | ^                                            | `Type`                             | Sequence type                | 0 to `SEQ_COUNT - 1`                                                     | See [Sequence type tables](Dn-FT%20module%20format.md#Sequence%20tyoe).                | 2+                         |
| char         | 1                       | ^                                            | `SeqCount`                         | Sequence run count           | 0 to `MAX_SEQUENCES_ITEMS - 1`                                           | `SeqCount` refers to the size of a different sequence scheme.                          | 1-2                        |
| char[]       | Sequence run count \* 2 | ^                                            | `Value`, `Length`                  | Sequence value, run length-1 |                                                                          | RLE encoded sequence data?                                                             | 1-2                        |
| char         | 1                       | ^                                            | `m_iItemCount`                     | Sequence item count          | 0 to `MAX_SEQUENCE_ITEMS`                                                | If the sequence count is larger, it clamps to `MAX_SEQUENCE_ITEMS`.                    | 3+                         |
| unsigned int | 4                       | ^                                            | `LoopPoint`                        | Sequence loop point          | -1 to `SeqCount`                                                         | -1 if it doesn't exist                                                                 | 3+                         |
| unsigned int | 4                       | ^                                            | `ReleasePoint`                     | Sequence release point       | -1 to `SeqCount`                                                         | -1 if it doesn't exist                                                                 | 4, 7                       |
| unsigned int | 4                       | ^                                            | `Settings`                         | Sequence setting             | [`seq_setting_t`](Datatypes%20and%20constants.md#`seq_setting_t`%20enum) | See table.                                                                             | 4, 7                       |
| char[]       | Sequence item count     | ^                                            | `Value`                            | Sequence value               |                                                                          |                                                                                        | 3+                         |
| int          | 4                       | Per 2A03 sequence (separate repeat sequence) | `ReleasePoint`                     | Sequence release point       | -1 to `SeqCount - 1`                                                     | Version 5 saved the release points incorrectly, fixed in ver 6. -1 if it doesn't exist | 5-6                        |
| unsigned int | 4                       | ^                                            | `Settings`                         | Sequence setting             | [`seq_setting_t`](Datatypes%20and%20constants.md#`seq_setting_t`%20enum) |                                                                                        | 5-6                        |

#### Sequence type

- See [`sequence_t`](Datatypes%20and%20constants.md#`sequence_t`%20enum)

| Value | 0      | 1        | 2     | 3        | 4                                |
| ----- | ------ | -------- | ----- | -------- | -------------------------------- |
| Type  | Volume | Arpeggio | Pitch | Hi-pitch | Duty / Noise (resp. Pulse width) |

#### Sequence arpeggio type (only in version 4)

| Value | 0        | 1     | 2        | 0?                                   |
| ----- | -------- | ----- | -------- | ------------------------------------ |
| Type  | Absolute | Fixed | Relative | Sequence is not an arpeggio sequence |


#### Notes

- Information is based on `CFamiTrackerDoc::WriteBlock_Header()`, `CFamiTrackerDoc::ReadBlock_Sequences()` and [http://famitracker.com/wiki/index.php?title=FamiTracker_module#SEQUENCES](https://web.archive.org/web/20201124070633/http://famitracker.com/wiki/index.php?title=FamiTracker_module#SEQUENCES)
- This stores 2A03 sequences. Most other sequences are similar to this format.
- Before block version 5, release points and sequence settings were stored in the same array sequence.
- Before block version 2, only 72 notes were defined.
- in FamiTracker 0.5.0 beta 10, this is only saved when any sequences exist in the module.
- in FamiTracker 0.5.0 beta 10, the sequence release point and setting has shifted back to version 4's place.

### Frames block

Block ID: `FRAMES`

| _Data type_  | _Unit size (bytes)_                    | _Repeat_                               | _Object/relevant variable in code_ | _Description_           | _Valid range_             | _Notes_                                                                                                                                       | _Present in block version_ |
| ------------ | -------------------------------------- | -------------------------------------- | ---------------------------------- | ----------------------- | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| unsigned int | 4                                      |                                        | `m_iFrameCount`                    | Frame count             | 0 to `MAX_FRAMES`         | Only one song is available.                                                                                                                   | 1                          |
| unsigned int | 4                                      |                                        | `m_iChannelsAvailable`             | Channel count           | 0 to `MAX_CHANNELS`       | Moved to `PARAMS` block. Only one song is available.                                                                                          | 1                          |
| char         | 1                                      | `m_iChannelsAvailable * m_iFrameCount` | `Pattern`                          | Pattern indices         | 0 to `MAX_PATTERN - 1`    | Only one song is available.                                                                                                                   | 1                          |
| unsigned int | 4                                      | Track count                            | `m_iFrameCount`                    | Track frame count       | 1 to `MAX_FRAMES`         |                                                                                                                                               | 2-3                        |
| unsigned int | 4                                      | ^                                      | `m_iSongSpeed`                     | Track default speed     | 0 to `MAX_TEMPO`          |                                                                                                                                               | 3                          |
| unsigned int | 4                                      | ^                                      | `m_iSongTempo`                     | Track default tempo     | 0 to `MAX_TEMPO`          | In block version 2, this either sets the tempo or the speed, depending if `Speed < 20`. In block version 3+, this exclusively sets the speed. | 2-3                        |
| unsigned int | 4                                      | ^                                      | `m_iPatternLength`                 | Track default row count | 1 to `MAX_PATTERN_LENGTH` | Length of patterns (in length)                                                                                                                | 2-3                        |
| char[]       | `m_iChannelsAvailable * m_iFrameCount` | ^                                      | `Pattern`                          | Pattern indices         | 0 to `MAX_PATTERN - 1`    |                                                                                                                                               | 1-3                        |
#### Notes

- Information is based on `CFamiTrackerDoc::WriteBlock_Header()`, `CFamiTrackerDoc::ReadBlock_Frames()`
- This stores frame row data, which is an array of pattern indices, of which has length channel count.
- in FamiTracker versions 0.2.5 and below, there was only one song per module, so the frame data was exclusive to one song only.
- replaced by `TRACK` block in FamiTracker 0.5.0 beta 10

### Patterns block

Block ID: `PATTERNS`

| _Data type_  | _Unit size (bytes)_      | _Repeat_    | _Object/relevant variable in code_ | _Description_                | _Valid range_             | _Notes_                                            | _Present in block version_ |
| ------------ | ------------------------ | ----------- | ---------------------------------- | ---------------------------- | ------------------------- | -------------------------------------------------- | -------------------------- |
| unsigned int | 4                        |             | `m_iPatternLength`                 | Pattern data count           | 0 to `MAX_PATTERN_LENGTH` | Moved to `FRAMES` block                            | 1                          |
| unsigned int | 4                        | Track count | `Track`                            | Pattern track index          | 0 to `MAX_TRACKS - 1`     | In version 1, there was only one track per module. | 2+                         |
| unsigned int | 4                        | ^           | `Channel`                          | Pattern channel index        | 0 to `MAX_CHANNELS - 1`   |                                                    | 1+                         |
| unsigned int | 4                        | ^           | `Pattern`                          | Pattern index                | 0 to `MAX_PATTERN - 1`    |                                                    | 1+                         |
| unsigned int | 4                        | ^           | `Items`                            | Pattern data count           | 0 to `MAX_PATTERN_LENGTH` |                                                    | 1+                         |
| char[]       | `Items` \* Row Data size | ^           |                                    | Row data (see section below) |                           |                                                    | 1+                         |

#### Row Data

| _Data type_  | _Unit size (bytes)_                        | _Repeat_ | _Object/relevant variable in code_ | _Description_                   | _Valid range_           | _Notes_                                                                                                                                         | _Present in block version_ |
| ------------ | ------------------------------------------ | -------- | ---------------------------------- | ------------------------------- | ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- |
| unsigned int | 4                                          | `Items`  | `Row`                              | Row index                       | 0x0000 to 0x00FF        |                                                                                                                                                 | 1-5                        |
| char         | 1                                          | ^        | `Row`                              | Row index                       | 0x00 to 0xFF            | In file version `0x0200` or block versions 6+, this is a char type  instead.                                                                    | 6+                         |
| char         | 1                                          | ^        | `Note`                             | Note value                      | `note_t`                | Blank note is indicated with value `NONE`                                                                                                       | 1+                         |
| char         | 1                                          | ^        | `Octave`                           | Octave value                    | 0 to `OCTAVE_RANGE - 1` | Blank octave is indicated with value 0                                                                                                          | 1+                         |
| char         | 1                                          | ^        | `Instrument`                       | Instrument index                | 0 to `MAX_INSTRUMENTS`  | Instrument indices equal to the hold note index will not be assigned to the pattern. Blank instrument is indicated with value `MAX_INSTRUMENTS` | 1+                         |
| char         | 1                                          | ^        | `Vol`                              | Channel volume                  | 0 to `MAX_VOLUME`       | Blank volume is indicated with value `MAX_VOLUME`                                                                                               | 1+                         |
| char[]       | `m_iEffectColumns` of current channel \* 2 | ^        |                                    | Effect data (see section below) |                         | In block versions 1-5, all effect columns were stored even if they are unused.                                                                  | 1+                         |

##### Effect data

| _Data type_ | _Unit size (bytes)_ | _Repeat_                              | _Object/relevant variable in code_ | _Description_    | _Valid range_               | _Notes_                                                                | _Present in block version_ |
| ----------- | ------------------- | ------------------------------------- | ---------------------------------- | ---------------- | --------------------------- | ---------------------------------------------------------------------- | -------------------------- |
| char        | 1                   | `m_iEffectColumns` of current channel | `EffectNumber`                     | Effect index     | `EF_NONE` to `EF_COUNT - 1` |                                                                        | 1+                         |
| char        | 1                   | ^                                     | `EffectParam`                      | Effect parameter | `0x00` to `0xFF`            | On block versions 6+, this is present only if effect index is nonzero. | 1+                         |

#### Notes

- Information is based on `CFamiTrackerDoc::WriteBlock_Patterns`, `CFamiTrackerDoc::ReadBlock_Patterns`, and [http://famitracker.com/wiki/index.php?title=FamiTracker_module#PATTERNS](https://web.archive.org/web/20201124070633/http://famitracker.com/wiki/index.php?title=FamiTracker_module#PATTERNS)
- replaced by `TRACK` block in FamiTracker 0.5.0 beta 10
- Support for multiple tracks was added in block version 2
- Most versions are due to changes in effect commands, than the pattern format:
	- if data was from block versions 2 and below:
		- convert `1xx` and `2xx` in previous versions into `3xx`
			- if effect index was `0x07`, convert to `EF_PORTAMENTO` and set parameter to 0
			- if effect index was `0x06`, interpret as `EF_PORTAMENTO` and increment parameter if it's less than `0xFF`
	- if data was from block version 3:
		- inverts `1xx` and `2xx` effects in VRC7 channels
		- adjust FDS pitch effects by inverting the parameter value if not `80`
	- if data was from block versions 5 and below:
		- raise all FDS notes by 2 octaves
	- if data was from file version `0x0200`:
		- adjust off-by-one `EF_SPEED` if parameter is less than `20`
		- adjust off-by-one volume command (set `0` to `MAX_VOLUME`, otherwise decrement and clamp within `0x0F)
		- set instrument to blank (`MAX_INSTRUMENTS`) if `note` is zero
	- if a `EF_SAMPLE_OFFSET` was found in an N163 channel, it gets converted to `EF_N163_WAVE_BUFFER`
	- if a module is a Dn-FamiTracker module or has a file version lower than `0x450`, parse effects as FT 050B+ effect ordering
		- more information can be found on [0CC vs FT effects type order](0CC%20vs%20FT%20effects%20type%20order.md)

### DPCM Samples block

Block ID: `DPCM SAMPLES`

| _Data type_  | _Unit size (bytes)_ | _Repeat_          | _Object/relevant variable in code_ | _Description_           | _Valid range_            | _Notes_                                                                                                    | _Present in block version_ |
| ------------ | ------------------- | ----------------- | ---------------------------------- | ----------------------- | ------------------------ | ---------------------------------------------------------------------------------------------------------- | -------------------------- |
| char         | 1                   |                   | `Count`                            | DPCM sample count       | 0 to `MAX_DSAMPLES`      |                                                                                                            | 1+                         |
| char         | 1                   | DPCM sample count | `Index`                            | DPCM sample index       | 0 to `MAX_DSAMPLES - 1`  |                                                                                                            | 1+                         |
| unsigned int | 4                   | ^                 | `Len`                              | DPCM sample name length | 0 to `MAX_NAME_SIZE - 1` |                                                                                                            | 1+                         |
| char[]       | `MAX_NAME_SIZE`     | ^                 | `Name`                             | DPCM sample name        | ASCII strings            | actual buffer size is `MAX_NAME_SIZE`, but only gets `Len` amount of bytes written.                        | 1+                         |
| unsigned int | 4                   | ^                 | `Size`                             | DPCM sample size        | 0 to `0x7FFF`'           | size actually must be `(L * 16) + 1` bytes, where L ranges from 0 to 255. the max DPCM size is 4081 bytes. | 1+                         |
| char[]       | `Size`              | ^                 | `pData`                            | DPCM sample data        |                          |                                                                                                            | 1+                         |

#### Notes

- Information is based on `CFamiTrackerDoc::ReadBlock_DSamples` and [http://famitracker.com/wiki/index.php?title=FamiTracker_module#DPCM_SAMPLES](https://web.archive.org/web/20201124070633/http://famitracker.com/wiki/index.php?title=FamiTracker_module#DPCM_SAMPLES)
- FT throws an error if the DPCM sample size is bigger than 4081 bytes, or if it is not `% 16==1`.
	- See more details on the DMC sample length register: https://www.nesdev.org/wiki/APU_DMC#Overview
- In 0CC-FT v0.3.14.1+, If DPCM data size is not `% 16 == 1`, it will be padded to a valid length with values `0xAA`.
	- padding is calculated as `TrueSize = Size + ((1 - Size) & 0x0F)`

### Comments block

Block ID: `COMMENTS`

| _Data type_  | _Unit size (bytes)_                     | _Repeat_ | _Object/relevant variable in code_ | _Description_                      | _Valid range_ | _Notes_                                     | _Present in block version_ |
| ------------ | --------------------------------------- | -------- | ---------------------------------- | ---------------------------------- | ------------- | ------------------------------------------- | -------------------------- |
| unsigned int | 4                                       |          | `m_bDisplayComment`                | Display module comment when opened | 0, 1          | Boolean value (0, 1) but stored as integer. | 1+                         |
| char[]       | Length of zero-terminated char[] string |          | `m_strComment`                     | Module comment data.               |               |                                             | 1+                         |

#### Notes

- Information is based on `CFamiTrackerDoc::ReadBlock_Comments` and [http://famitracker.com/wiki/index.php?title=FamiTracker_module#COMMENTS](https://web.archive.org/web/20201124070633/http://famitracker.com/wiki/index.php?title=FamiTracker_module#COMMENTS)

## Expansion blocks

### VRC6 Sequences block

Block ID: `SEQUENCES_VRC6`

#### Notes

- Data format is identical to that of the [Sequences block](Dn-FT%20module%20format.md#Sequences%20block).
- Information is based on `CFamiTrackerDoc::ReadBlock_SequencesVRC6` and [http://famitracker.com/wiki/index.php?title=FamiTracker_module#SEQUENCES_VRC6](https://web.archive.org/web/20201124070633/http://famitracker.com/wiki/index.php?title=FamiTracker_module#SEQUENCES_VRC6)

### N163 Sequences block

Block ID: `SEQUENCES_N163`\
Alternate block ID: `SEQUENCES_N106`

| _Data type_  | _Unit size (bytes)_ | _Repeat_            | _Object/relevant variable in code_ | _Description_          | _Valid range_                    | _Notes_                                                            | _Present in block version_ |
| ------------ | ------------------- | ------------------- | ---------------------------------- | ---------------------- | -------------------------------- | ------------------------------------------------------------------ | -------------------------- |
| unsigned int | 4                   |                     | `Count`                            | N163 sequence count    | 0 to `MAX_SEQUENCES * SEQ_COUNT` | Only used sequences are counted                                    | 1                          |
| unsigned int | 4                   | N163 sequence count | `Index`                            | Sequence index         | 0 to `MAX_SEQUENCES - 1`         |                                                                    | 1                          |
| unsigned int | 4                   | ^                   | `Type`                             | Sequence type          | 0 to `SEQ_COUNT - 1`             |                                                                    | 1                          |
| char         | 1                   | ^                   | `m_iItemCount`                     | Sequence item count    | 0 to `MAX_SEQUENCE_ITEMS`        | If the sequence count is larger, it clamps to `MAX_SEQUENCE_ITEMS` | 1                          |
| unsigned int | 4                   | ^                   | `m_iLoopPoint`                     | Sequence loop point    |                                  |                                                                    | 1                          |
| unsigned int | 4                   | ^                   |                                    | Sequence release point |                                  |                                                                    | 1                          |
| unsigned int | 4                   | ^                   | `Settings`                         | Sequence setting       | `seq_setting_t`                  | See table.                                                         | 1                          |
| char[]       | Sequence item count | ^                   | `Value`                            | Sequence value         |                                  |                                                                    | 1                          |

#### Notes

- Information is based on `CFamiTrackerDoc::WriteBlock_SequencesN163`, `CFamiTrackerDoc::ReadBlock_SequencesN163` and [http://famitracker.com/wiki/index.php?title=FamiTracker_module#SEQUENCES_N163](https://web.archive.org/web/20201124070633/http://famitracker.com/wiki/index.php?title=FamiTracker_module#SEQUENCES_N163)
- Data format is similar to that of the [Sequences block](Dn-FT%20module%20format.md#Sequences%20block).

### S5B Sequences block

Block ID: `SEQUENCES_S5B`

## 0CC-FamiTracker extension blocks

### Extra Parameters block

Block ID: `PARAMS_EXTRA`

### Detune Tables block

Block ID: `DETUNETABLES`

### Grooves block

Block ID: `GROOVES`

### Bookmarks block

Block ID: `BOOKMARKS`

## Dn-FamiTracker extension blocks

### JSON block

Block ID: `JSON`

### Emulation Parameters block

Block ID: `PARAMS_EMU`
