view all indices

```
curl 'localhost:9200/_cat/indices?v'
```

```
# curl 'localhost:9200/_cat/indices?v'
health status index               pri rep docs.count docs.deleted store.size pri.store.size
green  open   logstash-2016.10.30   5   1     513790            0      344mb          172mb
green  open   logstash-2016.10.31   5   1    5173119            0      3.1gb          1.5gb
yellow open   logstash-2016.10.10   5   1    3319640            0      1.6gb            1gb
green  open   logstash-2016.10.22   5   1     510457            0    336.6mb        168.3mb
green  open   logstash-2016.10.23   5   1     513830            0    340.4mb        170.2mb
green  open   logstash-2016.10.24   5   1    4717391            0      2.6gb          1.3gb
green  open   logstash-2016.10.25   5   1    4126262            0      2.8gb          1.4gb
green  open   logstash-2016.10.26   5   1    4752980            0      3.8gb          1.9gb
green  open   .kibana               1   1          4            0     40.2kb         20.1kb
green  open   logstash-2016.10.27   5   1    6175225            0      4.5gb          2.2gb
green  open   logstash-2016.10.28   5   1    3845367            0      2.1gb            1gb
yellow open   logstash-2016.10.07   5   1    1895737            0    642.6mb        459.1mb
green  open   logstash-2016.10.29   5   1     513413            0      344mb          172mb
yellow open   logstash-2016.10.08   5   1     765978            0    578.1mb        321.3mb
yellow open   logstash-2016.10.09   5   1     845999            0    568.5mb        355.5mb
green  open   logstash-2016.10.20   5   1    4453065            0        3gb          1.5gb
green  open   logstash-2016.10.21   5   1    4519648            0      2.6gb          1.3gb
green  open   logstash-2016.11.01   5   1    2433306            0      2.1gb            1gb
yellow open   logstash-2016.10.11   5   1    2778918            0      1.5gb        857.3mb
green  open   logstash-2016.10.12   5   1    3951185            0      2.2gb          1.1gb
green  open   logstash-2016.10.13   5   1    2624792            0      1.7gb        891.9mb
green  open   logstash-2016.10.14   5   1    3385119            0        2gb            1gb
green  open   logstash-2016.10.15   5   1     520055            0    343.1mb        171.5mb
green  open   logstash-2016.10.16   5   1     514427            0    342.2mb        171.1mb
green  open   logstash-2016.10.17   5   1    3258956            0      1.8gb        969.5mb
green  open   logstash-2016.10.18   5   1    3045541            0      1.8gb        930.3mb
green  open   logstash-2016.10.19   5   1    4022162            0      2.5gb          1.2gb
```
