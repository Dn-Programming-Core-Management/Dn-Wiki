# NSF Memory Layout

This article focuses on the memory layout of the NSF export in a player.
For details about the binary file format: https://www.nesdev.org/wiki/NSF

## Non-bankswitching

- NSFs are compiled in this format when total NSF data is less than 32kB
- Takes place within $8000-FFFF

### Compressed

- Arranged in this format when music + driver is less than 16kB
- "Relocated" driver and music data
	- Currently unknown why they have to be swapped
- Load address is relative to $C000, padding the music + driver data to be
  contiguous with sample data if necessary

```
+------------+
| System     | $0000-7FFF: C.I. RAM, unused registers, W. RAM
+------------+
| Padding    | $8000-....: Padding
| Music Data | $....-BFFF: Music data (Instruments, Frames, Patterns), NSF driver
| ...        |
| NSF Driver |
| ...        |
| DPCM Data  | $C000-FFFF: DPCM data
|            |
+------------+
```

### Non-compressed layout

- Arranged in this format when music + driver is more than 16kB

```
+------------+
| System     | $0000-7FFF: C.I. RAM, unused registers, W. RAM
+------------+
| NSF Driver | $8000-BFFF*: NSF driver, Music data (Instruments, Frames, Patterns)
| ...        |
| Music Data |
| ...        |
| DPCM Data  | $C000*-FFFF: DPCM data
|            |
+------------+
```

\* Music data may end at $C000 or later; this does not matter unless overall data ***exceeds 32KB***

## Bankswitching

- Multiple NSF bank pages are switched together to form a bigger sequential "chunk"
- Enabled when driver, music, and sample data exceeds 32KB
	- or when sample data exceeds 16KB

```
+------------+
| System     | $0000-7FFF: C.I. RAM, unused registers, W. RAM
+------------+
| Fixed Bank | $8000-AFFF: NSF driver, Music data (Instruments, Frames, Patterns)
|            | Non-bankswitching
|            |
+------------+
| Music Bank | $B000 - $BFFF: Bankswitching Frame/Pattern track data, 1 page
+------------+
| DPCM  Bank | $C000-EFFF: DPCM samples, 3 pages
|            |
|            |
+------------+
| Empty Bank | $F000-FFFF: Reserved fixed bank, nonswitching for TNS-HFC compatibility
+------------+
```
