---
name: fugue
description: Take a fugue from subject to finished render in the open neu.daw project, stopping for approval at each checkpoint.
---

Write a fugue in the open project, from subject to finished render. Write the subject within the first few calls, then one section at a time, so the user hears progress early. Plan in short written lines, not in your head.

Brief and checkpoints

- Ask for what the brief leaves out: key, mood, voices (three if unsure), instrument, length.
- Stop for the user's approval after the subject, after the answer and countersubject, on the plan of episodes and keys, before the stretto, and on the ending (a minor final chord or a major one). Start each stop with a line "Checkpoint n of 5:", the notes by name and bar and what the checks found, then wait. Write that line even when approval was given in advance.

Setup

- `get_project` first. One MIDI track per voice, ordered high to low. Tempo with `set_bpm`, instrument with `set_track_instrument`.
- Piano unless the brief names another instrument. It fades held notes within a second: hold suspensions and pedal points with pedal lines in the notes files, or re-strike them.
- Call `save_version` before each phase below.

Model

- Bach's C minor fugue (WTC I): three voices, 31 bars, episodes alternating with entries, the first in the relative major.

Subject

- Offer two or three subjects of different character. Each has a recognisable head that survives being cut into fragments, and starts on the first or fifth degree.
- Run `analyze_stretto` on it at once. If no delay fits, change it now.

Exposition

- Answer: tonal when the subject starts on or stresses the fifth degree early, or modulates; otherwise real.
- Countersubject: contrasting rhythm, contrary motion, mostly thirds and sixths, no fifths (they invert to fourths). Check with `validate_voice_leading` and enableInvertibleCounterpoint.
- Entries start at the same point in the bar; each new voice enters on top or bottom. `analyze_motif` rates a correct tonal answer approximate; accept that.

Plan

- Before the episodes, list each entry's key (relative major, dominant, subdominant) and one climax bar about three quarters through.
- Each episode states its start key, target key, the fragment it repeats and how many times. Give each episode its own figure. Episodes thin to two voices; the voice that enters next rests. `fragment_clip`, `sequence_clip` and `insert_bars` help; confirm keys with `analyze_key`.
- Never bring an entry in over a root-position tonic chord.

Stretto and ending

- Use a pairing `analyze_stretto` found without strong-beat clashes.
- Coda: a pedal point, a last statement of the subject's head, a ritardando over the last bars with `add_tempo_point` and `set_tempo_point_ramp`, then the final chord.

Checking

- Check new ABC with the drafts of `validate_voice_leading`, then write that same ABC with `write_abc_notes`; check the whole piece after. Harmony rules flag sevenths that resolve late or into another seventh; explain those, don't rewrite. Fix other errors or say why they stand.

Files

- With agent files, reshape written notes and do the dynamics pass through `export_notes` and `import_notes`, check after each import and `undo` it if worse; fix a few notes with `update_midi_notes`.

Dynamics and articulation

- Velocity rises to the climax; the voice with the subject a little louder; short notes shorter and softer; a tiny gap before leaps and repeated notes; no random timing.

Render and hand-over

- Render the piece and each voice with `render_range`. Aim near -18 LUFS, true peak below -1 dBTP, no clipped samples. Master effects with `add_effect`, pans with `set_track_pan`.
- Mark each part of the form with `add_section`, `save_version` with a name for the state, and report in plain words with a note-name sketch of each section.
