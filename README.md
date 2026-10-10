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
  an ultrasonic profile (~16–19 kHz). Four speed presets trade reliability
  for throughput (~5.5 to ~16.7 B/s raw).
- **Receiver** — FFT-based demodulator. The preamble lock correlates the
  two marker tones with the preamble's alternating rhythm, and the sync
  test compares each tone with its own level, so room echo and an uneven
  speaker/mic response do not break detection. Early/late gate clock
  tracking, echo subtraction before each tone decision, and a header
  search over several symbol grids (RS + CRC-16 pick the right one). The
  FEC setting travels in the header, so only the sender needs to choose it.

## Inspector

- The INSPECTOR panel shows the last frame sent and the last frame
  received: link settings, tone frequencies, header fields, Reed-Solomon
  corrections, symbol confidence, CRC check and a payload hex dump. A
  failed frame shows the stage where it failed, with a hint.
