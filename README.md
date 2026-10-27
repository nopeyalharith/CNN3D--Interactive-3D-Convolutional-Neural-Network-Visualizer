# CNN3D: Interactive 3D Convolutional Neural Network Visualizer

Draw a digit and watch a convolutional neural network process it, layer by layer, in 3D. The whole project is a single HTML file that runs in the browser. There is no backend, no build step and no install.

The network is implemented from scratch in plain JavaScript (forward pass, backpropagation and SGD with momentum). It trains itself in your browser when the page loads. The 3D view is rendered with [three.js](https://threejs.org/).

## Features

- **Live 3D view** of every layer: feature maps are drawn as image planes and fully connected units as cubes.
- **Draw to predict.** Sketch a digit on the canvas and all layers update as you draw.
- **Prediction panel** showing the first and second guess with probabilities.
- **Hover inspection.** Point at any pixel of a feature map or any unit to read its exact activation.
- **Weighted connections.** Lines between the later layers are coloured by their contribution to the result (orange for excitatory, blue for inhibitory).
- **Layer and connection toggles** to hide parts of the network and focus on one stage.
- **In-browser training** on synthetic digits, with a progress bar and held-out accuracy.
- **Teach it.** If a drawing is misread, click the correct digit and the network fine-tunes on it.
- **Bring your own weights.** Load a JSON file of weights (for example trained on MNIST) to replace the built-in training.

## Architecture

| Layer | Output shape | Notes |
|---|---|---|
| Input | 1 × 28 × 28 | Drawing is cropped, scaled to 20×20 and centred by centre of mass (MNIST-style) |
| Convolution 1 | 4 × 24 × 24 | 5×5 filters, ReLU |
| Downsampling 1 | 4 × 12 × 12 | 2×2 max pooling |
| Convolution 2 | 8 × 8 × 8 | 5×5 filters, ReLU |
| Downsampling 2 | 8 × 4 × 4 | 2×2 max pooling |
| Fully connected 1 | 32 | ReLU |
| Output | 10 | Softmax |

## Getting started

1. Download `index.html` (or clone this repository).
2. Open it in a modern browser. An internet connection is needed once to load three.js from a CDN.
3. Wait a few seconds for the initial training to finish, then draw a digit.

To host it, enable GitHub Pages on the repository and serve `index.html` from the root of the main branch.

## Controls

| Action | Input |
|---|---|
| Orbit the camera | Drag on the 3D view |
| Zoom | Mouse wheel |
| Pan | Shift + drag (or right-drag) |
| Inspect a value | Hover over a map or unit |
| Draw | Click and drag on the drawing canvas |

## Using your own weights

Load a `.json` file with the following keys. Array lengths must match exactly.

| Key | Length |
|---|---|
| `w1` | 100 (4 filters × 5 × 5) |
| `b1` | 4 |
| `w2` | 800 (8 × 4 × 5 × 5) |
| `b2` | 8 |
| `w3` | 4096 (32 × 128) |
| `b3` | 32 |
| `w4` | 320 (10 × 32) |
| `b4` | 10 |

## Limitations

- The built-in model is trained on digits rendered from system fonts, not on the MNIST dataset, so accuracy on real handwriting is lower. Use the "This drawing is a…" buttons or load MNIST-trained weights to improve it.
- Results depend on which fonts your system has, so accuracy can vary between machines.
- The network is deliberately small so that training and visualization both run smoothly in the browser.

## License

MIT
