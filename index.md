<script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
                inlineMath: [['\$', '\$'], ['\\(', '\\)']],
                processEscapes: true
            },
            CommonHTML: {
                matchFontHeight: false
            }
        });
        </script>
        <script type="text/javascript" async="" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script>

# Python備忘録
これはPythonをしばらくいじらなくなって課題などでよく使う操作を忘れてしまった時のための備忘録である。

[戻る](https://yumannimac.github.io/first/)

##  $x-y$ 座標でのグラフ

###  一つのグラフ
簡単なグラフの書き方を最低限復習する。
下のグラフは`arrange`を用いて $y=\sin x$ のグラフを $x-y$ 平面に $\Delta x=0.01$ おきに $0$ から $2\pi $ までプロットしたものである。
`linspace`を用いる方法もある。



```python
import numpy as np
import matplotlib.pylab as plt

x = np.arange(0, 2*np.pi, 0.01)
plt.plot(x,np.sin(x))
plt.title('y=sin x') 
plt.xlabel('x') 
plt.ylabel('y')
plt.show()
```


    
![png](python_bibouroku_files/python_bibouroku_4_0.png)
    


`plt.show`でグラフを表示する。

### 複数のグラフ


```python
import numpy as np
import matplotlib.pylab as plt

x = np.arange(0, 2*np.pi, 0.01)
plt.plot(x, np.sin(x),label="y=sin x")
plt.plot(x, np.cos(x), label="y=cos x")

plt.title('')
plt.xlabel('x')
plt.ylabel('y')
plt.legend()
plt.show()

```


    
![png](python_bibouroku_files/python_bibouroku_7_0.png)
    


グラフが複数になるとどのグラフだか判別するための凡例が必要。`plt.legend`で凡例を表示できる。そのとき`plot`の中でラベリングすることを忘れずに。

## 回帰分析


```python
import numpy as np
from scipy import optimize
import matplotlib.pylab as plt
x = np.array([1, 2, 3, 4])
y = np.array([1, 4, 9, 16])


def f(x, a, b):
  return a*x+b


print("y=", optimize.curve_fit(f, x, y)[0][0], "x+",optimize.curve_fit(f, x, y)[0][1])
a, b = optimize.curve_fit(f, x, y)[0]
x0 = np.linspace(0, 5, 1001)
y0 = f(x0, a, b)
plt.scatter(x, y)
plt.plot(x0, y0)
plt.show()

```

    y= 5.000000000008725 x+ -5.000000000013091



    
![png](python_bibouroku_files/python_bibouroku_10_1.png)
    


ExcelではなくPythonで回帰分析をする状況で使うためのものである。ネットで調べてもコードは出てくるがそのほとんどがcsvファイル等アップロードするものであるから正直言って使いにくい。上に書いたコードは単体で完結する。

なお"optimize.curve_fit(f, x, y)[0]"はなんかよくわからないけど線形にフィッティングした時の(傾き,$y$切片)が入ったリストである。([1]とかにすると標準偏差が出てきた記憶がある）
```
def f(x, a, b):
return a*(x)+b
```
の中身をたとえば $y=ax^2+b$ すなわち
`a*x**2+b`
とかにするとそのようにフィッティングされ、これまた(a,b)の値が出てくる。

なお
`plt.scatter(x, y)`
は
```
x = np.array([1, 2, 3, 4])
y = np.array([1, 4, 9, 16])
```
をそのままプロットしただけのものである。

## 微分方程式

### 一変数の場合
例えば関数$x \left(t\right)$の初期値問題

$$
x \left(0\right) =x_0=0.5,\frac{dx}{dt}=\frac{x}{10}
$$

の数値的な解は以下である。

なお解析的な解は

$$
x=\frac{1}{2}e^{\frac{t}{10}}
$$

である。


```python
import numpy as np
from scipy import integrate
import matplotlib.pyplot as plt


def f(x, t):
  return x/10


t = np.arange(0, 11, 0.01)
x0 = 0.5
result = integrate.odeint(f, x0, t)
plt.plot(t, result[:, 0], label='x')
plt.xlabel('Time')
plt.legend()
plt.show()

```


    
![png](python_bibouroku_files/python_bibouroku_14_0.png)
    


### 多変数の場合
ベクトル値関数${Y}\left(t\right)=\left(y_1,y_2\right)$の初期値問題

$$
\frac{dy_1}{dt}=10-y_2, \frac{dy_2}{dt}=y_1,  \left(y_1 \left(0\right),y_2 \left(0\right) \right) =\left(1,1\right) 
$$

の数値的な解は以下。



```python
import numpy as np
from scipy import integrate
import matplotlib.pyplot as plt


def dfunc(Y, t):
  y1, y2 = Y
  return (10-y2, y1)


y1 = 1
y2 = 1
time = 10
Y0 = (y1, y2)
t = np.arange(0, time, 0.01)
result = integrate.odeint(dfunc, Y0, t)
plt.plot(t, result[:, 0], label='y1')
plt.plot(t, result[:, 1], label='y2')
plt.xlabel('Time')
plt.ylabel('y1,y2')
plt.legend()
plt.show()

```


    
![png](python_bibouroku_files/python_bibouroku_16_0.png)
    


$3$変数以上の場合も同様。

[戻る](https://yumannimac.github.io/first/)

<script src="https://blz-soft.github.io/md_style/release/v1.2/md_style.js" ></script>
