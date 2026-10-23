# BLIP Image Captioning with Gradio

A simple web app that generates English captions for images using **Salesforce BLIP** (`blip-image-captioning-base`) from Hugging Face Transformers. Upload an image in the browser and get a caption in seconds. The app uses the GPU automatically when one is available.

## Features

- Image captioning with the pre-trained BLIP base model (no training required)
- Simple Gradio web interface: upload an image, read the caption
- Automatic GPU (CUDA) detection with CPU fallback
- Image preprocessing: conversion to RGB and resize to 384×384, the resolution BLIP works with
- Captions of up to 50 tokens

## Tech Stack

| Component | Purpose |
|---|---|
| Python | Language |
| PyTorch | Model execution on CPU or GPU |
| Hugging Face Transformers | `BlipProcessor` and `BlipForConditionalGeneration` |
| Gradio | Web interface |
| Pillow | Image loading and resizing |

## Project Structure

```
.
├── app.py              # Gradio app and caption generation
├── requirements.txt    # Python dependencies
└── README.md
```

## How It Works

1. On startup, the script detects whether a CUDA GPU is available and loads the BLIP processor and model (`Salesforce/blip-image-captioning-base`).
2. When you upload an image, it is converted to RGB and resized to 384×384.
3. `BlipProcessor` turns the image into model inputs.
4. The model generates a caption (up to 50 tokens), which is decoded to text and shown in the interface.

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```

### 2. Create a virtual environment and install dependencies

```bash
python -m venv .venv

# Windows
.venv\Scripts\activate
# Linux / macOS
source .venv/bin/activate

pip install -r requirements.txt
```

> **GPU note:** On Windows, `pip install torch` installs the CPU-only build by default. For NVIDIA GPUs, install the matching PyTorch build from <https://pytorch.org/get-started/locally>. The app works on CPU too, only slower.

### 3. Run

```bash
python app.py
```

The first run downloads the BLIP model from Hugging Face (a large download, roughly 1 GB), so it can take a few minutes. Later runs use the cached copy.

Then open the local address printed in the console (by default <http://127.0.0.1:7860>).

## Usage

1. Open the app in your browser.
2. Upload an image (drag and drop or click the upload area).
3. The generated English caption appears in the **Caption** box.

## Configuration

You can adjust behavior in `app.py`:

| Setting | Where | Effect |
|---|---|---|
| `max_length=50` | `caption_model.generate(...)` | Maximum caption length in tokens |
| `image.resize((384, 384))` | `generate_caption` | Input image size |
| `iface.launch(share=True)` | bottom of the file | Creates a temporary public link so others can try the app |

## Limitations

- Captions are generated in **English** only.
- Output quality depends on the BLIP base model and may be generic or occasionally inaccurate for unusual images.
- The 384×384 resize ignores the original aspect ratio.

## Possible Improvements

- Add a larger BLIP model (`blip-image-captioning-large`) as an option
- Add translation of captions into other languages (for example Persian)
- Let users control generation settings (beam count, caption length)
- Support conditional captioning with a text prompt
- Deploy the app on Hugging Face Spaces

## Acknowledgements

- [Salesforce BLIP](https://huggingface.co/Salesforce/blip-image-captioning-base) model
- Hugging Face Transformers, PyTorch, and Gradio

## License

Add a license of your choice (for example MIT) before publishing.
