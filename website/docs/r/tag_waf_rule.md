# myrasec_tag_waf_rule

Provides a Myra Security tag WAF rule resource.

## Example usage

```hcl
# Create a new tag WAF rule
resource "myrasec_tag_waf_rule" "tag_waf" {
  tag_id = myrasec_tag.example_tag.id
  name = "tag WAF rule name"
  description = "Some description of this WAF rule"
  log_identifier = "IDENTIFY_ME"
  direction = "in"
  sort = 1
  process_next = false
  enabled = true
  conditions {
      matching_type = "IREGEX"
      name = "url"
      value = "blockme"
  }
  actions {
    name = "Block"
    type = "block"
  }
}
```

## Import example
Importing an existing tag WAF rule requires the tag ID and the ID of the WAF rule you want to import.
```hcl
terraform import myrasec_tag_waf_rule.test_tag_waf_rule 0000000:0000000
```

## Argument Reference

The following arguments are supported:
* `rule_id` (Computed) ID of the rule.
* `created` (Computed) Date of creation.
* `modified` (Computed) Date of last modification.
* `rule_type` (Computed) The type of the rule.
* `tag_id` (Required) The tag ID for the rule.
* `name` (Required) The rule name identifies each rule.
* `direction` (Required) Phase specifies the condition under which a rule applies. Pre-origin means before your server (request), post-origin is past your server (response). Valid values are `in` for request or `out` for response.
* `description` (Optional) Your notes on this rule. Default `""`.
* `log_identifier` (Optional) A comment to identify the matching rule in the access log. Default `""`.
* `expire_date` (Optional) Expire date schedules the deaktivation of the WAF rule. If none is set, the rule will be active until manual deactivation.
* `sort` (Optional) The order in which the rules take action. Default `1`.
* `process_next` (Optional) After a rule has been applied, the rule chain will be executed as determined. Default `false`.
* `enabled` Define wether this rule is enabled or not. (Optional) Default `true`.
* `conditions` (Required) All conditions of a rule have to be true for a rule to be executed. See below for argument reference.
* `actions` (Optional) Refers to actions that are executed when all conditions of a rule are true. See below for argument reference.

### WAF rule conditions arguments
* `conditions.condition_id` (Computed) ID of the WAF rule condition.
* `conditions.created` (Computed) Date of creation.
* `conditions.modified` (Computed) Date of last modification.
* `conditions.name` (Required)
* `conditions.matching_type` (Required)
    IREGEX - Pattern matching using case insensitive regex  
    REGEX - Pattern matching using case sensitive regex

    EXACT - String matching using the whole string verbatim  
    SUFFIX - String matching at the end  
    PREFIX - String matching from the beginning  
* `conditions.value` (Required)
* `conditions.key` (Depends on the type)

### WAF rule actions arguments
* `actions.action_id` (Computed) ID of the WAF rule action.
* `actions.created` (Computed) Date of creation.
* `actions.modified` (Computed) Date of last modification.
* `actions.type` (Required)
* `actions.value` (Required)
* `actions.custom_key` (Depends on the type)


## Available WAF condtions
### Valid conditions for `direction` = `in` (request)
```hcl
name = "accept|accept_encoding|fingerprint|host|method|querystring|remote_addr|url|user_agent"
matching_type = "EXACT|IREGEX|PREFIX|REGEX|SUFFIX"
value = "SOME VALUE"
```
```hcl
name = "score"
matching_type = "EQUALS|GREATER_THAN|LESS_THAN"
value = "1"
```
```hcl
name = "arg|cookie|custom_header|postarg"
matching_type = "EXACT|IREGEX|PREFIX|REGEX|SUFFIX"
key = "SOME KEY"
value = "SOME VALUE"
```
### Valid conditions for `direction` = `out` (response)
```hcl
name = "content_type|fingerprint|remote_addr|set_cookie"
matching_type = "EXACT|IREGEX|PREFIX|REGEX|SUFFIX"
value = "SOME VALUE"
```
```hcl
name = "score"
matching_type = "EQUALS|GREATER_THAN|LESS_THAN"
value = "1"
```
```hcl
name = "custom_header"
matching_type = "EXACT|IREGEX|PREFIX|REGEX|SUFFIX"
key = "SOME KEY"
value = "SOME VALUE"
```

## Available WAF actions
### Valid actions for `direction` = `in` (request)
```hcl
type = "change_upstream|remove_header"
value = "SOME VALUE"
```
```hcl
type = "add_header|modify_header|uri_subst"
custom_key = "SOME KEY"
value = "SOME VALUE"
```
```hcl
type = "origin_rate_limit"
custom_key = "1|2|5|10|15|30|45|60|120|180|300|600|1200|3600"
value = "1"
```
```hcl
type = "score"
custom_key = "+|-|*"
value = "1"
```
```hcl
type = "set_http_status"
custom_key = "301|302|404"
value = "SOME VALUE"
```
```hcl
type = "allow|block|log|verify_human"
```

### Valid actions for `direction` = `out` (response)
```hcl
type = "change_upstream|remove_header"
value = "SOME VALUE"
```
```hcl
type = "add_header|modify_header|uri_subst"
custom_key = "SOME KEY"
value = "SOME VALUE"
```
```hcl
type = "origin_rate_limit"
custom_key = "1|2|5|10|15|30|45|60|120|180|300|600|1200|3600"
value = "1"
```
```hcl
type = "score"
custom_key = "+|-|*"
value = "1"
```
```hcl
type = "set_http_status"
custom_key = "301|302|404"
value = "SOME VALUE"
```