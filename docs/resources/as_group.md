---
subcategory: "Auto Scaling"
---

# huaweicloud\_as\_group

Manages a Autoscaling Group resource within HuaweiCloud.
This is an alternative to `huaweicloud_as_group_v1`

## Example Usage

### Basic Autoscaling Group

```hcl
resource "huaweicloud_as_group" "my_as_group" {
  scaling_group_name       = "my_as_group"
  scaling_configuration_id = "37e310f5-db9d-446e-9135-c625f9c2bbfc"
  desire_instance_number   = 2
  min_instance_number      = 0
  max_instance_number      = 10
  vpc_id                   = "1d8f7e7c-fe04-4cf5-85ac-08b478c290e9"
  delete_publicip          = true
  delete_instances         = "yes"

  networks {
    id = "ad091b52-742f-469e-8f3c-fd81cadf0743"
  }
  security_groups {
    id = "45e4c6de-6bf0-4843-8953-2babde3d4810"
  }
}
```

### Autoscaling Group with tags

```hcl
resource "huaweicloud_as_group" "my_as_group_tags" {
  scaling_group_name       = "my_as_group_tags"
  scaling_configuration_id = "37e310f5-db9d-446e-9135-c625f9c2bbfc"
  desire_instance_number   = 2
  min_instance_number      = 0
  max_instance_number      = 10
  vpc_id                   = "1d8f7e7c-fe04-4cf5-85ac-08b478c290e9"
  delete_publicip          = true
  delete_instances         = "yes"

  networks {
    id = "ad091b52-742f-469e-8f3c-fd81cadf0743"
  }
  security_groups {
    id = "45e4c6de-6bf0-4843-8953-2babde3d4810"
  }
  tags = {
    foo = "bar"
    key = "value"
  }
}
```

### Autoscaling Group Only Remove Members When Scaling Down

```hcl
resource "huaweicloud_as_group" "my_as_group_only_remove_members" {
  scaling_group_name       = "my_as_group_only_remove_members"
  scaling_configuration_id = "37e310f5-db9d-446e-9135-c625f9c2bbfc"
  desire_instance_number   = 2
  min_instance_number      = 0
  max_instance_number      = 10
  vpc_id                   = "1d8f7e7c-fe04-4cf5-85ac-08b478c290e9"
  delete_publicip          = true
  delete_instances         = "no"

  networks {
    id = "ad091b52-742f-469e-8f3c-fd81cadf0743"
  }
  security_groups {
    id = "45e4c6de-6bf0-4843-8953-2babde3d4810"
  }
}
```

### Autoscaling Group With ELB Listener

```hcl
resource "huaweicloud_as_group" "my_as_group_with_elb" {
  scaling_group_name       = "my_as_group_with_elb"
  scaling_configuration_id = "37e310f5-db9d-446e-9135-c625f9c2bbfc"
  desire_instance_number   = 2
  min_instance_number      = 0
  max_instance_number      = 10
  vpc_id                   = "1d8f7e7c-fe04-4cf5-85ac-08b478c290e9"
  lb_listener_id           = huaweicloud_elb_listener.my_listener.id
  delete_publicip          = true
  delete_instances         = "yes"

  networks {
    id = "ad091b52-742f-469e-8f3c-fd81cadf0743"
  }
  security_groups {
    id = "45e4c6de-6bf0-4843-8953-2babde3d4810"
  }
}

resource "huaweicloud_elb_listener" "my_listener" {
  name             = "my_listener"
  description      = "my test listener"
  protocol         = "TCP"
  backend_protocol = "TCP"
  port             = 12345
  backend_port     = 21345
  lb_algorithm     = "roundrobin"
  loadbalancer_id  = "cba48790-baf5-4446-adb3-02069a916e97"
  timeouts {
    create = "5m"
    update = "5m"
    delete = "5m"
  }
}

```

### Autoscaling Group With Enhanced Load Balancer Listener

```hcl
resource "huaweicloud_lb_loadbalancer" "loadbalancer_1" {
  name          = "loadbalancer_1"
  vip_subnet_id = "d9415786-5f1a-428b-b35f-2f1523e146d2"
}

resource "huaweicloud_lb_listener" "listener_1" {
  name            = "listener_1"
  protocol        = "HTTP"
  protocol_port   = 8080
  loadbalancer_id = huaweicloud_lb_loadbalancer.loadbalancer_1.id
}

resource "huaweicloud_lb_pool" "pool_1" {
  name        = "pool_1"
  protocol    = "HTTP"
  lb_method   = "ROUND_ROBIN"
  listener_id = huaweicloud_lb_listener.listener_1.id
}

resource "huaweicloud_as_group" "my_as_group_with_enhanced_lb" {
  scaling_group_name       = "my_as_group_with_enhanced_lb"
  scaling_configuration_id = "37e310f5-db9d-446e-9135-c625f9c2bbfc"
  desire_instance_number   = 2
  min_instance_number      = 0
  max_instance_number      = 10
  vpc_id                   = "1d8f7e7c-fe04-4cf5-85ac-08b478c290e9"

  networks {
    id = "ad091b52-742f-469e-8f3c-fd81cadf0743"
  }
  security_groups {
    id = "45e4c6de-6bf0-4843-8953-2babde3d4810"
  }
  lbaas_listeners {
    pool_id       = huaweicloud_lb_pool.pool_1.id
    protocol_port = huaweicloud_lb_listener.listener_1.protocol_port
  }
}
```

## Argument Reference

The following arguments are supported:

* `region` - (Optional) The region in which to create the AS group. If
    omitted, the `region` argument of the provider is used. Changing this
    creates a new AS group.

* `scaling_group_name` - (Required) The name of the scaling group. The name can contain letters,
    digits, underscores(_), and hyphens(-),and cannot exceed 64 characters.

* `scaling_configuration_id` - (Optional) The configuration ID which defines
    configurations of instances in the AS group.

* `desire_instance_number` - (Optional) The expected number of instances. The default
    value is the minimum number of instances. The value ranges from the minimum number of
    instances to the maximum number of instances.

* `min_instance_number` - (Optional) The minimum number of instances.
    The default value is 0.

* `max_instance_number` - (Optional) The maximum number of instances.
    The default value is 0.

* `cool_down_time` - (Optional) The cooling duration (in seconds). The value ranges
    from 0 to 86400, and is 300 by default.

* `lb_listener_id` - (Optional) The ELB listener IDs. The system supports up to
    three ELB listeners, the IDs of which are separated using a comma (,).

* `lbaas_listeners` - (Optional) An array of one or more enhanced load balancer.
    The system supports the binding of up to three load balancers. The field is
    alternative to lb_listener_id.  The lbaas_listeners object structure is
    documented below.

* `available_zones` - (Optional) The availability zones in which to create
    the instances in the autoscaling group.

* `networks` - (Required) An array of one or more network IDs.
    The system supports up to five networks. The networks object structure
    is documented below.

* `security_groups` - (Required) An array of one or more security group IDs
    to associate with the group. The security_groups object structure is
    documented below.

* `vpc_id` - (Required) The VPC ID. Changing this creates a new group.

* `health_periodic_audit_method` - (Optional) The health check method for instances
    in the AS group. The health check methods include `ELB_AUDIT` and `NOVA_AUDIT`.
    If load balancing is configured, the default value of this parameter is `ELB_AUDIT`.
    Otherwise, the default value is `NOVA_AUDIT`.

* `health_periodic_audit_time` - (Optional) The health check period for instances.
    The period has four options: 5 minutes (default), 15 minutes, 60 minutes, and 180 minutes.

* `instance_terminate_policy` - (Optional) The instance removal policy. The policy has
    four options: `OLD_CONFIG_OLD_INSTANCE` (default), `OLD_CONFIG_NEW_INSTANCE`,
    `OLD_INSTANCE`, and `NEW_INSTANCE`.

* `tags` - (Optional) The key/value pairs to associate with the scaling group.

* `notifications` - (Optional) The notification mode. The system only supports `EMAIL`
    mode which refers to notification by email.

* `delete_publicip` - (Optional) Whether to delete the elastic IP address bound to the
    instances of AS group when deleting the instances. The options are `true` and `false`.

* `delete_instances` - (Optional) Whether to delete the instances in the AS group
    when deleting the AS group. The options are `yes` and `no`.

The `networks` block supports:

* `id` - (Required) The network UUID.

The `security_groups` block supports:

* `id` - (Required) The UUID of the security group.

The `lbaas_listeners` block supports:

* `pool_id` - (Required) Specifies the backend ECS group ID.
* `protocol_port` - (Required) Specifies the backend protocol, which is the port on which
  a backend ECS listens for traffic. The number of the port ranges from 1 to 65535.
* `weight` - (Optional) Specifies the weight, which determines the portion of requests a
  backend ECS processes compared to other backend ECSs added to the same listener. The value
  of this parameter ranges from 0 to 100. The default value is 1.

## Attributes Reference

The following attributes are exported:

* `region` - See Argument Reference above.
* `scaling_group_name` - See Argument Reference above.
* `status` - Indicates the status of the AS group.
* `current_instance_number` - Indicates the number of current instances in the AS group.
* `desire_instance_number` - See Argument Reference above.
* `min_instance_number` - See Argument Reference above.
* `max_instance_number` - See Argument Reference above.
* `cool_down_time` - See Argument Reference above.
* `lb_listener_id` - See Argument Reference above.
* `health_periodic_audit_method` - See Argument Reference above.
* `health_periodic_audit_time` - See Argument Reference above.
* `instance_terminate_policy` - See Argument Reference above.
* `scaling_configuration_id` - See Argument Reference above.
* `delete_publicip` - See Argument Reference above.
* `notifications` - See Argument Reference above.
* `tags` - See Argument Reference above.
* `instances` - The instances IDs of the AS group.
