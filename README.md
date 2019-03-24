# Ansible role for Alertmanager

[![CircleCI](https://circleci.com/gh/angristan/ansible-alertmanager.svg?style=svg)](https://circleci.com/gh/angristan/ansible-alertmanager)

This role will setup [Alertmanager](https://github.com/prometheus/alertmanager) on any Linux machine using systemd.

## Requirements

- systemd on the target host
- gnu-tar on Mac deployer host (`brew install gnu-tar`)

## Role Variables

- `alertmanager_version`: the versions that will be installed and downloaded. (`0.16.1`).

The role will download the alertmanager release on the deployer and upload the `alertmanager` and `amtool` binaries on the target host.

If the `/usr/local/bin/alertmanager` binary already exists, the role will skip the install steps. You can force them (to update, for instance), by setting `alertmanager_force_install` to true.

- `alertmanager_config_dir`: the configuration directory (`/etc/alertmanager`)
- `alertmanager_config_file`: custom alertmanager configuration template name (`alertmanager.yml.j2`)
- `alertmanager_data_dir`: the data directory (`/var/lib/alertmanager`)
- `alertmanager_web_listen_address`: the listening address of the web interface (`0.0.0.0:9093`)
- `alertmanager_web_external_url` the external URL you will access the web interface with (`http://localhost:9093/`)

- `alertmanager_resolve_timeout`: time after which an alert is declared resolved if it has not been updated. (`3m`)
- `alertmanager_config_flags_extra`: extra flags passed to the alertmanager binary in the systemd unit (`{}`)

- `alertmanager_smtp`: SMTP (email) configuration. See `alertmanager.yml.j2`. (`{}`)
- `alertmanager_slack_api_url`: Slack webhook url (`""`)
- `alertmanager_pagerduty_url`: Pagerduty webhook url (`""`)
- `alertmanager_opsgenie_api_key`: Opsgenie webhook key (`""`)
- `alertmanager_opsgenie_api_url`: Opsgenie webhook url (`""`)
- `alertmanager_hipchat_api_url`: Hipchat webhook url (`""`)
- `alertmanager_hipchat_auth_token`: Hipchat authentication token (`""`)
- `alertmanager_wechat_url`: Enterprise WeChat webhook url (`""`)
- `alertmanager_wechat_secret`: Enterprise WeChat secret token (`""`)
- `alertmanager_wechat_corp_id`: Enterprise WeChat corporation id (`""`)

- `alertmanager_receivers`: A list of notification receivers. Configuration same as in [official docs](https://prometheus.io/docs/alerting/configuration/#%3Creceiver%3E) (`[]`)
- `alertmanager_inhibit_rules`: List of inhibition rules. Same as in [official docs](https://prometheus.io/docs/alerting/configuration/#inhibit_rule) (`[]`)
- `alertmanager_route`: Alert routing. More in [official docs](https://prometheus.io/docs/alerting/configuration/#%3Croute%3E) (`{}`)
- `alertmanager_child_routes`: List of child routes (`[]`)

## Example playbook

```yaml
---

- hosts: myhost
  roles: prometheus
  vars:
    alertmanager_web_listen_address: '127.0.0.1:9093'
    alertmanager_web_external_url: 'https://alertmanager.domain.tld/'

    alertmanager_slack_api_url: 'https://hooks.slack.com/services/xxx/xxx/xxx'

    alertmanager_receivers:
      - name: 'company-slack'
        slack_configs:
          - channel: 'alerts'

    alertmanager_route:
      group_by: ['instance', 'severity']
      group_wait: 30s
      group_interval: 5m
      repeat_interval: 3h
      receiver: company-slack
```

## License

MIT. See LICENSE for more details.

## Credit

This role is largely inspired by [cloudalchemy/ansible-alertmanager](https://github.com/cloudalchemy/ansible-alertmanager).

## Author Information

See my other Ansible roles at [angristan/ansible-roles](https://github.com/angristan/ansible-roles).
