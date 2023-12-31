# Berry-IMU
This module provides an implementation of the [BerryIMUv3](https://ozzmaker.com/product/berryimu-accelerometer-gyroscope-magnetometer-barometricaltitude-sensor/) using an i2c connection on ARM64 systems,
and is [available on the Viam registry](https://app.viam.com/module/viam-labs/berryimu).

## Setup

###  Wiring
Ensure i2c communication is enabled on your single board computer. If you are using a Raspberry Pi, see [Enable communications protocols](https://docs.viam.com/installation/prepare/rpi-setup/#enable-communication-protocols).
- Wire the SCL and SDA pins on the board to the corresponding pins on the IMU. The IMU can be powered with the 3.3v pin.
- You can verify the i2c connection by running `i2cdetect -y 1` on your board. If the IMU is wired correctly, you will see the i2c addresses `1c` and `6a`. 

### Add the berryimu module to a robot
To use this module, follow the instructions to [add a module from the Viam Registry](https://docs.viam.com/registry/configure/#add-a-modular-resource-from-the-viam-registry). Search for the `berryimu` movement sensor.
Click **Add module**, choose a name, and click **Create** to add the `berryimu` module and the modular movement sensor it provides.
You can also [run the module locally](https://docs.viam.com/registry/configure/#local-modules).


# Calibration
The BerryIMU's magnetometer needs to be calibrated prior to using this module. Running the calibration script in `src/calibration.py`
will provide configuration values to correct for soft and hard iron distortions. 

To run the calibration script, you must install the Viam Python SDK on your single board computer:

```
pip install viam-sdk
```

Then, clone the BerryIMU module and navigate to the `src` directory, and run the calibration script using the following command:

```sh
python3 calibration.py
```

The script will prompt you to rotate the IMU in all directions. Make sure you are rotating the IMU 360 degrees in the X, Y, and Z directions 
to ensure an accurate calibration. Keep rotating the IMU until the script outputs the calibration values. Copy and paste these values into your
IMU component config in the [Viam app](https://app.viam.com/robots).

# Attributes and Sample Config 
The attributes for the module are as follows:
   - `i2c_bus`: (**Required**) The name of the i2c bus wired to the IMU.
   - `calibrate`: Set to true to calibrate the magnetometer using the min and max values.
   - `hard_iron_x_max`, `hard_iron_y_max`, `hard_iron_x_min`, `hard_iron_y_min`, `soft_iron_x_max`, `soft_iron_y_max`, `soft_iron_x_min`, `soft_iron_y_min`: (**Required** when calibrate is `true`) The calibration values obtained from running the `calibration.py` script.

```
    {
    "modules": [
    {
      "type": "registry",
      "name": "berryimu",
      "module_id": "viam-labs:berryimu",
      "version": "0.0.2"
    }
    ],
      "components": [
    {
      "name": "myberryimu",
      "namespace": "rdk",
      "type": "movement_sensor",
      "model": "viam-labs:movement_sensor:berryimu",
      "attributes": {
        "hard_iron_y_max": 1953,
        "soft_iron_x_max": 1069,
        "hard_iron_x_min": -1953,
        "hard_iron_x_max": 118,
        "soft_iron_x_min": -1069,
        "soft_iron_y_max": 1069,
        "soft_iron_y_min": -1056,
        "hard_iron_y_min": -1029,
        "calibrate": true,
        "i2c_bus": "1"
      },
      "depends_on": []
            }
        ]
    }
```

# Troubleshooting
If you see `OSError: 5` something is likely wrong with the i2c connection. Double check your wiring and ensure the `i2cdetect` command shows
the device's i2c addresses.

# Relevant Links
- [BerryIMU v3](https://ozzmaker.com/product/berryimu-accelerometer-gyroscope-magnetometer-barometricaltitude-sensor/)
- [LSM6DSL Accelerometer and gyroscope datasheet](https://ozzmaker.com/wp-content/uploads/2020/08/lsm6dsl-datasheet.pdf)
- [LIS3MDL Magnetometer datasheet](https://ozzmaker.com/wp-content/uploads/2020/08/lis3mdl.pdf)


