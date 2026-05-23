---
layout: post
title: "Notes on Sparse Autoencoders"
date: 2026-03-02
category: "Interpretability"
excerpt: "A deep dive into sparse autoencoders as a tool for mechanistic interpretability — how they decompose neural network activations into interpretable features and what we learn from the decomposition."
---

Mechanistic interpretability aims to reverse-engineer the computations performed by neural networks. Rather than treating models as black boxes that produce outputs from inputs, we want to understand the internal algorithms — the features, circuits, and computations — that the model has learned. Sparse autoencoders have emerged as one of the most promising tools in this endeavor.

## The Superposition Hypothesis

Neural networks represent more features than they have dimensions. This is the superposition hypothesis: a model with a d-dimensional activation space can represent far more than d features by encoding them in overlapping, almost-orthogonal directions. The model exploits the fact that most features are sparse — only a small fraction are active at any given time — to pack features into a compressed representation.

This is good for the model's capacity. It is terrible for interpretability. If features are superimposed, looking at individual neurons cannot tell you what the model is computing. Each neuron participates in representing many features simultaneously.

> Superposition is not a bug. It is an efficient compression strategy that the model discovers during training because it allows more features to be represented in the same parameter budget. Our task as interpretability researchers is to undo this compression — to recover the underlying features from the superimposed representation.

## Sparse Autoencoders as a Decompiler

A sparse autoencoder is a simple two-layer network trained to reconstruct a model's activations while being forced to use a sparse hidden representation. The architecture is straightforward:

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class SparseAutoencoder(nn.Module):
    def __init__(self, d_input, d_hidden):
        super().__init__()
        self.encoder = nn.Linear(d_input, d_hidden, bias=True)
        self.decoder = nn.Linear(d_hidden, d_input, bias=True)
        self.bias_dec = nn.Parameter(torch.zeros(d_input))

    def forward(self, x):
        h_pre = self.encoder(x - self.bias_dec)
        h = F.relu(h_pre)
        x_hat = self.decoder(h) + self.bias_dec
        return x_hat, h

    def loss(self, x):
        x_hat, h = self.forward(x)
        recon_loss = F.mse_loss(x_hat, x)
        sparsity_loss = self.l1_coefficient * h.abs().sum(dim=-1).mean()
        return recon_loss + sparsity_loss
```

The encoder projects the d_input-dimensional activation vector into a d_hidden-dimensional space that is much larger — typically 4x to 16x the input dimension. The decoder projects back, attempting to reconstruct the original activation. By applying an L1 penalty on the hidden activations, we force the model to use as few hidden units as possible for each reconstruction.

### Why This Works

If features are superimposed in the model's activation space, the autoencoder must learn to separate them. Each hidden unit — by virtue of the sparsity penalty — should ideally correspond to a single interpretable feature. When that feature is present in the input, the corresponding hidden unit fires; when it is absent, the unit stays at zero.

The decoder weights for each hidden unit form a direction in the input space. When a feature is "on," the autoencoder adds that direction to the reconstruction. This gives us something invaluable: a dictionary that maps hidden units (which we can inspect and interpret) to the directions they encode in the model's activation space.

## Measuring Interpretability

How do we know if the features we have extracted are actually interpretable? The standard approach is to look at the inputs that maximally activate each hidden unit — a technique borrowed from feature visualization in computer vision. For language models, this means finding text sequences that cause a particular feature to fire strongly.

A feature that fires on mathematical expressions, regardless of language or specific numbers, is likely a genuine "mathematical content" feature. A feature that fires on a seemingly random collection of tokens is either poorly decomposed or represents a concept we have not yet identified.

### The Feature Density-Sparsity Tradeoff

Setting the L1 coefficient is an art. Too much sparsity pressure, and the features become too coarse — one feature might cover multiple distinct concepts. Too little, and you get feature splitting — the same concept distributed across multiple hidden units.

## Open Questions

Sparse autoencoders raise as many questions as they answer. Are the features they find the ones the model actually uses, or artifacts of the decomposition method? Can we verify that intervening on a sparse autoencoder feature produces the expected change in model behavior? How do features compose — do the decoder directions of interacting features simply add, or is there a more complex geometry?

These are empirical questions, and the field is actively working on them. What is clear is that sparse autoencoders give us something we did not have before: a systematic way to crack open the superimposed representations inside neural networks and examine their contents.
