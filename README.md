# Bitmask Zephyr Module

This repository contains a [Zephyr Module](https://docs.zephyrproject.org/latest/develop/modules.html) for Bitmask devices (BMD100).

## Setup

Include the module in your Zephyr application by updating your application `west.yml` as follows:

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

After updating the `west.yml` file, run `west update` to download and import the module.

## BMD100

The following targets for the BMD100 device are avaialble:
```
bmd100/nrf5340/cpuapp
bmd100/nrf5340/cpuapp/ns
bmd100/nrf5340/cpunet
```

### Sample Application

The [Nordic Peripheral UART Sample](https://docs.nordicsemi.com/bundle/ncs-latest/page/nrf/samples/bluetooth/peripheral_uart/README.html#peripheral-uart-sample-activating-variants) found at `nrf/samples/bluetooth/peripheral_uart`, can be used to quickly test the BLE functionality of the device.

#### Building & Flashing

``` shell
# Copy the Thingy:53 overlay files for BMD100
$ cp boards/thingy53_nrf5340_cpuapp.conf boards/bmd100_nrf5340_cpuapp.conf
$ cp boards/thingy53_nrf5340_cpuapp.overlay boards/bmd100_nrf5340_cpuapp.overlay
# Build the sample application
$ west build -b bmd100/nrf5340/cpuapp -p
# Flash the sample application
$ west flash
```

#### Testing
Follow instructions in the sample URL for more information.
- COM1 = Logging
- COM2 = nRF BLE UART

### !! Do not enable the nRF5340's DCDC converter !!

The nRF5340 has an internal DCDC converter, however, it is not used on the BMD100. Instead, the BMD100 uses an LDO to power both the nRF5340 and the nRF21540. This design decision means the nRF5340 does not possess the necessary passive components required for DCDC configuration.

Unfortunately, mistakenly enabling the nRF5340's DCDC converter is an easy way to soft-brick the device. See more: [Unbricking a nRF53 due to DCDC config](https://blog.mbedded.ninja/programming/microcontrollers/nordic/nrf53/unbricking-a-nrf53-due-to-dcdc-config/)

The `bmd100` board correctly disables the DCDC converter. However, care must be taken to disable the DCDC converter when building applications for other board targets (such as the nRF5340-DK) which enable the DCDC converter by default. 