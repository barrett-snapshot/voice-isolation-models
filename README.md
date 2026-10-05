# voice-isolation-models

ONNX exports of the [Sidon](https://huggingface.co/sarulab-speech/sidon-v0.1) speech-restoration
model, for offline voice isolation and clean-up. No Python is needed to run them; any ONNX Runtime
works.

| File | What it is | Size | SHA-256 |
|---|---|---|---|
| `sidon_fe.onnx` | Feature extractor: the w2v-BERT 2.0 speech encoder (8 layers) with Sidon's LoRA merged. Input `input_features` (1, frames, 160) → `last_hidden_state` (1, frames, 1024). | 795,539,651 bytes | `df510e203dc644a68b85399e580e2e5a7c4069af7a235dc670dd617aab5a5f1a` |
| `sidon_decoder.onnx` | Vocoder: `hidden` (1, 1024, frames) → `wav` (1, 1, samples) at 48 kHz. | 209,854,938 bytes | `8daa3f39d52d27b6fc4fd99258595f7a445c1a1dd77862a800d11000635581e8` |

The files are attached to the [releases](../../releases). They were exported from the released
TorchScript with `torch.onnx.export` (opset 17, dynamic length) and checked against the originals
to 1.6 × 10⁻⁴ on the waveform. The features are the `SeamlessM4TFeatureExtractor` fbank of 16 kHz
audio, as in Sidon's own demo.

## Licences

Both upstream works are MIT-licensed; their notices are in [NOTICE](NOTICE). Changes made here:
the TorchScript graphs were converted to ONNX, and in the feature extractor Sidon's LoRA weights
are merged into the encoder. Nothing was retrained.
