## Model Training
### 1. Dataset Collection:
We first collect 9-dimensional raw data:  
$$
\mathbf{d} = \begin{bmatrix}
a_{x} & a_{y} & a_{z} & g_{yro_x} & g_{yro_y} & g_{yro_z} & m_{2-1} & m_{3-1} & m_{4-1}
\end{bmatrix}^{\top}
$$  
<!-- 这里加上的FC-FE and CCS-FF ?-->
Using the FC-FE and CCS-FF algorithms, this raw data is transformed into 3-dimensional synthetic input data for the neural network:  
$$
\mathbf{c} = \begin{bmatrix}
a_{s} & \omega_{s} & m_{s}
\end{bmatrix}^{\top}
$$  
The final CSV input data for `dataset.ipynb` is structured as follows:  
$$[a_{s}, \omega_{s}, m_{s}, timestamp, label]$$  
Here, the label (0/1) represents edge detection. The `dataset.ipynb` script is used to extract sliding windows from the data with specified input and step sizes. It then normalizes the data using the mean and standard deviation, and forms feature matrices from the three columns. Our `.npy` files for five cases are available in the folder `xxxx`.

### 2. Model Training:
You can use our `cnn_edge_detection.ipynb` to train your own model. This script includes the main steps of training, pruning, and quantization. 

# 项目名称

这个项目包含了一些关于数学公式的示例。

## 公式示例

### 行内公式

这是一个行内公式：$E=mc^2$

### 行间公式

这是一个行间公式：

$$
\int_{a}^{b} f(x) \, dx = F(b) - F(a)
$$

## 公式说明

- $E$：能量
- $m$：质量
- $c$：光速
- $f(x)$：函数
- $F(x)$：函数的不定积分

## 如何显示公式

要正确显示这些数学公式，请确保 MathJax 能够正确加载。您可以按照以下方式在您的 README.md 文件中包含 MathJax：

```html
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
