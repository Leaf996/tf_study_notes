# Save and Load
## Questions
- 不同版本的tf和模型保存方法的对应关系
- 各种保存方法的优缺点及应用场景
- 各种保存格式的文件内容
## Method List
- checkpoint
    ```
    checkpoint
    model.ckpt-240000.data-00000-of-00001
    model.ckpt-240000.index
    model.ckpt-240000.meta
    ```
    - save
        ```
        tf.train.Saver().save()
        ```
    - load
        ```
        new_saver = tf.train.import_meta_graph('model.ckpt-240000.meta')
        with tf.Session() as sess:
            new_saver.restore(sess, tf.train.latest_checkpoint('./'))
        ```
    - partial model
        ```
        graph.get_tensor_by_name()
        ```
    - situation
        - training and fine-tuning
    - reference
        - [tensorflow的三种保存格式总结-1(.ckpt)][1]
        - [**tensorflow中的检查点checkpoint详解**][2]

- saved_model(pb, variables)
    ```
    assets/
    assets.extra/
    variables/
        variables.data-?????-of-?????
        variables.index
    saved_model.pb
    ```
    - save
        ```
        tf.compat.v1.saved_model.simple_save()
        ```
    - load
        ```
        tf.compat.v1.saved_model.load()
        tf.compat.v1.train.import_meta_graph()
        ```
    - situation
        - tensorflow serving inference
    - reference
        - [TensorFlow SavedModel][3]
        - [tensorflow1.x版本的savedmodel][4]
        - [详解tensorflow2.0的模型保存方法][5]
        - [tensorflow的三种保存格式总结-1(.ckpt)][1]

- single pb
    ```
    frozen.pb
    ```
    - save
        ```
        graph_util.convert_variables_to_constants()
        ```
    - load
        ```
        model_path = 'frozen.pb'
        with tf.gfile.GFile(model_path, "rb") as f:
            graph_def = tf.GraphDef()
            graph_def.ParseFromString(f.read())
        tf.import_graph_def(graph_def, name='frozen')
        ```
    - situation
        - inference
    - reference
        - [Tensorflow的三种储存格式-2][6]

[1]:https://zhuanlan.zhihu.com/p/60064947
[2]:https://blog.csdn.net/qq_27825451/article/details/105819752
[3]:https://github.com/tensorflow/tensorflow/blob/master/tensorflow/python/saved_model/README.md
[4]:https://blog.csdn.net/qq_27825451/article/details/105866464
[5]:https://blog.csdn.net/qq_27825451/article/details/105505033
[6]:https://zhuanlan.zhihu.com/p/60069860