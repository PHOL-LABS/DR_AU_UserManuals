# Connecting an Intake Manifold Pressure Sensor (MAP) to the DA2 Input

## Overview

The input stage of the pressure sensor on the Audi GFS board is
implemented as a differential amplifier using an operational amplifier.

The amplifier has:

-   positive input
-   negative input

The difference between these inputs forms the measured signal.

When no signal is present, the operational amplifier outputs a voltage
equal to half of the supply voltage:

Voffset ≈ 2.5 V

This acts as the measurement reference point.

------------------------------------------------------------------------

# Connector

The signal must be applied to the connector:

DA2

Required connector type:

WF-4 (2510-4P)

In the standard board configuration the connector is not installed. Only
the footprint is present.

------------------------------------------------------------------------

# Sensor connection

A typical MAP sensor outputs:

0 ... 5 V

However the input stage has a limited dynamic range, therefore the
signal must be attenuated before it is applied.

A resistive divider is used for this purpose.

------------------------------------------------------------------------

# Voltage divider

Recommended circuit:

MAP sensor output \| \|---- R1 = 90k \| +----\> DA2 OUT+ \| \|---- R2 =
10k \| GND

Divider ratio:

K = R2 / (R1 + R2)\
K = 10k / 100k\
K = 0.1

Resulting signal range:

0 ... 5 V → 0 ... 0.5 V

------------------------------------------------------------------------

# Amplifier output range

Measured data:

  Vin      Vout
  -------- ---------
  153 mV   2.573 V
  279 mV   2.835 V
  365 mV   3.000 V

With the divider applied:

Vin = 0 ... 0.5 V

Expected amplifier output range:

Vout ≈ 2.25 ... 3.27 V

------------------------------------------------------------------------

# ADC range

Assuming the ADC parameters:

Resolution: 12-bit\
Reference: 3.3 V

Conversion formula:

ADC = V / 3.3 \* 4095

Expected ADC values:

Vmin ≈ 2.25 V → ADC ≈ 2790\
Vmax ≈ 3.27 V → ADC ≈ 4055

Usable measurement range:

ADC ≈ 2800 ... 4050

------------------------------------------------------------------------

# Calibration

Each MAP sensor has its own transfer function.

After digitizing the signal the system must be calibrated:

1.  measure ADC values at several known pressures
2.  build the relation:

Pressure = f(ADC)

3.  apply linear or polynomial fitting.

------------------------------------------------------------------------

# Final circuit

MAP sensor (0--5V) \| \| R1 90k +----///----+ \| +---- DA2 OUT+ \|
+----///----+ \| R2 10k GND

The divided signal is applied to the differential amplifier input and
then digitized by the microcontroller ADC.
