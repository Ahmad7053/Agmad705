{
  "log": {
    "level": "warn",
    "output": "box.log",
    "timestamp": true
  },
  "dns": {
    "servers": [
      {
        "tag": "dns-remote",
        "address": "udp://1.1.1.1",
        "address_resolver": "dns-direct",
        "strategy": "prefer_ipv4"
      },
      {
        "tag": "dns-trick-direct",
        "address": "https://sky.rethinkdns.com/",
        "strategy": "ipv6_only",
        "detour": "direct-fragment"
      },
      {
        "tag": "dns-direct",
        "address": "10.202.10.11",
        "address_resolver": "dns-local",
        "strategy": "ipv6_only",
        "detour": "direct"
      },
      {
        "tag": "dns-local",
        "address": "local",
        "detour": "direct"
      },
      {
        "tag": "dns-block",
        "address": "rcode://success"
      }
    ],
    "rules": [
      {
        "domain_suffix": ".ru",
        "server": "dns-direct"
      },
      {
        "domain": "cp.cloudflare.com",
        "server": "dns-remote",
        "rewrite_ttl": 3000
      }
    ],
    "final": "dns-remote",
    "static_ips": {
      "sky.rethinkdns.com": [
        "188.114.96.3",
        "188.114.97.3",
        "2a06:98c1:3121::6",
        "2a06:98c1:3120::6",
        "104.17.148.22",
        "104.17.147.22",
        "188.114.97.3",
        "188.114.96.3",
        "2a06:98c1:3120::3",
        "2a06:98c1:3121::3"
      ]
    },
    "independent_cache": true
  },
  "inbounds": [
    {
      "type": "tun",
      "tag": "tun-in",
      "mtu": 9000,
      "inet4_address": "172.19.0.1/28",
      "auto_route": true,
      "strict_route": true,
      "endpoint_independent_nat": true,
      "stack": "system",
      "sniff": true,
      "sniff_override_destination": true
    },
    {
      "type": "mixed",
      "tag": "mixed-in",
      "listen": "127.0.0.1",
      "listen_port": 2334,
      "sniff": true,
      "sniff_override_destination": true
    },
    {
      "type": "direct",
      "tag": "dns-in",
      "listen": "127.0.0.1",
      "listen_port": 6450
    }
  ],
  "outbounds": [
    {
      "type": "selector",
      "tag": "select",
      "outbounds": [
        "auto",
        "ایران🇮🇷 § 0",
        "آلمان🇩🇪WoW § 1"
      ],
      "default": "auto"
    },
    {
      "type": "urltest",
      "tag": "auto",
      "outbounds": [
        "ایران🇮🇷 § 0",
        "آلمان🇩🇪WoW § 1"
      ],
      "url": "http://cp.cloudflare.com/",
      "interval": "10m0s",
      "idle_timeout": "1h40m0s"
    },
    {
      "type": "wireguard",
      "tag": "ایران🇮🇷 § 0",
      "local_address": [
        "172.16.0.2/24",
        "2606:4700:110:8aac:e3f2:3b83:5662:5003/128"
      ],
      "private_key": "eFFlRaOO3Im/jdoAZ4ZS1xiF/GBdIvnwXqxO/rgqN2M=",
      "server": "162.159.195.130",
      "server_port": 1010,
      "peer_public_key": "bmXOC+F1FxEMF9dyiK2H5/1SUtzH0JuVo51h2wPfgyo=",
      "reserved": "6ox/",
      "mtu": 1330,
      "fake_packets": "10-20",
      "fake_packets_size": "20-60",
      "fake_packets_delay": "5-10"
    },
    {
      "type": "wireguard",
      "tag": "آلمان🇩🇪WoW § 1",
      "detour": "ایران🇮🇷 § 0",
      "local_address": [
        "172.16.0.2/24",
        "2606:4700:110:80b4:3a4c:19b1:c488:9b51/128"
      ],
      "private_key": "YHxdmcD4B9AWVieKLxYkl3oO5rFG2AKXmTm+0itYzXY=",
      "server": "162.159.192.132",
      "server_port": 5956,
      "peer_public_key": "bmXOC+F1FxEMF9dyiK2H5/1SUtzH0JuVo51h2wPfgyo=",
      "reserved": "lo6Q",
      "mtu": 1280
    },
    {
      "type": "dns",
      "tag": "dns-out"
    },
    {
      "type": "direct",
      "tag": "direct"
    },
    {
      "type": "direct",
      "tag": "direct-fragment",
      "tls_fragment": {
        "enabled": true,
        "size": "1-500",
        "sleep": "0-500"
      }
    },
    {
      "type": "direct",
      "tag": "bypass"
    },
    {
      "type": "block",
      "tag": "block"
    }
  ],
  "route": {
    "geoip": {
      "path": "geo-assets/sagernet-sing-geoip-geoip.db"
    },
    "geosite": {
      "path": "geo-assets/sagernet-sing-geosite-geosite.db"
    },
    "rules": [
      {
        "inbound": "dns-in",
        "outbound": "dns-out"
      },
      {
        "port": 53,
        "outbound": "dns-out"
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
        "domain_suffix": ".ru",
        "geoip": "ru",
        "outbound": "bypass"
      }
    ],
    "final": "select",
    "auto_detect_interface": true,
    "override_android_vpn": true
  },
  "experimental": {
    "cache_file": {
      "enabled": true,
      "path": "clash.db"
    },
    "clash_api": {
      "external_controller": "127.0.0.1:6756",
      "secret": "NDCNMEX6MJ_M-p_W"
    }
  }
}
