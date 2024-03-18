# 简介

处理地理数据经常会涉及到地理编码的问题。地理编码指的是将地理信息转化成坐标关系的过程。分为正向和反向的编码。正向的是指将地址信息转换为坐标点，比如：武汉市武汉大学—>(114.3594147, 30.5401222)；反向地理编码就是将地理坐标转换为具体的地址，是一个与前面相反的过程。

基于Python的地理编码库geopy 是用于地理编码的常用工具，使用它可获取多种地图服务的坐标。Python开发者可以使用geopy很容易的获取全球的某个街道地址，城市，国家和地块的地理坐标，它是通过第三方的地理编码器和数据源来解析的。

主要有以下几个功能：

地理编码：将字符串转换为地理位置

逆地理编码：用于将地理坐标转换为具体地址

计算两个点的距离：经纬度距离和球面距离

# 功能

## 地理编码


点击复制代码
from geopy.geocoders import Nominatim
 
geolocator = Nominatim()                       # 定义对象
location = geolocator.geocode("北京天安门")      # 传入位置字符串
print(location.address)		                   # 打印地址信息
print((location.latitude,location.longitude))  # 打印纬度、经度
print(location.altitude)				       # 打印海拔
print(location.raw)							   # 打印地图信息



输出结果：

点击复制代码
天安门, 1, 西长安街, 崇文, 北京市, 东城区, 北京市, 100010, 中国
(39.9073426, 116.391264649167)
0.0
{'place_id': 242516302, 'licence': 'Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright', 'osm_type': 'relation', 'osm_id': 8847697, 'boundingbox': ['39.9071466', '39.9075289', '116.3905678', '116.3919619'], 'lat': '39.9073426', 'lon': '116.391264649167', 'display_name': '天安门, 1, 西长安街, 崇文, 北京市, 东城区, 北京市, 100010, 中国', 'class': 'historic', 'type': 'city_gate', 'importance': 0.001}

## 逆地理编码
点击复制代码
from geopy.geocoders import Nominatim
 
geolocator = Nominatim()
location = geolocator.reverse("39.9073426, 116.391264649167")   # 传入纬度、经度字符串
print(location.address)
print((location.latitude,location.longitude))
print(location.altitude)
print(location.raw)


输出结果：

点击复制代码
天安门, 1, 西长安街, 崇文, 北京市, 东城区, 北京市, 100010, 中国
(39.9073426, 116.391264649167)
0.0
{'place_id': 242516302, 'licence': 'Data © OpenStreetMap contributors, ODbL 1.0. https://osm.org/copyright', 'osm_type': 'relation', 'osm_id': 8847697, 'lat': '39.9073426', 'lon': '116.391264649167', 'display_name': '天安门, 1, 西长安街, 崇文, 北京市, 东城区, 北京市, 100010, 中国', 'address': {'address29': '天安门', 'house_number': '1', 'road': '西长安街', 'suburb': '崇文', 'city': '东城区', 'state': '北京市', 'postcode': '100010', 'country': '中国', 'country_code': 'cn'}, 'boundingbox': ['39.9071466', '39.9075289', '116.3905678', '116.3919619']}
## 计算经纬度距离
点击复制代码
from geopy.distance import geodesic
 
tian_an_men = (39.9073285, 116.391242416486)
xiaozhai = (34.2253171, 108.9426205)

可以是meters（米）、kilometers（千米）、miles（英里）、nautical（海里）、feet（英尺）
print(geodesic(tian_an_men, xiaozhai).meters)


输出结果：

点击复制代码
913925.3165094886

## 计算球面距离
点击复制代码
913925.3165094886


输出结果：

点击复制代码
913657.4596518732

# 参考

[《Python地理空间分析》学习仓库](https://github.com/MrWhitebare/Python-Geography-spatial-analysis/tree/master)