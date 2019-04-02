#### tensorflow dynamic_rnn与static_rnn使用注意


##### static rnn
``` python
def lstm_model(x,y):
  # x = tf.transpose(x, [1, 0, 2])
  # x = tf.reshape(x, [-1, 1])
  # x = tf.split(x, 10, axis=0)
#上面和下面这一句效果一样
   x=tf.unstack(x,axis=2)
   lstm_cell = rnn.BasicLSTMCell(20)
   outputs, states = rnn.static_rnn(lstm_cell, x, dtype=tf.float32)
   output=outputs[-1]
   prediction,loss=learn.models.linear_regression(output,y)
   train_op=tf.contrib.layers.optimize_loss(loss,tf.contrib.framework.get_global_step(),\
   optimizer='Adagrad',learning_rate=0.1)
   return prediction,loss,train_op
```
##### dynamic_rnn
``` python
#n_steps = 25
#n_hidden = 32
def lstm_model(x,y):
    lstm_cell = rnn.BasicLSTMCell(20)
    outputs, states = tf.nn.dynamic_rnn(lstm_cell, x, dtype=tf.float32)
    output=tf.transpose(outputs, [1, 0, 2])[-1]
    prediction,loss=learn.models.linear_regression(output,y)
    train_op=tf.contrib.layers.optimize_loss(loss,tf.contrib.framework.get_global_step(),\
    optimizer='Adagrad',learning_rate=0.1)
    return prediction,loss,train_op
```



在每一个train step，传入model的是一个**batch**的数据（这一个batch的数据forward得到predictions，计算loss，backpropagation更新参数），这一个batch内的数据一定是padding成相同长度的。那么，如果可以只在一个batch内部进行padding，例如一个batch中数据长度均在6-10这个范围内，就可以让这个batch中所有数据pad到固定长度10，而整个dataset上的数据最大长度很可能是100，**这样就不需要让这些数据也pad到100那么长，白白浪费空间。** 所以dynamic_rnn实现的功能就是可以让不同迭代传入的batch可以是**长度不同数据**，但同一次迭代一个batch内部的所有数据长度仍然是**固定的**。例如，第一时刻传入的数据shape=[batch_size, 10]，第二时刻传入的数据shape=[batch_size, 12]，第三时刻传入的数据shape=[batch_size, 8]等等。但是**rnn不能这样**，它要求每一时刻传入的batch数据的[batch_size,** max_seq**]，在每次迭代过程中**都保持不变**。这样不就必须要求全部数据都要pad到统一的max_seq长度了吗？是的，但也有个折中办法。——将数据集的sequence length做个初步统计，看会落在哪几个区间段内。然后根据区间段将数据进行归类，也就是所谓的放在不同buckets中。最后用rnn为每一个buckets都创建一个sub graph。训练的时候，根据当前batch data所归属的bucket id，找到它对应的sub graph，进行参数更新（虽然是不同的sub graph，但参数是共享的。至少tensorflow中是这么实现的~\(≧▽≦)/~）具体可参看：https://github.com/tensorflow/tensorflow/blob/master/tensorflow/contrib/legacy_seq2seq/python/ops/seq2seq.py#L1143另外，两者的输入形式确实不同，但你提到的“将填充的部分输出为0”，给rnn传入sequence_length这个参数后，也是可以的。



我从**具体的实现**上讲一下。调用static_rnn实际上是**生成了**rnn**按时间序列展开之后的图**。打开tensorboard你会看到sequence_length个rnn_cell stack在一起，只不过这些cell是share weight的。因此，sequence_length就和图的拓扑结构绑定在了一起，**因此也就限制了每个batch的sequence_length必须是一致。** 调用dynamic_rnn不会将rnn展开，而是利用tf.while_loop这个api，通过Enter, Switch, Merge, LoopCondition, NextIteration等这些control flow的节点，生成一个可以执行循环的图（这个图应该还是静态图，因为图的拓扑结构在执行时是不会变化的）。在tensorboard上，**你只会看到一个rnn_cell**, 外面被一群control flow节点包围着。对于dynamic_rnn来说，sequence_length仅仅代表着循环的次数，而和**图本身的拓扑没有关系**，**所以每个batch可以有不同sequence_length。**
