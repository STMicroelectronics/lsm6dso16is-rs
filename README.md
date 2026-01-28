# lsm6dso16is-rs
[![Crates.io][crates-badge]][crates-url]
[![BSD 3-Clause licensed][bsd-badge]][bsd-url]

[crates-badge]: https://img.shields.io/crates/v/lsm6dso16is-rs
[crates-url]: https://crates.io/crates/lsm6dso16is-rs
[bsd-badge]: https://img.shields.io/crates/l/lsm6dso16is-rs
[bsd-url]: https://opensource.org/licenses/BSD-3-Clause

Provides a platform-agnostic, no_std-compatible driver for the ST LSM6DSO16IS IMU, supporting both I2C and SPI communication interfaces.

## Sensor Overview

The LSM6DSO16IS is a system-in-package featuring a 3-axis digital accelerometer
and a 3-axis digital gyroscope, boosting performance at 0.59 mA in
high-performance
mode and enabling always-on low-power features for optimal motion results in
personal electronics and IoT solutions.

The LSM6DSO16IS has a full-scale acceleration range of ±2/±4/±8/±16 g and
an angular rate range of ±125/±250/±500/±1000/±2000 dps. The module features
programmable interrupts and an on-chip sensor hub which includes up to 6
sensors: the internal accelerometer & gyroscope and 4 external sensors.

The LSM6DSO16IS embeds a new ST category of processing, ISPU (intelligent
sensor processing unit) to support real-time applications that rely on sensor
data. The
ISPU is an ultra-low-power, high-performance programmable core which can
execute signal processing and AI algorithms in the edge. The main benefits of the ISPU
are C programming and an enhanced ecosystem with libraries and 3rd party tools/IDE.

Its optimized ultra-low-power hardware circuitry for real-time execution of the
algorithms is a state-of-the-art feature for any personal electronics, from
wearable accessories to high-end applications.

The LSM6DSO16IS is available in a plastic land grid array (LGA) package.

For more info, please visit the device page at [https://www.st.com/en/mems-and-sensors/lsm6dso16is.html](https://www.st.com/en/mems-and-sensors/lsm6dso16is.html)

## Installation

Add the driver to your `Cargo.toml` dependencies:

```toml
[dependencies]
lsm6dso16is-rs = "2.0.0"
```

Or, add it directly from the terminal:

```sh
cargo add lsm6dso16is-rs
```

## Usage

By default, the create exposes the **asynchronous** API, and it could be included using:
```rust
use lsm6dso16is_rs::asynchronous as lsm6dso16is;
use lsm6dso16is::*;
use lsm6dso16is::prelude::*;
```

### Blocking API (optional feature)

To use the **blocking** API instead of the asynchronous one, disable default features and enable the `blocking` feature in your Cargo.toml
```toml
[dependencies]
lsm6dso16is-rs = { version = "2.0.0", default-features = false, features = ["blocking"] }
```
or from the terminal:
```sh
cargo add lsm6dso16is-rs --no-default-features --features blocking
```

Then import the blocking API:
```rust
use lsm6dso16is_rs::blocking as lsm6dso16is;
use lsm6dso16is::*;
use lsm6dso16is::prelude::*;
```

### Create an instance

Create an instance of the driver with the `new_<bus>` associated function, by passing an I2C (`embedded_hal::i2c::I2c`) instance and I2C address, or an SPI (`embedded_hal::spi::SpiDevice`) instance, along with a timing peripheral.

An example with I2C:

```rust
let mut sensor = Lsm6dso16is::new_i2c(i2c, I2CAddress::I2cAddL, delay);
```

### Check "Who Am I" Register

This step ensures correct communication with the sensor. It returns a unique ID to verify the sensor's identity.

```rust
let whoami = sensor.device_id_get().unwrap();
if whoami != ID {
    panic!("Invalid sensor ID");
}
```

### Configure

See details in specific examples; the following are common api calls:

```rust
// Restore default configuration
sensor.software_reset().unwrap();

// Disable I3C interface (if needed)
// sensor.i3c_disable_set(0).unwrap();

// Enable Block Data Update (if needed)
sensor.block_data_update_set(1).unwrap();

// Set Output Data Rate for accelerometer and gyroscope
sensor.xl_data_rate_set(XlDataRate::_12_5hzHp).unwrap();
sensor.gy_data_rate_set(GyDataRate::_12_5hzHp).unwrap();

// Set full scale for accelerometer and gyroscope
sensor.xl_full_scale_set(XlFullScale::_2g).unwrap();
sensor.gy_full_scale_set(GyFullScale::_2000dps).unwrap();
```

## License

Distributed under the BSD-3 Clause license.

More Information: [http://www.st.com](http://st.com/MEMS).

**Copyright (C) 2025 STMicroelectronics**