# MATLAB

## 图像

https://ww2.mathworks.cn/help/matlab/graphics.html?s_tid=CRUX_lftnav

### 二维图和三维图

绘制**连续、离散、曲面以及三维体**数据图

![image-20220822101525590](C:\Users\31803\AppData\Roaming\Typora\typora-user-images\image-20220822101525590.png)

#### 创建二维线图

创建一个简单的线图并标记坐标区。通过更改**线条颜色、线型和添加标记**来自定义线图的外观。

##### 创建线图

使用 `plot` 函数创建二维线图。例如，绘制从 0 到 2*π* 之间的正弦函数值。

```matlab
x = linspace(0,2*pi,100);
y = sin(x);
plot(x,y)
# 标记坐标区并添加标题
xlabel('x')
ylabel('sin(x)')
title('Plot of the Sine Function')
```

##### 绘制多个线条

默认情况下，MATLAB 会在执行每个绘图命令之前清空图窗。使用 `figure` 命令打开一个新的图窗窗口。可以使用 `hold on` 命令绘制多个线条。在使用 `hold off` 或关闭窗口之前，当前图窗窗口中会显示所有绘图。

```matlab
figure
x = linspace(0,2*pi,100);
y = sin(x);
plot(x,y)

hold on 
y2 = cos(x);
plot(x,y2)
hold off
```

##### 更改线条外观

通过在调用 `plot` 函数时包含可选的线条设定，可以更改线条颜色、线型或添加标记。例如：

- `':'` 绘制点线。
- `'g:'` 绘制绿色点线。
- `'g:*'` 绘制带有星号标记的绿色点线。
- `'*'` 绘制不带线条的星号标记。

符号可以按任意顺序显示。不需要同时指定所有三个特征（线条颜色、线型和标记）

###### plot

https://ww2.mathworks.cn/help/matlab/ref/plot.html#btzitot_sep_mw_3a76f056-2882-44d7-8e73-c695c0c54ca8

**语法**

```
plot(X,Y)
plot(X,Y,LineSpec)
plot(X1,Y1,...,Xn,Yn)
plot(X1,Y1,LineSpec1,...,Xn,Yn,LineSpecn)
plot(Y)
plot(Y,LineSpec)
plot(___,Name,Value)
plot(ax,___)
p = plot(___)
```

###### `LineSpec` - 线型、标记和颜色

线型、标记和颜色，指定为包含符号的字符向量或字符串。符号可以按任意顺序显示。您不需要同时指定所有三个特征（线型、标记和颜色）。例如，如果忽略线型，只指定标记，则绘图只显示标记，不显示线条。

**示例：** `'--or'` 是带有圆形标记的红色虚线

| 线型   | 说明   | 表示的线条                                                   |
| :----- | :----- | :----------------------------------------------------------- |
| `'-'`  | 实线   | ![Sample of solid line](https://ww2.mathworks.cn/help/matlab/ref/linestyle_solid.png) |
| `'--'` | 虚线   | ![Sample of dashed line](https://ww2.mathworks.cn/help/matlab/ref/linestyle_dashed.png) |
| `':'`  | 点线   | ![Sample of dotted line](https://ww2.mathworks.cn/help/matlab/ref/linestyle_dotted.png) |
| `'-.'` | 点划线 | ![Sample of dash-dotted line, with alternating dashes and dots](https://ww2.mathworks.cn/help/matlab/ref/linestyle_dashdotted.png) |

| 标记  | 说明     | 生成的标记                                                   |
| :---- | :------- | :----------------------------------------------------------- |
| `'o'` | 圆圈     | ![Sample of circle marker](https://ww2.mathworks.cn/help/matlab/ref/marker_o.png) |
| `'+'` | 加号     | ![Sample of plus sign marker](https://ww2.mathworks.cn/help/matlab/ref/marker_plus.png) |
| `'*'` | 星号     | ![Sample of asterisk marker](https://ww2.mathworks.cn/help/matlab/ref/marker_asterisk.png) |
| `'.'` | 点       | ![Sample of point marker](https://ww2.mathworks.cn/help/matlab/ref/marker_point.png) |
| `'x'` | 叉号     | ![Sample of cross marker](https://ww2.mathworks.cn/help/matlab/ref/marker_x.png) |
| `'_'` | 水平线条 | ![Sample of horizontal line marker](https://ww2.mathworks.cn/help/matlab/ref/marker_hline.png) |
| `'|'` | 垂直线条 | ![Sample of vertical line marker](https://ww2.mathworks.cn/help/matlab/ref/marker_vline.png) |
| `'s'` | 方形     | ![Sample of square marker](https://ww2.mathworks.cn/help/matlab/ref/marker_s.png) |
| `'d'` | 菱形     | ![Sample of diamond line marker](https://ww2.mathworks.cn/help/matlab/ref/marker_d.png) |
| `'^'` | 上三角   | ![Sample of upward-pointing triangle marker](https://ww2.mathworks.cn/help/matlab/ref/marker_uptriangle.png) |
| `'v'` | 下三角   | ![Sample of downward-pointing triangle marker](https://ww2.mathworks.cn/help/matlab/ref/marker_downtriangle.png) |
| `'>'` | 右三角   | ![Sample of right-pointing triangle marker](https://ww2.mathworks.cn/help/matlab/ref/marker_righttriangle.png) |
| `'<'` | 左三角   | ![Sample of left-pointing triangle marker](https://ww2.mathworks.cn/help/matlab/ref/marker_lefttriangle.png) |
| `'p'` | 五角形   | ![Sample of pentagram marker](https://ww2.mathworks.cn/help/matlab/ref/marker_p.png) |
| `'h'` | 六角形   | ![Sample of hexagram marker](https://ww2.mathworks.cn/help/matlab/ref/marker_h.png) |

| 颜色名称    | 短名称 | RGB 三元组 | 外观                                                         |
| :---------- | :----- | :--------- | :----------------------------------------------------------- |
| `'red'`     | `'r'`  | `[1 0 0]`  | ![Sample of the color red](https://ww2.mathworks.cn/help/matlab/ref/hg_red.png) |
| `'green'`   | `'g'`  | `[0 1 0]`  | ![Sample of the color green](https://ww2.mathworks.cn/help/matlab/ref/hg_green.png) |
| `'blue'`    | `'b'`  | `[0 0 1]`  | ![Sample of the color blue](https://ww2.mathworks.cn/help/matlab/ref/hg_blue.png) |
| `'cyan'`    | `'c'`  | `[0 1 1]`  | ![Sample of the color cyan](https://ww2.mathworks.cn/help/matlab/ref/hg_cyan.png) |
| `'magenta'` | `'m'`  | `[1 0 1]`  | ![Sample of the color magenta](https://ww2.mathworks.cn/help/matlab/ref/hg_magenta.png) |
| `'yellow'`  | `'y'`  | `[1 1 0]`  | ![Sample of the color yellow](https://ww2.mathworks.cn/help/matlab/ref/hg_yellow.png) |
| `'black'`   | `'k'`  | `[0 0 0]`  | ![Sample of the color black](https://ww2.mathworks.cn/help/matlab/ref/hg_black.png) |
| `'white'`   | `'w'`  | `[1 1 1]`  | ![Sample of the color white](https://ww2.mathworks.cn/help/matlab/ref/hg_white.png) |



##### 更改线条对象的属性

通过更改用来创建绘图的 `Line` 对象的属性，还可以自定义绘图的外观。

创建一个线图。将创建的 `Line` 对象赋给变量 `ln`。显示画面上显示常用属性，例如 `Color`、`LineStyle` 和 `LineWidth`。

```
x = linspace(0,2*pi,25);
y = sin(x);
ln = plot(x,y)

ln = 
  Line with properties:

              Color: [0 0.4470 0.7410]
          LineStyle: '-'
          LineWidth: 0.5000
             Marker: 'none'
         MarkerSize: 6
    MarkerFaceColor: 'none'
              XData: [0 0.2618 0.5236 0.7854 1.0472 1.3090 1.5708 1.8326 ... ]
              YData: [0 0.2588 0.5000 0.7071 0.8660 0.9659 1 0.9659 ... ]
              ZData: [1x0 double]

  Show all properties
```

要访问各个属性，请使用圆点表示法：

```
ln.LineWidth = 2;
ln.Color = [0 0.5 0.5];
ln.Marker = 'o';
ln.MarkerEdgeColor = 'b';
```

##### 为图添加标题和轴标签

说明如何使用 `title`、`xlabel` 和 `ylabel` 函数向图中添加标题和轴标签。它还说明如何通过更改字体大小来自定义坐标区文本的外观。

###### 添加标题

使用 `title` 函数向图中添加标题。要显示希腊符号 *π*，请使用 TeX 标记 `\pi`。

```
title('Line Plot of Sine and Cosine Between -2\pi and 2\pi')
```

###### 添加坐标轴标签

使用 `xlabel` 和 `ylabel` 函数向图中添加轴标签。

```
xlabel('-2\pi < x < 2\pi') 
ylabel('Sine and Cosine Values') 
```

###### 添加图例

使用 `legend` 函数向图中添加标识每个数据集的图例。按照绘制线条的顺序指定图例说明。（可选）使用八个基本或斜角方位之一指定图例位置，在本例中为 `'southwest'`。

```
legend({'y = sin(x)','y = cos(x)'},'Location','southwest')
```

![image-20220822111838915](C:\Users\31803\AppData\Roaming\Typora\typora-user-images\image-20220822111838915.png)

###### legend

**语法**

```
legend
legend(label1,...,labelN)
legend(labels)
legend(subset,___)
legend(target,___)
legend(___,'Location',lcn)
legend(___,'Orientation',ornt)
legend(___,Name,Value)
legend(bkgd)
lgd = legend(___)
legend(vsbl)
legend('off')
```

**说明**

[示例](https://ww2.mathworks.cn/help/matlab/ref/legend.html#bu_szh3-1)

`legend` 为每个绘制的数据序列创建一个带有描述性标签的图例。对于标签，图例使用数据序列的 `DisplayName` 属性中的文本。如果 `DisplayName` 属性为空，则图例使用 `'dataN'` 形式的标签。当您在坐标区上添加或删除数据序列时，图例会自动更新。此命令在由 `gca` 命令返回的当前坐标区中创建一个图例。如果当前坐标区为空，则图例为空。如果不存在坐标区，则 `legend` 创建一个笛卡尔坐标区。

[示例](https://ww2.mathworks.cn/help/matlab/ref/legend.html#bt6ef_q-2_1)

`legend(label1,...,labelN)` 设置图例标签。以字符向量或字符串列表形式指定标签，例如 `legend('Jan','Feb','Mar')`。

`legend(labels)` 使用字符向量元胞数组、字符串数组或字符矩阵设置标签，例如 `legend({'Jan','Feb','Mar'})`。

[示例](https://ww2.mathworks.cn/help/matlab/ref/legend.html#bu_sz6u-1)

`legend(subset,___)` 仅在图例中包括 `subset` 中列出的数据序列的项。`subset` 以图形对象向量的形式指定。您可以在指定标签之前或不指定其他输入参数的情况下指定 `subset`。

[示例](https://ww2.mathworks.cn/help/matlab/ref/legend.html#bu_szhx-1)

`legend(target,___)` 使用 `target` 指定的坐标区或独立可视化，而不是使用当前坐标区。指定 target 作为第一个输入参数。

[示例](https://ww2.mathworks.cn/help/matlab/ref/legend.html#bt6r30y)

`legend(___,'Location',lcn)` 设置图例位置。例如，`'Location','northeast'` 将在坐标区的右上角放置图例。请在其他输入参数之后指定位置。

[示例](https://ww2.mathworks.cn/help/matlab/ref/legend.html#bt6r30y)

`legend(___,'Orientation',ornt)`（其中 `ornt` 为 `'horizontal'`）并排显示图例项。`ornt` 的默认值为 `'vertical'`，即垂直堆叠图例项。

[示例](https://ww2.mathworks.cn/help/matlab/ref/legend.html#bvaamk1-1)

`legend(___,Name,Value)` 使用一个或多个名称-值对组参数来设置图例属性。

[示例](https://ww2.mathworks.cn/help/matlab/ref/legend.html#bt6tbh9)

`legend(bkgd)`（其中 `bkgd` 为 `'boxoff'`）删除图例背景和轮廓。`bkgd` 的默认值为 `'boxon'`，即显示图例背景和轮廓。

`lgd = legend(___)` 返回 `Legend` 对象。可使用 `lgd` 在创建图例后查询和设置图例属性。有关属性列表，请参阅 [Legend 属性](https://ww2.mathworks.cn/help/matlab/ref/matlab.graphics.illustration.legend-properties.html)。



`legend(vsbl)` 控制图例的可见性，其中 `vsbl` 为 `'hide'`、`'show'` 或 `'toggle'`。

`legend('off')` 删除图例。

###### 带有变量值的标题

通过使用 `num2str` 函数将值转换为文本，可在标题文本中包含变量值。您可以使用类似的方法为轴标签或图例条目添加变量值。

添加带有 sin(*π*)/2 值的标题。

```
k = sin(pi/2);
title(['sin(\pi/2) = ' num2str(k)])
```
