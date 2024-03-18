## Geopandas基础知识

geopandas是一个python地理工具包，需要先安装shapely才能使用（geopandas中的点、线、多边形类都继承自Shapely）能够从.shp文件中读取到坐标点信息，返回DataFrame形式的数据。

pandas 的 DataFrames 表示表格数据集。同样地，geopandas 的 DataFrames 表示的是有两个扩展功能的表格数据集：

- geometry列定义了一个与其他列相关的点、线或多边形。这一列是一个shapely对象的集合。你能对shapley的形状对象做什么，你也能对geometry
  对象做什么。
- CRS列是geometry
  列的坐标参考系统，它告诉我们一个点、线或多边形在地球表面的位置。Geopandas将一个几何体映射到地球表面（例如，WGS84）。

相比`pyshp`库，`geopandas`库的数据读取、展示、分析、拓展的效果要更好。它可以读取zip中的`shapefile`，还可以读取GeoJson、ArcGIS中地理数据库`gdb`，以及`QGIS`中`GeoPackage` 存放的矢量数据。

```python
import geopandas as gpd
from matplotlib import pyplot as plt

data = gpd.read_file(r'E:\gisData\行政区划数据2019\省.shp')#读取磁盘上的矢量文件
#data = gpd.read_file('shapefile/china.gdb', layer='province')#读取gdb中的矢量数据
print(data.crs)  # 查看数据对应的投影信息
print(data.head())  # 查看前5行数据
data.plot()
plt.show()#简单展示
```

显示效果：

![img](https://image.malagis.com/gis/2020/fungis2/image-20200820200607950.png)

让我们动手试试

```text
# !pip install geopandas
import matplotlib
import geopandas as gpd
```

### 加载数据

让我们先加载一个随geopandas提供的数据集，叫做 'naturalearth_lowres'。这个数据集包括世界上每个国家的几何形状，并附有一些进一步的细节，如人口和GDP估计。

```text
world_gdf = gpd.read_file(
    gpd.datasets.get_path('naturalearth_lowres')
)
world_gdf
```

|      | pop_est   | continent     | name                     | iso_a3 | gdp_md_est | geometry                                          |
| ---- | --------- | ------------- | ------------------------ | ------ | ---------- | ------------------------------------------------- |
| 0    | 920938    | Oceania       | Fiji                     | FJI    | 8374.0     | MULTIPOLYGON (((180.00000 -16.06713, 180.00000... |
| 1    | 53950935  | Africa        | Tanzania                 | TZA    | 150600.0   | POLYGON ((33.90371 -0.95000, 34.07262 -1.05982... |
| 2    | 603253    | Africa        | W. Sahara                | ESH    | 906.5      | POLYGON ((-8.66559 27.65643, -8.66512 27.58948... |
| 3    | 35623680  | North America | Canada                   | CAN    | 1674000.0  | MULTIPOLYGON (((-122.84000 49.00000, -122.9742... |
| 4    | 326625791 | North America | United States of America | USA    | 18560000.0 | MULTIPOLYGON (((-122.84000 49.00000, -120.0000... |
| ...  | ...       | ...           | ...                      | ...    | ...        | ...                                               |
| 172  | 7111024   | Europe        | Serbia                   | SRB    | 101800.0   | POLYGON ((18.82982 45.90887, 18.82984 45.90888... |
| 173  | 642550    | Europe        | Montenegro               | MNE    | 10610.0    | POLYGON ((20.07070 42.58863, 19.80161 42.50009... |
| 174  | 1895250   | Europe        | Kosovo                   | -99    | 18490.0    | POLYGON ((20.59025 41.85541, 20.52295 42.21787... |
| 175  | 1218208   | North America | Trinidad and Tobago      | TTO    | 43570.0    | POLYGON ((-61.68000 10.76000, -61.10500 10.890... |
| 176  | 13026129  | Africa        | S. Sudan                 | SSD    | 20880.0    | POLYGON ((30.83385 3.50917, 29.95350 4.17370, ... |

177 rows × 6 columns

如果你忽略geometry列（一个shapely 对象），这看起来就像一个普通的DataFrames，所有列的意义都很容易明白。

### CRS

数据框架还包括一个CRS，将geometry列中定义的多边形映射到地球表面。

```text
world_gdf.crs
```

在我们的案例中，CRS是EPSG:4326。该CRS使用纬度和经度作为坐标。

> 注意事项：
> **CRS的组成部分**
>
> **基准** - 参考系统，在我们的例子中，它定义了测量的起点（主子午线）和地球形状的模型（椭球体）。最常见的基准是WGS84，但它不是唯一的基准。
> **使用区域** - 在我们的案例中，使用区域是整个世界，但也有许多CRS是为某个特定的区域而优化的。
> **轴和单位** - 通常，经度和纬度是以度为单位的。X、Y坐标的单位通常以米为单位。

让我们看看一个我们必须改变CRS的例子。

让我们来测量每个国家的人口密度! 我们可以测量每个几何体的面积，但请记住，我们首先需要转换为以米为单位的等面积投影。

```text
world_gdf = world_gdf.to_crs("+proj=eck4 +lon_0=0 +x_0=0 +y_0=0 +datum=WGS84 +units=m +no_defs")
world_gdf.crs
```

现在我们可以通过用人口估计数除以面积来计算每个国家的人口密度。

**注意**：我们可以像访问普通列一样访问几何体的面积。虽然没有列包含几何体的面积，但面积是几何体对象的一个属性。

```text
world_gdf['pop_density'] = world_gdf.pop_est / world_gdf.area * 10**6

world_gdf.sort_values(by='pop_density', ascending=False)
```

|      | pop_est   | continent               | name                   | iso_a3 | gdp_md_est | geometry                                          | pop_density |
| ---- | --------- | ----------------------- | ---------------------- | ------ | ---------- | ------------------------------------------------- | ----------- |
| 99   | 157826578 | Asia                    | Bangladesh             | BGD    | 628400.00  | POLYGON ((8455037.031 2862141.705, 8469605.972... | 1174.967806 |
| 79   | 4543126   | Asia                    | Palestine              | PSE    | 21220.77   | POLYGON ((3127401.561 4023733.541, 3087561.638... | 899.418534  |
| 140  | 23508428  | Asia                    | Taiwan                 | TWN    | 1127000.00 | POLYGON ((11034560.069 3156825.603, 11032285.2... | 681.899108  |
| 77   | 6229794   | Asia                    | Lebanon                | LBN    | 85160.00   | POLYGON ((3141154.397 4236334.349, 3117804.289... | 615.543551  |
| 96   | 51181299  | Asia                    | South Korea            | KOR    | 1929000.00 | POLYGON ((10835604.955 4755864.739, 10836040.9... | 515.848728  |
| ...  | ...       | ...                     | ...                    | ...    | ...        | ...                                               | ...         |
| 97   | 3068243   | Asia                    | Mongolia               | MNG    | 37000.00   | POLYGON ((7032142.671 6000941.853, 7107939.605... | 1.987486    |
| 20   | 2931      | South America           | Falkland Is.           | FLK    | 281.80     | POLYGON ((-4814015.486 -6253920.632, -4740858.... | 0.179343    |
| 22   | 57713     | North America           | Greenland              | GRL    | 2173.00    | POLYGON ((-2555525.099 8347965.820, -2346518.8... | 0.026295    |
| 23   | 140       | Seven seas (open ocean) | Fr. S. Antarctic Lands | ATF    | 16.00      | POLYGON ((5550199.759 -5932855.132, 5589906.67... | 0.012091    |
| 159  | 4050      | Antarctica              | Antarctica             | ATA    | 810.00     | MULTIPOLYGON (((-2870542.982 -8180812.656, -28... | 0.000331    |

177 rows × 7 columns

请注意，现在的几何对象的数值与之前的单位完全不同。

仅仅看一下上面的数据框架，我们就可以迅速识别出异常值。孟加拉国的人口密度约为$1175 persons/km^2$. 南极洲的人口密度几乎为零，只有810人生活在一个广阔的空间里。

不过，将地图可视化总是更好。所以让我们可视化吧!

### 可视化

我们可以在world_gdf上调用.plot()，就像调用pandas的dataframe一样。

```text
figsize = (20, 11)

world_gdf.plot('pop_density', legend=True, figsize=figsize);
```

![img](https://pic1.zhimg.com/80/v2-974e7a3412b851fa7cc95fe7dba8c220_1440w.webp)

上面的地图看起来不是很有帮助，所以让我们通过以下方式使它变得更好：

- 改为墨卡托投影，因为它更熟悉。使用参数to_crs('epsg:4326')
  可以做到这一点
- 将色条转换为对数刻度，这可以用matplotlib.colors.LogNorm(vmin=world.pop_density.min(), vmax=world.pop_density.max())
  来实现

我们可以像在matplotlib上直接传递不同的参数给绘图函数。

```text
norm = matplotlib.colors.LogNorm(vmin=world_gdf.pop_density.min(), vmax=world_gdf.pop_density.max())

world_gdf.to_crs('epsg:4326').plot("pop_density", 
                                   figsize=figsize, 
                                   legend=True,  
                                   norm=norm);
```

![img](https://pic1.zhimg.com/80/v2-0c2643b194eb7b637a9e08f1ee6e2090_1440w.webp)

到目前为止，我们已经了解shapely和geopandas的基本知识，但现在是时候进入一个完整的案例研究。





Python中的contextily是一个非常方便的库，它提供了一个简单的方法来添加基于Web的地图背景到你的地理数据可视化中。



使用contextily库，你可以轻松地将OpenStreetMap、Stamen Maps和其他在线地图服务作为地图背景添加到你的Python可视化中。此外，contextily还提供了一些有用的函数，例如web_mercator_autozoom()，它可以帮助你将地理数据投影到Web Mercator坐标系，并自动缩放地图。

如果您想在Python中使用Contextily库来在地图上添加底图，可以通过以下步骤进行安装：

1. 确保您已经安装了Python环境。您可以从Python官方网站下载安装程序并进行安装。

2. 打开命令行终端（Windows上的命令提示符或者Linux/Mac上的终端），输入以下命令来安装Contextily：

   ```
   复制代码pip install contextily
   ```

   如果您使用的是Python 3.x版本，请确保您使用的是pip3来进行安装，例如：

   ```
   复制代码pip3 install contextily
   ```

3. 安装完成后，您就可以在Python中使用Contextily库了。如果您需要使用OpenStreetMap、Stamen Terrain等地图服务，您需要在使用之前调用`contextily.tile`函数下载对应的地图瓦片数据。

   以下是一个简单的示例代码：

   ```python
   import geopandas as gpd
   import matplotlib.pyplot as plt
   import contextily as ctx
   
   # 加载示例数据
   world = gpd.read_file(gpd.datasets.get_path('naturalearth_lowres'))
   ax = world.plot()
   
   # 添加地图底图
   ctx.add_basemap(ax)
   
   # 显示地图
   plt.show()
   ```

   在上面的代码中，我们使用`geopandas`库加载了一个示例数据，然后在地图上添加了一个OpenStreetMap的底图。最后，我们使用`matplotlib`库将地图显示在屏幕上。



下面是一个简单的例子，演示了如何使用contextily库在地图上添加OpenStreetMap背景：

```python
python复制代码import geopandas as gpd
import matplotlib.pyplot as plt
import contextily as ctx

# 读取地理数据
gdf = gpd.read_file('path/to/your/data.shp')

# 投影到Web Mercator坐标系
gdf = gdf.to_crs(epsg=3857)

# 创建地图
ax = gdf.plot(figsize=(10, 10), alpha=0.5)

# 添加OpenStreetMap背景
ctx.add_basemap(ax)

# 显示地图
plt.show()
```

在这个例子中，我们首先读取了地理数据，然后将其投影到Web Mercator坐标系。接下来，我们创建了一个地图，并使用ctx.add_basemap()函数将OpenStreetMap背景添加到地图上。最后，我们使用plt.show()函数显示地图。

这只是contextily库的一个简单示例。如果你需要更多的帮助，可以参考contextily的文档和示例代码，以获取更多关于如何使用contextily库的详细信息。