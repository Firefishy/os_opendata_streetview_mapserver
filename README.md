os_opendata_streetview_mapserver
================================

Mapserver config for Ordnance Survey OpenData Street View

# Basic Setup Guide
* Download the OS Street View file to a single directory: https://www.ordnancesurvey.co.uk/opendatadownload/products.html
* `./osstvw_process SOURCE_DIR DESTINATION_DIR`
* Create combined gdal vrt:
	`gdalbuildvrt -resolution highest -hidenodata -vrtnodata "209" ossv-combined.vrt DESTINATION_DIR/*.tif`
* Create overview to speed up zoomed out:
	`gdaladdo -ro --config COMPRESS DEFLATE --config COMPRESS_OVERVIEW DEFLATE --config ZLEVEL 9 --config BIGTIFF_OVERVIEW IF_SAFER --config GDAL_TIFF_OVR_BLOCKSIZE 512 -r average ossv-combined.vrt 4 16 64 256 1024 4096 16384`

# Chef
Chef code for setting up mapserver with nginx is available here: https://github.com/openstreetmap/chef/tree/master/cookbooks/imagery
