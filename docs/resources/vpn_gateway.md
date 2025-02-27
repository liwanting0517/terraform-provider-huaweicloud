---
subcategory: "Virtual Private Network (VPN)"
---

# huaweicloud_vpn_gateway

Manages a VPN gateway resource within HuaweiCloud.

## Example Usage

### Basic Usage

```hcl
variable "name" {}
variable "vpc_id" {}
variable "subnet_id" {}
variable "eip_id1" {}
variable "eip_id2" {}

data "huaweicloud_vpn_gateway_availability_zones" "test" {
  flavor          = "professional1"
  attachment_type = "vpc"
}

resource "huaweicloud_vpn_gateway" "test" {
  name               = var.name
  vpc_id             = var.vpc_id
  local_subnets      = ["192.168.0.0/24", "192.168.1.0/24"]
  connect_subnet     = var.subnet_id
  availability_zones = [
    data.huaweicloud_vpn_gateway_availability_zones.test.names[0],
    data.huaweicloud_vpn_gateway_availability_zones.test.names[1]
  ]

  master_eip {
    id = var.eip_id1
  }

  slave_eip {
    id = var.eip_id2
  }
}
```

### Creating a VPN gateway with creating new EIPs

```hcl
variable "name" {}
variable "vpc_id" {}
variable "subnet_id" {}
variable "bandwidth_name1" {}
variable "bandwidth_name2" {}

data "huaweicloud_vpn_gateway_availability_zones" "test" {
  flavor          = "professional1"
  attachment_type = "vpc"
}

resource "huaweicloud_vpn_gateway" "test" {
  name               = var.name
  vpc_id             = var.vpc_id
  local_subnets      = ["192.168.0.0/24", "192.168.1.0/24"]
  connect_subnet     = var.subnet_id
  availability_zones = [
    data.huaweicloud_vpn_gateway_availability_zones.test.names[0],
    data.huaweicloud_vpn_gateway_availability_zones.test.names[1]
  ]

  master_eip {
    bandwidth_name = var.bandwidth_name1
    type           = "5_bgp"
    bandwidth_size = 5
    charge_mode    = "traffic"
  }

  slave_eip {
    bandwidth_name = var.bandwidth_name2
    type           = "5_bgp"
    bandwidth_size = 5
    charge_mode    = "traffic"
  }
}
```

### Creating a private VPN gateway with Enterprise Router

```hcl
variable "name" {}
variable "er_id" {}
variable "access_vpc_id" {}
variable "access_subnet_id" {}

data "huaweicloud_vpn_gateway_availability_zones" "test" {
  flavor          = "professional1"
  attachment_type = "er"
}

resource "huaweicloud_vpn_gateway" "test" {
  name               = var.name
  network_type       = "private"
  attachment_type    = "er"
  er_id              = var.er_id
  availability_zones = [
    data.huaweicloud_vpn_gateway_availability_zones.test.names[0],
    data.huaweicloud_vpn_gateway_availability_zones.test.names[1]
  ]

  access_vpc_id      = var.access_vpc_id
  access_subnet_id   = var.access_subnet_id
}
```

## Argument Reference

The following arguments are supported:

* `region` - (Optional, String, ForceNew) Specifies the region in which to create the resource.
  If omitted, the provider-level region will be used. Changing this parameter will create a new resource.

* `name` - (Required, String) The name of the VPN gateway. Only letters, digits, underscores(_) and hypens(-) are supported.

* `availability_zones` - (Required, List, ForceNew) The list of availability zone IDs.

  Changing this parameter will create a new resource.

* `flavor` - (Optional, String, ForceNew) The flavor of the VPN gateway.
  The value can be **Basic**, **Professional1**, **Professional2** and **GM**. Defaults to **Professional1**.

  Changing this parameter will create a new resource.

* `attachment_type` - (Optional, String, ForceNew) The attachment type. The value can be **vpc** and **er**.
  Defaults to **vpc**.

  Changing this parameter will create a new resource.

* `network_type` - (Optional, String, ForceNew) The network type. The value can be **public** and **private**.
  Defaults to **public**.

  Changing this parameter will create a new resource.

* `vpc_id` - (Optional, String, ForceNew) The ID of the VPC to which the VPN gateway is connected.
  This parameter is mandatory when `attachment_type` is **vpc**.

  Changing this parameter will create a new resource.

* `local_subnets` - (Optional, List) The list of local subnets.
  This parameter is mandatory when `attachment_type` is **vpc**.

* `connect_subnet` - (Optional, String, ForceNew) The Network ID of the VPC subnet used by the VPN gateway.
  This parameter is mandatory when `attachment_type` is **vpc**.

  Changing this parameter will create a new resource.

* `er_id` - (Optional, String, ForceNew) The enterprise router ID to attach with to VPN gateway.
  This parameter is mandatory when `attachment_type` is **er**.

  Changing this parameter will create a new resource.

* `master_eip` - (Optional, List, ForceNew) The master EIP configurations.
  This parameter is mandatory when `network_type` is **public** or left empty.
  The [object](#Gateway_CreateRequestEip) structure is documented below.

  Changing this parameter will create a new resource.

* `slave_eip` - (Optional, List, ForceNew) The slave EIP configurations.
  This parameter is mandatory when `network_type` is **public** or left empty.
  The [object](#Gateway_CreateRequestEip) structure is documented below.

  Changing this parameter will create a new resource.

* `access_vpc_id` - (Optional, String, ForceNew) The access VPC ID.
  The defaul value is the value of `vpc_id`.

  Changing this parameter will create a new resource.

* `access_subnet_id` - (Optional, String, ForceNew) The access subnet ID.
  The defaul value is the value of `connect_subnet`.

  Changing this parameter will create a new resource.

* `asn` - (Optional, Int, ForceNew) The ASN number of BGP. The value ranges from **1** to **4294967295**.
  Defaults to **64512**

  Changing this parameter will create a new resource.

* `enterprise_project_id` - (Optional, String, ForceNew) The enterprise project ID.

  Changing this parameter will create a new resource.

<a name="Gateway_CreateRequestEip"></a>
The `master_eip` or `slave_eip` block supports:

* `id` - (Optional, String, ForceNew) The public IP ID.

  Changing this parameter will create a new resource.

* `type` - (Optional, String, ForceNew) The EIP type. The value can be **5_bgp** and **5_sbgp**.

  Changing this parameter will create a new resource.

* `bandwidth_name` - (Optional, String, ForceNew) The bandwidth name.

  Changing this parameter will create a new resource.

* `bandwidth_size` - (Optional, Int, ForceNew) Bandwidth size in Mbit/s. When the `flavor` is **Basic**, the value
  cannot be greater than **100**. When the `flavor` is **Professional1**, the value cannot be greater than **300**.
  When the `flavor` is **Professional2**, the value cannot be greater than **1000**.

  Changing this parameter will create a new resource.

* `charge_mode` - (Optional, String, ForceNew) The charge mode of the bandwidth. The value can be **bandwidth** and **traffic**.

  Changing this parameter will create a new resource.

  ~> You can use `id` to specify an existing EIP or use `type`, `bandwidth_name`, `bandwidth_size` and `charge_mode` to
    create a new EIP.

## Attribute Reference

In addition to all arguments above, the following attributes are exported:

* `id` - The ID of the VPN gateway

* `status` - The status of VPN gateway.

* `created_at` - The create time.

* `updated_at` - The update time.

* `used_connection_group` - The number of used connection groups.

* `used_connection_number` - The number of used connections.

* `master_eip` - The master EIP configurations.
  The [object](#Gateway_GetResponseEip) structure is documented below.

* `slave_eip` - The slave EIP configurations.
  The [object](#Gateway_GetResponseEip) structure is documented below.

<a name="Gateway_GetResponseEip"></a>
The `master_eip` or `slave_eip` block supports:

* `bandwidth_id` - The bandwidth ID.

* `ip_address` - The public IP address.

* `ip_version` - The public IP version.

## Timeouts

This resource provides the following timeouts configuration options:

* `create` - Default is 10 minutes.
* `update` - Default is 10 minutes.
* `delete` - Default is 10 minutes.

## Import

The gateway can be imported using the `id`, e.g.

```bash
$ terraform import huaweicloud_vpn_gateway.test 0ce123456a00f2591fabc00385ff1234
```
