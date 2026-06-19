# proxy-rules

Personal proxy rulesets, organized by category and provider. Designed to be
consumed remotely by Surge and Clash/mihomo via their rule-provider / RULE-SET
mechanisms.

## Layout

```
rule/
└── <Category>/
    └── <Provider>/
        └── <Provider>.list
```

Each `.list` file is in the **classical** format — plain `DOMAIN-SUFFIX` /
`DOMAIN` / `DOMAIN-KEYWORD` / `IP-CIDR` lines with **no policy** embedded. The
policy is assigned where you reference the set, so the same file works across
clients.

Current rulesets:

| Category | Provider | File |
| --- | --- | --- |
| Broker | Futu (富途 / moomoo) | [`rule/Broker/Futu/Futu.list`](rule/Broker/Futu/Futu.list) |

## Usage

Raw URL pattern:

```
https://raw.githubusercontent.com/KylinWu/proxy-rules/main/<path-to>.list
```

### Clash / mihomo

```yaml
rule-providers:
  Futu:
    type: http
    behavior: classical
    format: text
    url: https://raw.githubusercontent.com/KylinWu/proxy-rules/main/rule/Broker/Futu/Futu.list
    path: ./ruleset/Futu.list
    interval: 86400

rules:
  - RULE-SET,Futu,🚀 Proxy
```

### Surge

```
[Rule]
RULE-SET,https://raw.githubusercontent.com/KylinWu/proxy-rules/main/rule/Broker/Futu/Futu.list,🚀 Proxy
```

Replace `🚀 Proxy` with whatever policy group you want these domains to use.

## Adding a new ruleset

1. Create `rule/<Category>/<Provider>/<Provider>.list`.
2. Start with a header comment block (`# NAME`, `# REPO`, `# UPDATED`, rule counts).
3. Add one rule per line, no policy. Keep entries sorted and deduplicated —
   drop any host already covered by a broader `DOMAIN-SUFFIX`.
4. Update the table above.

## License

[MIT](LICENSE)
