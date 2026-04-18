<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
</head>
<body>
<div class="container">

<h1>🚀 Real-time Crowd Density Estimation with CSRNet</h1>

<p>
This project implements a deep learning pipeline for <b>crowd counting and density estimation</b> using the CSRNet architecture on the ShanghaiTech dataset.
</p>

<div>
  <span class="badge">PyTorch</span>
  <span class="badge">Computer Vision</span>
  <span class="badge">Deep Learning</span>
  <span class="badge">Crowd Counting</span>
</div>

<hr>

<h2>📌 Overview</h2>

<div class="card">
<p>
The goal of this project is to estimate the number of people in crowded scenes using density maps instead of direct detection. This approach is highly effective in dense scenarios where traditional object detection fails.
</p>

<p>
The model predicts a <b>density map</b>, and the total crowd count is obtained by summing all pixel values.
</p>
</div>

<hr>

<h2>🧠 Model Architecture (CSRNet)</h2>

<div class="card">
<p>
CSRNet consists of two main parts:
</p>

<ul>
<li><b>Frontend:</b> Extracts features using convolutional layers (similar to VGG-style blocks)</li>
<li><b>Backend:</b> Uses <b>dilated convolutions</b> to expand receptive fields without reducing resolution</li>
</ul>

<pre>
Input Image → Conv Layers → MaxPool → Conv Layers → Dilated Conv → Density Map
</pre>

<p>
Output is a single-channel density map.
</p>
</div>

<hr>

<h2>📂 Dataset</h2>

<div class="card">
<p>
Dataset used:
</p>

<ul>
<li>ShanghaiTech Part B</li>
<li>~400 training images</li>
<li>~316 test images</li>
</ul>

<p>
Each image comes with annotation points representing head locations.
</p>
</div>

<hr>

<h2>⚙️ Key Techniques</h2>

<div class="card">
<ul>
<li>✅ Gaussian kernel for density map generation</li>
<li>✅ Ground-truth resizing with count preservation</li>
<li>✅ Mixed Precision Training (AMP)</li>
<li>✅ Adam optimizer</li>
<li>✅ Memory optimization (resize to 512x512)</li>
</ul>
</div>

<hr>

<h2>🔥 Training Details</h2>

<div class="card">
<ul>
<li>Epochs: <b>100</b></li>
<li>Batch size: <b>2</b></li>
<li>Learning rate: <b>1e-5</b></li>
<li>Loss: <code>MSELoss</code></li>
</ul>
</div>

<hr>

<h2>📊 Results</h2>

<div class="result">
<pre>
MAE      : 20.72
RMSE     : 29.46
Accuracy : 77.00%
</pre>
</div>

<p>
Model shows stable convergence and reasonable performance for Part B dataset.
</p>

<hr>

<h2>🖼️ Inference Pipeline</h2>

<div class="card">
<ul>
<li>Load image</li>
<li>Resize (if too large)</li>
<li>Normalize</li>
<li>Predict density map</li>
<li>Sum values → Crowd count</li>
<li>Apply calibration factor</li>
</ul>
</div>

<pre>
count = sum(density_map) * CALIB_FACTOR
</pre>

<hr>

<h2>⚠️ Important Fix</h2>

<div class="warning">
<p>
When resizing ground-truth density maps to match model output size, you MUST preserve total count:
</p>

<pre>
scale = (H_original * W_original) / (H_new * W_new)
gt_resized = gt_resized * scale
</pre>

<p>
Without this step → model learns wrong crowd counts ❌
</p>
</div>

<hr>

<h2>🎯 Demo Output</h2>

<div class="card">
<ul>
<li>Input image</li>
<li>Predicted density heatmap</li>
<li>Estimated crowd count</li>
</ul>

<p>
Visualization uses <code>matplotlib</code> with heatmap overlay.
</p>
</div>

<hr>

<h2>🚀 Future Improvements</h2>

<div class="card">
<ul>
<li>Use pretrained VGG16 frontend</li>
<li>Train on Part A (dense crowds)</li>
<li>Apply data augmentation</li>
<li>Use MAE-based loss combination</li>
<li>Deploy real-time inference (video stream)</li>
</ul>
</div>

<h2>👨‍💻 Author - Phat - An Enthusiast in Computer Science</h2>

<div class="card">
<p>
Built with ❤️ using PyTorch & Google Colab.
</p>
</div>

</div>
</body>
</html>
