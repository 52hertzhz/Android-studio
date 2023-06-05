TensorFlow训练石头剪刀布数据集


导入依赖库
```python
import os
import zipfile

local_zip = 'F:/python_pros/jupyterProject/AndroidStudio/data/rps.zip'
zip_ref = zipfile.ZipFile(local_zip, 'r')
zip_ref.extractall('F:/python_pros/jupyterProject/AndroidStudio/data/')
zip_ref.close()

local_zip = 'F:/python_pros/jupyterProject/AndroidStudio/data/rps-test-set.zip'
zip_ref = zipfile.ZipFile(local_zip, 'r')
zip_ref.extractall('F:/python_pros/jupyterProject/AndroidStudio/data/')
zip_ref.close()

```

检测数据集的解压结果，打印相关信息

```python
rock_dir = os.path.join('F:/python_pros/jupyterProject/AndroidStudio/data/rps/rock')
paper_dir = os.path.join('F:/python_pros/jupyterProject/AndroidStudio/data/rps/paper')
scissors_dir = os.path.join('F:/python_pros/jupyterProject/AndroidStudio/data/rps/scissors')

print('total training rock images:', len(os.listdir(rock_dir)))
print('total training paper images:', len(os.listdir(paper_dir)))
print('total training scissors images:', len(os.listdir(scissors_dir)))

rock_files = os.listdir(rock_dir)
print(rock_files[:10])

paper_files = os.listdir(paper_dir)
print(paper_files[:10])

scissors_files = os.listdir(scissors_dir)
print(scissors_files[:10])

```

    total training rock images: 840
    total training paper images: 840
    total training scissors images: 840
    ['rock01-000.png', 'rock01-001.png', 'rock01-002.png', 'rock01-003.png', 'rock01-004.png', 'rock01-005.png', 'rock01-006.png', 'rock01-007.png', 'rock01-008.png', 'rock01-009.png']
    ['paper01-000.png', 'paper01-001.png', 'paper01-002.png', 'paper01-003.png', 'paper01-004.png', 'paper01-005.png', 'paper01-006.png', 'paper01-007.png', 'paper01-008.png', 'paper01-009.png']
    ['scissors01-000.png', 'scissors01-001.png', 'scissors01-002.png', 'scissors01-003.png', 'scissors01-004.png', 'scissors01-005.png', 'scissors01-006.png', 'scissors01-007.png', 'scissors01-008.png', 'scissors01-009.png']
    

各打印两张石头剪刀布训练集图片

```python
%matplotlib inline

import matplotlib.pyplot as plt
import matplotlib.image as mpimg

pic_index = 2

next_rock = [os.path.join(rock_dir, fname) 
                for fname in rock_files[pic_index-2:pic_index]]
next_paper = [os.path.join(paper_dir, fname) 
                for fname in paper_files[pic_index-2:pic_index]]
next_scissors = [os.path.join(scissors_dir, fname) 
                for fname in scissors_files[pic_index-2:pic_index]]

for i, img_path in enumerate(next_rock+next_paper+next_scissors):
  #print(img_path)
  img = mpimg.imread(img_path)
  plt.imshow(img)
  plt.axis('Off')
  plt.show()

```


    
![png](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%BA%94_2/image/output_2_0.png)
    



    
![png](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%BA%94_2/image/output_2_1.png)
    



    
![png](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%BA%94_2/image/output_2_2.png)
    



    
![png](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%BA%94_2/image/output_2_3.png)
    



    
![png](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%BA%94_2/image/output_2_4.png)
    



    
![png](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%BA%94_2/image/output_2_5.png)
    



```python
# 报错原因：
# Tensorflow1.x版本和2.x版本部分方法名不兼容。
# 一定是你安装使用的是Tensorflow2.0及以上版本，而在程序中使用了Tensorflow1.x版本的方法或者情况相反。
# 解决办法（一）：
# 1.如果你导入Tensorflow模块的代码为：

# import tensorflow 
# 1
# 替换为：

# import tensorflow.compat.v1
# 1
# 2.如果你导入Tensorflow模块的代码为：

# import tensorflow as tf
# 1
# 则替换为：

# import tensorflow.compat.v1 as tf

调用TensorFlow的keras进行数据模型的训练和评估。Keras是开源人工神经网络库，TensorFlow集成了keras的调用接口，可以方便的使用

import tensorflow as tf
import keras_preprocessing
from keras_preprocessing import image
from keras_preprocessing.image import ImageDataGenerator
import keras

TRAINING_DIR = "F:/python_pros/jupyterProject/AndroidStudio/data/rps/"
training_datagen = ImageDataGenerator(
      rescale = 1./255,
      rotation_range=40,
      width_shift_range=0.2,
      height_shift_range=0.2,
      shear_range=0.2,
      zoom_range=0.2,
      horizontal_flip=True,
      fill_mode='nearest')

VALIDATION_DIR = "F:/python_pros/jupyterProject/AndroidStudio/data/rps-test-set/"
validation_datagen = ImageDataGenerator(rescale = 1./255)

train_generator = training_datagen.flow_from_directory(
    TRAINING_DIR,
    target_size=(150,150),
    class_mode='categorical',
  batch_size=126
)

validation_generator = validation_datagen.flow_from_directory(
    VALIDATION_DIR,
    target_size=(150,150),
    class_mode='categorical',
  batch_size=126
)

model = keras.models.Sequential([
    # Note the input shape is the desired size of the image 150x150 with 3 bytes color
    # This is the first convolution
    tf.keras.layers.Conv2D(64, (3,3), activation='relu', input_shape=(150, 150, 3)),
    tf.keras.layers.MaxPooling2D(2, 2),
    # The second convolution
    tf.keras.layers.Conv2D(64, (3,3), activation='relu'),
    tf.keras.layers.MaxPooling2D(2,2),
    # The third convolution
    tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
    tf.keras.layers.MaxPooling2D(2,2),
    # The fourth convolution
    tf.keras.layers.Conv2D(128, (3,3), activation='relu'),
    tf.keras.layers.MaxPooling2D(2,2),
    # Flatten the results to feed into a DNN
    tf.keras.layers.Flatten(),
    tf.keras.layers.Dropout(0.5),
    # 512 neuron hidden layer
    tf.keras.layers.Dense(512, activation='relu'),
    tf.keras.layers.Dense(3, activation='softmax')
])


model.summary()

model.compile(loss = 'categorical_crossentropy', optimizer='rmsprop', metrics=['accuracy'])

history = model.fit(train_generator, epochs=25, steps_per_epoch=20, validation_data = validation_generator, verbose = 1, validation_steps=3)

model.save("rps.h5")

```

    Found 2520 images belonging to 3 classes.
    Found 372 images belonging to 3 classes.
    Model: "sequential"
    _________________________________________________________________
    Layer (type)                 Output Shape              Param #   
    =================================================================
    conv2d (Conv2D)              (None, 148, 148, 64)      1792      
    _________________________________________________________________
    max_pooling2d (MaxPooling2D) (None, 74, 74, 64)        0         
    _________________________________________________________________
    conv2d_1 (Conv2D)            (None, 72, 72, 64)        36928     
    _________________________________________________________________
    max_pooling2d_1 (MaxPooling2 (None, 36, 36, 64)        0         
    _________________________________________________________________
    conv2d_2 (Conv2D)            (None, 34, 34, 128)       73856     
    _________________________________________________________________
    max_pooling2d_2 (MaxPooling2 (None, 17, 17, 128)       0         
    _________________________________________________________________
    conv2d_3 (Conv2D)            (None, 15, 15, 128)       147584    
    _________________________________________________________________
    max_pooling2d_3 (MaxPooling2 (None, 7, 7, 128)         0         
    _________________________________________________________________
    flatten (Flatten)            (None, 6272)              0         
    _________________________________________________________________
    dropout (Dropout)            (None, 6272)              0         
    _________________________________________________________________
    dense (Dense)                (None, 512)               3211776   
    _________________________________________________________________
    dense_1 (Dense)              (None, 3)                 1539      
    =================================================================
    Total params: 3,473,475
    Trainable params: 3,473,475
    Non-trainable params: 0
    _________________________________________________________________
    Epoch 1/25
    20/20 [==============================] - 40s 2s/step - loss: 1.3816 - accuracy: 0.3536 - val_loss: 1.0834 - val_accuracy: 0.3414
    Epoch 2/25
    20/20 [==============================] - 40s 2s/step - loss: 1.0759 - accuracy: 0.4242 - val_loss: 1.0891 - val_accuracy: 0.4086
    Epoch 3/25
    20/20 [==============================] - 39s 2s/step - loss: 0.9880 - accuracy: 0.5131 - val_loss: 0.9977 - val_accuracy: 0.5780
    Epoch 4/25
    20/20 [==============================] - 39s 2s/step - loss: 0.8917 - accuracy: 0.5948 - val_loss: 0.4861 - val_accuracy: 0.7796
    Epoch 5/25
    20/20 [==============================] - 39s 2s/step - loss: 0.9928 - accuracy: 0.6202 - val_loss: 0.5291 - val_accuracy: 0.6774
    Epoch 6/25
    20/20 [==============================] - 40s 2s/step - loss: 0.7001 - accuracy: 0.6889 - val_loss: 0.5713 - val_accuracy: 0.6935
    Epoch 7/25
    20/20 [==============================] - 40s 2s/step - loss: 0.6585 - accuracy: 0.7290 - val_loss: 0.6888 - val_accuracy: 0.6828
    Epoch 8/25
    20/20 [==============================] - 40s 2s/step - loss: 0.5396 - accuracy: 0.7845 - val_loss: 0.1624 - val_accuracy: 0.9973
    Epoch 9/25
    20/20 [==============================] - 40s 2s/step - loss: 0.4735 - accuracy: 0.8198 - val_loss: 0.7620 - val_accuracy: 0.6667
    Epoch 10/25
    20/20 [==============================] - 41s 2s/step - loss: 0.4426 - accuracy: 0.8198 - val_loss: 0.6193 - val_accuracy: 0.6344
    Epoch 11/25
    20/20 [==============================] - 41s 2s/step - loss: 0.3765 - accuracy: 0.8516 - val_loss: 0.4950 - val_accuracy: 0.7419
    Epoch 12/25
    20/20 [==============================] - 41s 2s/step - loss: 0.3268 - accuracy: 0.8730 - val_loss: 0.0545 - val_accuracy: 1.0000
    Epoch 13/25
    20/20 [==============================] - 41s 2s/step - loss: 0.2502 - accuracy: 0.9083 - val_loss: 0.0274 - val_accuracy: 1.0000
    Epoch 14/25
    20/20 [==============================] - 41s 2s/step - loss: 0.2303 - accuracy: 0.9103 - val_loss: 0.4023 - val_accuracy: 0.7366
    Epoch 15/25
    20/20 [==============================] - 41s 2s/step - loss: 0.2770 - accuracy: 0.8921 - val_loss: 0.0327 - val_accuracy: 1.0000
    Epoch 16/25
    20/20 [==============================] - 43s 2s/step - loss: 0.1836 - accuracy: 0.9357 - val_loss: 0.0207 - val_accuracy: 1.0000
    Epoch 17/25
    20/20 [==============================] - 42s 2s/step - loss: 0.1529 - accuracy: 0.9405 - val_loss: 0.0681 - val_accuracy: 0.9812
    Epoch 18/25
    20/20 [==============================] - 42s 2s/step - loss: 0.1540 - accuracy: 0.9468 - val_loss: 0.0286 - val_accuracy: 1.0000
    Epoch 19/25
    20/20 [==============================] - 42s 2s/step - loss: 0.1747 - accuracy: 0.9381 - val_loss: 0.0535 - val_accuracy: 1.0000
    Epoch 20/25
    20/20 [==============================] - 42s 2s/step - loss: 0.1135 - accuracy: 0.9667 - val_loss: 0.0391 - val_accuracy: 0.9839
    Epoch 21/25
    20/20 [==============================] - 42s 2s/step - loss: 0.1454 - accuracy: 0.9452 - val_loss: 0.0372 - val_accuracy: 1.0000
    Epoch 22/25
    20/20 [==============================] - 42s 2s/step - loss: 0.1281 - accuracy: 0.9560 - val_loss: 0.1813 - val_accuracy: 0.9946
    Epoch 23/25
    20/20 [==============================] - 42s 2s/step - loss: 0.1387 - accuracy: 0.9508 - val_loss: 0.0358 - val_accuracy: 0.9919
    Epoch 24/25
    20/20 [==============================] - 42s 2s/step - loss: 0.0636 - accuracy: 0.9782 - val_loss: 0.0618 - val_accuracy: 1.0000
    Epoch 25/25
    20/20 [==============================] - 42s 2s/step - loss: 0.1101 - accuracy: 0.9595 - val_loss: 0.0227 - val_accuracy: 1.0000
    
完成模型训练之后，我们绘制训练和验证结果的相关信息

```python
import matplotlib.pyplot as plt
acc = history.history['accuracy']
val_acc = history.history['val_accuracy']
loss = history.history['loss']
val_loss = history.history['val_loss']

epochs = range(len(acc))

plt.plot(epochs, acc, 'r', label='Training accuracy')
plt.plot(epochs, val_acc, 'b', label='Validation accuracy')
plt.title('Training and validation accuracy')
plt.legend(loc=0)
plt.figure()
plt.show()

```


    
![png](https://github.com/52hertzhz/Android-studio/blob/main/%E5%AE%9E%E9%AA%8C%E4%BA%94_2/image/output_4_0.png)
    



    <Figure size 640x480 with 0 Axes>



```python

```
