os_opendata_streetview_mapserver
================================

Mapserver config for Ordnance Survey OpenData Street View

Basic Setup Guide:
* Download OS Street View: https://www.ordnancesurvey.co.uk/opendatadownload/products.html
* Unzip to shared folder (including *.tfw)
* (optional) Recompress/Retile TIFs for speed/size:
	 ls *.tif | xargs -I '{}' -P8 gdal_translate -of GTiff -co TILED=YES -co COMPRESS=DEFLATE -co ZLEVEL=9 -co BLOCKXSIZE=512 -co BLOCKYSIZE=512 -co BIGTIFF=NO -a_srs epsg:27700 '{}' processed/'{}'
* Create combined vrt:
	gdalbuildvrt -resolution highest -hidenodata -vrtnodata "209" ossv-combined.vrt *.tif
* Create overview to speed up zoomed out:
	gdaladdo --config GDAL_CACHEMAX=1000 -ro --config COMPRESS DEFLATE --config COMPRESS_OVERVIEW DEFLATE --config ZLEVEL 9 --config BIGTIFF_OVERVIEW IF_SAFER --config GDAL_TIFF_OVR_BLOCKSIZE 512 -r average ossv-combined.vrt 4 16 64 256 1024 4096 16384
