# Hybrid-CPU-HD-Fan-Zone-Controller-To-Telegraf

Basis telegraf logparser using grok for https://www.ixsystems.com/community/threads/script-hybrid-cpu-hd-fan-zone-controller.46159/

influx logparser
```
[[inputs.logparser]]
  files = ["/mnt/vol01/system/Fancontrol-scripts/fan_control.log"]
  from_beginning = false
  ## Method used to watch for file updates.  Can be either "inotify" or "poll".
  watch_method = "poll"
  ## Parse logstash-style "grok" patterns:
  [inputs.logparser.grok]
    patterns = ["%{TS_CUSTOM}: %{DATA:Location:tag}: %{INT:Metrics:int}"]
    ## Name of the outputted measurement name.
    measurement = "fan_control_log_3"
    ## Full path(s) to custom pattern files.
    custom_pattern_files = []
    ## Custom patterns can also be defined here. Put one pattern per line.
    custom_patterns = '''
    TS_CUSTOM %{YEAR}-%{MONTHNUM}-%{MONTHDAY} %{HOUR}:%{MINUTE}:%{SECOND}
    '''
    ## Timezone allows you to provide an override for timestamps that
    ## don't already include an offset
    ## e.g. 04/06/2016 12:41:45 data one two 5.43µs
    ##
    ## Default: "" which renders UTC
    ## Options are as follows:
    ##   1. Local             -- interpret based on machine localtime
    ##   2. "Canada/Eastern"  -- Unix TZ values like those found in https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
    ##   3. UTC               -- or blank/unspecified, will return timestamp in UTC
    timezone = "Europe/Amsterdam"
```

grafana dashboard json
```
{
  "annotations": {
    "list": [
      {
        "builtIn": 1,
        "datasource": "-- Grafana --",
        "enable": true,
        "hide": true,
        "iconColor": "rgba(0, 211, 255, 1)",
        "name": "Annotations & Alerts",
        "type": "dashboard"
      }
    ]
  },
  "editable": true,
  "gnetId": null,
  "graphTooltip": 0,
  "id": 28,
  "links": [],
  "panels": [
    {
      "aliasColors": {},
      "bars": false,
      "dashLength": 10,
      "dashes": false,
      "datasource": null,
      "decimals": 0,
      "fill": 1,
      "gridPos": {
        "h": 9,
        "w": 12,
        "x": 0,
        "y": 0
      },
      "id": 4,
      "legend": {
        "alignAsTable": true,
        "avg": false,
        "current": true,
        "max": true,
        "min": true,
        "rightSide": true,
        "show": true,
        "total": false,
        "values": true
      },
      "lines": true,
      "linewidth": 1,
      "links": [],
      "nullPointMode": "null",
      "percentage": false,
      "pointradius": 5,
      "points": false,
      "renderer": "flot",
      "seriesOverrides": [],
      "spaceLength": 10,
      "stack": false,
      "steppedLine": false,
      "targets": [
        {
          "alias": "$tag_Location",
          "groupBy": [
            {
              "params": [
                "$__interval"
              ],
              "type": "time"
            },
            {
              "params": [
                "Location"
              ],
              "type": "tag"
            },
            {
              "params": [
                "linear"
              ],
              "type": "fill"
            }
          ],
          "measurement": "fan_control_log_3",
          "orderByTime": "ASC",
          "policy": "default",
          "refId": "A",
          "resultFormat": "time_series",
          "select": [
            [
              {
                "params": [
                  "Metrics"
                ],
                "type": "field"
              },
              {
                "params": [],
                "type": "mean"
              }
            ]
          ],
          "tags": [
            {
              "key": "Location",
              "operator": "=",
              "value": "CPU Temp"
            }
          ]
        }
      ],
      "thresholds": [],
      "timeFrom": null,
      "timeShift": null,
      "title": "CPU Tempature",
      "tooltip": {
        "shared": true,
        "sort": 0,
        "value_type": "individual"
      },
      "type": "graph",
      "xaxis": {
        "buckets": null,
        "mode": "time",
        "name": null,
        "show": true,
        "values": []
      },
      "yaxes": [
        {
          "format": "celsius",
          "label": null,
          "logBase": 2,
          "max": null,
          "min": null,
          "show": true
        },
        {
          "format": "short",
          "label": null,
          "logBase": 1,
          "max": null,
          "min": null,
          "show": true
        }
      ],
      "yaxis": {
        "align": false,
        "alignLevel": null
      }
    },
    {
      "aliasColors": {},
      "bars": false,
      "dashLength": 10,
      "dashes": false,
      "datasource": null,
      "decimals": 0,
      "fill": 1,
      "gridPos": {
        "h": 9,
        "w": 12,
        "x": 12,
        "y": 0
      },
      "id": 5,
      "legend": {
        "alignAsTable": true,
        "avg": false,
        "current": true,
        "max": true,
        "min": true,
        "rightSide": true,
        "show": true,
        "total": false,
        "values": true
      },
      "lines": true,
      "linewidth": 1,
      "links": [],
      "nullPointMode": "null",
      "percentage": false,
      "pointradius": 5,
      "points": false,
      "renderer": "flot",
      "seriesOverrides": [],
      "spaceLength": 10,
      "stack": false,
      "steppedLine": false,
      "targets": [
        {
          "$$hashKey": "object:828",
          "alias": "$tag_Location",
          "groupBy": [
            {
              "params": [
                "$__interval"
              ],
              "type": "time"
            },
            {
              "params": [
                "Location"
              ],
              "type": "tag"
            },
            {
              "params": [
                "linear"
              ],
              "type": "fill"
            }
          ],
          "measurement": "fan_control_log_3",
          "orderByTime": "ASC",
          "policy": "default",
          "refId": "A",
          "resultFormat": "time_series",
          "select": [
            [
              {
                "params": [
                  "Metrics"
                ],
                "type": "field"
              },
              {
                "params": [],
                "type": "mean"
              }
            ]
          ],
          "tags": [
            {
              "key": "Location",
              "operator": "=",
              "value": "CPU Fan speed"
            },
            {
              "condition": "OR",
              "key": "Location",
              "operator": "=",
              "value": "HD Fan speed"
            }
          ]
        }
      ],
      "thresholds": [],
      "timeFrom": null,
      "timeShift": null,
      "title": "Fanspeed",
      "tooltip": {
        "shared": true,
        "sort": 0,
        "value_type": "individual"
      },
      "type": "graph",
      "xaxis": {
        "buckets": null,
        "mode": "time",
        "name": null,
        "show": true,
        "values": []
      },
      "yaxes": [
        {
          "$$hashKey": "object:891",
          "format": "rpm",
          "label": null,
          "logBase": 1,
          "max": null,
          "min": null,
          "show": true
        },
        {
          "$$hashKey": "object:892",
          "format": "short",
          "label": null,
          "logBase": 1,
          "max": null,
          "min": null,
          "show": true
        }
      ],
      "yaxis": {
        "align": false,
        "alignLevel": null
      }
    },
    {
      "aliasColors": {},
      "bars": false,
      "dashLength": 10,
      "dashes": false,
      "datasource": null,
      "decimals": 0,
      "fill": 1,
      "gridPos": {
        "h": 9,
        "w": 12,
        "x": 0,
        "y": 9
      },
      "id": 6,
      "legend": {
        "alignAsTable": true,
        "avg": false,
        "current": true,
        "max": true,
        "min": true,
        "rightSide": true,
        "show": true,
        "total": false,
        "values": true
      },
      "lines": true,
      "linewidth": 1,
      "links": [],
      "nullPointMode": "null",
      "percentage": false,
      "pointradius": 5,
      "points": false,
      "renderer": "flot",
      "seriesOverrides": [],
      "spaceLength": 10,
      "stack": false,
      "steppedLine": false,
      "targets": [
        {
          "alias": "$tag_Location",
          "groupBy": [
            {
              "params": [
                "$__interval"
              ],
              "type": "time"
            },
            {
              "params": [
                "Location"
              ],
              "type": "tag"
            },
            {
              "params": [
                "linear"
              ],
              "type": "fill"
            }
          ],
          "measurement": "fan_control_log_3",
          "orderByTime": "ASC",
          "policy": "default",
          "refId": "A",
          "resultFormat": "time_series",
          "select": [
            [
              {
                "params": [
                  "Metrics"
                ],
                "type": "field"
              },
              {
                "params": [],
                "type": "mean"
              }
            ]
          ],
          "tags": [
            {
              "key": "Location",
              "operator": "=",
              "value": "/dev/ada0"
            },
            {
              "condition": "OR",
              "key": "Location",
              "operator": "=",
              "value": "/dev/ada2"
            },
            {
              "condition": "OR",
              "key": "Location",
              "operator": "=",
              "value": "/dev/ada3"
            },
            {
              "condition": "OR",
              "key": "Location",
              "operator": "=",
              "value": "/dev/ada4"
            },
            {
              "condition": "OR",
              "key": "Location",
              "operator": "=",
              "value": "/dev/ada5"
            },
            {
              "condition": "OR",
              "key": "Location",
              "operator": "=",
              "value": "/dev/ada1"
            }
          ]
        }
      ],
      "thresholds": [],
      "timeFrom": null,
      "timeShift": null,
      "title": "HDD Tempature",
      "tooltip": {
        "shared": true,
        "sort": 0,
        "value_type": "individual"
      },
      "type": "graph",
      "xaxis": {
        "buckets": null,
        "mode": "time",
        "name": null,
        "show": true,
        "values": []
      },
      "yaxes": [
        {
          "format": "celsius",
          "label": null,
          "logBase": 1,
          "max": null,
          "min": null,
          "show": true
        },
        {
          "format": "short",
          "label": null,
          "logBase": 1,
          "max": null,
          "min": null,
          "show": true
        }
      ],
      "yaxis": {
        "align": false,
        "alignLevel": null
      }
    }
  ],
  "refresh": "5s",
  "schemaVersion": 16,
  "style": "dark",
  "tags": [],
  "templating": {
    "list": []
  },
  "time": {
    "from": "now-1h",
    "to": "now"
  },
  "timepicker": {
    "refresh_intervals": [
      "5s",
      "10s",
      "30s",
      "1m",
      "5m",
      "15m",
      "30m",
      "1h",
      "2h",
      "1d"
    ],
    "time_options": [
      "5m",
      "15m",
      "1h",
      "6h",
      "12h",
      "24h",
      "2d",
      "7d",
      "30d"
    ]
  },
  "timezone": "",
  "title": "Hardware Metrics",
  "uid": "8__yhIvZk",
  "version": 9
}
```
