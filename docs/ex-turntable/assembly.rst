.. include:: /include/include.rst
.. include:: /include/include-l1.rst
.. include:: /include/include-ex-tt.rst
|EX-TT-LOGO|

****************
Assembly & Setup
****************

|tinkerer| |engineer| |support-button| |githublink-ex-turntable-button2|

.. sidebar:: 

  .. contents:: On this page
    :depth: 3
    :local:

Assembly
========

For assembly, we will assume the default ULN2003/28BYJ-48 combo is in use with an Arduino Nano V3, a standard 3 pin Arduino compatible hall effect sensor, and a dual relay board.

We will also assume a prototyping shield is available that provides regulated 5V power sufficient for driving the ULN2003/28BYJ-48 stepper combo, and that there is a power supply with a suitable DC power plug to suit the prototyping shield.

Throughout the assembly process, you can refer to this Fritzing diagram to help validate your connections are correct (open this image in a new tab or window and zoom in to see the detail):

.. image:: /_static/images/ex-turntable/assembly.png
  :alt: Fritzing Diagram
  :scale: 25%

.. sidebar:: Using prototype or strip boards

  |tinkerer| |engineer|

  For the Tinkerers and Engineers, a much neater solution is to use a prototyping or strip board with much shorter (and soldered) connections to ensure reliability of the connections.

Connection summary
------------------

Summary table of all connections required during assembly:

.. list-table::
    :widths: auto
    :header-rows: 1
    :class: command-table

    * - Device Pin
      - Arduino Pin
      - Nano Shield Pin
    * - ULN2003 IN1
      - A0
      - A0 S
    * - ULN2003 IN2
      - A1
      - A1 S
    * - ULN2003 IN3
      - A2
      - A2 S
    * - ULN2003 IN4
      - A3
      - A3 S
    * - ULN2003 \+
      - 5V
      - A0 V
    * - ULN2003 \-
      - GND
      - A0 G
    * - Hall effect \- (Left)
      - GND
      - 5 G
    * - Hall effect Unmarked (middle)
      - 5V
      - 5 V
    * - Hall effect S (Right)
      - 5
      - 5 S
    * - Dual relay VCC
      - 5V
      - 3 V
    * - Dual relay GND
      - GND
      - 3 G
    * - Dual relay IN1
      - 3
      - 3 S
    * - Dual relay IN2
      - 4
      - 4 S
    * - CommandStation 20 (SDA)
      - A4
      - A4 S or SDA
    * - CommandStation 21 (SCL)
      - A5
      - A5 S or SCL
    * - CommandStation GND
      - GND
      - A4 G or |I2C| GND

Of course for the Tinkerers and Engineers, if you're not using a Nano or a prototyping shield, adapt the details as suits your configuration.

Using a two wire stepper driver (e.g. A4988/DRV8825/TMC2208)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For those using a NEMA17 or similar stepper motor with a two wire type driver (e.g. A4988 or DRV8825), then the four Arduino pins A0 - A3 map like this, with the rest remaining the same as the table above:

.. list-table::
  :widths: auto
  :header-rows: 1
  :class: command-table

  * - Device Pin
    - Arduino Pin
    - Nano Shield Pin
  * - A4988/DRV8825 Step pin
    - A0
    - A0 S
  * - A4988/DRV8825 Dir pin
    - A1
    - A1 S
  * - A4988/DRV8825 En pin
    - A2
    - A2 S
  * - N/A
    - A3 (not connected)
    - A3 S (not connected)

Note when using an A4988 stepper driver, you must connect the RESET (RST) and SLEEP (SLP) pins together.

.. note:: 

  When utilising a two wire driver such as the A4988, DRV8825, or TMC2208, you should adjust the current limiting value to suit your specific requirements. This typically requires measuring the voltage and adjusting a potentiometer on the driver board. A good guide on making this adjustment is at `circuitist.com <https://www.circuitist.com/how-to-set-driver-current-a4988-drv8825-tmc2208-tmc2209/>`_.

Further to this, when using a stepper motor such as the NEMA17 with a two wire driver, you should also incorporate a 47uF electrolytic capacitor as physically close to the stepper driver motor power input as possible to protect from surges generated by the stepper motor.

.. figure:: /_static/images/ex-turntable/a4988-nema17-nano.png
  :alt: A4988 NEMA Option
  :scale: 25%

  Connections for an A4988 stepper driver with NEMA17 stepper motor

.. figure:: /_static/images/ex-turntable/drv8825-nema17-nano.png
  :alt: DRV8825 NEMA Option
  :scale: 25%

  Connections for a DRV8825 stepper driver with NEMA17 stepper motor

Credit to Dejan at HowToMechatronics.com for outlining the connections for a TMC2208 stepper driver with a NEMA17 stepper motor along with the microsteps table included below. Also note that with the TMC2208 driver, it typically rotates the stepper in the reverse direction to the A4988 and DRV8825, and likely requires inverting the DIR pin in order for it to rotate in the same direction. This is possible via a configuration option introduced in |EX-TT| version 0.7.0, see :ref:`ex-turntable/configure:invert_direction`.

.. figure:: /_static/images/ex-turntable/tmc2208.jpg
  :alt: TMC2208
  :scale: 30%

  Connections for a TMC2208 stepper driver with NEMA17 stepper motor

1. BEFORE you start
--------------------

Gather all your components and visually check them all for any obvious damage, paying particular attention to pins on the Arduino to make sure they are straight.

.. image:: /_static/images/ex-turntable/components.png
  :alt: Components
  :scale: 50%

.. image:: /_static/images/ex-turntable/check-pins.png
  :alt: Nano Pins
  :scale: 50%

2. Insert the Nano into the shield
----------------------------------

Insert the Nano into the prototype shield socket, taking care to ensure the USB socket is located at the same end as the DC power jack, and that all pins are straight and aligned correctly with the female headers.

The various pin numbers may also be printed on the prototyping shield to confirm the correct orientation.

.. image:: /_static/images/ex-turntable/insert-nano.png
  :alt: Insert Nano
  :scale: 50%

.. image:: /_static/images/ex-turntable/nano-inserted.png
  :alt: Nano Inserted
  :scale: 50%

At this point, it's a good idea to take careful note of the various pin markings on your prototype shield as it's critical that these are correct when connecting the various components.

With the shield used in these assembly photos, you will note that each of the Nano GPIO pins has three pins associated with it marked "G" for ground, "V" for 5V, and "S" for signal, with this last pin being the actual Nano GPIO pin.

.. image:: /_static/images/ex-turntable/proto-shield-pins.png
  :alt: Prototype Shield Pins
  :scale: 50%

3. Connect the stepper controller and motor
-------------------------------------------

Firstly, note that the ULN2003 controller will have four pins marked "IN1" through "IN4", as well as a pair of pins with "+" and "-". There is a likely a jumper installed across two pins beside these that is unmarked, leave this in place.

You will need to connect six of the female to female Dupont wires from the ULN2003 pins to the Arduino prototype shield as below:

.. list-table::
    :widths: auto
    :header-rows: 1
    :class: command-table

    * - ULN2003 Pin
      - Nano Shield Pin
    * - IN1
      - A0 S
    * - IN2
      - A1 S
    * - IN3
      - A2 S
    * - IN4
      - A3 S
    * - \+
      - A0 V
    * - \-
      - A0 G
  
.. image:: /_static/images/ex-turntable/uln2003-pins.png
  :alt: ULN2003 Pins
  :scale: 40%

.. image:: /_static/images/ex-turntable/shield-uln2003-pins.png
  :alt: Shield to ULN2003 pins
  :scale: 50%

Insert the stepper motor connector into the receptacle on the ULN2003 controller. Note that it will only go in one way, so check the orientation and simply plug it in.

.. image:: /_static/images/ex-turntable/28byj-48-connector1.png
  :alt: 28BYJ-48 Connector
  :scale: 50%

.. image:: /_static/images/ex-turntable/28byj-48-connector2.png
  :alt: 28BYJ-48 Connector
  :scale: 50%

4. Connect the hall effect sensor
---------------------------------

The hall effect sensor has three pins, and likely only two of these pins are marked, the left with "-" and right with "S". The middle pin is likely to be unmarked, and will be the 5V pin. There are probably many different varieties of sensors and designs out there, but both that I have (from different suppliers) are marked identically.

Use three of the Dupont wires and connect these from the hall effect sensor to the Arduino prototype shield as below:

.. list-table::
    :widths: auto
    :header-rows: 1
    :class: command-table

    * - Hall Effect Pin
      - Nano Shield Pin
    * - \- (Left)
      - 5 G
    * - Unmarked (middle)
      - 5 V
    * - S (Right)
      - 5 S

.. image:: /_static/images/ex-turntable/hall-effect-pins.png
  :alt: Hall Effect Pins
  :scale: 50%

.. image:: /_static/images/ex-turntable/hall-effect-shield.png
  :alt: Hall Effect to Shield
  :scale: 50%

5. Connect the dual relay board
-------------------------------

Note there should be six pins on the dual relay board marked "VCC", "GND", "IN1", "IN2", "COM", and "GND". The "COM" and "GND" pins should have a jumper installed to connect these together. Leave this in place.

Use four Dupont wires to connect the other four pins as below:

.. list-table::
    :widths: auto
    :header-rows: 1
    :class: command-table

    * - Dual Relay Pin
      - Nano Shield Pin
    * - VCC
      - 3 V
    * - GND
      - 3 G
    * - IN1
      - 3 S
    * - IN2
      - 4 S

.. image:: /_static/images/ex-turntable/dual-relay-pins.png
  :alt: Dual Relay Pins
  :scale: 50%

.. image:: /_static/images/ex-turntable/dual-relay-shield-pins.png
  :alt: Dual Relay to Shield Pins
  :scale: 50%

6. Connect power and test
-------------------------

At this point, it should be safe to plug in the power supply to the DC power jack on the prototyping shield.

When the power supply is turned on, the power LEDs on the Arduino Nano and dual relay board should be lit. Note there is likely no power LED on the ULN2003 stepper controller, and testing of this will require loading the |EX-TT| software on to the Nano in step 7 below.

.. image:: /_static/images/ex-turntable/power-on.png
  :alt: Powered On
  :scale: 50%

To validate the hall effect sensor is connected correctly, put a magnet in close proximity (within a millimetre or so) of the sensor IC, and the onboard LED should light up.

.. image:: /_static/images/ex-turntable/hall-effect-inactive.png
  :alt: Hall Effect Inactive
  :scale: 50%

.. image:: /_static/images/ex-turntable/hall-effect-active.png
  :alt: Hall Effect Active
  :scale: 50%

7. Load the EX-Turntable software
---------------------------------

.. tip:: 

  Please read through this entire section prior to loading any software onto your Arduino. It is also recommended that the turntable is able to trigger the homing sensor correctly to ensure the automatic calibration works correctly at first startup.

Installing with EX-Installer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

|EX-I| can be used to install both |EX-CS| and |EX-TT|. The process is the same for both, with the exception of the configuration options, therefore we will only outline the configuration options here. Refer to :doc:`/ex-installer/installing` for the full documentation on using |EX-I|.

When you reach the "Select Product" screen, select |EX-TT|.

.. figure:: /_static/images/ex-installer/select_product.png
   :alt: EX-Installer - Select Product
   :scale: 40%
   :align: center

   EX-Installer - Product Screen

We always recommend selecting the latest available version for |EX-TT| unless advised otherwise, but note you will only see Development versions while it remains in Beta testing.

.. figure:: /_static/images/ex-installer/select_tt_version.png
   :alt: EX-Installer - Select Version
   :scale: 40%
   :align: center

   EX-Installer - Product Screen

Once the version has been selected, you will be able to configure the necessary options.

.. figure:: /_static/images/ex-installer/ex_turntable.png
   :alt: EX-Installer - Configure EX-Turntable
   :scale: 40%
   :align: center

   EX-Installer - EX-Turntable configuration

If you have a need to configure any other settings, you can enable ``Advanced Config`` and edit the config file on the following page. You will need to do this if you intend to manually specify the steps per rotation for the stepper.

.. figure:: /_static/images/ex-installer/ex_turntable_advanced.png
   :alt: EX-Installer - EX-Turntable advanced config
   :scale: 40%
   :align: center

   EX-Installer - EX-Turntable Advanced Config

Continue through the rest of the |EX-I| process to load the software on to your device, then skip to :ref:`ex-turntable/assembly:first start and automatic calibration` to continue.

Installing with the Arduino IDE
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

  Further to this, note that you will need to end up with two separate folders; one containing the |EX-CS| software as per  :doc:`/ex-commandstation/advanced-setup/installation-options/arduino-ide`, and an additional folder containing the |EX-TT| software. The |EX-TT| software is not a component of |EX-CS| or vice versa, and as such they should not exist in the same folder.

We recommend using |EX-I| to install |EX-TT| as outlined above, however you can use the Arduino IDE to load the software onto the Arduino manually.

As noted in the tip above, you should have a |EX-TT| folder alongside the |EX-CS| folder, and neither should reside in the other (the |EX-TT| software is required in the next step):

.. image:: /_static/images/ex-turntable/two-folders.png
  :alt: Two folders
  :scale: 60%

The process here is the same as installing CommandStation-EX via the Arduino IDE which you can find on the :doc:`/ex-commandstation/advanced-setup/installation-options/arduino-ide` page.

When you get to the point of opening the sketch, ensure you open the EX-Turntable sketch:

.. image:: /_static/images/ex-turntable/open-turntable-ex-sketch.png
  :alt: Open EX-Turntable sketch
  :scale: 60%

Use Windows Explorer to either copy or rename "config.example.h" to "config.h".

If you need to make adjustments to config.h, refer to the :doc:`/ex-turntable/configure`.

Set the board type to "Nano" and set the correct Processor type (typically ATMega328P):

.. image:: /_static/images/ex-turntable/select-nano.png
  :alt: Select Nano
  :scale: 60%

After any adjustments are made and "config.h" has been created, the software can be uploaded to the Arduino with the upload button:

.. image:: /_static/images/arduino-ide/upload_arrow.jpg
  :alt: Upload
  :scale: 70%

Once the software is loaded successfully on to |EX-TT|, the stepper motor should automatically start rotating in an attempt to find its "home" position, which will be activated when the magnet at one end of the turntable comes in close proximity to the hall effect sensor.

If you don't have the magnet installed at this point, or if it is too far from the sensor, |EX-TT| will rotate several turns prior to flagging that homing has failed, and will then cease turning. The automatic calibration process will not commence if homing has failed.

If your testing of the hall effect sensor in step 6 above succeeded, then the issue is likely to be the distance the magnet is from the sensor, and this will require adjustment. See :doc:`/support/ex-tt-troubleshooting` for further assistance if required.

Configuration for two wire stepper drivers (e.g. A4988/DRV8825)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If using a two wire stepper driver such as the A4988 or DRV8825 with a bipolar stepper motor such as a NEMA17 or similar, you will need to update "config.h" to reflect this.

While the provided "config.example.h" file includes instructions, they are repeated here for clarity, as well as being outlined on the :doc:`/ex-turntable/configure` page.

Locate this section in "config.h", comment out the line defining the use of "ULN2003_HALF_CW" by adding ``//``, and uncomment the line defining the use of "A4988" by removing ``//``:

.. code-block:: cpp

  /////////////////////////////////////////////////////////////////////////////////////
  //  Define the stepper controller in use according to those available below, refer to the
  //  documentation for further details on which to select for your application.
  // 
  //  ULN2003_HALF_CW     : ULN2003 in half step mode, clockwise homing/calibration
  //  ULN2003_HALF_CCW    : ULN2003 in half step mode, counter clockwise homing/calibration
  //  ULN2003_FULL_CW     : ULN2003 in full step mode, clockwise homing/calibration
  //  ULN2003_FULL_CCW    : ULN2003 in full step mode, counter clockwise homing/calibration
  //  A4988               : Two wire drivers (e.g. A4988, DRV8825)
  //  A4988_INV           : Two wire drivers (e.g. A4988, DRV8825), with enable pin inverted
  // 
  //  NOTE: If you are using a different controller than those already defined, refer to
  //  the documentation to define the appropriate configuration variables. Note there are
  //  some controllers that are pin-compatible with an existing defined controller, and
  //  in those instances, no custom configuration would be required.
  // 
  // #define STEPPER_DRIVER ULN2003_HALF_CW
  // #define STEPPER_DRIVER ULN2003_HALF_CCW
  // #define STEPPER_DRIVER ULN2003_FULL_CW
  // #define STEPPER_DRIVER ULN2003_FULL_CCW
  #define STEPPER_DRIVER A4988
  // #define STEPPER_DRIVER A4988_INV   <--- Versions before 0.7.0
  // #define INVERT_STEP                <--- Version 0.7.0 on

.. note:: 

  If operating EX-Turntable does not disable the stepper driver after movements complete, you will likely hear a buzzing or humming from the driver. In this instance, you may find you need to have the "Enable" pin inverted. In versions prior to 0.7.0, you will need to use the "A4988_INV" option instead (``#define STEPPER_DRIVER A4988_INV``), and from version 0.7.0, you will need to enable the "INVERT_ENABLE" option (``#define INVERT_ENABLE``).

First start and automatic calibration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: 

  If you have loaded the code too soon, and the automatic calibration has succeeded and recorded an inaccurate step count, then have no fear as there is a command you can run on the CommandStation to reinitiate the calibration sequence which is outlined in the :ref:`ex-turntable/test-and-tune:ex-turntable commands` section.

  As of v0.5.0-Beta and up to version 0.6.0, you can also execute the command ``<0 3>`` in the serial console to initiate the calibration sequence. In version 0.7.0 you can execute ``<C>`` instead to intiate the sequence.

  Also, if you have enabled the `FULL_STEP_COUNT` option in "config.h", that will prevent automatic calibration occurring, refer to :ref:`ex-turntable/configure:full_step_count`.

When |EX-TT| is first loaded onto your Arduino, and it has successfully performed the homing process outlined above, it will commence an automatic calibration sequence. This involves several rotations of the turntable to ensure it is homed accurately, and is then able to count the steps required to complete a full rotation of the turntable.

Once the calibration sequence has completed, it will display the step count for an entire rotation, which you should take note of for calculating the various positions in :ref:`ex-turntable/test-and-tune:tuning your turntable positions`.

On the first start, the output in the serial console should look similar to the below:

.. code-block::

  License GPLv3 fsf.org (c) dcc-ex.com
  EX-Turntable version 0.5.0-Beta
  Available at I2C address 0x60
  EX-Turntable in TURNTABLE mode
  EX-Turntable has not been calibrated yet
  Automatic phase switching enabled at 45 degrees
  Phase will switch at 0 steps from home, and revert at 0 steps from home
  Calibrating...
  Homing started
  Turntable homed successfully
  CALIBRATION: Phase 1, homing...
  CALIBRATION: Phase 2, counting full turn steps...
  CALIBRATION: Completed, storing full turn step count: 4100                    <<== This is the full turn step count to record for later reference
  EX-Turntable has been calibrated for 4100 steps per revolution
  Automatic phase switching enabled at 45 degrees
  Phase will switch at 495 steps from home, and revert at 2475 steps from home
  Turntable homed successfully

At this point, the full turn step count is written to the Arduino's EEPROM so that it can be retrieved each time |EX-TT| starts up, preventing the need to repeat the calibration sequence at each subsequent start.

You can now safely power off |EX-TT| and remove the USB cable from your PC as it is no longer required for normal operation, and all further commands will be issued by the CommandStation.

8. Add the EX-Turntable device driver to EX-CommandStation
----------------------------------------------------------

Add with EX-Installer
^^^^^^^^^^^^^^^^^^^^^

If you are using |EX-I| to load software, you can add your |EX-TT| device and routes on the Advanced Configuration screen by adding them to your myAutomation.h file.

.. figure:: /_static/images/ex-installer/ex_cs_advanced_turntable.png
   :alt: EX-Installer - Configure EX-Turntable in myAutomation.h
   :scale: 40%
   :align: center

   EX-Installer - Add EX-Turntable support in myAutomation.h

Add manually
^^^^^^^^^^^^

.. note:: 

  As mentioned previously, your CommandStation needs to be running |EX-CS| version 5.0.0 or later.

  If you receive compile errors that the file "IO_EXTurntable.h" is missing when attempting to upload the CommandStation software later in this process, this indicates you are using the incorrect version of |EX-CS|.

  Previous versions of this documentation referred to editing "myHal.cpp". While this is possible, the recommended method is to define your device in "myAutomation.h" instead.

Before you will be able to test or use |EX-TT|, you need to configure the |EX-CS| software to load the appropriate device driver.

This requires creating or editing the myAutomation.h file in the |EX-CS| code and uploading it to your CommandStation.

.. tip:: 

  It is helpful to have a high level understanding of how device drivers and the HAL works in the CommandStation as explained on the :doc:`/reference/developers/hal-config` page. However, if that page is more information than you require at this point, then follow the steps below to add the required |EX-TT| device driver and device.

You will need to include the |EX-TT| device driver file "IO_EXTurntable.h" and add a ``HAL()`` command to create the device:

.. code-block:: cpp

  #include "IO_EXTurntable.h"     // Include the device driver
  HAL(EXTurntable, 600, 1, 0x60)  // Create the device

In the device setup above, there are three parameters provided, but only two may need to change in your environment if you have other devices that may conflict with these two settings:

- VPIN=600 - This is the default virtual pin (Vpin) ID that is used to send |EX-TT| commands to. Vpin IDs need to be unique, so if this ID is used elsewhere, change as necessary (refer :ref:`reference/developers/hal:overview`).
- |I2C| address=0x60 - This is the default address on the |I2C| bus that the |EX-TT| is configured to use. This address also needs to be unique, so change this also if it is in use elsewhere, both in "myAutomation.h" and in "config.h" in the |EX-TT| software.

If you already have an existing "myAutomation.h" file, then you simply need to add these entries in the appropriate sections of your existing file, noting that the "#include" needs to be before ``HAL()``.

Follow the rest of the directions for :ref:`reference/developers/hal-config:adding a new device` all the way through to the :ref:`reference/developers/hal-config:upload the new version of the software` step to upload your newly configured CommandStation.

Note there is no point in checking the driver at this stage as |EX-TT| is not connected, and will show as "OFFLINE".

New development functionality and changes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

|NOT-IN-PROD-VERSION|

If you are using the new developmental features for turntables and traversers available from development version 5.1.5 and on, there is one key change to the above.

You must remove any reference to including "IO_EXTurntable.h" as this file no longer exists, and the device driver is included automatically.

To create your turntable device in "myAutomation.h", you are still required to create a device driver first with the `HAL()` command, followed by the `EXTT_TURNTABLE()` command:

.. code-block:: 

  HAL(EXTurntable, vpin, 1, i2c_address)
  EXTT_TURNTABLE(id, vpin, home [, "description"])

For complete information, refer to :ref:`ex-turntable/test-and-tune:development version commands`.

9. Connect EX-Turntable to your EX-CommandStation
-------------------------------------------------

To control |EX-TT| from your CommandStation, you will need a connection to the |I2C| (SDA, SCL) pins.

.. danger:: 

  Ensure you turn the power off to both your CommandStation and |EX-TT| prior to making any of these connections.

On the CommandStation, assuming this is a Mega2560 or Mega2560 + WiFi, the SDA (pin 20) and SCL (pin 21) pins are typically labelled as such, so should be easy to identify.

On an Arduino Nano (and Uno) however, the SDA and SCL pins are shared with analog pins A4 and A5, and therefore aren't explicitly labelled. The SDA pin is A4, and the SCL pin is A5.

Connect these pins to your CommandStation as shown in the table below, noting that it is important to ensure the ground is also connected to ensure the |I2C| communication is reliable.

.. list-table::
    :widths: auto
    :header-rows: 1
    :class: command-table

    * - CommandStation Pin
      - Nano Shield Pin
    * - 20 (SDA)
      - A4 S (SDA)
    * - 21 (SCL)
      - A5 S (SCL)
    * - Any spare ground
      - A4 G
  
.. image:: /_static/images/ex-turntable/nano-i2c.png
  :alt: Nano I2C pins
  :scale: 40%

.. image:: /_static/images/ex-turntable/commandstation-i2c.png
  :alt: Nano I2C pins
  :scale: 40%

.. image:: /_static/images/ex-turntable/commandstation-gnd.png
  :alt: Nano I2C pins
  :scale: 40%

Now you're ready!
=================

At this point, you should have a fully assembled |EX-TT| with the software loaded, a default configuration, and the device driver installed and configured in your CommandStation.

In addition, |EX-TT| should be connected to your CommandStation ready to test, tune your turntable positions, and configure EX-RAIL ready for use on your layout.

Click the "next" button to get cracking!
