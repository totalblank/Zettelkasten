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
* *Images* $\rightarrow$ Rank 4 tensors of shape (`samples, height, width, channels`), where each 2D grid of pixels, and each pixel is represented by a vector of values ("channels").
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

Training a neural network revolves around the following concepts,

1. Low level tensor manipulation $\rightarrow$ the infrastructure that underlines all modern machine learning. This translates to TensorFlow APIs:
	* *Tensors*, including special tensors that store the network's state (variables)
	* *Tensor operations* such as addition, `relu, matmul`
	* *Backpropagation*, a way to compute the gradient of mathematical expressions (handled in TensorFlow via the `GradientTape` object)
2. High level deep learning concepts. This translates to Keras APIs:
	* *Layers*, which are combined into a *model*.
	* A *loss function*, which defines the feedback signal used for learning.
	* An *optimizer*, which determines how learning proceeds.
	* *Metrics* to evaluate model performance, such as accuracy.
	* A *training loop* that performs mini-batch stochastic gradient descent

NumPy arrays are assignable, but TensorFlow tensors are not assignable. That is why the following gives an error,

```Python
x = tf.ones(shape=(2, 2))
x[0, 0] = 0.
```

`tf.Variable` is the class meant to manage modifiable state in TensorFlow.

### Keras hyperparameters

* **Units:** defines the size of the dense layer output. It is always a positive integer.
* **Activation:** transforms neuron input values. By introducing non-linearity, neural networks are able to understand the relationship between input and output values.
* **The kernel weight matrix:** used to multiply input data and extract its main characteristics. Here, several parameters can be used to initialize, regularize, or constrain the matrix.
* **The bias vector:** these are the additional data sets that require no input and corresponds to the output layer. 

In a dense neural network, the results of the previous layers are transmitted to the dense layer. There is thus hyper-connection between the different layers making up the architecture of the learning model. 

To learn from data, you have to make assumptions about it. These assumptions define what can be learned. As such, the structure of your hypothesis space $\rightarrow$ the architecture of your model $\rightarrow$ is extremely important. It encodes the assumptions you make about your problem, the prior knowledge that the model starts with. For instance, if you're working on a two-class classification problem with a model made of a single `Dense` layer with no activation (a pure affine transformation), you are assuming that your two classes are linearly separable.

Once the model architecture is defined, you still have to choose three more things:

* Loss function (objective function) $\rightarrow$ The quantity that will be minimized during training. It represents a measure of success for the task at hand.
* Optimizer $\rightarrow$ Determines how the network will be updated based on the loss function. It implements a specific variant of Stochastic Gradient Descent (SGD).
* Metrics $\rightarrow$ The measures of success you want to monitor during training and validation, such as classification accuracy. Unlike the loss, training will not optimize directly for these metrics. As such, metrics don't need to be differentiable.

```Python
# Define a linear classifier
model = keras.Sequential([keras.layers.Dense(1)])

model.compile(
	optimizer="rmsprop", # becomes keras.optimizers.RMSprop()
	loss="mean_squared_error",
	metrics=["accuracy"]
)
```

### Picking a loss function

Choosing the right loss function for the right problem is extremely important: your network will take any shortcut it can to minimize the loss, so if the objective doesn't fully correlate with success for the task at hand, your network will end up doing  things you may not have wanted.

### Calling `fit()` with NumPy data

```Python
history = model.fit(
	inputs,            # the input examples, as a NumPy array
	targets,           # the corresponding training targets
	epochs=5,          # the training loop will iterate over the data 5 times
	batch_size=128     # the training loop will iterate over the data in batches of 128 examples
)
```
The call to `fit()` returns a `History` object. 

The goal of machine learning is to obtain models that perform well in general, and particularly on data points that the model has never encountered before. Reserve a subset of the training data as *validation data*: you won't be training the model on this data, but you will use it to compute a loss value and metrics value. You do this by using the `validation_data` argument in `fit()`. The value of the loss on the validation data is called the "validation loss" to distinguish it from the "training loss". It's essential to keep the training data and validation data strictly separate: the purpose of validation is to monitor whether what the model is learning is actually useful on new data.

### Inference: Using a model after training

Use the `model.predict()` method. It will iterate over the data in small batches and return a NumPy array of predictions.

## Chapter 4: Classification and regression

### Glossary

* *Sample* or *input* $\rightarrow$ One data point that goes into your model
* *Prediction* or *output* $\rightarrow$ What comes out of your model
* *Target* $\rightarrow$ The truth. What your model should ideally have predicted, according to an external source of data.
* *Prediction error* or *loss value* $\rightarrow$ A measure of the distance between your model's prediction and the target.
* *Classes* $\rightarrow$ A set of possible labels to choose from in a classification problem. For example, when classifying cat and dog pictures, "dog" and "cat" are the two classes.
* *Label* $\rightarrow$ A specific instance of a class annotation in a classification problem. For instance, if picture #1234 is annotated as containing the class "dog", then "dog" is a label of picture #1234.
* *Ground-truth* or *annotations* $\rightarrow$ All targets for a dataset, typically collected by humans.
* *Binary classification* $\rightarrow$ A classification task where each input sample should be categorized into two exclusive categories.
* *Multiclass classification*
* *Multilabel classification*
* *Scalar regression*
* *Vector regression*
* *Mini-batch or batch*

### Summary of IMDB Analysis

* You usually need to do quite a bit of preprocessing on your raw data to be able to feed it as tensors into a neural network.
* Stacks of `Dense` layers with `relu` activation can solve a wide range of problems (including sentiment classification), and you'll likely use them frequently.
* In a binary classification problem (two output classes), your model should end with a `Dense` layer with one unit and a `sigmoid` activation: the output of your model should be a scalar between 0 and 1, encoding a probability.
* With such a scalar sigmoid output on a binary classification problem, the loss function you should use is `binary_crossentropy`.
* The `rmsprop` optimizer is generally a good enough choice, whatever your problem.
* As they get better on their training data, neural networks eventually start over-fitting and end up obtaining increasingly worse results on data they've never seen before. Be sure to always monitor performance on data that is outside of the training set.

### Summary of Reuters analysis

* If you're trying to classify data points among $N$ classes, your model should end with a `Dense` layer of size $N$.
* In a single-label, multiclass classification problem, your model should end with a `softmax` activation so that it will output a probability distribution over the $N$ output classes.
* Categorical cross-entropy is almost always the loss function you should use for such problems. It minimizes the distance between the probability distributions output by the model and the true distribution of the targets.
* There are two ways to handle labels in multiclass classification:
	* Encoding the labels via categorical encoding (also known as one-hot encoding) and using `categorical_crossentropy` as a loss function.
	* Encoding the labels as integers and using the `sparse_categorical_crossentropy` loss function.
* If you need to classify data into a large number of categories, you should avoid creating information bottlenecks in your model due to intermediate layers that are too small.

### Summary of Boston housing analysis

* Regression is done using different loss functions than we used for classification. Mean Squared Error (MSE) is a loss function commonly used for regression.
* Similarly, evaluation metrics to be used for regression differ from those used for classification; naturally, the concept of accuracy doesn't apply for regression. A common regression metric is mean absolute error (MAE).
* When features in the input data have values in different ranges, each feature should be scaled independently as a preprocessing step. The quantities used for normalizing the test data are computed using the training data. You should never use any quantity computed on the test data in your workflow, even for something as simple as data normalization.
* When there is little data available, using K-fold validation is a great way to reliably evaluate a model.
* When little training data is available, it's preferable to use a small model with few intermediate layers (typically only one or two), in order to avoid severe overfitting.