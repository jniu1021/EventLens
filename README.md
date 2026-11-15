# EventLens

EventLens is a local visual narration research prototype. It explores whether event-triggered narration can preserve important information while reducing repeated speech and model calls compared with periodic inference. The current Windows prototype supports on-demand scene description and Mandarin speech.

## Prototype

EventLens describes a scene when the user asks. YOLO26n detects objects, Qwen3-VL 4B generates a short description through a local Ollama endpoint, and Piper synthesizes Mandarin speech. The manual pipeline links the displayed frame to its detection results, keeps at most two frame snapshots, and handles one narration request at a time. Users can cancel, pause, or mute. Timing logs omit images, prompts, descriptions, and audio.

**Validation.** On a Windows workstation with an RTX 5080, the pipeline completed 20 requests across two passes over 10 synthetic images. Ten requests used the audio output device and ten used a silent sink after real synthesis. The default test suite recorded 331 passing tests.

| Timing metric | Median | P95 |
| --- | ---: | ---: |
| First PCM submission, 10 audio-output requests | 4.48 s | 4.81 s |
| Request completion, including playback and cleanup | 7.27 s | 7.75 s |

First PCM submission measures the application's first successful audio write. Audible onset remains unmeasured. The timing sample excludes an initial cold-start diagnostic that timed out. Model integration was tested on synthetic scenes. Live camera operation and description factuality are pending evaluation.

**Next experiment.** Validate the manual workflow on consented indoor scenes. Then implement temporal tracking and event triggers, and compare against periodic inference using event recall, narration factuality, repeat narration rate, latency, and model calls per minute.
