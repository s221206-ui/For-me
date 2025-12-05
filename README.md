# Clash Meta 配置文件
# 版本: 1.22-250718
# 基础配置 - 请修改以下部分

# 订阅配置
use: &use
  type: url-test  # 自动选择最快节点，改为 select 可手动选择
  use:
    - 订阅一
    # - 订阅二  # 如需多个订阅，取消注释

# 订阅链接配置
proxy-providers:
  订阅一:
    type: http
    interval: 3600
    health-check:
      enable: true
      url: https://cp.cloudflare.com
      interval: 300
      timeout: 1000
      tolerance: 100
    url: "YOUR_SUBSCRIPTION_URL_HERE"  # 替换为你的订阅链接
    # path: ./proxy_provider/订阅一.yaml  # 本地缓存文件
  
  # 订阅二:  # 如需第二个订阅，取消注释以下部分
  #   type: http
  #   interval: 3600
  #   health-check:
  #     enable: true
  #     url: https://cp.cloudflare.com
  #     interval: 300
  #     timeout: 1000
  #     tolerance: 100
  #   url: "YOUR_SECOND_SUBSCRIPTION_URL_HERE"

# 以下是配置文件的完整部分（不要修改）
# --------------------------------------------------
mixed-port: 7890
allow-lan: true
mode: rule
log-level: info
ipv6: true

# 规则订阅
rule-providers:
  AWAvenue-Ads:
    type: http
    behavior: domain
    format: yaml
    url: "https://ghfast.top/https://raw.githubusercontent.com/TG-Twilight/AWAvenue-Ads-Rule/main/Filters/AWAvenue-Ads-Rule-Clash-Classical.yaml"
    interval: 600

# DNS 设置
dns:
  enable: true
  listen: :1053
  ipv6: true
  enhanced-mode: redir-host
  nameserver:
    - 'tls://1.0.0.1#dns'
    - 'tls://8.8.4.4#dns'
  default-nameserver:
    - 223.5.5.5
    - 119.29.29.29

# 代理分组
proxy-groups:
  - name: 节点选择
    type: select
    proxies:
      [全部节点, 自动选择, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, DIRECT]
  
  - name: 广告拦截
    type: select
    proxies: [REJECT, DIRECT, 节点选择]
  
  - name: Google
    type: select
    proxies: [节点选择, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择, DIRECT]
  
  - name: YouTube
    type: select
    proxies: [节点选择, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择, DIRECT]
  
  - name: 国内
    type: select
    proxies: [DIRECT, 节点选择, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择]
  
  - name: 其他
    type: select
    proxies: [节点选择, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择, DIRECT]
  
  # 地区分组
  - name: 香港
    type: url-test
    use: [订阅一]
    filter: "(?i)港|hk|hongkong|hong kong"
  
  - name: 台湾
    type: url-test
    use: [订阅一]
    filter: "(?i)台|tw|taiwan"
  
  - name: 日本
    type: url-test
    use: [订阅一]
    filter: "(?i)日本|jp|japan"
  
  - name: 美国
    type: url-test
    use: [订阅一]
    filter: "(?i)美|us|unitedstates|united states"
  
  - name: 新加坡
    type: url-test
    use: [订阅一]
    filter: "(?i)(新|sg|singapore)"
  
  - name: 其它地区
    type: url-test
    use: [订阅一]
    filter: "(?i)^(?!.*(?:港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore|美|us|unitedstates)).*"
  
  - name: 全部节点
    type: select
    use: [订阅一]
  
  - name: 自动选择
    type: url-test
    use: [订阅一]
    tolerance: 100

# 规则
rules:
  - RULE-SET,AWAvenue-Ads,广告拦截
  - GEOSITE,youtube,YouTube
  - GEOSITE,google,Google
  - GEOSITE,google-cn,Google
  - GEOSITE,telegram,节点选择
  - GEOSITE,netflix,节点选择
  - GEOSITE,tiktok,节点选择
  - GEOSITE,github,节点选择
  - GEOSITE,geolocation-!cn,其他
  - GEOSITE,CN,国内
  - GEOIP,CN,国内
  - MATCH,其他# Clash Meta 配置文件
# 版本: 1.22-250718
# 基础配置 - 请修改以下部分

# 订阅配置
use: &use
  type: url-test  # 自动选择最快节点，改为 select 可手动选择
  use:
    - 订阅一
    # - 订阅二  # 如需多个订阅，取消注释

# 订阅链接配置
proxy-providers:
  订阅一:
    type: http
    interval: 3600
    health-check:
      enable: true
      url: https://cp.cloudflare.com
      interval: 300
      timeout: 1000
      tolerance: 100
    url: "YOUR_SUBSCRIPTION_URL_HERE"  # 替换为你的订阅链接
    # path: ./proxy_provider/订阅一.yaml  # 本地缓存文件
  
  # 订阅二:  # 如需第二个订阅，取消注释以下部分
  #   type: http
  #   interval: 3600
  #   health-check:
  #     enable: true
  #     url: https://cp.cloudflare.com
  #     interval: 300
  #     timeout: 1000
  #     tolerance: 100
  #   url: "YOUR_SECOND_SUBSCRIPTION_URL_HERE"

# 以下是配置文件的完整部分（不要修改）
# --------------------------------------------------
mixed-port: 7890
allow-lan: true
mode: rule
log-level: info
ipv6: true

# 规则订阅
rule-providers:
  AWAvenue-Ads:
    type: http
    behavior: domain
    format: yaml
    url: "https://ghfast.top/https://raw.githubusercontent.com/TG-Twilight/AWAvenue-Ads-Rule/main/Filters/AWAvenue-Ads-Rule-Clash-Classical.yaml"
    interval: 600

# DNS 设置
dns:
  enable: true
  listen: :1053
  ipv6: true
  enhanced-mode: redir-host
  nameserver:
    - 'tls://1.0.0.1#dns'
    - 'tls://8.8.4.4#dns'
  default-nameserver:
    - 223.5.5.5
    - 119.29.29.29

# 代理分组
proxy-groups:
  - name: 节点选择
    type: select
    proxies:
      [全部节点, 自动选择, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, DIRECT]
  
  - name: 广告拦截
    type: select
    proxies: [REJECT, DIRECT, 节点选择]
  
  - name: Google
    type: select
    proxies: [节点选择, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择, DIRECT]
  
  - name: YouTube
    type: select
    proxies: [节点选择, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择, DIRECT]
  
  - name: 国内
    type: select
    proxies: [DIRECT, 节点选择, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择]
  
  - name: 其他
    type: select
    proxies: [节点选择, 香港, 台湾, 日本, 新加坡, 美国, 其它地区, 全部节点, 自动选择, DIRECT]
  
  # 地区分组
  - name: 香港
    type: url-test
    use: [订阅一]
    filter: "(?i)港|hk|hongkong|hong kong"
  
  - name: 台湾
    type: url-test
    use: [订阅一]
    filter: "(?i)台|tw|taiwan"
  
  - name: 日本
    type: url-test
    use: [订阅一]
    filter: "(?i)日本|jp|japan"
  
  - name: 美国
    type: url-test
    use: [订阅一]
    filter: "(?i)美|us|unitedstates|united states"
  
  - name: 新加坡
    type: url-test
    use: [订阅一]
    filter: "(?i)(新|sg|singapore)"
  
  - name: 其它地区
    type: url-test
    use: [订阅一]
    filter: "(?i)^(?!.*(?:港|hk|hongkong|台|tw|taiwan|日|jp|japan|新|sg|singapore|美|us|unitedstates)).*"
  
  - name: 全部节点
    type: select
    use: [订阅一]
  
  - name: 自动选择
    type: url-test
    use: [订阅一]
    tolerance: 100

# 规则
rules:
  - RULE-SET,AWAvenue-Ads,广告拦截
  - GEOSITE,youtube,YouTube
  - GEOSITE,google,Google
  - GEOSITE,google-cn,Google
  - GEOSITE,telegram,节点选择
  - GEOSITE,netflix,节点选择
  - GEOSITE,tiktok,节点选择
  - GEOSITE,github,节点选择
  - GEOSITE,geolocation-!cn,其他
  - GEOSITE,CN,国内
  - GEOIP,CN,国内
  - MATCH,其他
