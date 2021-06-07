---
layout: post
title:  "Customize keras layer"
date:   2021-06-07 00:00:42 +0530
categories: Python TensorFlow Keras
---

# Class

This note is to build a keras customized layer which could be used like a python function.

```python
y = SampleLayer(initial parameters)(x)
```
The keras layers can be defined as a class which inherits from tf.keras.layers.Layer. A layer encapsulates both a state (the layer's "weights") and a transformation from inputs to outputs (a "call", the layer's forward pass).

## State (weights)
```python
class SampleLayer(tf.keras.layers.Layer):
```
### ```__init___``` and ```super().__init__()```
It include a __init__ function followed by super().__init__() to call the initiation function of tf.keras.layers.Layer. Make sure the first variable in the super function is the name of the customized layer. Then start to code the initialize parameters and functions. The layers and functions can be initialized here.


```python
class SampleLayer(tf.keras.layers.Layer):
	def __init__(self, <load your initial parameters here>):
		super(SampleLayer, self).__init__()
		<Something you want to initialize here>
		self.layer1 = LayerA(**kwargs)
		self.layer2 = LayerB(**kwargs)
```

Note for non-trainable weights:

```
self.total = tf.Variable(initial_value=tf.zeros((input_dim,)), trainable=False)
```

## Forward pass
### call()
The layer's forward pass is defined in call function. It's recommended to use tf.Modules, like tf.multiply and tf.sigmoid in this part. Here is the example:

```python
def call(self, inputs):

        x = self.layer1(inputs)
        x1 = self.layer2(x)
        output = tf.multiply(x, tf.sigmoid(x1))

        return output

```
# Some templates 

## Sample 1
```python
class DownSample_2D(tf.keras.layers.Layer):

    def __init__(self, filter_num=1024, kernel_size= (3,3), stride=1):
        super(DownSample_2D, self).__init__()

        self.conv1 = tf.keras.layers.Conv2D(filters=filter_num,
                                        kernel_size=(3, 3),
                                        strides=stride,
                                        padding="same")
        self.bn1 = tf.keras.layers.LayerNormalization(epsilon = 1e-06)
        self.conv2 = tf.keras.layers.Conv2D(filters=filter_num,
                                        kernel_size=(3, 3),
                                        strides=stride,
                                        padding="same")
        self.bn2 = tf.keras.layers.LayerNormalization(epsilon = 1e-06)


    def call(self, inputs, training=None, **kwargs):

        x1 = self.conv1(inputs)
        x1 = self.bn1(x1)
        x2 = self.conv2(inputs)
        x2 = self.bn2(x2)
        output = tf.multiply(x1, tf.sigmoid(x2))

        return output
```

## Sample 2
```python
class pixel_shuffler(tf.keras.layers.Layer):

    def __init__(self, shuffle_size = 2, name = None):
        super(pixel_shuffler, self).__init__()
        self.shuffle_size = shuffle_size

    def call(self, inputs, training=None, **kwargs):
        n = tf.shape(inputs)[0]
        w = tf.shape(inputs)[1]
        c = inputs.get_shape().as_list()[2]
        oc = c // self.shuffle_size
        ow = w * self.shuffle_size

        outputs = tf.reshape(tensor = inputs, shape = [n, ow, oc])
        return outputs

```
# Reference
[https://www.tensorflow.org/guide/keras/custom_layers_and_models](https://www.tensorflow.org/guide/keras/custom_layers_and_models)