# Tempo

- Last updated: 2026-04-09

FamiTracker's tempo system is ticked upon every engine tick.

The tempo system consists of three components: the Module Tempo, the Engine Speed, and the Tick Speed. This allows FamiTracker to replicate any given tempo BPM, for a given engine speed. If you want a tick-based tempo, set BPM to 150 (or set the BPM mode to Fixed in 0CC-FT and its derivatives).

Grooves are sequenced Tick Speed values that update upon every new row. This may be thought of as an "Fxx macro". Grooves were added in 0CC-FamiTracker 0.3.4. To enable Grooves, set the Speed mode to Grooves.

Groove processing is composed of two parts: the Groove Index and the Groove Position. The Groove index selects which groove to use, and the Groove position indicates what current part of the groove to index.

The Engine Speed dictates the playback speed of the module. By default it is set to the region engine speed, which is 60 Hz for NTSC and 50 Hz for PAL. Note that this is a whole integer, and not the actual video refresh rate of the NTSC/PAL NES.

Some NSF players do not support changing the engine speed, being at a fixed 60/50 Hz refresh rate due to it being the only constant and convenient timing source the NES has. If your exported NSF has a specified custom engine speed, it may be played faster or slower than expected on these NSF players.

Accurate VSync-based engine speed was later added in FT 0.5.0 beta 10, where the tempo system was overhauled to use microsecond-based Engine speed instead of integer Hertz.

The tempo system uses a timer with three parts: the Accumulator, the Remainder (Modulus), and the Decrement. This allows any errors in timing to cancel out over time, although when using tempo values that result in a non-integer Tick Speed, the unevenness in tempo ("tempo aliasing") might be audible.

## Tracker

See [The Tracker player update sequence](Tracker_player_update_sequence.md) to see the entire update sequence.

First, in `CSoundGen::BeginPlayer`, the tempo is initialized in `CSoundGen::ResetTempo`:

- Fetch the saved Tick Speed/Groove Index, Module Tempo, and Groove Position from the module document
- If the Tick Speed and Tempo has been updated during playback (for example, grooves or `Fxx` commands), fetch the latest global state of those. (`CSoundGen::ApplyGlobalTempoState`)
	- If grooves are not enabled (Groove Position < 0), set Groove Index to -1
	- Else, Set Groove Index and Groove Position from current channel state
- Then, Setup the Decrement and Remainder `CSoundGen::SetupSpeed`:
	- If we are in fixed tempo mode (`m_iTempo == 0`), set Decrement to 1 and Remainder to 0.
	- Else,
		- Decrement = Module Tempo \* 24 / Tick Speed
		- Remainder = Module Tempo \* 24 \% Tick Speed

The tempo is then updated during each playback tick in `CSoundGen::OnIdle()`:

- `CSoundGen::RunFrame`: First, if the Accumulator is <= 0:
	- If grooves are enabled (Groove Index != -1 and current groove is not NULL):
		- Set Tick Speed to the value found at the Groove Position of the current Groove Index.
		- Setup the Decrement and Remainder again in `CSoundGen::SetupSpeed` (see above for algorithm)
		- Increment Groove Position for next row
	- read a row, enable row updates and cache the current BPM tick rate (for average BPM)
- Then read note data and global effects
	- process groove, tempo, and BPM effects and update Module Tempo, Tick Speed, Groove Index, and Groove Position if applicable.
- `CSoundGen::UpdatePlayer()`: If playing,
	- `CheckControl()` (not relevant, handles pattern jumping)
	- If Accumulator is <= to 0
		- If Module Tempo is nonzero, increment Accumulator with Tick Speed subtracted with Remainder
		- Else, increment Accumulator with Engine Speed multiplied by 60, subtracted with Remainder
		- C++ pseudocode: `Accumulator += (Module_Tempo ? 60 * EngineSpeed : Tick_Speed) - Remainder;`
	- Decrement Accumulator with Decrement

## NSF Driver

The NSF driver's tempo system works much the same as the tracker's.

Groove Position however, is a little more complicated. It is an index into the Groove Table, which contains all the groove patterns of the module. The groove table also contains the return index for the Groove Position, so when the end of a groove is encountered, that gets written into the Groove Position variable.

See [The NSF driver update sequence](NSF-driver-update-sequence.md) to see the entire update sequence.

The tempo is initialized:
- `ft_load_song`
	- Get Engine Speed Divider (3600) and store in `var_Tempo_Dec`
	- `ft_load_track`
		- Load Groove Index and Groove Position
		- Load track Module Tempo and Tick Speed
- Calculate speed counter (`ft_calculate_speed`)
	- If we are in fixed tempo mode (`Module_Tempo == 0`), set Decrement to 1 and Remainder to 0.
	- Else,
		- Decrement = Module Tempo \* 24 / Tick Speed
		- Remainder = Module Tempo \* 24 \% Tick Speed
- The Accumulator is initialized to 0

Upon every engine tick:

- First, the Accumulator is checked if it's less than or equal to 0
	- if so, trigger a new row update
	- then restore speed for incoming tempo tick (`ft_restore_speed`)
		- If we are in fixed tempo mode (`Module_Tempo == 0`)
			- Set Accumulator to current Tick Speed (`ft_fetch_speed`)
				- if Tick Speed setting is zero, fetch the value found at the Groove Position of the current Groove Index.
		- Else
			- Accumulator += Engine Speed Divider
			- Accumulator -= Remainder
	- If Groove Position != 0,
		- Calculate speed counter (see above for algorithm)
		- Increment Groove Position
			- If we reached the end of the current Groove table (`Groove Position == 0`), set Groove Position to the beginning
- Then, clock the tempo accumulator
	- Calculate speed counter (see above for algorithm)
	- Then Accumulator -= Decrement
