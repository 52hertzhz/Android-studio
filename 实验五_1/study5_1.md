使用TensorFlow Lite Model Maker生成图像分类模型

预备工作
安装依赖库

```python
import os

import numpy as np

import tensorflow as tf
assert tf.__version__.startswith('2')

from tflite_model_maker import model_spec
from tflite_model_maker import image_classifier
from tflite_model_maker.config import ExportFormat
from tflite_model_maker.config import QuantizationConfig
from tflite_model_maker.image_classifier import DataLoader

import matplotlib.pyplot as plt

```

模型训练

获取数据

```python
image_path = tf.keras.utils.get_file(
      'flower_photos.tgz',
      'https://storage.googleapis.com/download.tensorflow.org/example_images/flower_photos.tgz',
      extract=True)
image_path = os.path.join(os.path.dirname(image_path), 'flower_photos')

```
从storage.googleapis.com获取训练所需的数据集，可以指定image_path目录，默认为image_path

第一步：加载数据集，并将数据集分为训练数据和测试数据
```python
data = DataLoader.from_folder(image_path)
train_data, test_data = data.split(0.9)


第二步：训练Tensorflow模型

```

    INFO:tensorflow:Load image with size: 3670, num_label: 5, labels: daisy, dandelion, roses, sunflowers, tulips.
    
第三步：评估模型

```python
model = image_classifier.create(train_data)

```

    INFO:tensorflow:Retraining the models...
    Model: "sequential"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     hub_keras_layer_v1v2 (HubKe  (None, 1280)             3413024   
     rasLayerV1V2)                                                   
                                                                     
     dropout (Dropout)           (None, 1280)              0         
                                                                     
     dense (Dense)               (None, 5)                 6405      
                                                                     
    =================================================================
    Total params: 3,419,429
    Trainable params: 6,405
    Non-trainable params: 3,413,024
    _________________________________________________________________
    None
    Epoch 1/5
    

    2023-06-05 02:52:51.331855: W tensorflow/core/framework/cpu_allocator_impl.cc:82] Allocation of 19267584 exceeds 10% of free system memory.
    2023-06-05 02:52:51.387397: W tensorflow/core/framework/cpu_allocator_impl.cc:82] Allocation of 19267584 exceeds 10% of free system memory.
    2023-06-05 02:52:51.402235: W tensorflow/core/framework/cpu_allocator_impl.cc:82] Allocation of 51380224 exceeds 10% of free system memory.
    2023-06-05 02:52:51.445357: W tensorflow/core/framework/cpu_allocator_impl.cc:82] Allocation of 19267584 exceeds 10% of free system memory.
    2023-06-05 02:52:51.501725: W tensorflow/core/framework/cpu_allocator_impl.cc:82] Allocation of 19267584 exceeds 10% of free system memory.
    

    103/103 [==============================] - 61s 566ms/step - loss: 0.8554 - accuracy: 0.7697
    Epoch 2/5
    103/103 [==============================] - 55s 528ms/step - loss: 0.6594 - accuracy: 0.8899
    Epoch 3/5
    103/103 [==============================] - 53s 518ms/step - loss: 0.6211 - accuracy: 0.9117
    Epoch 4/5
    103/103 [==============================] - 53s 514ms/step - loss: 0.5990 - accuracy: 0.9284
    Epoch 5/5
    103/103 [==============================] - 54s 519ms/step - loss: 0.5846 - accuracy: 0.9366
    


```python
loss, accuracy = model.evaluate(test_data)

```

    12/12 [==============================] - 8s 449ms/step - loss: 0.6158 - accuracy: 0.9210
    
第四步，导出Tensorflow Lite模型

```python
model.export(export_dir='.')

```

    2023-06-05 02:58:31.302142: W tensorflow/python/util/util.cc:368] Sets are not currently considered sequences, but this may change in the future, so consider avoiding using them.
    

    INFO:tensorflow:Assets written to: /tmp/tmpxvc87npv/assets
    

    INFO:tensorflow:Assets written to: /tmp/tmpxvc87npv/assets
    2023-06-05 02:58:35.689735: I tensorflow/core/grappler/devices.cc:66] Number of eligible GPUs (core count >= 8, compute capability >= 0.0): 0
    2023-06-05 02:58:35.689896: I tensorflow/core/grappler/clusters/single_machine.cc:358] Starting new session
    2023-06-05 02:58:35.742309: I tensorflow/core/grappler/optimizers/meta_optimizer.cc:1164] Optimization results for grappler item: graph_to_optimize
      function_optimizer: Graph size after: 913 nodes (656), 923 edges (664), time = 23.456ms.
      function_optimizer: function_optimizer did nothing. time = 0.01ms.
    
    /opt/conda/envs/py38/lib/python3.8/site-packages/tensorflow/lite/python/convert.py:746: UserWarning: Statistics for quantized inputs were expected, but not specified; continuing anyway.
      warnings.warn("Statistics for quantized inputs were expected, but not "
    2023-06-05 02:58:36.886089: W tensorflow/compiler/mlir/lite/python/tf_tfl_flatbuffer_helpers.cc:357] Ignored output_format.
    2023-06-05 02:58:36.886138: W tensorflow/compiler/mlir/lite/python/tf_tfl_flatbuffer_helpers.cc:360] Ignored drop_control_dependency.
    

    INFO:tensorflow:Label file is inside the TFLite model with metadata.
    

    fully_quantize: 0, inference_type: 6, input_inference_type: 3, output_inference_type: 3
    INFO:tensorflow:Label file is inside the TFLite model with metadata.
    

    INFO:tensorflow:Saving labels in /tmp/tmp9_9qwuyu/labels.txt
    

    INFO:tensorflow:Saving labels in /tmp/tmp9_9qwuyu/labels.txt
    

    INFO:tensorflow:TensorFlow Lite model exported successfully: ./model.tflite
    

    INFO:tensorflow:TensorFlow Lite model exported successfully: ./model.tflite
    


```python

```

训练完毕保存的模型
