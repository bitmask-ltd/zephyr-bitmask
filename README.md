# Bitmask Zephyr Module

This repository contains the Zephyr module for Bitmask products.

Below is an example of how to include the module in your Zephyr application `west.yml` file:

``` yaml
manifest:
  # ...
  remotes:
    # ...
    - name: bitmask
      url-base: https://github.com/bitmask-ltd
  projects:
    # ...
    - name: bitmask
      repo-path: zephyr-bitmask
      remote: bitmask
      revision: main
      path: modules/lib/bitmask
```

After adding, run `west update` to import the module.

## BMD100

The module contains the Zephyr board definition files for the BMD100 device. This board is available as a `bmd100` board target.

The valid board targets for `bmd100` are:

```
bmd100/nrf5340/cpuapp
bmd100/nrf5340/cpuapp/ns
bmd100/nrf5340/cpunet
```

### !! Do not enable the nRF5340's DCDC converter !!

The nRF5340 has an internal DCDC converter, however, it is unused on the BMD100. Instead, the BMD100 uses an LDO to power both the nRF5340 and the nRF21540. This mean that the nRF5340 does not possess the necessary passive components required for DCDC configuration.

Unfortunately, mistakenly enabling the nRF5340's DCDC converter is an easy way to soft-brick the device. See more: [Unbricking a nRF53 due to DCDC config](https://blog.mbedded.ninja/programming/microcontrollers/nordic/nrf53/unbricking-a-nrf53-due-to-dcdc-config/)

The `bmd100` board target correctly disables the DCDC converter. However, care must be taken to disable the DCDC converter when building applications for other board targets (such as the nRF5340-DK) which enable the DCDC converter by default. 