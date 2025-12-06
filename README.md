# Clash Meta 优化配置 - 增强TikTok支持
# 基于原配置优化，添加TikTok分流和性能优化

# 分组模板优化
pr:
  &pr {
    type: select,
    proxies:
      [
        节点选择,
        香港,
        台湾,
        日本,
        新加坡,
        美国,
        其它地区,
        全部节点,
        自动选择,
        DIRECT,
      ],
  }

# TikTok专用组模板
tiktok_pr:
  &tiktok_pr {
    type: select,
    proxies:
      [
        TikTok优选,
        香港,
        台湾,
        日本,
        新加坡,
        美国,
        其它地区,
        节点选择,
        自动选择,
        DIRECT,
      ],
  }

# 延迟检测优化
p:
  &p {
    type: http,
    interval: 1800,
    health-check:
      {
        enable: true,
        url: https://www.gstatic.com/generate_204,
        interval: 300,
        timeout: 3000,
      },
  }

# 订阅配置
use: &use
  type: select
  use:
    - 订阅一
    - 订阅二
    - 订阅三

proxy-providers:
  订阅一:
    <<: *p
    url: "https://example.com/airport?type=clash&protocol=shadowsocks&rule=default"

  订阅二:
    <<: *p
    url: "https://example.com/api/v1/client/subscribe?token=8964xjp"

  订阅三:
    <<: *p
    url: "https://example.com/api/v1/client/subscribe?token=52chinaccp"

# 规则订阅
rule-providers:
  anti-AD:
    type: http
    behavior: domain
    format: yaml
    path: ./anti-AD.yaml
    url: "https://raw.githubusercontent.com/privacy-protection-tools/anti-AD/master/anti-ad-clash.yaml?"
    interval: 600
  
  anti-AD-white:
    type: http
    behavior: domain
    format: yaml
    path: ./anti-AD-white.yaml
    url: "https://raw.githubusercontent.com/privacy-protection-tools/dead-horse/master/anti-ad-white-for-clash.yaml?"
    interval: 600
  
  # TikTok规则集
  tiktok:
    type: http
    behavior: domain
    format: yaml
    path: ./tiktok.yaml
    url: "https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Clash/TikTok/TikTok.yaml"
    interval: 86400
  
  # Reject规则集
  reject:
    type: http
    behavior: domain
    format: yaml
    path: ./reject.yaml
    url: "https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Clash/Reject/Reject.yaml"
    interval: 86400

# 基础配置
mode: rule
ipv6: true
log-level: info
allow-lan: true
mixed-port: 7890
external-controller: :9090

# 性能优化
tcp-concurrent: true
unified-delay: true
find-process-mode: off

geodata-mode: true
geox-url:
  geoip: "https://download.fgit.cf/MetaCubeX/meta-rules-dat/releases/download/latest/geoip.dat"
  geosite: "https://download.fgit.cf/MetaCubeX/meta-rules-dat/releases/download/latest/geosite.dat"
  mmdb: "https://download.fgit.cf/MetaCubeX/meta-rules-dat/releases/download/latest/country.mmdb"

# DNS优化
dns:
  enable: true
  listen: :1053
  ipv6: true
  enhanced-mode: redir-host
  fake-ip-range: 28.0.0.1/8
  fake-ip-filter:
    - '*'
    - '+.lan'
    - '+.local'
    - '+.tiktok.com'
    - '+.tiktokcdn.com'
    - '+.byteoversea.com'
  
  default-nameserver:
    - 223.5.5.5
    - 119.29.29.29
    - 114.114.114.114
  
  nameserver:
    - 'tls://8.8.4.4#dns'
    - 'tls://1.0.0.1#dns'
    - 'https://cloudflare-dns.com/dns-query'
  
  fallback-filter:
    geoip: true
    ipcidr:
      - 240.0.0.0/4
    domain:
      - '+.tiktok.com'
      - '+.tiktokcdn.com'
      - '+.byteoversea.com'
  
  nameserver-policy:
    "geosite:cn,private":
      - https://doh.pub/dns-query
      - https://dns.alidns.com/dns-query
    "geosite:tiktok":
      - 'tls://8.8.4.4#dns'
      - 'tls://1.0.0.1#dns'

# 域名嗅探
sniffer:
  enable: true
  sniff:
    TLS:
      ports: [443, 8443]
    HTTP:
      ports: [80, 8080-8880]
      override-destination: true
  skip-domain:
    - "Mijia Cloud"
    - "*.apple.com"
    - "*.icloud.com"

# Tun模式（按需启用）
tun:
  enable: false
  stack: system
  dns-hijack:
    - "any:53"
    - "tcp://any:53"
  auto-route: true
  auto-detect-interface: true

# 代理组配置
proxy-groups:
  # TikTok优选组
  - {
      name: TikTok优选,
      type: url-test,
      use: [订阅一, 订阅二, 订阅三],
      filter: "(?i)(港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore)",
      url: "https://www.tiktok.com/favicon.ico",
      interval: 300,
      tolerance: 50
    }
  
  # 主选择组
  - {
      name: 节点选择,
      type: select,
      proxies:
        [全部节点, 自动选择, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, DIRECT],
    }
  
  - {
      name: dns,
      type: select,
      proxies:
        [
          节点选择,
          自动选择,
          香港,
          台湾,
          日本,
          新加坡,
          美国,
          其它地区,
          全部节点,
          DIRECT,
        ],
    }
  
  # 分流组
  - { name: 广告拦截白名单, type: select, proxies: [DIRECT, REJECT, 节点选择] }
  - { name: 广告拦截, type: select, proxies: [REJECT, DIRECT, 节点选择] }
  - { name: TikTok主站, <<: *tiktok_pr }
  - { name: Google, <<: *pr }
  - { name: Telegram, <<: *pr }
  - { name: Twitter, <<: *pr }
  - { name: Pixiv, <<: *pr }
  - { name: ehentai, <<: *pr }
  - { name: 巴哈姆特, <<: *pr }
  - { name: YouTube, <<: *pr }
  - { name: NETFLIX, <<: *pr }
  - { name: Spotify, <<: *pr }
  - { name: Github, <<: *pr }
  - {
      name: 国内,
      type: select,
      proxies:
        [
          DIRECT,
          节点选择,
          香港,
          台湾,
          日本,
          新加坡,
          美国,
          其它地区,
          全部节点,
          自动选择,
        ],
    }
  - { name: 其他, <<: *pr }

  # 地区分组
  - { name: 香港, <<: *use, filter: "(?i)港|hk|hongkong|hong kong" }
  - { name: 台湾, <<: *use, filter: "(?i)台|tw|taiwan" }
  - { name: 日本, <<: *use, filter: "(?i)日本|jp|japan" }
  - { name: 美国, <<: *use, filter: "(?i)美|us|unitedstates|united states" }
  - { name: 新加坡, <<: *use, filter: "(?i)(新|sg|singapore)" }
  - {
      name: 其它地区,
      <<: *use,
      filter: "(?i)^(?!.*(?:🇭🇰|🇯🇵|🇺🇸|🇸🇬|🇨🇳|港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore|美|us|unitedstates)).*",
    }
  - { name: 全部节点, <<: *use }
  - { name: 自动选择, <<: *use, tolerance: 2, type: url-test }

# 规则设置
rules:
  # TikTok规则优先
  - RULE-SET,tiktok,TikTok主站
  
  # TikTok广告屏蔽（可选）
  - DOMAIN-SUFFIX,ads.tiktok.com,REJECT
  - DOMAIN-SUFFIX,ads-sg.tiktok.com,REJECT
  - DOMAIN-SUFFIX,ads-us.tiktok.com,REJECT
  
  # 广告拦截
  - RULE-SET,anti-AD-white,广告拦截白名单
  - RULE-SET,anti-AD,广告拦截
  - RULE-SET,reject,REJECT
  
  # 其他应用分流
  - GEOSITE,ehentai,ehentai
  - GEOSITE,github,Github
  - GEOSITE,twitter,Twitter
  - GEOSITE,youtube,YouTube
  - GEOSITE,google,Google
  - GEOSITE,telegram,Telegram
  - GEOSITE,netflix,NETFLIX
  - GEOSITE,bahamut,巴哈姆特
  - GEOSITE,spotify,Spotify
  - GEOSITE,geolocation-!cn,其他
  - GEOSITE,pixiv,Pixiv
  
  # GEOIP规则
  - GEOIP,google,Google
  - GEOIP,netflix,NETFLIX
  - GEOIP,telegram,Telegram
  - GEOIP,twitter,Twitter
  
  # 国内流量
  - GEOSITE,CN,国内
  - GEOIP,CN,国内
  
  # 最终规则
  - MATCH,其他
