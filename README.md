# Tippecanoe helm chart
Helm 3 chart for [Tippecanoe](https://github.com/mapbox/tippecanoe). The chart setups a Kubernetes `CronJob` where you can specify how often it should run and what arguments Tippecanoe should be called with. The Helm chart also allows you to run a pre-job using [GDAL](https://github.com/OSGeo/gdal) this can be enabled using the Chart configurations.
## Install/Upgrade
First add the repo
```sh
helm repo add dax https://daxgrid.github.io/charts/
helm repo update
```

Example of usage.
```sh
helm upgrade --install my-release-name tippecanoe \
     --set schedule="*/5 * * * *" \
     --set 'commandArgs={tippecanoe, -zg, -o, /data/out.mbtiles, --drop-densest-as-needed, /data/output.geojson, --force}' \
     --set storage.enabled=true \
```

Example of using with a GDAL job before the Tippecanoe job.
```sh
helm upgrade --install my-release-name tippecanoe -n openftth \
     --set schedule="*/5 * * * *" \
     --set 'commandArgs={tippecanoe, -zg, -o, /data/out.mbtiles, --drop-densest-as-needed, /data/output.geojson, --force}' \
     --set storage.enabled=true \
     --set gdal.enabled=true \
     --set 'gdal.commandArgs={ogr2ogr, -f, GeoJSON, /data/output.geojson, PG:host=localhost dbname=MY_DB user=postgres password=mypassword, -sql, select id, ST_Transform(wkb_geometry\, 4326) as wkb_geometry from my_table}'
```
## Parameters
Parameters for the helm chart.
### Tippecanoe
| Parameter          | Description                  | Default                              |
|--------------------|------------------------------|--------------------------------------|
| `image.repository` | The image repository         | `openftth/tippecanoe`                |
| `image.tag`        | The image version tag        | `v1.36.0`                            |
| `schedule`         | How often the job should run | `0 0 * * *` (once a day at midnight) |
| `commandArgs`      | Tippecanoe command arguments | `- "tippecanoe"`                     |
| `restartPolicy`    | Restartpolicy                | `Never`                              |
| `backOffLimit`     | BackoffLimit                 | `0`                                  |

#### GDAL
You also have option to enable GDAL. This is a job that runs before the Tippecanoe cronjob. This can for-example be used to generate `geojson` to be used by Tippecanoe.
| Parameter               | Description             | Default              |
|-------------------------|-------------------------|----------------------|
| `gdal.enabled`          | GDAL enabled            | `false`              |
| `gdal.commandArgs`      | GDAL commands arguments | `- "ogr2ogr"`        |
| `gdal.image.repository` | GDAL image repository   | `osgeo/gdal`         |
| `gdal.image.tag`        | GDAL image tag          | `alpine-small-3.2.2` |
