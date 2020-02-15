## Prometheus disk full

## Prometheus retention size

## Using long term storage
Remember prometheus is not intended for long term storage and remember the default retention period is **'15d'**. The retention policy is slightly lenient if the disk is not full, so you may have some data even after the retention period has expired, but read the documentation again. Also you could use the metric about the `retention` on the prometheus query UI.

Look for remote storage integration, and see how sort of requirements you have on the remote storage, like do you need write only or read-write integrationa and choose accordingly. I choose influxdb integration as we needed both read and write support.

Tweak your prometheus.yml settings for the remote storage, so that you send only the metrics that you need for LTS to the remote otherwise you will end up filling it up quickly. Use the label matchers wisely to filter out unecessary metrics.

## Largest metrics
```
 topk(10, count by (__name__)({__name__=~".+"}))
```

## Delete wrong series
