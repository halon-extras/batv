# BATV

This example provides a [BATV](http://mipassoc.org/batv/draft-levine-smtp-batv-01.txt) (Bounce Tag Address Validation) implementation, our [documentation](https://docs.halon.io/kb/signing-and-validation/batv/) shows how to implement it properly.

## Installation

Follow the [instructions](https://docs.halon.io/manual/comp_install.html#installation) in our manual to add our package repository and then run the below command.

### Ubuntu

```
apt-get install halon-extras-batv
```

### RHEL

```
yum install halon-extras-batv
```

## Exported functions

These functions needs to be [imported](https://docs.halon.io/hsl/structures.html#import) from the `extras://batv` module path.

### batv_sign(address, key, opts)

Sign a email address.

**Params**

- address `string` - The email address
- key `string` - The key used for signing
- opts `array` - options array

**Returns**: A `string` containing a BATV signed email address

The following options are available in the **opts** array.

- keyid `number` - A key id between 0-9. The default is 0.
- days `number` - Days the tag should be valid. The default is 7 days.

### batv_verify(address, keys, opts)

Verify a BATV signed email address.

**Params**

- address `string` - The email address
- keys `array` - Array of key to validate (indexed by keyid)
- opts `array` - options array

**Returns**: A `string` containing the BATV result.

The following options are available in the **opts** array.

- days `number` - Days the tag is valid. The default is 7 days.

The result string may be any of

| Result   | Status  | Description                                                           |
|----------|---------|-----------------------------------------------------------------------|
| pass     | good    | The BATV tag is valid! Message should be batv_stripped and delivered. |
| missing  | neutral | No BATV tag                                                           |
| invalid  | bad     | BATV tag is invalid                                                   |
| checksum | bad     | BATV tag is bad (invalid key)                                         |
| expired  | bad     | BATV tag has expired                                                  |

### batv_strip(address)

Strip the BATV tag from an email address, this is the inverse process of batv_sign. If the address is not signed, it's returned as is.

**Params**

- address `string` - The email address

**Returns**: A `string` containing the original email address.
