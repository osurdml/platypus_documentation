Sensors on the Platypus Lutra
=============================

The sensors on-board the Lutra vehicle interface via a (Platypus) custom shield over an Arduino Due. There are three available sensor ports (S0-S2) on the board (note that there are four physical ports, but S3 only supplies power). The sensors must interface to the ports using either RS232 or RS422(485) serial communications. To select the correct serial protocol you must set some flags in the Arduino firmware. Pin definitions::

    PWR_ENABLE  -   HIGH to enable 12V output
    RX_DISABLE  -   HIGH to disable serial receiver
    TX_ENABLE   -   HIGH to enable serial transmit
    RS_485TE    -   HIGH to enable RS485 termination resitor (HIGH for HDS)
    RS485_232   -   HIGH for RS232, LOW for RS485

The Arduino software will format messages in a particular way and send them (as text) to the Android server
