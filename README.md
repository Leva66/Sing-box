# 注意事项

1. `"route"-"rules"`里面放在

```bash
      {"rule_set":"Custom_Rules","action":"route","outbound":"🇨🇳 China"},  #添加这条
      {"clash_mode":"Direct","action":"route","outbound":"🇨🇳 China"},
      {"clash_mode":"Global","action":"route","outbound":"GLOBAL"},
```

前面时，就算全局代理时也会走直连。

2. `"dns"`-`"rules"`要指定走哪个DNS

```bash
    "rules": [
      {"query_type":["HTTPS","SVCB"],"action":"reject"},
      {"clash_mode":"Direct","server":"ali"},
      {"clash_mode":"Global","server":"fakeip"},

	  {"rule_set":"Custom_Rules","server":"ali"},  #添加这条

      {"rule_set": ["geosite-cn", "geosite-fakeipfilter-cn", "geosite-steamcn"], "server": "ali"},
      {"query_type":["A","AAAA"],"server":"fakeip","rewrite_ttl":1}

```

3. `"route"-"rule_set"`里面的`"format: "source"` 就是告诉 sing-box 这个远程文件是 JSON source rule-set，而不是 `.srs` 二进制规则集。

```bash
    "rule_set": [
      {"tag":"Custom_Rules","type":"remote","format":"source","url":"https://git.cnma.top/https://raw.githubusercontent.com/qichiyuhub/rule/refs/heads/main/rules/fakeipfilter-cn.json","initial_path":"/etc/sing-box/rule-set/geosite-fakeipfilter-cn.json","update_interval": "12h"},

```

`"update_interval": "12h`意思就是12小时更新一次，不写就是默认24小时更新一次
