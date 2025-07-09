# Datatypes and constants

- as of Dn-FamiTracker version 0.5.1.1
- last update: 2025-07-09
- based on FamiTrackerTypes.h

## structs and enums

### `stChanNote` struct

- a "Channel Note" struct which represents a single row in a single channel.
- consists of:
	- `Note`: char
	- `Octave`: char
	- `Vol`: char
	- `Instrument`: `MAX_INSTRUMENTS`
	- `EffNumber`: `effect_t[MAX_EFFECT_COLUMNS]`
	- `EffParam`: char`[MAX_EFFECT_COLUMNS]`

### `note_t` enum

- base type: unsigned char

| *Name*    | *Numeric value* |
| --------- | --------------- |
| `NONE`    | 0x00            |
| `NOTE_C`  | 0x01            |
| `NOTE_Cs` | 0x02            |
| `NOTE_D`  | 0x03            |
| `NOTE_Ds` | 0x04            |
| `NOTE_E`  | 0x05            |
| `NOTE_F`  | 0x06            |
| `NOTE_Fs` | 0x07            |
| `NOTE_G`  | 0x08            |
| `NOTE_Gs` | 0x09            |
| `NOTE_A`  | 0x0A            |
| `NOTE_As` | 0x0B            |
| `NOTE_B`  | 0x0C            |
| `RELEASE` | 0x0D            |
| `HALT`    | 0x0E            |
| `ECHO`    | 0x0F            |

- Additional constants:

| *Name*             | Value          |
| ------------------ | -------------- |
| `ECHO_BUFFER_NONE` | char: `'\xFF'` |
| `ECHO_BUFFER_HALT` | char: `'\x7F'` |
| `ECHO_BUFFER_ECHO` | char: `'\x80'` |

### `effect_t` struct

- base type: unsigned char

| *`effect_t` value*       | *Char*   | *Numeric value* |
| ------------------------ | -------- | --------------- |
| `EF_NONE`                | `'\xff'` | 0x00            |
| `EF_SPEED`               | `F`      | 0x01            |
| `EF_JUMP`                | `B`      | 0x02            |
| `EF_SKIP`                | `D`      | 0x03            |
| `EF_HALT`                | `C`      | 0x04            |
| `EF_VOLUME`              | `E`      | 0x05            |
| `EF_PORTAMENTO`          | `3`      | 0x06            |
| `EF_PORTAOFF`            | `'\xff'` | 0x07            |
| `EF_SWEEPUP`             | `H`      | 0x08            |
| `EF_SWEEPDOWN`           | `I`      | 0x09            |
| `EF_ARPEGGIO`            | `0`      | 0x0A            |
| `EF_VIBRATO`             | `4`      | 0x0B            |
| `EF_TREMOLO`             | `7`      | 0x0C            |
| `EF_PITCH`               | `P`      | 0x0D            |
| `EF_DELAY`               | `G`      | 0x0E            |
| `EF_DAC`                 | `Z`      | 0x0F            |
| `EF_PORTA_UP`            | `1`      | 0x10            |
| `EF_PORTA_DOWN`          | `2`      | 0x11            |
| `EF_DUTY_CYCLE`          | `V`      | 0x12            |
| `EF_SAMPLE_OFFSET`       | `Y`      | 0x13            |
| `EF_SLIDE_UP`            | `Q`      | 0x14            |
| `EF_SLIDE_DOWN`          | `R`      | 0x15            |
| `EF_VOLUME_SLIDE`        | `A`      | 0x16            |
| `EF_NOTE_CUT`            | `S`      | 0x17            |
| `EF_RETRIGGER`           | `X`      | 0x18            |
| `EF_DELAYED_VOLUME`      | `M`      | 0x19            |
| `EF_FDS_MOD_DEPTH`       | `h`      | 0x1A            |
| `EF_FDS_MOD_SPEED_HI`    | `I`      | 0x1B            |
| `EF_FDS_MOD_SPEED_LO`    | `j`      | 0x1C            |
| `EF_DPCM_PITCH`          | `W`      | 0x1D            |
| `EF_SUNSOFT_ENV_TYPE`    | `H`      | 0x1E            |
| `EF_SUNSOFT_ENV_HI`      | `I`      | 0x1F            |
| `EF_SUNSOFT_ENV_LO`      | `J`      | 0x20            |
| `EF_SUNSOFT_NOISE`       | `W`      | 0x21            |
| `EF_VRC7_PORT`           | `H`      | 0x22            |
| `EF_VRC7_WRITE`          | `I`      | 0x23            |
| `EF_NOTE_RELEASE`        | `L`      | 0x24            |
| `EF_GROOVE`              | `O`      | 0x25            |
| `EF_TRANSPOSE`           | `T`      | 0x26            |
| `EF_N163_WAVE_BUFFER`    | `Z`      | 0x27            |
| `EF_FDS_VOLUME`          | `E`      | 0x28            |
| `EF_FDS_MOD_BIAS`        | `Z`      | 0x29            |
| `EF_PHASE_RESET`         | `=`      | 0x2A            |
| `EF_HARMONIC`            | `K`      | 0x2B            |
| `EF_TARGET_VOLUME_SLIDE` | `N`      | 0x2C            |
| `EF_COUNT`               |          | 0x2D            |

### `machine_t` enum

- base type: unsigned char

| *Name* | *Numeric value* |
| ------ | --------------- |
| `NTSC` | 0               |
| `PAL`  | 1               |

### `vibrato_t` enum

- base type: unsigned char

| *Name*        | *Numeric value* |
| ------------- | --------------- |
| `VIBRATO_OLD` | 0               |
| `VIBRATO_NEW` | 1               |

### `sequence_t` enum

- base type: int

| *Name*          | *Numeric value* |
| --------------- | --------------- |
| `SEQ_VOLUME`    | 0               |
| `SEQ_ARPEGGIO`  | 1               |
| `SEQ_PITCH`     | 2               |
| `SEQ_HIPITCH`   | 3               |
| `SEQ_DUTYCYCLE` | 4               |
| `SEQ_COUNT`     | 5               |

## Constants

| *Constant name*      | *Value*                          | Description                                                                                                       |
| -------------------- | -------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `MAX_INSTRUMENTS`    | int: 64                          | Maximum number of instruments to use                                                                              |
| `HOLD_INSTRUMENT`    | int: `0xFF`                      | Hold instrument index                                                                                             |
| `MAX_SEQUENCES`      | int: 128                         | Maximum number of sequence lists                                                                                  |
| `MAX_SEQUENCE_ITEMS` | int: 252                         | Maximum number of items in each sequence                                                                          |
| `MAX_PATTERN`        | int: 256                         | Maximum number of patterns per channel                                                                            |
| `MAX_FRAMES`         | int: 256                         | Maximum number of frames                                                                                          |
| `MAX_PATTERN_LENGTH` | int: 256                         | Maximum length of patterns (in rows).                                                                             |
| `MAX_DSAMPLES`       | int: 64                          | Maximum number of DPCM samples,                                                                                   |
| `MAX_SAMPLE_SPACE`   | int: `0x40000`                   | 256kB; Sample space available (from \$C000-\$FFFF), may now switch banks                                          |
| `MAX_EFFECT_COLUMNS` | int: 4                           | Number of effect columns allowed                                                                                  |
| `MAX_TRACKS`         | int: 64                          | Maximum numbers of tracks allowed                                                                                 |
| `MAX_TEMPO`          | int: 255                         | Max tempo                                                                                                         |
| `MIN_SPEED`          | int: 1                           | Min speed                                                                                                         |
| `MAX_GROOVE`         | int: 32                          | Maximum number of grooves                                                                                         |
| `ECHO_BUFFER_LENGTH` | int: 3                           | Maximum number of entries in the echo buffer                                                                      |
| `MAX_CHANNELS`       | int: 28                          | Maximum number of available channels (deprecated)                                                                 |
| `CHANNELS_DEFAULT`   | int: 5                           | Default number of channels (deprecated)                                                                           |
| `CHANNELS_VRC6`      | int: 3                           | Number of VRC6 channels (deprecated)                                                                              |
| `CHANNELS_VRC7`      | int: 6                           | Number of VRC7 channels (deprecated)                                                                              |
| `OCTAVE_RANGE`       | int: 8                           | Range of available octaves                                                                                        |
| `NOTE_RANGE`         | int: 12                          | Range of available notes within an octave                                                                         |
| `NOTE_COUNT`         | int: `OCTAVE_RANGE * NOTE_RANGE` | Range of all available notes                                                                                      |
| `INVALID_INSTRUMENT` | int: -1                          | Index of an invalid instrument. Might collide with `HOLD_INSTRUMENT`?                                             |
| `MAX_VOLUME`         | int: `0x10`                      | Max allowed value in volume column. The actual meaning is no specific volume information, rather than max volume. |

