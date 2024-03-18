# gdal介绍

GDAL 是读写大量的栅格空间数据格式的广泛应用的开源库。GDAL 是 Geospatial Data Abstraction Library 的缩写, 最开始的时候是一个用来处理栅格空间数据的类库,OGR 则是则是来处 理矢量数据的。 后来,这两个库合并成为合并成为一个,在下载安装的时候,都是使用GDAL 这一个名字。 

GDAL/OGR 使用面向对象的 C++ 语言编写,这令该库在支持百余种格式的同时,还具有很高的执行效率。 GDAL/OGR 同时还提供多种主流编程语言的绑定,==除了 C 和 C++语言之外,用户还可以在 Perl、python、VB6、Ruby、Java、C# 等语言中调用 GDAL==,这令 GDAL 的应用变得非常广泛。GDAL 项目维护了使用 SWIG 生成的 Python 的 GDAL/OGR 绑定。 总体看,Python 中的类与方法与C++的类大体上匹配。 在最新版的 GDAL 1.9.0 中,GDAL 支持超过 120 种栅格数据格式,而在下一版本中,支持的数据格式会达到 200 种。 在 GDAL 缺省的编译选项设置下,一部分数据格式直接创建文件,并对他们进行几何配准。

由于 GDAL 对多种栅格数据格式都提供了支持,很多软件都使用它作为底层数据处理的库。 这其中比较有名的有:ArcGIS、Google Earth、OpenEV、GRASS GIS、OSSIM、Quantum GIS、MapServer、World Wind.

## 导入gdal



1.6以后，推荐使用下面的语句导入：

```
>>> from osgeo import gdal
```

当然早期版本也是支持的，但是在使用的时候会产生一个弃用警告：

为了保持兼容性，可以使用下面的语句来导入：

```
>>> try:
>>>     import gdal
>>> except:
>>>     from osgeo import gdal
```

除了gdal包，还有一个gdalconst包最好也导入。 gdalconst也是osgeo的一个包，它只是在代码中对GDAL中用到的一些常量进行了绑定。 其中gdalconst中的常量都加了前缀，力图与其他模块冲突最小。 所以对gdalconst你可以直接这样导入：

```
>>> from osgeo.gdalconst import *
```

## 2.2.2. 驱动

要读取某种类型的数据，必须要先载入数据驱动，也就是初始化一个对象，让它“知道”某种数据结构。 可以使用下面语句一次性注册所有的数据驱动，但是只能读不能写：

```
>>> gdal.AllRegister()
```

单独注册某一类型的数据驱动，这样的话可以读也可以写，可以新建数据集（这最终还要取决于GDAL是否已经进行了实现）。下面的语句注册了Erdas的栅格数据类型。

```
>>> driver = gdal.GetDriverByName('HFA')
>>> driver.Register()
7
```

可以使用下面的语句判断driver是否注册成功。

```
>>> driver = gdal.GetDriverByName('GeoTiff')
>>> driver == True
False
```

上面的注册就失败了，因为不存在名称为“GeoTiff”的数据格式（正确的格式为“GTiff”）。

## 2.2.3. 查看系统支持的数据格式

除了使用GetDriverByName()， GDAL还可以使用GetDriver()来获得驱动。 下面的代码获取了系统支持的所有的驱动的名称。

```
>>> from osgeo import gdal
>>> drv_count = gdal.GetDriverCount()
>>> drv_count
210
>>> for idx in range(drv_count):
>>>     driver = gdal.GetDriver(idx)
>>>     print( "%10s: %s" % (driver.ShortName, driver.LongName))
       VRT: Virtual Raster
   DERIVED: Derived datasets using VRT pixel functions
     GTiff: GeoTIFF
       COG: Cloud optimized GeoTIFF generator
      NITF: National Imagery Transmission Format
    RPFTOC: Raster Product Format TOC format
   ECRGTOC: ECRG TOC format
       HFA: Erdas Imagine Images (.img)
  SAR_CEOS: CEOS SAR Image
      CEOS: CEOS Image
JAXAPALSAR: JAXA PALSAR Product Reader (Level 1.1/1.5)
       GFF: Ground-based SAR Applications Testbed File Format (.gff)
      ELAS: ELAS
     ESRIC: Esri Compact Cache
       AIG: Arc/Info Binary Grid
   AAIGrid: Arc/Info ASCII Grid
GRASSASCIIGrid: GRASS ASCII Grid
       ISG: International Service for the Geoid
      SDTS: SDTS Raster
      DTED: DTED Elevation Raster
       PNG: Portable Network Graphics
      JPEG: JPEG JFIF
       MEM: In Memory Raster
      JDEM: Japanese DEM (.mem)
       GIF: Graphics Interchange Format (.gif)
    BIGGIF: Graphics Interchange Format (.gif)
      ESAT: Envisat Image Format
      FITS: Flexible Image Transport System
       BSB: Maptech BSB Nautical Charts
       XPM: X11 PixMap Format
       BMP: MS Windows Device Independent Bitmap
     DIMAP: SPOT DIMAP
    AirSAR: AirSAR Polarimetric Image
       RS2: RadarSat 2 XML Product
      SAFE: Sentinel-1 SAR SAFE Product
    PCIDSK: PCIDSK Database File
  PCRaster: PCRaster Raster File
     ILWIS: ILWIS Raster Map
       SGI: SGI Image File Format 1.0
   SRTMHGT: SRTMHGT File Format
  Leveller: Leveller heightfield
  Terragen: Terragen heightfield
    netCDF: Network Common Data Format
      HDF4: Hierarchical Data Format Release 4
 HDF4Image: HDF4 Dataset
     ISIS3: USGS Astrogeology ISIS cube (Version 3)
     ISIS2: USGS Astrogeology ISIS cube (Version 2)
       PDS: NASA Planetary Data System
      PDS4: NASA Planetary Data System 4
     VICAR: MIPL VICAR file
       TIL: EarthWatch .TIL
       ERS: ERMapper .ers Labelled
JP2OpenJPEG: JPEG-2000 driver based on OpenJPEG library
       L1B: NOAA Polar Orbiter Level 1b Data Set
       FIT: FIT Image
      GRIB: GRIdded Binary (.grb, .grb2)
       RMF: Raster Matrix Format
       WCS: OGC Web Coverage Service
       WMS: OGC Web Map Service
      MSGN: EUMETSAT Archive native (.nat)
       RST: Idrisi Raster A.1
      GSAG: Golden Software ASCII Grid (.grd)
      GSBG: Golden Software Binary Grid (.grd)
     GS7BG: Golden Software 7 Binary Grid (.grd)
     COSAR: COSAR Annotated Binary Matrix (TerraSAR-X)
       TSX: TerraSAR-X Product
     COASP: DRDC COASP SAR Processor Raster
         R: R Object Data Store
       MAP: OziExplorer .MAP
KMLSUPEROVERLAY: Kml Super Overlay
      WEBP: WEBP
       PDF: Geospatial PDF
Rasterlite: Rasterlite
   MBTiles: MBTiles
  PLMOSAIC: Planet Labs Mosaics API
      CALS: CALS (Type 1)
      WMTS: OGC Web Map Tile Service
 SENTINEL2: Sentinel 2
       MRF: Meta Raster Format
       PNM: Portable Pixmap Format (netpbm)
      DOQ1: USGS DOQ (Old Style)
      DOQ2: USGS DOQ (New Style)
      PAux: PCI .aux Labelled
       MFF: Vexcel MFF Raster
      MFF2: Vexcel MFF2 (HKV) Raster
       GSC: GSC Geogrid
      FAST: EOSAT FAST Format
        BT: VTP .bt (Binary Terrain) 1.3 Format
       LAN: Erdas .LAN/.GIS
       CPG: Convair PolGASP
       NDF: NLAPS Data Format
       EIR: Erdas Imagine Raw
     DIPEx: DIPEx
       LCP: FARSITE v.4 Landscape File (.lcp)
       GTX: NOAA Vertical Datum .GTX
    LOSLAS: NADCON .los/.las Datum Grid Shift
      NTv2: NTv2 Datum Grid Shift
   CTable2: CTable2 Datum Grid Shift
      ACE2: ACE2
    SNODAS: Snow Data Assimilation System
       KRO: KOLOR Raw
   ROI_PAC: ROI_PAC raster
   RRASTER: R Raster
       BYN: Natural Resources Canada's Geoid
       ARG: Azavea Raster Grid format
       RIK: Swedish Grid RIK (.rik)
   USGSDEM: USGS Optional ASCII DEM (and CDED)
       GXF: GeoSoft Grid Exchange Format
       BAG: Bathymetry Attributed Grid
      HDF5: Hierarchical Data Format Release 5
 HDF5Image: HDF5 Dataset
   NWT_GRD: Northwood Numeric Grid Format .grd/.tab
   NWT_GRC: Northwood Classified Grid Format .grc/.tab
      ADRG: ARC Digitized Raster Graphics
       SRP: Standard Raster Product (ASRP/USRP)
       BLX: Magellan topo (.blx)
PostGISRaster: PostGIS Raster driver
      SAGA: SAGA GIS Binary Grid (.sdat, .sg-grd-z)
       XYZ: ASCII Gridded XYZ
       HF2: HF2/HFZ heightfield raster
       OZI: OziExplorer Image File
       CTG: USGS LULC Composite Theme Grid
      ZMap: ZMap Plus Grid
  NGSGEOID: NOAA NGS Geoid Height Grids
      IRIS: IRIS data (.PPI, .CAPPi etc)
       PRF: Racurs PHOTOMOD PRF
     EEDAI: Earth Engine Data API Image
      EEDA: Earth Engine Data API
      DAAS: Airbus DS Intelligence Data As A Service driver
    SIGDEM: Scaled Integer Gridded DEM .sigdem
      HEIF: ISO/IEC 23008-12:2017 High Efficiency Image File Format
       TGA: TGA/TARGA Image File Format
    OGCAPI: OGCAPI
    STACTA: Spatio-Temporal Asset Catalog Tiled Assets
    STACIT: Spatio-Temporal Asset Catalog Items
   GNMFile: Geographic Network generic file based model
GNMDatabase: Geographic Network generic DB based model
ESRI Shapefile: ESRI Shapefile
MapInfo File: MapInfo File
   UK .NTF: UK .NTF
     LVBAG: Kadaster LV BAG Extract 2.0
  OGR_SDTS: SDTS
       S57: IHO S-57 (ENC)
       DGN: Microstation DGN
   OGR_VRT: VRT - Virtual Datasource
    Memory: Memory
       CSV: Comma Separated Value (.csv)
       NAS: NAS - ALKIS
       GML: Geography Markup Language (GML)
       GPX: GPX
    LIBKML: Keyhole Markup Language (LIBKML)
       KML: Keyhole Markup Language (KML)
   GeoJSON: GeoJSON
GeoJSONSeq: GeoJSON Sequence
  ESRIJSON: ESRIJSON
  TopoJSON: TopoJSON
Interlis 1: Interlis 1
Interlis 2: Interlis 2
   OGR_GMT: GMT ASCII Vectors (.gmt)
      GPKG: GeoPackage
    SQLite: SQLite / Spatialite
      ODBC:
      WAsP: WAsP .map format
      PGeo: ESRI Personal GeoDatabase
MSSQLSpatial: Microsoft SQL Server Spatial Database
  OGR_OGDI: OGDI Vectors (VPF, VMAP, DCW)
PostgreSQL: PostgreSQL/PostGIS
     MySQL: MySQL
OpenFileGDB: ESRI FileGDB
       DXF: AutoCAD DXF
       CAD: AutoCAD Driver
FlatGeobuf: FlatGeobuf
Geoconcept: Geoconcept
    GeoRSS: GeoRSS
       VFK: Czech Cadastral Exchange Data Format
    PGDUMP: PostgreSQL SQL dump
       OSM: OpenStreetMap XML and PBF
  GPSBabel: GPSBabel
   OGR_PDS: Planetary Data Systems TABLE
       WFS: OGC WFS (Web Feature Service)
     OAPIF: OGC API - Features
      SOSI: Norwegian SOSI Standard
    EDIGEO: French EDIGEO exchange format
       SVG: Scalable Vector Graphics
    Idrisi: Idrisi Vector (.vct)
       XLS: MS Excel format
       ODS: Open Document/ LibreOffice / OpenOffice Spreadsheet
      XLSX: MS Office Open XML spreadsheet
Elasticsearch: Elastic Search
     Carto: Carto
AmigoCloud: AmigoCloud
       SXF: Storage and eXchange Format
   Selafin: Selafin
       JML: OpenJUMP JML
  PLSCENES: Planet Labs Scenes API
       CSW: OGC CSW (Catalog  Service for the Web)
       VDV: VDV-451/VDV-452/INTREST Data Format
     GMLAS: Geography Markup Language (GML) driven by application schemas
       MVT: Mapbox Vector Tiles
       NGW: NextGIS Web
     MapML: MapML
     TIGER: U.S. Census TIGER/Line
    AVCBin: Arc/Info Binary Coverage
    AVCE00: Arc/Info E00 (ASCII) Coverage
    GenBin: Generic Binary (.hdr Labelled)
      ENVI: ENVI .hdr Labelled
      EHdr: ESRI .hdr Labelled
      ISCE: ISCE raster
      Zarr: Zarr
      HTTP: HTTP Fetching Wrapper
```

上面第4行，直接使用了索引值来获得驱动，而在第5行则打印了驱动的名称。注意到驱动有ShortName与LongName。ShortName与栅格数据格式在GDAL中定义的编码是一致的，而LongName则可以看成是描述性的文字。 对于不同的Linux发行版，以及安装的GDAL的版本与编译选项的不同，上面程序的结果是不一样的。所以一般情况下要避免使用gdal.GetDriver()这个函数来获取驱动。 我使用的系统是Debian Squeeze，返回的驱动的个数是88。

```
>>> from osgeo import ogr
>>> drv_count = ogr.GetDriverCount()
>>> drv_count
>>>
>>> for idx in range(drv_count):
>>>     driver = gdal.GetDriver(idx)
>>>     print( "%10s: %s" % (driver.ShortName, driver.LongName))
       VRT: Virtual Raster
   DERIVED: Derived datasets using VRT pixel functions
     GTiff: GeoTIFF
       COG: Cloud optimized GeoTIFF generator
      NITF: National Imagery Transmission Format
    RPFTOC: Raster Product Format TOC format
   ECRGTOC: ECRG TOC format
       HFA: Erdas Imagine Images (.img)
  SAR_CEOS: CEOS SAR Image
      CEOS: CEOS Image
JAXAPALSAR: JAXA PALSAR Product Reader (Level 1.1/1.5)
       GFF: Ground-based SAR Applications Testbed File Format (.gff)
      ELAS: ELAS
     ESRIC: Esri Compact Cache
       AIG: Arc/Info Binary Grid
   AAIGrid: Arc/Info ASCII Grid
GRASSASCIIGrid: GRASS ASCII Grid
       ISG: International Service for the Geoid
      SDTS: SDTS Raster
      DTED: DTED Elevation Raster
       PNG: Portable Network Graphics
      JPEG: JPEG JFIF
       MEM: In Memory Raster
      JDEM: Japanese DEM (.mem)
       GIF: Graphics Interchange Format (.gif)
    BIGGIF: Graphics Interchange Format (.gif)
      ESAT: Envisat Image Format
      FITS: Flexible Image Transport System
       BSB: Maptech BSB Nautical Charts
       XPM: X11 PixMap Format
       BMP: MS Windows Device Independent Bitmap
     DIMAP: SPOT DIMAP
    AirSAR: AirSAR Polarimetric Image
       RS2: RadarSat 2 XML Product
      SAFE: Sentinel-1 SAR SAFE Product
    PCIDSK: PCIDSK Database File
  PCRaster: PCRaster Raster File
     ILWIS: ILWIS Raster Map
       SGI: SGI Image File Format 1.0
   SRTMHGT: SRTMHGT File Format
  Leveller: Leveller heightfield
  Terragen: Terragen heightfield
    netCDF: Network Common Data Format
      HDF4: Hierarchical Data Format Release 4
 HDF4Image: HDF4 Dataset
     ISIS3: USGS Astrogeology ISIS cube (Version 3)
     ISIS2: USGS Astrogeology ISIS cube (Version 2)
       PDS: NASA Planetary Data System
      PDS4: NASA Planetary Data System 4
     VICAR: MIPL VICAR file
       TIL: EarthWatch .TIL
       ERS: ERMapper .ers Labelled
JP2OpenJPEG: JPEG-2000 driver based on OpenJPEG library
       L1B: NOAA Polar Orbiter Level 1b Data Set
       FIT: FIT Image
      GRIB: GRIdded Binary (.grb, .grb2)
       RMF: Raster Matrix Format
       WCS: OGC Web Coverage Service
       WMS: OGC Web Map Service
      MSGN: EUMETSAT Archive native (.nat)
       RST: Idrisi Raster A.1
      GSAG: Golden Software ASCII Grid (.grd)
      GSBG: Golden Software Binary Grid (.grd)
     GS7BG: Golden Software 7 Binary Grid (.grd)
     COSAR: COSAR Annotated Binary Matrix (TerraSAR-X)
       TSX: TerraSAR-X Product
     COASP: DRDC COASP SAR Processor Raster
         R: R Object Data Store
       MAP: OziExplorer .MAP
KMLSUPEROVERLAY: Kml Super Overlay
      WEBP: WEBP
       PDF: Geospatial PDF
Rasterlite: Rasterlite
   MBTiles: MBTiles
  PLMOSAIC: Planet Labs Mosaics API
      CALS: CALS (Type 1)
      WMTS: OGC Web Map Tile Service
 SENTINEL2: Sentinel 2
       MRF: Meta Raster Format
```
