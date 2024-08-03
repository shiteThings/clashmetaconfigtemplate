配置文件-仅供参考

```json
{
  "log": {
      "disabled": false,
      "level": "info",
      "timestamp": true
    },

    "dns": {
      "servers": [
        {
          "tag": "proxydns",
          "address": "tls://8.8.8.8/dns-query",
          "detour": "select"
        },
        {
          "tag": "localdns",
          "address": "h3://223.5.5.5/dns-query",
          "detour": "direct"
        },
        {
          "address": "rcode://refused",
          "tag": "block"
        }
      ],
      "rules": [
        {
          "outbound": "any",
          "server": "localdns",
          "disable_cache": true
        },
        {
          "clash_mode": "Global",
          "server": "proxydns"
        },
        {
          "clash_mode": "Direct",
          "server": "localdns"
        },
        {
          "rule_set": "geosite-cn",
          "server": "localdns"
        },
        {
          "rule_set": "geosite-geolocation-!cn",
          "server": "proxydns"
        },
        {
          "rule_set": "geosite-geolocation-!cn",
          "server": "proxydns"
        }
      ],
      "independent_cache": true,
      "final": "proxydns"
    },
    "inbounds": [
      {
        "type": "tun",
        "inet4_address": "172.19.0.1/30",
        "inet6_address": "fd00::1/126",
        "auto_route": true,
        "strict_route": true,
        "sniff": true,
        "sniff_override_destination": true,
        "domain_strategy": "prefer_ipv4"
      }
    ],
    "outbounds": [
      {
        "tag": "select",
        "type": "selector",
        "default": "自动选择",
        "outbounds": [
          "自动选择",
          "节点1",
          "节点2",  
          "节点3"
        ]
      },
      {
        "server": "csgo.com",
        "server_port": 80,
        "tag": "节点1",
        "packet_encoding": "packetaddr",
        "transport": {
          "headers": {
            "Host": [
              "young.amazinglinyy.workers.dev"
            ]
          },
          "path": "/?ed=2560",
          "type": "ws"
        },
        "type": "vless",
        "uuid": "77a571fb-4fd2-4b37-8596-1b7d9728bb5c"
      },
      {
        "server": "www.visa.com.sg",
        "server_port": 8080,
        "tag": "节点2",
        "packet_encoding": "packetaddr",
        "transport": {
          "headers": {
            "Host": [
              "young.amazinglinyy.workers.dev"
            ]
          },
          "path": "/?ed=2560",
          "type": "ws"
        },
        "type": "vless",
        "uuid": "77a571fb-4fd2-4b37-8596-1b7d9728bb5c"
      },
      {
        "server": "198.41.212.167",
        "server_port": 2052,
        "tag": "节点3",
        "packet_encoding": "packetaddr",
        "transport": {
          "headers": {
            "Host": [
              "young.amazinglinyy.workers.dev"
            ]
          },
          "path": "/?ed=2560",
          "type": "ws"
        },
        "type": "vless",
        "uuid": "77a571fb-4fd2-4b37-8596-1b7d9728bb5c"
      },
      {
        "tag": "direct",
        "type": "direct"
      },
      {
        "tag": "block",
        "type": "block"
      },
      {
        "tag": "dns-out",
        "type": "dns"
      },
      {
        "tag": "自动选择",
        "type": "urltest",
        "outbounds": [
          "节点1",
          "节点2",  
          "节点3"
        ],
        "url": "https://www.gstatic.com/generate_204",
        "interval": "5m",
        "tolerance": 50,
        "interrupt_exist_connections": false
      }
    ],
    "route": {
      "rule_set": [
        {
          "tag": "geosite-geolocation-!cn",
          "type": "remote",
          "format": "binary",
          "url": "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/geolocation-!cn.srs",
          "download_detour": "select",
          "update_interval": "1d"
        },
        {
          "tag": "geosite-cn",
          "type": "remote",
          "format": "binary",
          "url": "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geosite/geolocation-cn.srs",
          "download_detour": "select",
          "update_interval": "1d"
        },
        {
          "tag": "geoip-cn",
          "type": "remote",
          "format": "binary",
          "url": "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@sing/geo/geoip/cn.srs",
          "download_detour": "select",
          "update_interval": "1d"
        }
      ],
      "auto_detect_interface": true,
      "final": "select",
      "rules": [
        {
          "outbound": "dns-out",
          "protocol": "dns"
        },
        {
          "clash_mode": "Direct",
          "outbound": "direct"
        },
        {
          "clash_mode": "Global",
          "outbound": "select"
        },
        {
          "rule_set": "geoip-cn",
          "outbound": "direct"
        },
        {
          "rule_set": "geosite-cn",
          "outbound": "direct"
        },
        {
          "ip_is_private": true,
          "outbound": "direct"
        },
        {
          "rule_set": "geosite-geolocation-!cn",
          "outbound": "select"
        }
      ]
    },
    "experimental": {
      "clash_api": {
        "external_controller": "127.0.0.1:9090",
        "external_ui": "ui",
        "external_ui_download_url": "",
        "external_ui_download_detour": "",
        "secret": "",
        "default_mode": "Rule"
      },
      "cache_file": {
        "enabled": true,
        "path": "cache.db"
      }
    }
  }
```

singbox官方文档：https://sing-box.sagernet.org/zh/configuration/

界面ui下载：https://github.com/MetaCubeX/Yacd-meta/archive/gh-pages.zip

singbox下载：https://github.com/SagerNet/sing-box/releases/tag/v1.9.3

mix模式

```json
  {
      "type": "mixed",
      "listen": "::",
      "listen_port": 5353,
      "sniff": true,
      "sniff_override_destination": true,
      "domain_strategy": "prefer_ipv4",
      "set_system_proxy": true
    }
```

