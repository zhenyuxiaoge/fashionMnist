# fashionMnist

一.分类 fashion mnist数据集
使用cnn训练，预测准确率为91.65%。
代码见cnn.py


二.
淘宝上有几亿的买家和千万级的商家，我们需要在每个月初给头部的商家（已知商家的名单）推荐一定数量的用户，便于商家有针对性的进行营销活动。
要求：
a.    请给出具体的方案和实现方法，包括所用的模型、数据、训练和预测的方法。
b.    已有的数据是过去半年所有买家的行为数据，即过去半年的每一天里，单个买家在每个商品上的浏览、收藏、推荐，和购买数量。

该问题为推荐系统问题。

    考虑用户的购买行为是有序列关系的(前面买的物品会某种程度上影响后面买的物品)。
    得到海量的商品序列，参考word2vec的思想，可以做一个item2vec，将item(商品)转换为vector (embedding)。
    之后预测用户当前时间购买某类商品的意愿，可以类比于自然语言中，一段话之后可能出现哪个词(购买了一系列商品后，现在要购买什么商品)。
    可以使用rnn进行建模和预测。
    预测得到该用户当前想购买的商品(vector)，与商户出售的商品(vector)进行相似度计算。
    越近，越优先推荐给商户。

    具体操作：
        1.
        数据：形如  冰箱,彩电,电磁炉,...,电饭锅,洗衣机,牙刷  的用户购买商品序列
        对商品进行合适的编码(one-hot、Huffman)使用浅层神经网络(word2vec使用)，得到每种商品的vector(embedding）
        2.
        数据：形如
                  浏览，冰箱(vector)，时间
                  收藏，彩电(vector)，时间
                  购买，冰箱(vector)，时间
                  购买，彩电(vector)，时间
                  推荐，彩电(vector)，时间
                  购买，电磁炉(vector)，时间
        使用rnn或者LSTM进行训练和预测
        3.
        训练过程中，可以分时间划分训练集和测试集。
        如，拥有1-6月数据，划分数据集如下：
            训练集      测试集
            1月         2月
            2月         3月
            1-2月       3月
            3月         4月
            2-3月       4月
            1-3月       4月
            4月         5月
            3-4月       5月
            2-4月       5月
            1-4月       5月
            5月         6月
            4-5月       6月
            3-5月       6月
            2-5月       6月
            1-5月       6月
        验证设想：越近的数据越重要，以及判断多久之前的数据可以舍弃(或者降低权重)。
        4.
        其他一些设计：
        该用户已经关注了该商户或经常在该商户下买东西，可以回避推荐
        商品如果有自身的描述或者图像的，可以将这部分信息也加入到item2vec中
        用户行为习惯显示对某类商家有所偏爱(比如喜欢地址在江浙沪的商家)，推荐时有所侧重
        ...



