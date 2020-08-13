# go-stash

go-stash is a free and open server-side data processing pipeline that ingests data from Kafka, transforms it, and then sends it to ElasticSearch.

## Quick Start

```shell
gostash -f etc/config.json
```

config.json looks like:

```json
{
    "Input": {
        "Kafka": {
            "Name": "gostash",
            "Brokers": [
                "172.16.186.16:19092",
                "172.16.186.17:19092"
            ],
            "Topic": "k8slog",
            "Group": "pro",
            "NumProducers": 16,
            "MetricsUrl": "http://localhost:2222/add"
        }
    },
    "Filters": [
        {
            "Action": "drop",
            "Conditions": [
                {
                    "Key": "k8s_container_name",
                    "Value": "-rpc",
                    "Type": "contains"
                },
                {
                    "Key": "level",
                    "Value": "info",
                    "Type": "match",
                    "Op": "and"
                }
            ]
        },
        {
            "Action": "remove_field",
            "Fields": [
                "message",
                "_source",
                "_type",
                "_score",
                "_id",
                "@version",
                "topic",
                "index",
                "beat",
                "docker_container",
                "offset",
                "prospector",
                "source",
                "stream"
            ]
        }
    ],
    "Output": {
        "ElasticSearch": {
            "Hosts": [
                "172.16.141.4:9200",
                "172.16.141.5:9200"
            ],
            "DailyIndexPrefix": "k8s_pro-"
        }
    }
}
```

### 微信交流群

添加我的微信：kevwan，请注明go-stash，我拉进go-stash社区群🤝