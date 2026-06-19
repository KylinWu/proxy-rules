# proxy-rules

Personal proxy rulesets, organized by category and provider. Designed to be
consumed remotely by Surge and Clash/mihomo via their rule-provider / RULE-SET
mechanisms.

## Layout

```
rule/
└── <Category>/
    └── <Provider>/
        └── <Provider>.list      # the ruleset (classical format)
module/
└── <Provider>.module           # Shadowrocket / Surge module wrapping the ruleset
```

Each `.list` file is in the **classical** format — plain `DOMAIN-SUFFIX` /
`DOMAIN` / `DOMAIN-KEYWORD` / `IP-CIDR` lines with **no policy** embedded. The
policy is assigned where you reference the set, so the same file works across
clients.

Current rulesets:

| Category | Provider | Ruleset | Module |
| --- | --- | --- | --- |
| Broker | Futu / moomoo | [`rule/Broker/Futu/Futu.list`](rule/Broker/Futu/Futu.list) | [`module/Futu.module`](module/Futu.module) |
| Apple | APNs (push notifications) | [`rule/Apple/APNs/APNs.list`](rule/Apple/APNs/APNs.list) | [`module/APNs.module`](module/APNs.module) |

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

### Shadowrocket (module)

If your main config is a remote subscription you periodically reload, do **not**
add the `RULE-SET` line into that config — it gets wiped on every update. Use a
module instead: modules are stored separately and layered on top of the active
config, so they survive subscription updates.

In Shadowrocket: **Home → Modules → +**, add this URL, then toggle it on:

```
https://raw.githubusercontent.com/KylinWu/proxy-rules/main/module/Futu.module
https://raw.githubusercontent.com/KylinWu/proxy-rules/main/module/APNs.module
```

Each module just references its remote `RULE-SET`, so the domain/IP list keeps
auto-updating from this repo. Edit the `PROXY` policy in a module if your config
uses a named proxy group.

## Adding a new ruleset

1. Create `rule/<Category>/<Provider>/<Provider>.list`.
2. Start with a header comment block (`# NAME`, `# REPO`, `# UPDATED`, rule counts).
3. Add one rule per line, no policy. Keep entries sorted and deduplicated —
   drop any host already covered by a broader `DOMAIN-SUFFIX`.
4. Optionally add `module/<Provider>.module` wrapping the ruleset via `RULE-SET`.
5. Update the table above.

## License

[MIT](LICENSE)
