Theory of Operation
===================

References
----------

1. Aaron Lanterman, "ECE4450 L4.1: Voltage Controlled Amplifiers" [`youtube <https://www.youtube.com/watch?v=96j2tNKFCPI>`_]


Linear V-to-I Converter
-----------------------


.. image:: ./_static/images/lin_voltage_to_current.png
    :width: 480px

Following the derivation for the linear V-to-I converter, the output current :math:`I_{out}` is found assuming

* an ideal op-amp (:math:`v_n = v_p = 0`)
* negligible base current (:math:`i_e = i_c + i_b \approx i_c`)

such that

.. math::

    0 &= i_e + v_1 \left(\frac{1}{R_2} + \frac{1}{R_3}\right) \\
    0 &= \frac{v_{in}}{R_1} + \frac{v_1}{R_2} \\
    \to i_c \approx i_e &= v_{in}\left(\frac{R_2}{R_1}\right)\left(\frac{R_2 + R_3}{R_2 R_3}\right) \\
    \to i_c &= v_{in}\left(\frac{R_2 + R_3}{R_1 R_3}\right)

The input CV is assumed to be a positive envelope with a range from 0 to 10V. The control current for the OTA should not exceed 1mA. The input impedance is :math:`R_1 = 100k\Omega`. With these constraints, 

.. math::

    \to 0.001 &= 10\left(\frac{R_2 + R_3}{10^{5} R_3}\right) \\
    \to 10 &= \frac{R_2}{R_3} + 1 \\
    \to R_2 &= 9 R_3

Choosing :math:`R_3 = 12k\Omega` and :math:`R_2 = 100k\Omega` ensures that the current is limited to less than 1mA. A diode in the feedback loop ensures that negative voltages are not passed to the BJT. The gain of the CV stage is then

.. math::

    \frac{i_c}{v_{in}} = \frac{R_2 + R_3}{R_1 R_3} = 93.3 \frac{\mu A}{V}

and at an input level of 5V, the output current is 0.467mA. 

OTA and Output Buffer
---------------------

.. image:: ./_static/images/OTA_vin_to_vout.png
    :width: 480px

For the OTA, the transconductance is

.. math::

    g_m = \frac{i_{abc}}{2 V_T} \simeq 19.2 i_{abc}

where :math:`i_{abc}` is the amplifier bias current and the thermal voltage :math:`V_T = 25.6` mV at room temperature. The output current is given by

.. math::

    i_{out} &= g_m (v_{p} - v_n) \\
    &= 19.2 i_{abc} (v_{p} - v_n) \\

The amplifier bias current should fall in the range of 1uA to 1mA. 

For this analysis, assume that :math:`v_p = 0` (the non-inverting input can be trimmed to remove any DC offset). Consequently, the output current is :math:`i_{out} = -19.2 i_{abc} v_n`, which then passes through the resistor :math:`R` in the feedback loop of the op-amp to produce a voltage output of (two inverting stages cancel the sign)

.. math::

    v_{out} = 19.2 i_{abc} v_n R 

A voltage divider is used to reduce the input signal voltage: :math:`v_n = \frac{R_2}{R_1 + R_2} v_{in}` with :math:`R_2 \ll R_1`. The linear region for the OTA is nominally in the range where :math:`|v_{p} - v_{n}| < 10mV`. Assuming a 10Vpp input signal, choose the ratio for :math:`R_2` and :math:`R_1` 

.. math::

    0.01 &= \frac{R_2}{R_1} 5 \\
    \to R_1 &= 500 R_2

For :math:`R_1 = 100k\Omega`, :math:`R_2 = 220\Omega` will approximately satisfy this condition. The gain of the OTA stage is then

.. math::

    v_{out} &\simeq 19.2 \frac{220}{10^5} i_{abc} R v_{in} \\
    &= 0.04224 i_{abc} v_{in} R

This design will assume unity gain for the audio signal with a 5V CV input. With a 5V CV input, :math:`i_{abc} = 0.467 mA`, 

.. math::

    \frac{v_{out}}{v_{in}} = 1 &= 0.04224 \times 0.467(10^{-3}) R \\
    \to R &\simeq 50.7 k\Omega

Letting :math:`R = 47k\Omega` should be close enough here.


Measurement and Calibration
---------------------------

Calibration: apply a test signal (440Hz sine, 8Vpp) to the audio input and a DC 1V level to the CV input. Adjust the trim pot until the output is balanced.

Measurement: apply a test signal (440Hz sine, 8Vpp) to the audio input and vary the DC level to the CV input.

.. list-table:: Gain Response
    :header-rows: 1

    * - CV (V)
      - Output Amplitude (Vpp)
      - Gain (V/V)
    * - 1.05
      - 1.62
      - 0.202
    * - 2.02
      - 3.02
      - 0.378
    * - 3.09
      - 4.6
      - 0.575
    * - 4.08
      - 6.00
      - 0.750
    * - 4.51
      - 6.60
      - 0.825

The linear regression of the gain response is 

.. math::

    A = 0.014 + 0.18V

where :math:`A` is the gain in V/V and :math:`V` is the CV input in Volts. With :math:`R = 47k\Omega` in the output stage, the gain at 5V CV input is expected to be 0.927 V/V. The measured gain at 5V CV input is 0.914 (within 2% of expected). Using a 49.9k resistor for R would get a bit closer to the design target.

.. image:: ./_static/images/gain_vs_CV_2025-06-19.png
    :width: 480px

Linear to Exponential Conversion
--------------------------------

**References**

1. Aaron Lanterman, "ECE4450 L18: Exponential Voltage-to-Current Conversion", [`youtube <https://www.youtube.com/watch?v=ZWJhApUmfEU>`_]
2. Ken Stone, "R6 Gate (VCA)/Ring Modulator" (drawn from the Serge R6), [`archive <https://web.archive.org/web/20211206140530/http://serge.synth.net/modules/r6_mod/index.html>`_]
3. Ray Wilson, "Dual Log/Linear VCA", [`MFOS <https://musicfromouterspace.com/analogsynth_new/DUALVCA/DLLVCA001.html>`_]
4. Rene Schmitz, "A tutorial on exponential converters and temperature compensation", [`schmitzbits.de <https://schmitzbits.de/expo_tutorial/index.html>`_]
5. Hal Chamberlain, "Musical Applications of Microprocessors", 2nd ed., Hayden Books, 1987 

This section is an effort to collect theory and derivations related to linear to exponential conversion circuits. There are a few conventions to be aware of:

* *Exponential* behaviour in circuits is often refered to as "logarithmic," e.g. "log/lin VCA". Mathemtically, it's an exponential function.
* Voltage controlled amplifier (VCA) circuits are often referred to as "gates."
* "Ring modulators" are equivalent to a mulitplier. The name comes from the ring of diodes used in some implementations (e.g. frequency mixers in radio applications.).  



