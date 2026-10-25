---
name: piano-arrange
description: Arrange tracks of the open neu.daw project so one pianist can play them, one track per hand, checking every round at finger level.
---

Arrange music in the open project so one pianist can play it, one MIDI track per hand. Work in rounds: thin the music to melody and bass, then add voices back one at a time; the user judges each round.

Brief

- Ask which tracks to arrange and the player's hand size: the widest span from thumb to little finger. Ask before the first edit. Without an answer use a minor seventh (10 semitones); allow an octave or wider only when the user agrees.

Setup

- `get_project` first. Call `save_version` before the first edit and before each round.
- Keep the originals: `duplicate_track` each source track, mute the copies with `set_track_mute`, and `rename_track` them to say they are the originals. Never edit them.
- One track for the right hand and one for the left, each on piano with `set_track_instrument`.

Rounds

- Round one: the melody in the right hand and the bass in the left, nothing else.
- Each later round adds back one voice, or part of one, in the hand that can take it. Leave a note out, move it an octave, or give it to the other hand rather than fail the finger check.
- Run each round on notes files: `export_notes`, split and thin the voices with a script, check, then `import_notes`. A round is then one undo step, and a refused import changes nothing. Without file access, replace bar ranges with `write_abc_notes`.
- Send edits that depend on each other one after another, never in parallel.

Finger check

- Script this check against the notes files and run it on both hands before every import. Import only a round with no finding.
- A note's length is how long its key is held. At each note start, the hand's keys are its track's notes that have started and not ended.
- At most five keys per hand. A held key may not be struck again by either hand.
- Fingers 1 (thumb) to 5 go by rising pitch in the right hand, falling in the left. A held key keeps its finger until it ends; no finger holds two keys. Search for a fingering that meets every rule.
- With S the hand size in semitones, two held keys may span at most: fingers 1 and 2: S minus 4; 1 and 3: S minus 3; 1 and 4: S minus 1; 1 and 5: S; 2 and 5: S minus 4; 2 and 4, 3 and 5: 5; neighbours from 2 to 5: 3, or 4 when S is 12 or more.
- Each finding names bar, beat, hand, keys and reason: too many keys, a held key struck again, or two fingers and their span. Fix it in the same round, or report its bar and beat to the user when no placement fits.

Pedal

- To keep a note sounding after its finger must move, shorten it and hold it with "<position> pedal down" and "<position> pedal up" lines in the notes files. Change the pedal with the harmony. A pedalled note frees its finger once its key ends.

Showing each round

- The agent cannot hear the result. After each round list, bar by bar, the notes of each hand by name, what was left out or moved, and what the check found. Ask the user to check the piano roll and listen, and wait before the next round.

Hand-over

- `save_version` with a name for the arranged state, and report what each hand plays, which voices were dropped, and every place the check could not clear.
