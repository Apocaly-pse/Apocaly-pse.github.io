---
tags: Python DL
---

# 写在前面

前几天改了一份代码, 是关于深度学习中卷积神经网络的Python代码, 用于解决分类问题. 代码是用TensorFlow的Keras接口写的, 需求是转换成pytorch代码, 鉴于两者的api相近, 盖起来也不会太难, 就是一些细节需要注意, 在这里记录一下, 方便大家参考. 



# 关于库函数

首先来看看在库函数的导入方面这两个流行的深度学习框架有什么区别, 这就需要简单了解一下二者的主要结构了. 为方便叙述, 下面提到的TF都是指TensorFlow2.X with Keras, Torch都是指PyTorch.



首先来看模型的构建, 对于TF, 模型的构建可以方便地通过`sequential`方法得到, 这就需要引入该方法:

```python
from tensorflow.keras.models import Sequential
```

在Torch中, 当然也可以通过`sequential`进行模型的构建, (不过官方还是更推荐采用面向对象的方式)

这里需要引入:

```python
from torch.nn import Sequential
```



说到模型构建, 就不得不提在卷积神经网络里面十分常用的几个层: conv层, maxpool层和全连接层(softmax), 这些在两个框架中都有现成的, 下面来看看如何调用这些方法:

在TF中:

```python
from tensorflow.keras.layers import Conv2D, MaxPooling2D
from tensorflow.keras.layers import Activation, Dropout, Flatten, Dense
```

而在Torch中:

```python
from torch.nn import Conv2d, MaxPool2d
from torch.nn import Flatten, Linear, CrossEntropyLoss
from torch.optim import SGD
```

可见二者只有些微的不同, TF中将一些激活函数的调用放在了参数里面, 而Torch都是以库函数的形式给出的. 



最后来看看数据的导入部分, 在TF中可以很方便地使用下面的方法进行数据(图片)的处理和读取:

```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator
```

在Torch中, 需要类似导入:

```python
from torchvision import transforms, datasets
```



# 数据读取/处理部分的api差异







# 模型构建部分的api差异





# 模型训练/预测部分的api差异





```python

#导入数据
# 图像维度
img_width, img_height = 32, 32

train_data_dir = './train'
validation_data_dir = './test'
nb_train_samples = 2700
nb_validation_samples = 700
epochs = 135
batch_size = 20

if K.image_data_format() == 'channels_first':
    input_shape = (3, img_width, img_height)
else:
    input_shape = (img_width, img_height, 3)



# 创建模型
model = Sequential()
model.add(Conv2D(filters=6, kernel_size=(5, 5), padding='valid', input_shape=input_shape, activation='tanh'))

model.add(MaxPooling2D(pool_size=(2, 2)))

model.add(Conv2D(filters=16, kernel_size=(5, 5), padding='valid', activation='tanh'))

model.add(MaxPooling2D(pool_size=(2, 2)))

model.add(Flatten())

model.add(Dense(120, activation='tanh'))

model.add(Dense(84, activation='tanh'))

model.add(Dense(4, activation='softmax'))

#编译模型
model.compile(loss='categorical_crossentropy',
              optimizer='sgd',
              metrics=['accuracy'])

# model.summary();exit()
# 训练集图像增强
train_datagen = ImageDataGenerator(
    rescale=1. / 255,
    shear_range=0.2,
    zoom_range=0.2,
    horizontal_flip=True)

# 测试集图像增强（only rescaling）
test_datagen = ImageDataGenerator(rescale=1. / 255)

train_generator = train_datagen.flow_from_directory(
    train_data_dir,
    target_size=(img_width, img_height),
    batch_size=batch_size,
    class_mode='categorical')  # 多分类
# print(train_generator);exit()
validation_generator = test_datagen.flow_from_directory(
    validation_data_dir,
    target_size=(img_width, img_height),
    batch_size=batch_size,
    class_mode='categorical')  # 多分类

#训练模型
history=model.fit_generator(
    train_generator,
    steps_per_epoch=nb_train_samples // batch_size,
    epochs=epochs,
    validation_data=validation_generator,
    validation_steps=nb_validation_samples // batch_size)

#图标说明
fig = plt.figure()#新建一张图
plt.plot(history.history['accuracy'],label='training acc')
plt.plot(history.history['val_accuracy'],label='val acc')
plt.title('model accuracy')
plt.ylabel('accuracy')
plt.xlabel('epoch')
plt.legend(labels=['train','test'],loc='lower right')
fig.savefig('accuracy_epoch.jpg')
fig = plt.figure()
plt.plot(history.history['loss'],label='training loss')
plt.plot(history.history['val_loss'], label='val loss')
plt.title('model loss')
plt.ylabel('loss')
plt.xlabel('epoch')
plt.legend(labels=['train','test'],loc='upper right')
fig.savefig('loss_epoch.jpg')


```





```python
# from tensorflow.keras.preprocessing.image import ImageDataGenerator
# from tensorflow.keras.models import Sequential
# from tensorflow.keras.layers import Conv2D, MaxPooling2D
# from tensorflow.keras.layers import Activation, Dropout, Flatten, Dense
# from tensorflow.keras import backend as K
import matplotlib.pyplot as plt
import torch
from torch.nn import Sequential
# from torch.utils.data import DataLoader
from torchvision import transforms, datasets
from torch.nn import Conv2d, MaxPool2d
from torch.nn import Flatten, Linear, CrossEntropyLoss
from torch.optim import SGD


#导入数据
# 图像维度
img_width, img_height = 32, 32

train_data_dir = './train'
validation_data_dir = './test'
nb_train_samples = 2700
nb_validation_samples = 700
num_epochs = 30
batch_size = 20

input_shape = (img_width, img_height, 3)


# 创建模型
model = Sequential(
    Conv2d(in_channels=3, out_channels=6, kernel_size=(5, 5), padding='valid'),
    MaxPool2d(kernel_size=(2, 2)),
    Conv2d(in_channels=6, out_channels=16, kernel_size=(5, 5), padding='valid'),
    MaxPool2d(kernel_size=(2, 2)),
    Flatten(),
    Linear(400, 120),
    Linear(120, 84), 
    Linear(84, 4)
)

#编译模型
# model.compile(loss='categorical_crossentropy',
#               optimizer='sgd',
#               metrics=['accuracy'])
learning_rate = 0.01
optimizer = SGD(model.parameters(), lr=learning_rate)



# 训练集图像增强
train_datagen = transforms.Compose([
    transforms.ToTensor(),
    transforms.RandomHorizontalFlip(),
    transforms.Resize((img_width, img_height))
])

#     rescale=1. / 255,
#     shear_range=0.2,
#     zoom_range=0.2,
#     horizontal_flip=True

# 测试集图像增强（only rescaling）
test_datagen = transforms.Compose([  # 对读取的图片进行以下指定操作
    transforms.ToTensor(), 
    transforms.Resize((img_width, img_height))
])
# rescale=1. / 255
train_generator = datasets.ImageFolder(train_data_dir, transform=train_datagen)

#  = train_datagen.flow_from_directory(
#     train_data_dir,
#     target_size=(img_width, img_height),
#     batch_size=batch_size,
#     class_mode='categorical')  # 多分类

validation_generator = datasets.ImageFolder(validation_data_dir, transform=test_datagen)

#  = test_datagen.flow_from_directory(
#     validation_data_dir,
#     target_size=(img_width, img_height),
#     batch_size=batch_size,
#     class_mode='categorical')  # 多分类

train_loader = torch.utils.data.DataLoader(train_generator, batch_size=batch_size,
                                           shuffle=True)

test_loader = torch.utils.data.DataLoader(validation_generator, batch_size=batch_size,
                                          shuffle=False)


criterion = CrossEntropyLoss()
optimizer = SGD(model.parameters(), lr=0.001)
def train():
    n_total_steps = len(train_loader)
    for epoch in range(num_epochs):
        for i, (images, labels) in enumerate(train_loader):
            # Forward pass
            outputs = model(images)
            loss = criterion(outputs, labels)

            # Backward and optimize
            optimizer.zero_grad()
            loss.backward()
            optimizer.step()

            if (i+1) % 5 == 0:
                print(
                    f'Epoch [{epoch+1}/{num_epochs}], Step [{i+1}/{n_total_steps}], Loss: {loss.item():.4f}')

train()
print('Finished Training')
PATH = './cnn.pth'
torch.save(model.state_dict(), PATH)
# exit()
with torch.no_grad():
    n_correct = 0
    n_samples = 0
    n_class_correct = [0 for i in range(4)]
    n_class_samples = [0 for i in range(4)]
    for images, labels in test_loader:
        outputs = model(images)
        # max returns (value ,index)
        _, predicted = torch.max(outputs, 1)
        n_samples += labels.size(0)
        n_correct += (predicted == labels).sum().item()

        for i in range(batch_size):
            label = labels[i]
            pred = predicted[i]
            if (label == pred):
                n_class_correct[label] += 1
            n_class_samples[label] += 1

    acc = 100.0 * n_correct / n_samples
    print(f'Accuracy of the network: {acc} %')

    classes = ('0', '1', '2', '3')

    for i in range(4):
        acc = 100.0 * n_class_correct[i] / n_class_samples[i]
        print(f'Accuracy of {classes[i]}: {acc} %')



```



# 小结

