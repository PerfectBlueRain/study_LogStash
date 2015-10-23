* 서버의 상태 정보를 제공하는 configure 파일
input {
  exec {
    command => "free | grep buffers/cache | awk '{print int($3/($3+$4)*100)}'"
    interval => "5"
    type => "mem"
  }
  exec {
    command => "cat /proc/stat | grep 'cpu ' | awk '{print int(($2+$3+$4)/($2+$3+$4+$5)*100)}'"
    interval => "5"
    type => "cpu"
  }
  exec {
    command => "df -k | grep /was | awk '{print ($5*1)}'"
    interval => "5"
    type => "hdd"
  }
}
filter {
  mutate {
    convert => ["message", "integer"]
  }
}
output {
  elasticsearch{
        cluster => "kibana_cluster"
        node_name => "kibana_node"
        protocol => "node"
        host => "[elasticsearch가 설치된 ip 주소]"
        index => "server-status-%{+YYYY.MM.dd}"
  }
}
