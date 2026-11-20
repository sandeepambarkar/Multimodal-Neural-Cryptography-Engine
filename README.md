# Multimodal Neural Cryptography Engine

A PyTorch deep learning project that encrypts and decrypts RGB images and secret text messages together using a secret key, with a live Gradio web interface.

Instead of using traditional math-based ciphers, this project trains a **CNN + GRU + Transformer** neural network to scramble and reconstruct data on its own.

## Demo Preview

<!-- Replace with a screenshot of your Gradio app -->
![Gradio Demo Screenshot](assets/demo_screenshot.png)

*Left: Original Image and Text | Middle: Scrambled Cipher | Right: Decrypted Output (Correct Key vs. Wrong Key)*

## How It Works

The model uses an Encoder-Decoder setup (Alice and Bob) conditioned on a 16-number secret key:

1. **CNN (Images):** Turns `64x64` RGB images into `8x8` feature patches during encryption, and rebuilds the image during decryption.
2. **GRU (Text):** Reads ASCII characters one by one using the secret key as its starting memory, so changing a single letter or key changes the entire output.
3. **Transformer (Scrambler):** Mixes the image patches, text tokens, and key together using self-attention to produce a scrambled cipher-tensor.

## Loss Function

The model is trained to balance three goals at once:
- **Image Loss (L1 + MSE):** Keeps recovered images sharp and accurate.
- **Text Loss (Cross-Entropy):** Recovers exact ASCII characters.
- **Wrong-Key Penalty:** Forces the decoder to output noise whenever an incorrect key is used.

## V1 vs. V2 Improvements

| Version | Image Size | Latent Space | Loss Function | Result |
| :--- | :--- | :--- | :--- | :--- |
| **V1** | `32x32` | Compressed (3x lossy) | MSE only | Blurry recovered images |
| **V2** | `64x64` | 1:1 capacity + Positional Embeddings | L1 + MSE + CE + Security | Sharp images (`L1 < 0.04`) and 98%+ text accuracy |

## Project Structure

```text
multimodal-neural-cryptography/
├── notebooks/
│   └── train_multimodal_crypto_v2.ipynb   # Google Colab training notebook
├── app.py                                 # Gradio web interface
├── multimodal_neural_crypto_v2.pth        # Pre-trained model weights
├── requirements.txt                       # Dependencies
└── README.md
