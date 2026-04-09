# NSF Compilation

In Dn-FamiTracker, NSFs are compiled by translating the module data into binary data, before concatenating it to a compiled NSF driver. Of course, this is as complicated as it sounds. It's mostly done inside `Compiler.cpp`.

## Overview

Nearly every format export is the same with subtle differences:
- clear log messages with `CCompiler::ClearLog()`
- detect if any NSFe features are used in a non-NSFe compatible format
- `CCompiler::CompileData()`
	- if this fails, we clean up `CCompiler::Cleanup()` and return
	- otherwise data and driver fits within 32kB, or it's `m_bBankSwitched`
- if it's `m_bBankSwitched`:
	- we need to `ResolveLabelsBankswitched()`; this can fail so return if we do fail
	- then we need to `UpdateFrameBanks()`
	- and `UpdateSongBanks()`
	- finally, we `EnableBankswitching()` in the driver by patching some flags in the song header
- otherwise, for nonbankswitched songs:
	- we `ResolveLabels()`
	- and we `ClearSongBanks()`
- finally, we deal with the rest:
	- update DPCM sample pointers `UpdateSamplePointers()`
	- `CalculateLoadAddresses()` depending on what mode we're in
- load the driver identifier with `LoadNSFDRV()` and driver proper with `LoadDriver()`
- set the addresses of the song and the driver `SetDriverSongAddress()`
- then we write out the file, specific to the format

## Data Chunks

Each angstrom of data is stored in what's known as a Chunk.

## `CCompiler::CompileData()`

