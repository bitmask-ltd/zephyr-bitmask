# Bitmask Zephyr Module

This repository contains the Zephyr module for Bitmask products.

Below is an example of how to include the module in your Zephyr application `west.yml` file:

``` yaml
manifest:
  self:
    path: application
  remotes:
    - name: ncs
      url-base: https://github.com/nrfconnect
    - name: bitmask
      url-base: git@github.com:bitmask-ltd
  projects:
    - name: nrf
      repo-path: sdk-nrf
      remote: ncs
      revision: main
    - name: bitmask
      repo-path: zephyr-bitmask
      remote: bitmask
      path: modules/lib/bitmask
      revision: main
```

After adding, run `west update` to import the module.

## BMD100

The module currently contains the Zephyr board definition files for the BMD100 board.

This board is available as a board target with `bmd100`.

