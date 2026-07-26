# terraform-hcloud-placement-group

> Spread the replicas across hosts, and fail the plan at server eleven.

**Status:** 🚧 In development

## Overview

Terraform module that manages a Hetzner Cloud placement group so servers in a group are spread across distinct physical hosts, with a precondition on the ten-server-per-group API limit.

## Features

- Creates an `hcloud_placement_group` with type `spread`, so group members land on distinct physical hosts
- Precondition on the ten-server-per-group API limit, failing at plan time instead of mid-apply on the eleventh server
- Optional creation of several groups from a map, for sharding a fleet larger than one group can hold
- Outputs the group ID and a name-to-ID map for the server module's `placement_group_id`
- Reports current membership in outputs, so drift between intended and actual members is visible
- Labels passed through for fleet inventory and selector-based firewall rules

## Stack

Terraform + the hetznercloud/hcloud provider.

## Usage

```hcl
module "placement_group" {
  source = "github.com/moveeeax/terraform-hcloud-placement-group"

  name   = "etcd-spread"
  type   = "spread"

  member_count = 3

  labels = {
    role = "etcd"
    env  = "prod"
  }
}
```

## License

MIT
