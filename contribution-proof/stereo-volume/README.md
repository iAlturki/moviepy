# MoviePy stereo-volume proof

Prepared by **iAlturki** with OpenAI Codex assistance.

This demo uses only generated 440 Hz (left) and 880 Hz (right) sine waves at
0.12 peak amplitude with short fades. It contains no personal media, recordings,
account data, credentials, desktop captures, external scripts or trackers.

## See, hear and try

- [Animated comparison](before-after.gif)
- [Four-second video with audio](before-after.mp4): before first, then after
- [Self-contained interactive demo](interactive-demo.html): download and open in
  a browser; switch between one-sample and 256-sample batches and play either result
- [Before WAV](before-1.wav), [after WAV](after-1.wav), [original stereo reference](reference.wav)
- [Machine-readable measurements](results.json)
- [Download the complete proof and reproduction scripts](proof.zip)

The requested effect is `MultiplyStereoVolume(left=0, right=1)`. With one-sample
vectorized calls, the original effect mutes both channels. The fix keeps the
880 Hz right channel. The 256-sample control works in both versions.

Every audio sample comes from the actual MoviePy effect. The waveform graphic
visualizes those outputs; it is not a recording of a music application.
The WAV files contain 25,600 stereo samples at 16 kHz (1.6 seconds).
The MP4 preserves all 120 generated video frames at 30 fps and includes the real
before/after audio. Its only container tags are standard FFmpeg format metadata.

## Reproduce

Use a virtual environment, install the fixed MoviePy checkout with `pip install -e .`,
download and unpack `proof.zip`, then run `python reproduce.py` in the unpacked
folder. The script loads the original effect from `baseline_effect.py`, copied
unchanged from upstream commit `211e4b15f6ce4f34a6a9efbfff40590e43a68f77`, and compares
it with the installed effect. It writes a fresh WAV pair and prints measurements.

`build_demo.py` additionally regenerates the labelled images, video and interactive
page in a `share/` subdirectory. It needs Pillow and FFmpeg.

## Validation

- Focused stereo-volume checks: original effect **15 failed / 10 passed**;
  fixed effect **25 passed / 0 failed**, using the corrected mono fixture.
- Full suite on Windows / Python 3.12: **622 passed, 7 skipped, 1 xpassed,
  3 failed**. The three video-rotation tests fail with the original effect too:
  `test_PR_529`, `test_ffmpeg_parse_video_rotation`, `test_correct_video_rotation`.
- The project's Black, isort and flake8 pre-commit checks pass.

The proposed upstream change consists of the effect and regression tests.
The demo assets live separately in this fork's proof branch.
MoviePy and the copied baseline effect retain their MIT license and original
authors; see the repository's `LICENCE.txt`.
