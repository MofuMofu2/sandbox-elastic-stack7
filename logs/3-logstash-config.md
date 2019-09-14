# Logastashのconfig記載方法を試す

何もないプラグインで起動

```conf
# 標準入力を受け付ける
input {
  stdin { }
}
# 標準出力を行う
output {
  stdout { codec => rubydebug }
}
```

出力結果

```bash
mofumofu at mofunoMac-mini in ~/elastic/logstash-7.3.0
$ bin/logstash -f config/first-pipeline.conf
OpenJDK 64-Bit Server VM warning: Option UseConcMarkSweepGC was deprecated in version 9.0 and will likely be removed in a future release.
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by com.headius.backport9.modules.Modules (file:/Users/mofumofu/elastic/logstash-7.3.0/logstash-core/lib/jars/jruby-complete-9.2.7.0.jar) to field java.io.FileDescriptor.fd
WARNING: Please consider reporting this to the maintainers of com.headius.backport9.modules.Modules
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
Thread.exclusive is deprecated, use Thread::Mutex
Sending Logstash logs to /Users/mofumofu/elastic/logstash-7.3.0/logs which is now configured via log4j2.properties
[2019-09-14T11:29:06,705][WARN ][logstash.config.source.multilocal] Ignoring the 'pipelines.yml' file because modules or command line options are specified
[2019-09-14T11:29:06,719][INFO ][logstash.runner          ] Starting Logstash {"logstash.version"=>"7.3.0"}
[2019-09-14T11:29:07,301][INFO ][org.reflections.Reflections] Reflections took 27 ms to scan 1 urls, producing 19 keys and 39 values
[2019-09-14T11:29:07,575][WARN ][org.logstash.instrument.metrics.gauge.LazyDelegatingGauge] A gauge metric of an unknown type (org.jruby.RubyArray) has been create for key: cluster_uuids. This may result in invalid serialization.  It is recommended to log an issue to the responsible developer/development team.
[2019-09-14T11:29:07,577][INFO ][logstash.javapipeline    ] Starting pipeline {:pipeline_id=>"main", "pipeline.workers"=>12, "pipeline.batch.size"=>125, "pipeline.batch.delay"=>50, "pipeline.max_inflight"=>1500, :thread=>"#<Thread:0x4d92d4a2 run>"}
[2019-09-14T11:29:07,618][INFO ][logstash.javapipeline    ] Pipeline started {"pipeline.id"=>"main"}
The stdin plugin is now waiting for input:
[2019-09-14T11:29:07,653][INFO ][logstash.agent           ] Pipelines running {:count=>1, :running_pipelines=>[:main], :non_running_pipelines=>[]}
[2019-09-14T11:29:07,775][INFO ][logstash.agent           ] Successfully started Logstash API endpoint {:port=>9600}
# 標準入力してみる
aaa
/Users/mofumofu/elastic/logstash-7.3.0/vendor/bundle/jruby/2.5.0/gems/awesome_print-1.7.0/lib/awesome_print/formatters/base_formatter.rb:31: warning: constant ::Fixnum is deprecated
{
      "@version" => "1",
       "message" => "aaa",
          "host" => "mofunoMac-mini.local",
    "@timestamp" => 2019-09-14T02:31:35.228Z
}
test
{
      "@version" => "1",
       "message" => "test",
          "host" => "mofunoMac-mini.local",
    "@timestamp" => 2019-09-14T02:31:39.375Z
}

# Ctl + Cで停止
^C[2019-09-14T11:51:34,353][WARN ][logstash.runner          ] SIGINT received. Shutting down.
[2019-09-14T11:51:34,500][INFO ][logstash.javapipeline    ] Pipeline terminated {"pipeline.id"=>"main"}
[2019-09-14T11:51:34,517][INFO ][logstash.runner          ] Logstash shut down.
```

標準入力したものと同じ結果が``message``欄に入力されて返ってくる

## json形式にする

```conf
# 標準入力を受け付ける
input {
  stdin { }
}
# 標準出力を行う
output {
  stdout { codec => json }
}
```

出力結果

```bash
mofumofu at mofunoMac-mini in ~/elastic/logstash-7.3.0
$ bin/logstash -f config/first-pipeline.conf
OpenJDK 64-Bit Server VM warning: Option UseConcMarkSweepGC was deprecated in version 9.0 and will likely be removed in a future release.
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by com.headius.backport9.modules.Modules (file:/Users/mofumofu/elastic/logstash-7.3.0/logstash-core/lib/jars/jruby-complete-9.2.7.0.jar) to field java.io.FileDescriptor.fd
WARNING: Please consider reporting this to the maintainers of com.headius.backport9.modules.Modules
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
Thread.exclusive is deprecated, use Thread::Mutex
Sending Logstash logs to /Users/mofumofu/elastic/logstash-7.3.0/logs which is now configured via log4j2.properties
[2019-09-14T11:56:13,015][WARN ][logstash.config.source.multilocal] Ignoring the 'pipelines.yml' file because modules or command line options are specified
[2019-09-14T11:56:13,022][INFO ][logstash.runner          ] Starting Logstash {"logstash.version"=>"7.3.0"}
[2019-09-14T11:56:13,586][INFO ][org.reflections.Reflections] Reflections took 20 ms to scan 1 urls, producing 19 keys and 39 values
[2019-09-14T11:56:13,810][WARN ][org.logstash.instrument.metrics.gauge.LazyDelegatingGauge] A gauge metric of an unknown type (org.jruby.RubyArray) has been create for key: cluster_uuids. This may result in invalid serialization.  It is recommended to log an issue to the responsible developer/development team.
[2019-09-14T11:56:13,812][INFO ][logstash.javapipeline    ] Starting pipeline {:pipeline_id=>"main", "pipeline.workers"=>12, "pipeline.batch.size"=>125, "pipeline.batch.delay"=>50, "pipeline.max_inflight"=>1500, :thread=>"#<Thread:0x53e6980f run>"}
[2019-09-14T11:56:13,850][INFO ][logstash.javapipeline    ] Pipeline started {"pipeline.id"=>"main"}
The stdin plugin is now waiting for input:
[2019-09-14T11:56:13,883][INFO ][logstash.agent           ] Pipelines running {:count=>1, :running_pipelines=>[:main], :non_running_pipelines=>[]}
[2019-09-14T11:56:14,016][INFO ][logstash.agent           ] Successfully started Logstash API endpoint {:port=>9600}
aaa
{"host":"mofunoMac-mini.local","@version":"1","message":"aaa","@timestamp":"2019-09-14T02:57:11.594Z"}
```

json形式にもできる。本は見やすさ優先でrubydebugを利用する
