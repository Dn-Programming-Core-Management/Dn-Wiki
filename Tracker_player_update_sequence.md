# Tracker player update sequence

- Last updated: 2026-04-09

Documents the player update order of the tracker.

This loop happens within `CSoundGen::OnIdle`:

```cpp
// Access the document object, skip if access wasn't granted to avoid gaps in audio playback
if (m_pDocument->LockDocument(0)) {

	// Read module framerate
	m_iFrameRate = m_pDocument->GetFrameRate();

	RunFrame();

	// Play queued notes
	PlayChannelNotes();

	// Update player
	UpdatePlayer();

	// Channel updates (instruments, effects etc)
	UpdateChannels();

	// Unlock document
	m_pDocument->UnlockDocument();
}

// Update APU registers
UpdateAPU();
```

General unfolded function calls:

```cpp
m_iFrameRate = m_pDocument->GetFrameRate(); // Read module framerate
RunFrame();	// If m_iTempoAccum <= 0, read a row, enable row updates and cache the current BPM tick rate
CSoundGen::PlayChannelNotes();
    CSoundGen::PlayNote();
        CSoundGen::EvaluateGlobalEffects();
        CChannelHandler::HandleDelay();
        CChannelHandler::HandleNoteData();
            // (handle echo buffer)
                CChannelHandler::WriteEchoBuffer();
            // (clear note cut and note release)
            // (handle effects)
                CChannelHandler::HandleEffect();
            // (handle volume command and Nxy)
            // (handle instrument command)
            switch (pNoteData->Note) {
                case NONE:
                    HandleEmptyNote();
                    break;
                case HALT:
                    m_bRelease = false;
                    HandleCut();
                    break;
                case RELEASE:
                    HandleRelease();
                    break;
                default:
                    HandleNote(pNoteData->Note, pNoteData->Octave);
                    break;
            }
            // (handle note slide)
            if (new_instrument || note_trigger)
                CChannelHandler::HandleInstrument();
                    if (new_instrument)
                        m_pInstHandler->LoadInstrument(pInstrument);
                    if (note_trigger)
                        m_pInstHandler->TriggerInstrument();
CSoundGen::UpdatePlayer();
	// first, check if there's a jump or skip command to be processed
	// then, tick tempo system
CSoundGen::UpdateChannels();  // run instruments, effects, etc.
    CChannelHandler::ProcessChannel();
        UpdateDelay();
        UpdateNoteCut();
        UpdateNoteRelease();
        UpdateNoteVolume();           // // //
        UpdateTranspose();            // // //
        if (m_iVolSlideTarget < 0)    // // !!
            UpdateVolumeSlide();
        else
            UpdateTargetVolumeSlide();
        UpdateVibratoTremolo();
        UpdateEffects();
        if (m_pInstHandler) m_pInstHandler->UpdateInstrument();       // // //


CSoundGen::UpdateAPU();  // write to registers
	// this function also happens to blit the current audio buffer
	// to either the wave output or the sound driver
```
