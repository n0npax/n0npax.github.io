# State management

Please check this repo for full [description and code](https://github.com/n0npax/tf-foreach-vs-count)

## List vs set

In case you need to keep some resources in terraform using list like data structure please consider using set. Due to terraform design, in case you are going to update item in the beginning of the list, terraform will recreate all existing resources. If your data will be in set or map, terraform will only add new item.

list
```hcl
variable namespaces {
  type = list
}

resource "kubernetes_namespace" "set-example" {
  for_each = toset(var.namespaces)
  metadata {
    name = "set-${each.value}"
  }
}
```

set
```hcl
resource "kubernetes_namespace" "list-example" {
  count = length(var.namespaces)
  metadata {
    name = "list-${var.namespaces[count.index]}"
  }
}
```

State file will be different as:

set
```json
    {
          "index_key": "baz",
          "schema_version": 0,
          "attributes": {
```

list
```json
 {
          "index_key": 1,
          "schema_version": 0,
          "attributes": {
```