# ApexForge

Apex-level multi-chair live step-grid + instrument desk.

Elevated from SummitForge / Zenith / Peak / Apex desks. Full implementation of WorkinWithAI Audio Engine rules. Not a stub.

**Free.**

## Next-level features
- 10 chairs: kick, snare, hat, crash, tom, bass, piano, guitar, violin, trumpet
- **Per-step velocity** (click = 100, shift-click cycles 60/80/100/127)
- **Per-chair pan** (StereoPannerNode)
- Master delay + simple reverb send
- Live MIDI keyboard play (note-on/off/velocity) for selected chair; on-screen keys as fallback
- Stronger pad sustain with sample looping where buffer allows
- Editable 16-step grids, recipes including new "break"
- Live BPM (60–180), key, 4-chord progression — persist in localStorage
- Per-chair mute, volume, lowpass filter
- Swing + deterministic humanize (downbeats exact, seeded)
- Seamless OfflineAudioContext 48 kHz 24-bit stereo WAV with folded ring-out tail (plus with-tail option)
- Matching multi-track MIDI (one track per chair + tempo)
- Stem export path (per-chair)
- First transient at sample 0, exact length Math.round(bars * 4 * 60 / bpm * sampleRate)
- Playhead and current-step highlight from AudioContext.currentTime

## Audio Engine compliance (sections 1–4 non-negotiable)
1. Timing on AudioContext clock only. LOOKAHEAD_MS=25, SCHEDULE_AHEAD=0.12. Playhead from currentTime in rAF.
2. Live mutable state object. Scheduler reads current values every step. Changes within one bar. Stop calls .stop() on tracked sources.
3. WAV via OfflineAudioContext, first downbeat at sample 0, exact length, folded tail, 48 kHz 24-bit, peak ≤ −1 dBFS. Matching MIDI. Reuses loaded ArrayBuffers. No Math.random in export path.
4. User BPM + key + 4-chord entry first. Persist and used in both playback and export.

## Acceptance checks (section 9)
1. Export 92 BPM / 8 bars / 48 kHz → exactly 1,001,739 samples, first transient ≤ 1 ms of sample 0.
2. Import at bar 1 of 92 BPM DAW and loop 4×: no seam clicks, no grid drift.
3. Play, switch tab 30 s, return: stopped cleanly, no stacked notes.
4. Change recipe / mute / BPM / key / chords / velocity while playing: heard within one bar, no restart.
5. Block sample host and reload: UI names the missing instruments.
6. Hold longest pad at slowest tempo: sustains to end of note (looped where possible).
7. Grep: no @main or @gh-pages, no Math.random in export path, voice names match samples.

Open `index.html` in a modern browser (or serve statically). Tap to start audio.

Repo: https://github.com/workinwithai-create/ApexForge
