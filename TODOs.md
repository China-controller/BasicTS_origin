先做完这个，后面的不是很重要：
- Step 1: 写一个脚本能够处理pd.DataFrame形式的数据，并将其转换为我们现在的版本的格式。然后把现有的所有数据集都变成这个pd.DataFrame样式。



- Step 2: 在预处理的过程中不进行归一化，仅产生mean和std，产生两种形式，norm_each_channel还是norm_all_channel（仅产生一个文件）
    - 以什么样的形式存储和加载？
- Step 3: 修改Runner。
    假如归一化函数和参数存在：
        Rescale=True，就用norm_all_channel
        Rescale=False，就用norm_each_channel
    假如归一化函数不存在：
        怎么确定使用哪个归一化？
        Rescale=True，即时norm_all_channel
        Rescale=False，即时norm_all_channel
    假如不需要归一化呢？（像M4一样）
    即时归一化的mean和std要怎么更新、怎么兼容？

class Scaler:
    def __init__(self):

    def transform(self, x):
        raise NotImplementedError

    def re_transform(self, x):
        raise NotImplementedError

class MinMaxScaler(Scaler):
    def __init__(self, min, max):
        self.min = min
        self.max = max

    def transform(self, x, min=None, max=None):
        return (x - self.min) / (self.max - self.min)

    def re_transform(self, x, min=None, max=None):
        return x * (self.max - self.min) + self.min
