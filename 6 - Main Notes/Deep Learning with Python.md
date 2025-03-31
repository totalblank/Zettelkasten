# Chapter 2

## Tensors

A *Tensor* with *rank* $n$ can be thought of being an $n$ dimensional space. Suppose there is a point in 4D space, we will need a tuple of 4 numbers to determine its co-ordinate. In a similar manner, a rank 4 tensor can store numbers in 4 axes.

A tensor can be of any rank.
* Scalars (rank 0 tensors)
* Vectors (rank 1 tensors)
* Matrices (rank 2 tensors)

Real world examples of data tensors

* *Vector data* $\rightarrow$ Rank 2 tensors of shape `(samples, features)`, where each sample is a vector of numerical attributes ("features").
* *Timeseries data or sequence data* $\rightarrow$ Rank 3 tensors of shape `(samples, timesteps, features)`, where each sample is a sequence (of length `timesteps`) of feature vectors. Time axis is always the second axis (`axis=1`).
* *Images* $\rightarrow$ Rank 4 tensors of shape (`samples, height, width, channels`), where each 2D gride of pixels, and each pixel is represented by a vector of values ("channels").
* *Video* $\rightarrow$ Rank 5 tensors of shape `(samples, frames, height, width, channels)`, where each sample is a sequence (of length `frames`) of images.

## Tensor operations

* *Element-wise operations*: Such as `relu` operation, addition, subtraction.
* *Broadcasting*: When possible, and if there's no ambiguity, the smaller tensor will be *broadcast* to match the shape of the larger tensor. Broadcasting consists of two steps:
	* Axes (called *broadcast* axes) are added to the smaller tensor to match the `ndim` of the larger tensor.
	* The smaller tensor is repeated alongside these new axes to match the full shape of the larger tensor.
* *Tensor product*: Also known as *dot product*.
* *Tensor reshaping*: Re-arranging its rows and columns to match a target shape. The reshaped tensor has the same total number of coefficients as the initial tensor.

## Geometric interpretation of tensor operations

Elementary geometric operations are translation, rotation, scaling, skewing.

* **Translation**: Adding a vector to a point will move the point by a fixed amount in a fixed direction
* **Rotation**: A CCW rotation of a 2D vector by an angle $\theta$ can be achieved via a dot product with a $2\times2$ matrix, called the *Rotation matrix*.
* **Scaling**: A vertical and horizontal scaling of the image can be achieved via a dot product with a $2\times2$ matrix. This matrix is a diagonal matrix.

Two kinds of transformation,

* *Linear transformation*: A dot product with an arbitrary matrix implements a linear transformation.
* *Affine transformation*: An affine transformation is the combination of a linear transformation (achieved via a dot product with some matrix) and a translation (achieved via a vector addition). Dense layer computation without the activation function ( $y = W \cdot x + b$ ). So, a dense layer without an activation function is an affine layer.
	* If many affine transformation are applied repeatedly, you still end up with an affine transformation, which would be equivalent to a single `Dense` layer. For example, $W_2 \cdot (W_1 \cdot x + b_1) + b_2 = (W_2 \cdot W_1) \cdot x + (W_2 \cdot b_1 + b_2)$
* *Dense layer with `relu` activation*: By incorporating activation function in each layer, we can implement very complex, non-linear geometric transformations, resulting in very rich hypothesis space for the deep neural networks. Here $W$ is called *Kernel*, and $b$ is called *bias*.

## Training loop

1. Draw a batch of training samples,$x$, and corresponding targets, `y_true`.
2. run the model on $x$ (a step called the *forward pass*) to obtain predictions, `y_pred`.
3. Compute the loss of the model on the batch, a measure of the mismatch between `y_pred` and `y_true`.
4. Update all weights of the model in a way that slightly reduces the loss on this batch.

The fourth step is achieved by incorporating *gradient descent*.

**Stochastic Gradient Descent:**

This is a stochastic process because each batch of data is drawn at random.

1. Draw a batch of training samples, $x$, and corresponding targets, $y_{true}$.
2. Run the model on $x$ to obtain predictions, $y_{pred}$. This is called the *forward pass*.
3. Compute the loss of the model on the batch, a measure of the mismatch between $y_{pred}$ and $y_{true}$.
4. Compute the gradient of the loss with regard to the model's parameters. This is called the *backward pass*.
5. Move the parameters a little in the opposite direction from the gradient. For example, `W -= learning_rate * gradient`, thus reducing the loss on the batch a bit. The `learning_rate` would be a scalar factor modulating the "speed" of the gradient descent process.

### Automatic differentiation with computation graphs

A computation graph is a directed acyclic graph of operations (in this case, tensor operations). Computation graphs have been an extremely successful abstraction in computer science because they enable us to treat computation as data: a computable expression is encoded as a machine-readable data structure that can be used as the input or output of another program. For instance, you could imagine a program that receives a computation graph and returns a new computation graph that implements a large-scale distributed version of the same computation $\rightarrow$ this would mean that you could distribute any computation without having to write the distribution logic yourself.

## Chapter 3

TensorFlow is a Python-based, free, open source machine learning platform, developed primarily by Google. Much like NumPy, the primary purpose of TensorFlow is to enable engineers and researchers to manipulate mathematical expressions over numerical tensors. But TensorFlow goes far beyond the scope of NumPy in the following ways,

* It can automatically compute the gradient of any differentiable expression, making it highly suitable for machine learning.
* It can run not only on CPUs, but also GPUs and TPUs, highly parallel hardware accelerators.
* Computation defined in TensorFlow can be easily distributed across many machines.
* TensorFlow programs can be exported to other runtimes, such as C++, JavaScript, or TensorFlow Lite etc. This makes TensorFlow applications easy to deploy in practical settings.

TF-Agents $\rightarrow$ for reinforcement learning.
TFX $\rightarrow$ for industry-strength machine learning workflow management.

Keras is a deep learning API for Python, built on top of TensorFlow, that provides a convenient way to define and train any kind of deep leanring model.

![[Keras and Tensorflow.png]]

