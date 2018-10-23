# logstash docker service 布署(待完善)  

*启动命令*
<pre><code>
$ docker run -dit -p 4560:4560 --rm logstash -e 'input { tcp {port => 4560 codec => json_lines}}output {elasticsearch {id => "service_id" index => "service_id" hosts => ["host:9200"]}}'
</code></pre>