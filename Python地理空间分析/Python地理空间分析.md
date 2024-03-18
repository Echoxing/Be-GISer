# 介绍

# 用于地理空间数据分析的 22 个 Python 库列表
Shapely
使用 Shapely，可以创建有形状的几何对象（例如点、多边形、多多边形）并对其进行操作，例如缓冲、计算面积或交点等。

Fiona
Fiona 可以使用多层 GIS 格式和压缩虚拟文件系统读取和写入真实世界的数据，并很容易与其他 Python GIS 包（如pyproj {.dt .iz}、Rtree和 Shapely ）集成。

GeoPandas
Geopandas 就如 pandas 遇见 GIS。它扩展了 pandas 使用的数据类型以允许对几何类型进行空间操作。几何运算由 Shapely 执行，它还依赖于 Fiona 进行文件访问和 matplotlib 进行绘图。

GeoPlot
高级地理空间绘图库。它是对 cartopy 和 matplotlib 的扩展，使制图变得更加容易。它建立在其他几个流行的地理空间库之上，以简化通常需要的编码。

Arcpy
如果您使用 Esri ArcGIS，那么可能熟悉会 ArcPy 库。ArcPy 用于地理处理操作。但它不仅用于空间分析，还用于 Esri ArcGIS 的数据转换、管理和地图制作。

Scikit-Image
图像处理库，例如直方图调整、过滤器、分割、边缘检测操作、纹理特征提取等。

SciKit-Learn
最好的同时易于使用的 Python 机器学习库，可进行回归、分类、降维等。

Descartes
支持将形状几何图形绘制为 matplotlib 路径、补丁，也是 geopandas 的几何绘图功能的依赖项。

RasterStats
用于分区统计。根据几何图形从栅格文件或 numpy 数组中提取统计信息。

Rasterio
Rasterio 是处理栅格数据的首选库。它允许从 numpy {.dt .iz} 数组（Python 数组操作的实际标准）读取、写入光栅文件，能够提供许多简便的方法来操作这些数组（例如掩码、矢量化等），并且可以处理坐标参考系的变换。就像任何其他 numpy 数组一样，数据也可以很容易地绘制出来，例如使用 matplotlib 库。

ipyleaflet
如果想创建交互式地图，那么 ipyleaflet 是 Jupyter notebook 和 Leaflet 的融合。可以控制各种自定义设置，例如加载底图、geojson 和小部件。它还提供了多种地图类型可供选择，包括等值线图、速度数据和并排视图。

Folium
就像 ipyleaflet 一样，Folium 允许利用 leaflet 来构建交互式网络地图。它能够在 Python 中操作数据，可以使用领先的开源 JavaScript 库将其可视化。

Geemap
Geemap 更倾向于使用 Google Earth Engine (GEE) 进行科学和数据分析。

PySAL
Python 空间分析库包含大量用于空间分析、统计建模和绘图的函数。它旨在支持高级应用程序的开发。

xarray
xarray 非常适合处理大量图像时间序列堆栈，想象一下 5 个植被指数 x 24 个日期 x 256 像素 x 256 像素。xarray 允许标记多维 numpy 数组的维度，并将其与许多函数和 pandas 库的语法（例如 groupby、滚动窗口、绘图）相结合。

PyProj
PyProj 库的主要目的是它如何与空间参考系统一起工作。它可以用一系列地理参考系统投影和转换坐标，PyProj 还可以为任何给定的基准执行大地测量计算和距离。

GDAL/OGR
GDAL/OGR 库用于在 GIS 格式和扩展名之间进行转换。QGIS、ArcGIS、ERDAS、ENVI 和 GRASS GIS 以及几乎所有 GIS 软件都以某种方式使用它进行翻译。目前，GDAL/OGR 支持 97 个矢量和 162 个光栅驱动程序。

RSGISLib
RSGISLib 库是一套用于栅格处理和分析的遥感工具。它对图像进行分类、过滤和执行统计，很多人最喜欢的是基于对象的分割和分类 (GEOBIA) 模块。

ReportLab
ReportLab 是此列表中最令人满意的库之一，原因在于 GIS 通常缺乏足够的报告能力。特别是，如果想创建一个报告模板，这是一个很好的选择，但不知为什么 ReportLab 库有点不受关注。

Imageio
它是一个 Python 库，提供了一个简单的界面来读取和写入各种图像数据，包括动画图像、体积数据和科学格式。

MapClassify
它为 choropleth 图实施了一系列分类方案。重点是确定分类的数量，并将观察结果分配给这些分类。

RTree
它是 lib_spatial_index 的 ctypes Python 包装器，提供了许多高级空间索引功能。

# 资源

[]