# SOLR 6.x Prometheus Exporter

Using SOLR 7.3.1's exporter files with fixed `solr-exporter-config.xml`.

## How to use

1. Copy files inside `contrib` folder into `/opt/solr/contrib` (your directory may different)
2. Exporter jar file inside `dist` folder into `/opt/solr/dist` (your directory may different)
3. Then running solr exporter using these reference :
    * [Solr Reference Guide's section on Monitoring Solr with Prometheus and Grafana](https://lucene.apache.org/solr/guide/monitoring-solr-with-prometheus-and-grafana.html)


## Whats fixed

### 1. solr_metrics_jvm_gc_seconds_total
```
@@ -141,7 +141,7 @@
           <str>
             .metrics["solr.jvm"] | to_entries | .[] | select(.key | startswith("gc.")) | select(.key | endswith(".time")) as $object |
             $object.key | split(".")[1] as $item |
-            ($object.value / 1000) as $value |
+            ($object.value.value / 1000) as $value |
             {
               name         : "solr_metrics_jvm_gc_seconds_total",
               type         : "COUNTER",
```

### 2. solr_metrics_node_time_seconds_total
```
@@ -354,7 +354,7 @@
             .metrics["solr.node"] | to_entries | .[] | select(.key | endswith(".totalTime")) as $object |
             $object.key | split(".")[0] as $category |
             $object.key | split(".")[1] as $handler |
-            ($object.value / 1000) as $value |
+            ($object.value.count / 1000) as $value |
             {
               name         : "solr_metrics_node_time_seconds_total",
               type         : "COUNTER",
```

### 3. solr_metrics_core_time_seconds_total
```
@@ -649,7 +649,7 @@
             $object.key | split(".")[0] as $category |
             $object.key | split(".")[1] as $handler |
             select($handler | startswith("/")) |
-            ($object.value / 1000) as $value |
+            ($object.value.count / 1000) as $value |
             if $parent_key_item_len == 3 then
             {
               name: "solr_metrics_core_time_seconds_total",
```