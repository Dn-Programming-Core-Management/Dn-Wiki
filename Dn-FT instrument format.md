# Dn-FamiTracker Instrument format

- Last updated 2025-01-27
- Version 2.4

---

## File header

| _Data type_  | _Unit size (bytes)_    | _Repeat_ | _Object/relevant variable in code_                               | _Description_          | _Valid values_                     | _Notes_                |
| ------------ | ---------------------- | -------- | ---------------------------------------------------------------- | ---------------------- | ---------------------------------- | ---------------------- |
| char[4]      | 4                      |          | `INST_HEADER`                                                    | Identifier string      | `FTI`                              | Null-terminated string |
| unsigned int | 4                      |          | `INST_VERSION`                                                   | Module version         | `2.4`                              | Null-terminated string |
| char         | 1                      |          | `m_pInstrumentManager->GetInstrument()->m_iType`                 | Instrument type        | `enum inst_type_t`                 | See table.             |
| int          | 4                      |          | `strlen()` of `m_pInstrumentManager->GetInstrument()->GetName()` | Instrument name length | 0 to `CInstrument::INST_NAME_MAX`. |                        |
| char[]       | Instrument name length |          | `m_pInstrumentManager->GetInstrument()->GetName()`               | Instrument name        |                                    |                        |

## Instrument type value

| *Chip* | *Index* |
| ---- | ----- |
| 2A03 | 0x01  |
| VRC6 | 0x02  |
| VRC7 | 0x03  |
| FDS  | 0x04  |
| N163 | 0x05  |
| S5B  | 0x06  |

## Base instrument format

| _Data type_ | _Unit size (bytes)_ | _Repeat_           | _Object/relevant variable in code_ | _Description_          | _Valid range_             | _Notes_                                                                              | _Present in instrument version_ |
| ----------- | ------------------- | ------------------ | ---------------------------------- | ---------------------- | ------------------------- | ------------------------------------------------------------------------------------ | ------------------------------- |
| int         | 4                   |                    | `SEQ_COUNT`                        | Sequence count         | `enum sequence_t`         | Number of defined sequence types.  <br>Volume, Arpeggio, Pitch, Hi-Pitch, Duty cycle | ?                               |
| char        | 1                   | Per sequence count | `CSeqInstrument::m_iSeqEnable[]`   | Sequence enable        | 0 to 1                    | 0 = sequence is disabled, 1 = sequence is enabled                                    | ?                               |
| int         | 4                   | ^                  | `CSequence::m_iItemCount`          | Sequence item count    | 0 to `MAX_SEQUENCE_ITEMS` | Written only when sequence is enabled.                                               | 2.0+                            |
| int         | 4                   | ^                  | `CSequence::m_iLoopPoint`          | Sequence loop point    | -1 to `(m_iItemCount -1)` | Written only when sequence is enabled                                                | 2.0+                            |
| int         | 4                   | ^                  | `CSequence::m_iReleasePoint`       | Sequence release point | -1 to `(m_iItemCount -1)` | Written only when sequence is enabled                                                | 2.0+                            |
| int         | 4                   | ^                  | `CSequence::m_iSetting`            | Sequence setting       | `seq_setting_t`           |                                                                                      | 2.2+                            |
| char[]      | Sequence item count | ^                  | `CSequence::m_cValues[]`           | Sequence data          |                           |                                                                                      | 2.0+                            |

### Notes

- Information is based on `CSeqInstrument::SaveFile()`

### 2A03 instruments

| _Data type_    | _Unit size (bytes)_     | _Repeat_                     | _Object/relevant variable in code_              | _Description_                                                                            | _Valid range_            | _Notes_                                                                       | _Present in instrument version_ |
| -------------- | ----------------------- | ---------------------------- | ----------------------------------------------- | ---------------------------------------------------------------------------------------- | ------------------------ | ----------------------------------------------------------------------------- | ------------------------------- |
| CSeqInstrument | size of CSeqInstrument  |                              |                                                 | See [Base instrument format](Dn-FT%20instrument%20format.md#Base%20instrument%20format). |                          |                                                                               |                                 |
| int            | 4                       |                              | `CInstrument2A03::GetSampleCount()`             | DPCM sample assignment count                                                             | 0 to `MAX_DSAMPLES`      |                                                                               |                                 |
| char           | 1                       | DPCM sample assignment count | `Note`                                          | DPCM assignment count                                                                    | 0 to `NOTE_COUNT`        | Written only when a sample exists at that note.                               |                                 |
| char           | 1                       | ^                            | `CInstrument2A03::m_cSamples[Octave][Note]`     | DPCM sample assignment index                                                             | 0 to 0x7F                | If assignement is greater than `MAX_DSAMPLES`, it gets assigned to 0 instead. |                                 |
| char           | 1                       | ^                            | `CInstrument2A03::m_cSamplePitch[Octave][Note]` | DPCM sample pitch                                                                        | 0x0 to 0xF               | Written only when a sample exists at that note in FT 050b1+.                  |                                 |
| char           | 1                       | ^                            | `CInstrument2A03::m_cSampleDelta[Octave][Note]` | DPCM delta offset of a given note                                                        | -1 to 0x7F               | Versions before 2.4 will write no offset                                      | 2.4+                            |
| int            | 4                       |                              | `SampleCount`                                   | DPCM sample count                                                                        | 0 to `MAX_DSAMPLES`      |                                                                               |                                 |
| int            | 4                       | DPCM sample count            |                                                 | DPCM sample index                                                                        | 0 to `MAX_DSAMPLES - 1`  |                                                                               |                                 |
| int            | 4                       | ^                            |                                                 | DPCM sample name length                                                                  | 0 to `MAX_NAME_SIZE - 1` |                                                                               |                                 |
| char[]         | DPCM sample name length | ^                            |                                                 | DPCM sample Name                                                                         |                          | Not null terminated.                                                          |                                 |
| int            | 4                       | ^                            |                                                 | DPCM sample size                                                                         | Not bounded?             |                                                                               |                                 |
| char[]         | DPCM sample size        | ^                            |                                                 | DPCM sample data                                                                         |                          | the code currently does not check for DPCM length.                            |                                 |
#### Notes

- Information is based on `CInstrument2A03::SaveFile()`
- v2.4 adds DPCM delta settings.
- Only 72 notes are defined in version 1. Version 2+ has all 96 notes defined.
- In FamiTracker 0.5.0 beta, the DPCM format has been changed to only count notes with DPCM assignments.
- DPCM samples in a given module are limited to either `MAX_DSAMPLES` number of samples, or 256kiB total memory use
- If an identical DPCM sample in name and data is found, it will not be loaded

### VRC6 instruments

- Same format as [Base instrument format](Dn-FT%20instrument%20format.md#Base%20instrument%20format).
- Fifth sequence type is named `Pulse Width`.

#### Notes

- Information is based on `CSeqInstrument::SaveFile()`
- Similar to 2A03 instruments but with no special considerations for DPCM

### VRC7 instruments

| _Data type_ | _Unit size (bytes)_ | _Repeat_ | _Object/relevant variable in code_ | _Description_         | _Valid range_ | _Notes_                                  | _Present in instrument version_ |
| ----------- | ------------------- | -------- | ---------------------------------- | --------------------- | ------------- | ---------------------------------------- | ------------------------------- |
| int         | 4                   |          | `CInstrumentVRC7::m_iPatch`        | VRC7 patch number     |               | Hardware patch number of the instrument. | 2+                              |
| char[8]     | 8                   |          | `CInstrumentVRC7::m_iRegs[]`       | Custom patch settings |               | Patch settings of hardware patch 0.      | 2+                              |

#### Notes

- Information is based on `CInstrumentVRC7::SaveFile()`

### FDS instruments

| _Data type_     | _Unit size (bytes)_ | _Repeat_                         | _Object/relevant variable in code_   | _Description_               | _Valid range_             | _Notes_ | _Present in instrument version_ |
| --------------- | ------------------- | -------------------------------- | ------------------------------------ | --------------------------- | ------------------------- | ------- | ------------------------------- |
| char[WAVE_SIZE] | `WAVE_SIZE`         |                                  | `CInstrumentFDS::m_iSamples[]`       | Wave data                   |                           |         |                                 |
| char[MOD_SIZE]  | `MOD_SIZE`          |                                  | `CInstrumentFDS::m_iModulation[]`    | Modulation table            |                           |         |                                 |
| int             | 4                   |                                  | `CInstrumentFDS::m_iModulationSpeed` | Instrument modulation rate  |                           |         |                                 |
| int             | 4                   |                                  | `CInstrumentFDS::m_iModulationDepth` | Instrument modulation depth |                           |         |                                 |
| int             | 4                   |                                  | `CInstrumentFDS::m_iModulationDelay` | Instrument modulation delay |                           |         |                                 |
| char            | 1                   | `CInstrumentFDS::SEQUENCE_COUNT` | `CSequence::m_iItemCount`            | Sequence item count         | 0 to `MAX_SEQUENCE_ITEMS` |         |                                 |
| int             | 4                   | ^                                | `CSequence::m_iLoopPoint`            | Sequence loop point         | -1 to`SeqCount - 1`       |         |                                 |
| int             | 4                   | ^                                | `CSequence::m_iReleasePoint`         | Sequence release point      | -1 to`SeqCount - 1`       |         |                                 |
| int             | 4                   | ^                                | `CSequence::m_iSetting`              | Sequence setting type       | `seq_setting_t`           |         |                                 |
| char[]          | Sequence item count | ^                                | `CSequence::m_cValues[Index]`        | Sequence data               |                           |         |                                 |

#### Notes

- Information is based on `CInstrumentFDS::LoadFile()`
- FDS instruments stores its own sequences via `CInstrumentFDS::StoreSequence()`, separate from `CSeqInstrument::Store()`
	- These sequences only store 3 types, Volume, Arpeggio, and Pitch.
- In version 2.2, volume range was 0-15. Later versions have volume ranges from 0-31.
	- Loading FDS instrument versions 2.2 and older will have their volume doubled for compatibility.

### N163 instruments

| _Data type_       | _Unit size (bytes)_    | _Repeat_       | _Object/relevant variable in code_          | _Description_                                                                            | _Valid range_            | _Notes_                                                                           | _Present in instrument version_ |
| ----------------- | ---------------------- | -------------- | ------------------------------------------- | ---------------------------------------------------------------------------------------- | ------------------------ | --------------------------------------------------------------------------------- | ------------------------------- |
| CSeqInstrument    | size of CSeqInstrument |                |                                             | See [Base instrument format](Dn-FT%20instrument%20format.md#Base%20instrument%20format). |                          |                                                                                   |                                 |
| int               | 4                      |                | `m_iWaveSize`                               | N163 wave size                                                                           | 4 to `MAX_WAVE_SIZE`     | In FT 0.5.0 beta, `m_iWaveSize` is determined by `m_iWaveCount / remaining_bytes` |                                 |
| int               | 4                      |                | `m_iWavePos`                                | N163 wave position                                                                       | 0 to `MAX_WAVE_SIZE - 1` | Ignored if `m_bAutoWavePos` is enabled                                            |                                 |
| int               | 4                      |                | `m_bAutoWavePos`                            | Automatic wave data RAM allocation                                                       | 0, nonzero               |                                                                                   |                                 |
| int               | 4                      |                | `m_iWaveCount`                              | N163 wave count                                                                          | 1 to `MAX_WAVE_COUNT`    |                                                                                   |                                 |
| char[m_iWaveSize] | `m_iWaveSize`          | `m_iWaveCount` | `m_iSamples[MAX_WAVE_COUNT][MAX_WAVE_SIZE]` | N163 wave sample                                                                         | 0 to 15                  |                                                                                   |                                 |

#### Notes

- Information is based on `CInstrumentN163::Store()` and instrument file binary analysis
- Automatic wave data RAM allocation feature is from FT 0.5.0 beta.
- Fifth sequence type is named `Wave Index`.

### S5B instruments

- Same format as [Base instrument format](Dn-FT%20instrument%20format.md#Base%20instrument%20format).
- Fifth sequence type is named `Noise / Mode`.

#### Notes

- Information is based on `CSeqInstrument::Store()`
- Similar to 2A03 instruments but with no special considerations for DPCM
