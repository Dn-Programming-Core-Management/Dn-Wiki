# NSF driver update sequence

- Last updated: 2026-04-09

Documents the player update order (`ft_music_play`) of the NSF driver.

In order:

- Run delayed channels
- Check tempo accumulator to do a row update
	- if counter <= 0:
		- `ft_do_row_update`
			- frame handling
			- `ft_read_channels` (foreach channels do:)
				- `ft_read_pattern`
					- `ft_read_note`
						- switch case pattern command
							- handle volume commands
							- handle instrument commands
							- handle effect commands
						- `ft_push_echo_buffer`
						- handle note off
						- handle note release
						- load echo buffer
						- `ft_push_echo_buffer`
						- handle musical note
				- `ft_read_is_done`
		- `ft_restore_speed`
	- else:
		- `ft_skip_row_update` (jumps to after row update code)
- Decrement tempo accumulator counter
- `ft_loop_fx_state`
	- Sxx handling
	- delayed transpose/release handling
- `ft_loop_channels` (foreach channels do:)
	- `ft_run_effects` (handles the rest of the other effects)
	- `ft_run_instrument`
	- `ft_calc_period`
	- Nxy handling
- `ft_update_<chip>` (foreach chip do:)
	- (register writes)

