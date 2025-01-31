# myrasec_dns_record

Provides a Myra Security DNS record resource.

## Example usage

```hcl
# Create a DNS record
resource "myrasec_dns_record" "www" {
    domain_name = "example.com"
    name = "www"
    record_type = "A"
    value = "192.168.0.1"
    ttl = 300
    active = true
    enabled = true
    upstream_options {
        backup = false
        down = false
        fail_timeout = "1"
        max_fails = 100
        weight = 1
    }
}
```

## Import example
Importing an existing DNS record requires the domain name and the ID of the DNS record you want to import.
```hcl
terraform import myrasec_dns_record.www example.com:0000000
```

## Argument Reference

The following arguments are supported:

* `record_id` (Computed) ID of the DNS record.
* `created` (Computed) Date of creation.
* `modified` (Computed) Date of last modification.
* `domain_name` (Required) The Domain for the DNS record.
* `record_type` (Required) A record type to identify the type of a record. Valid types are: `A`, `AAAA`, `MX`, `CNAME`, `TXT`, `NS`, `SRV` and `CAA`.
* `name` (Required) Subdomain name of a DNS record.
* `value` (Required) Depends on the record type. Typically an IPv4/6 address or a domain entry.
* `ttl` (Required) Time to live.
* `alternative_cname` (Optional) The alternative CNAME that points to the record.
* `active` (Optional) Define wether this subdomain should be protected by Myra or not. Default `true`.
* `enabled` (Optional) Define wether this DNS record is enabled or not. Default `true`.
* `priority` (Optional) Priority of MX records.
* `port` (Optional) Port for SRV records.
* `comment` (Optional) A comment to describe this DNS record. Default `""`.
* `upstream_options` (Optional) Loadbalancing settings.
* `upstream_options.upstream_id` (Computed) ID of the upstream settings.
* `upstream_options.created` (Computed) Date of creation.
* `upstream_options.modified` (Computed) Date of last modification.
* `upstream_options.backup` (Optional) Marks the server as a backup server. It will be used when the primary servers are unavailable. Cannot be used in combination with "Preserve client IP on the same upstream". Default `false`.
* `upstream_options.down` (Optional) Marks the server as unavailable. Default `false`.
* `upstream_options.fail_timeout` (Optional) Double usage: 1. Time period in which the max_fails must occur until the upstream is deactivated. 2. Time period the upstream is deactivated until it is reactivated. The time during which the specified number of unsuccessful attempts "Max fails" to communicate with the server should happen to consider the server unavailable. Also the period of time the server will be considered unavailable. Default `"1"`.
* `upstream_options.max_fails` (Optional) The number of unsuccessful attempts to communicate with the server that should happen in the duration set by "Fail timeout" to consider the server unavailable. Also the server is considered unavailable for the duration set by "Fail timeout". By default, the number of unsuccessful attempts is set to 1. Setting the value to zero disables the accounting of attempts. What is considered an unsuccessful attempt is defined by the "Next upstream error handling". Default `100`.
* `upstream_options.weight` (Optional) Weight defines the count of requests a upstream handles before the next upstream is selected. Default `1`.
