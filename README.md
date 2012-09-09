os_opendata_streetview_mapserver
================================

Mapserver config for Ordnance Survey OpenData Street View

Basic Setup Guide:
1: Download OS Street View: https://www.ordnancesurvey.co.uk/opendatadownload/products.html
2: Unzip to shared folder (including *.tfw)
3: (optional) Recompress/Retile TIFs for speed/size: for f in `ls *.tif`; do GDAL_CACHEMAX=2000 gdal_translate -of GTiff -co TILED=YES -co COMPRESS=DEFLATE -co ZLEVEL=9 -co BLOCKXSIZE=512 -co BLOCKYSIZE=512 -co BIGTIFF=NO -a_srs epsg:27700 $f processed/$f; done
4: gdalbuildvrt -resolution highest -hidenodata -vrtnodata "209" ossv-combined.vrt *.tif
5: gdaladdo --config GDAL_CACHEMAX=1000 -ro --config COMPRESS DEFLATE --config PREDICTOR 2 --config COMPRESS_OVERVIEW DEFLATE --config ZLEVEL 9 --config BIGTIFF_OVERVIEW YES --config GDAL_TIFF_OVR_BLOCKSIZE 512 -r average ossv-combined.vrt 4 16 64 256 1024 4096 16384
