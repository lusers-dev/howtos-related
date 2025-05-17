# Linux Users - Flush specific FQDN in Infoblox by CLI


```shell

> set dns flush name foo.example.com "InternalView"

```

To explain the "set dns flush name foo.example.com 'InternalView'" Infoblox CLI command:

- **set dns flush**: This command is used to flush the DNS cache for a specific domain name.
- **name foo.example.com**: Specifies the domain name for which the DNS cache will be flushed. In this case, it is set to "foo.example.com".
- **"InternalView"**: Refers to the DNS view in which the domain resides. Flushing the DNS cache in a specific view ensures that any cached records related to the specified domain within that view are removed.

This command essentially clears the cached DNS records for the domain "foo.example.com" within the "InternalView" DNS view.
Usefull when doing fqdn changes/updates on existing records and the TTL has not expired. 



