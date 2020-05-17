---
layout: post
title:  "Kaggle study note: concatenate different types of layers in keras"
date:   2020-05-04 12:06:14 +0530
categories: Python keras
---
In Kaggle competition of COVID-19 case prediction, we sometimes need to combind different types of layers and branch the output. It is efficient to create a template for the next application.

Here is my study note and template from [this notebook](https://www.kaggle.com/frlemarchand/covid-19-forecasting-with-an-rnn) published by Francois Lemarchand.

```python 
#A input branch
A_input_layer = Input(shape=(13,5))
main_LSTM_layer = layers.LSTM(64, return_sequences=True, recurrent_dropout=0.2)(A_input_layer)

#B input branch
B_input_layer = Input(shape=(5,))
B_dense = layers.Dense(16)(B_input_layer)
B_dropout = layers.Dropout(0.2)(B_dense)

#cases output branch
LSTM_c = layers.LSTM(32)(main_LSTM_layer)
merge_c = layers.Concatenate(axis=-1)([LSTM_c,B_dropout])
dense_c = layers.Dense(128)(merge_c)
dropout_c = layers.Dropout(0.3)(dense_c)
cases = layers.Dense(1, activation=layers.LeakyReLU(alpha=0.1),name="cases")(dropout_c)

#fatality output branch
LSTM_f = layers.LSTM(32)(main_LSTM_layer)
merge_f = layers.Concatenate(axis=-1)([LSTM_f,B_dropout])
dense_f = layers.Dense(128)(merge_f)
dropout_f = layers.Dropout(0.3)(dense_f)
fatalities = layers.Dense(1, activation=layers.LeakyReLU(alpha=0.1), name="fatalities")(dropout_f)


model = Model([temporal_input_layer,demographic_input_layer], [cases,fatalities])

model.summary()
```

## Model visualization

```python
plot_model(model, show_shapes=True, show_layer_names=True)
```

## Setting training

```python

callbacks = [ReduceLROnPlateau(monitor='val_loss', patience=4, verbose=1, factor=0.6),
             EarlyStopping(monitor='val_loss', patience=20),
             ModelCheckpoint(filepath='best_model.h5', monitor='val_loss', save_best_only=True)]
model.compile(loss=[tf.keras.losses.MeanSquaredLogarithmicError(),tf.keras.losses.MeanSquaredLogarithmicError()], optimizer="adam")
```

## Training
```python
history = model.fit([X_temporal_train,X_demographic_train], [Y_cases_train, Y_fatalities_train], 
          epochs = 250, 
          batch_size = 16, 
          validation_data=([X_temporal_test,X_demographic_test],  [Y_cases_test, Y_fatalities_test]), 
          callbacks=callbacks)

```