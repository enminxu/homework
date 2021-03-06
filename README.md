# homework

# 机器学习

## 基本概念简介

Machine Learning相当于寻找函数，根据函数给出相应的输出。

这些函数的类型有3种（机器学习的任务）：

- Regression：这个函数的输出是一个数值。

- Classification：给出一些选择，这个函数输出正确的那一个选择。

- Structured Learning：创造一些有结构的物件。

寻找函式：

**1.设置一个带有未知数的函式**

Model:y=b+wx~2~(利用已知进行b（bias），w（weight）的求解)

**2.从训练资料里定义误差（loss）**

y=b+wx~1~→y=0.5k+1x1

![](README.assets/7-16349590584521.png)

这样的话可以得到2017年1月1号到12月31号每天的误差值（e~1~，e~2~，e~3~，…e~n~）
$$
lOSS: L=\frac{1}{N}\sum_{N}{e_n}
$$

$$
e_1=|y-\hat{y}|
$$

$$
e_2=(y-\hat{y})^2
$$

e~1~: is mean absolute error (MAE)

e~2~: is mean square error(MSE)

**3.优化**
$$
w^*,b^*=arg\,\mathop{min}\limits_{w,b}\,L
$$
利用gradient descent（梯度下降算法）找到使得误差L最小的w和b。

分析改进：

![](README.assets/8-16349590929133.png)

我们观察真实数据可以看出一个规律：每7天是一个循环，而且有两天数据很低。

但是我们现在的模型是按照前一天的数据来推算后一天的数据，那么由于这个规律性，如果我们使用的是7天的数据来预测之后天数的数据岂不是更好。

所以：

![](README.assets/9-16349623773491.png)



我们得到的这种模型叫做Linear models:

![](README.assets/10-16349624163822.png)

Linear models太过于简单（因为他永远是一条直线）。

而现实情况可能是红色这条线的样子：

![](README.assets/11-16349624496443.png)

所以我们要写一个更复杂的model：

![](README.assets/12-16349625044454.png)

红色的线叫做Piecewise linear Curves(可以由一个常数和多个蓝色的线组成)

Piecewise linear Curves可以以下图形式逼近任意曲线：

![](README.assets/13-16349625403385.png)

而蓝色的线（Hard Sigmoid）用可以用Sigmoid函数来逼近：

![](README.assets/14-16349625664916.png)

我们可以通过调整b，w得到不同形状的sigmoid函数：

![](README.assets/15-16349625911207.png)

所以最终Piecewise linear Curves可以表示为：

![](README.assets/16-16349592763394.png)

我们可以得到一个新的模型：
$$
y=b+wx_1
$$

$$
\Downarrow
$$

$$
y=b+\sum_{i}{sigmoid(b_i+w_ix_1)}
$$

$$
y=b+\sum_{j}{w_jx_j}
$$

$$
\Downarrow
$$

$$
y=b+\sum_i{c_isigmoid(b_i+\sum_j{w_{ij}x_i})}
$$

![](README.assets/18-16349593755555.png)

用矩阵的形式可以表示为：

![](README.assets/19-16349594342866.png)
$$
\Downarrow
$$
![](README.assets/20-16349594620007.png)
$$
\Downarrow
$$
![](README.assets/21-16349594831248.png)
$$
\Downarrow
$$
![](README.assets/22-16349594982859.png)
$$
接下来让我们看看y=b+c^T\delta(b^{'}+wx)中包含哪些未知参数：
$$
![](README.assets/23-163495953962910.png)
$$
(w,b,c^T,b^{'})中包含很多未知参数，将这四个矩阵里面的未知数拉直，比如w里面的每一行拉直排列加上其它的未知\\
参数就得到了\theta\hspace{18.2cm}
$$
我们回头再看机器学习的三大步骤：

1.设置一个带有未知数的函式：
$$
y=b+c^T\delta(b^{'}+wx)
$$
2.从训练资料里定义误差(loss):

![](README.assets/24-163495956915011.png)

3.优化：

![](README.assets/25-163495958324012.png)

然而我们实际的优化过程是将一笔资料等分成若干个batch，每一个batch更新一次参数，更新次数就是batch的个数，将这一笔资料的所有batch更新完成就是完成一个epoch。

![](README.assets/26-163495959787713.png)

另外，对于Hard Sigmoid我们也可以不采用sigmoid函数来逼近，可以组合两个ReLU来得到Hard Sigmoid：

![](README.assets/27-163495961224714.png)
$$
\Downarrow
$$


![](README.assets/28-163495962958815.png)

### 补充内容

#### 1.pandas

##### <1>.creating,reading and writing

###### **Getting started**

To use pandas, you'll typically start with the following line of code.

```python
import pandas as pd
```

###### Creating data

There are two core objects in pandas: the DataFrame and the series.

DataFrame 

A DataFrame is a table. It contains an array of individual *entries*, each of which has a certain *value*. Each entry corresponds to a row (or *record*) and a *column*.

For example, consider the following simple DataFrame:

```python
pd.DataFrame({'Yes': [50, 21], 'No': [131, 2]})
```

![](README.assets/29-163495964936916.png)

In this example, the "0, No" entry has the value of 131. The "0, Yes" entry has a value of 50, and so on.

DataFrame entries are not limited to integers. For instance, here's a DataFrame whose values are strings:

```python
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 'Sue': ['Pretty good.', 'Bland.']})
```

![](README.assets/30-163495966289917.png)

We are using the `pd.DataFrame()` constructor to generate these DataFrame objects. The syntax for declaring a new one is a dictionary whose keys are the column names (`Bob` and `Sue` in this example), and whose values are a list of entries. This is the standard way of constructing a new DataFrame, and the one you are most likely to encounter.

linkcode

The dictionary-list constructor assigns values to the *column labels*, but just uses an ascending count from 0 (0, 1, 2, 3, ...) for the *row labels*. Sometimes this is OK, but oftentimes we will want to assign these labels ourselves.

The list of row labels used in a DataFrame is known as an **Index**. We can assign values to it by using an `index` parameter in our constructor:

```python
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 
              'Sue': ['Pretty good.', 'Bland.']},
             index=['Product A', 'Product B'])
```

![](README.assets/31-163495968068418.png)

Series

A Series, by contrast, is a sequence of data values. If a DataFrame is a table, a Series is a list. And in fact you can create one with nothing more than a list:

```python
pd.Series([1, 2, 3, 4, 5])
```

![](README.assets/32-163495970113019.png)

A Series is, in essence, a single column of a DataFrame. So you can assign column values to the Series the same way as before, using an `index` parameter. However, a Series does not have a column name, it only has one overall `name`:

```python
pd.Series([30, 35, 40], index=['2015 Sales', '2016 Sales', '2017 Sales'], name='Product A')
```

![](README.assets/33-163495971405220.png)

The Series and the DataFrame are intimately related. It's helpful to think of a DataFrame as actually being just a bunch of Series "glued together". We'll see more of this in the next section of this tutorial.

###### Reading data files

Being able to create a DataFrame or Series by hand is handy. But, most of the time, we won't actually be creating our own data by hand. Instead, we'll be working with data that already exists.

Data can be stored in any of a number of different forms and formats. By far the most basic of these is the humble CSV file. When you open a CSV file you get something that looks like this:

![](README.assets/34-163495979434622.png)So a CSV file is a table of values separated by commas. Hence the name: "Comma-Separated Values", or CSV.

Let's now set aside our toy datasets and see what a real dataset looks like when we read it into a DataFrame. We'll use the `pd.read_csv()` function to read the data into a DataFrame. This goes thusly:

```python
wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv")
```

We can use the `shape` attribute to check how large the resulting DataFrame is:

```python
wine_reviews.shape
```

![](README.assets/35-163495980785023.png)

So our new DataFrame has 130,000 records split across 14 different columns. That's almost 2 million entries!

We can examine the contents of the resultant DataFrame using the `head()` command, which grabs the first five rows:

```
wine_reviews.head()
```

![](README.assets/36-163495982020024.png)

The `pd.read_csv()` function is well-endowed, with over 30 optional parameters you can specify. For example, you can see in this dataset that the CSV file has a built-in index, which pandas did not pick up on automatically. To make pandas use that column for the index (instead of creating a new one from scratch), we can specify an `index_col`.

```python
wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv", index_col=0)
wine_reviews.head()
```

![](README.assets/37-163495983120025.png)

