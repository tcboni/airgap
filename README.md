# AIRGAP · MK-1

Send short text messages between nearby devices using sound — no network, no pairing, no install.

**[→ Open AirGap](https://tcboni.github.io/airgap/)**

## Usage

- **Sender** — type a message and press TRANSMIT. The message is encoded
  as audible (or ultrasonic) tones and played through the speaker.
- **Receiver** — switch to RECEIVE mode and press START LISTENING. With
  AUTO on (the default) the receiver detects the sender's band, channel
  and speed by itself.

## How it works

- **Modulation** — 16-tone MFSK, 4 bits per symbol, with an alternating
  two-tone preamble and a dual-tone sync marker for symbol-clock lock.
- **Framing** — a length header protected by Reed-Solomon and CRC-16,
  then the payload (up to 8 KB of UTF-8 text) chunked into RS blocks with
  selectable parity (FEC LOW/MED/HIGH) and a CRC-32 trailer.
- **Bands** — an audible profile (~1.5–5 kHz, three channels A/B/C) and
  an ultrasonic profile (~16–19 kHz). Four speed presets trade robustness
  for throughput (~5.5 to ~16.7 B/s raw).
- **Receiver** — FFT-based demodulator with early/late gate clock
  tracking, phase refinement at lock, and header decode retries; the FEC
  setting travels in the header, so only the sender needs to choose it.

## Diagnostics

- The DIAGNOSTICS panel runs an in-memory loopback (modulate → demodulate)
  plus the full suite: CRC vectors, RS error injection, FFT sanity, and
  round trips with noise and clock drift.
- Open the page with `?selftest` appended to the URL to run everything at
  boot.
- The DSP core is pure JavaScript with no browser dependencies — the
  `AIRGAP CORE` section of `index.html` can be extracted and run under
  Node for testing.
