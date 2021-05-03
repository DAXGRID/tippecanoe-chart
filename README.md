# Tippecanoe helm chart

Helm 3 chart for [Tippecanoe](https://github.com/mapbox/tippecanoe).

## Install/Upgrade

First add the repo
```sh
helm repo add dax https://daxgrid.github.io/charts/
helm repo update
```

```sh
helm upgrade --install my-release-name tippecanoe \
     --set schedule="*0 0 * * *" \
     --set commandsArgs={"--output=/data/output.mbtiles /data/example.geojson"}
```

## Parameters
Parameters for the helm chart.

| Parameter          | Description                  | Default                                                   |
|--------------------|------------------------------|-----------------------------------------------------------|
| `image.repository` | The image repository         | `openftth/tippecanoe`                                     |
| `image.tag`        | The image version tag        | `v1.36.0`                                                 |
| `schedule`         | How often the job should run | `0 0 * * *` (once a day at midnight)                      |
| `commandArgs`      | Tippecanoe command arguments | `- "--output=/data/output.mbtiles /data/example.geojson"` |
| `restartPolicy`    | Restartpolcy                 | `Never`                                                   |
