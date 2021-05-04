# Tippecanoe helm chart

Helm 3 chart for [Tippecanoe](https://github.com/mapbox/tippecanoe). The chart setups a Kubernetes `CronJob` where you can specify how often it should run and what arguments Tippecanoe should be called with. The Helm chart also allows you to run a pre-job using [GDAL](https://github.com/OSGeo/gdal) this can be enabled using the Chart configurations.

## Install/Upgrade

First add the repo
```sh
helm repo add dax https://daxgrid.github.io/charts/
helm repo update
```
