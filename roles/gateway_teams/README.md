# Ansible Role infra.aap_configuration.teams

## Description

An Ansible Role to add Teams on Ansible Automation gateway.

## Variables

Detailed description of variables are provided in the [top-level README](../../README.md)

Variables specific for this role are following:

| Variable Name                                  |                    Default Value                    | Required | Description                                                                                                                                                 |                                                      |
|:-----------------------------------------------|:---------------------------------------------------:|:--------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------:|
| `platform_teams` (Alias: `teams`)               |          [below](#Team Arguments)           |   yes    | Data structure describing your team entries described below.                                                                                                |                |
| `platform_teams_secure_logging`   |  `aap_configuration_secure_logging` OR `false`  |    no    | Whether or not to include the sensitive team role tasks in the log. Set this value to `True` if you will be providing your sensitive values from elsewhere. |      |
| `platform_teams_enforce_defaults` | `aap_configuration_enforce_defaults` OR `false` |    no    | Whether or not to enforce default option values on only the team role.                                                                                      |      README.md#enforcing-defaults)      |
| `platform_teams_async_retries`    |    `aap_configuration_async_retries` OR `30`    |    no    | This variable sets the number of retries to attempt for the role.                                                                                           |  |
| `platform_teams_async_delay`      |     `aap_configuration_async_delay` OR `1`      |    no    | This sets the delay between retries for the role.                                                                                                           |  |

## Data Structure

### Team Arguments

Options for the `teams` variable:

| Variable Name      | Default Value | Required | Type | Description                                                                       |
|:-------------------|:-------------:|:--------:|:----:|:----------------------------------------------------------------------------------|
| `name`             |      N/A      |   yes    | str  | The name of the resource                                                          |
| `new_name`         |      N/A      |    no    | str  | Setting this option will change the existing name (looked up via the name field)  |
| `description`      |      N/A      |    no    | str  | Description of the organization                                                   |
| `organization`     |      N/A      |   yes    | str  | The name or ID referencing the [Organization](../gateway_organizations/README.md) |
| `new_organization` |      N/A      |    no    | str  | The name or ID referencing newly associated organization                          |
| `state`            |   `present`   |    no    | str  | Desired state of the resource.                                                    |

### Unique value

- [`name`, `organization`]

## Usage

### Json Example

- Create 2 Teams

```json
{
  "teams": [
    {
      "name": "Team 1",
      "description": "Best team",
      "organization": "IT Department"
    },
    {
      "name": "Team 2",
      "organization": "1"
    }
  ]
}
```

### Yaml Example

- Check that Happy Team in Productive Organization exists
- Check that Managers Team doesn't exist, or delete it
- Rename Team X and Reassign it to another organization

File name: `data/gateway_teams.yml`

```yaml
---
teams:
- name: "Happy Team"
  organization: "Productive Organization"
  state: exists
- name: "Managers"
  organization: "Org X"
  state: absent
- name: "Team X"
  new_name: "Secret Team"
  organization: "Org X"
  new_organization: "Secret Organization"
```

### Run Playbook

File name: [manage_data.yml](../../README.md#example-ansible-playbook) can be found in the top-level README.

```shell
ansible-playbook manage_data.yml -e @data/gateway_teams.yml
```

## License

[GPL-3.0](https://github.com/redhat-cop/aap_configuration#licensing)
