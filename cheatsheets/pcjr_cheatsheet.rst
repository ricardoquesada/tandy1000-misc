IBM PCjr cheatsheet
===================

.. contents:: Contents
   :depth: 2

PIC 8259
--------

Only one (master) PIC `8259`_ in the PCjr. There is no slave PIC.

+-------+------------------------------------+
| 0x20  |Initialization Command Word 1 (ICW1)|
+=======+====================================+
|bit 0  | ``0``: ICW4 needed                 |
|       | ``1``: not needed                  |
+-------+------------------------------------+
|bit 1  | ``0``: Cascade mode                |
|       | ``1``: Single (should be 1 in PCjr)|
+-------+------------------------------------+
|bit 2  | Ignored in x86. Should be ``0``    |
+-------+------------------------------------+
|bit 3  | ``0``: Edge triggered mode         |
|       | ``1``: Level triggered mode        |
+-------+------------------------------------+
|bit 4  | ``1``: must be 1 to initialize PIC |
+-------+------------------------------------+
|bit 5-7| Not used. Should be ``0`` in x86   |
+-------+------------------------------------+

+-------+--------------------------------------+
| 0x21  | Initialization Command Word 2 (ICW2) |
+=======+======================================+
|bit 0-2| Not used in x86                      |
+-------+--------------------------------------+
|bit 3-7| Specifies the x86 interrupt vector   |
|       | address times 8                      |
+-------+--------------------------------------+

+-------+--------------------------------------+
| 0x21  | Initialization Command Word 4 (ICW4) |
+=======+======================================+
|bit 0  | Set to 1 in x86                      |
+-------+--------------------------------------+
|bit 1  |0: manual EOI                         |
|       |1: controller perform automatic EOI   |
+-------+--------------------------------------+
|bit 2  | if bit 3 == 1:                       |
|       | 0: buffer slave                      |
|       | 1: buffer master                     |
+-------+--------------------------------------+
|bit 3  | 0: Non-buffer mode                   |
|       | 1: Buffer mode                       |
+-------+--------------------------------------+
|bit 4  | Special Fully Nested Mode. Not used  |
+-------+--------------------------------------+
|bit 5-7| Not used. Should be 0                |
+-------+--------------------------------------+

+-------+--------------------------------------+
| 0x20  | Operation Command Word 2 (OCW2)      |
+=======+======================================+
|bit 0-2| Interrupt level upon which controller|
|       | must react.                          |
+-------+--------------------------------------+
|bit 3-7| Specifies the x86 interrupt vector   |
|       | address times 8                      |
+-------+--------------------------------------+

Example:
~~~~~~~~

.. code:: asm

    ; This is how the PCjr initializes the PIC
    mov al,0b0001_0011          ;ICW1
    out 0x20,al
    mov al,0b0000_1000          ;ICW2. IVT starts at 8 (1*8)
    out 0x21,al
    mov al,0b0000_1001          ;ICW4
    out 0x21,al


Timer 8253-5
------------

+--------------------------+
|Timer 8253-5              |
+==========================+
|0x40                      |
+--------------------------+
|0x41                      |
+--------------------------+
|0x42                      |
+--------------------------+
|0x43                      |
+--------------------------+

+--------------------------+
|PPI 8255-5                |
+==========================+
|0x60                      |
+--------------------------+
|0x61                      |
+--------------------------+
|0x62                      |
+--------------------------+

+--------------------------+
|NMI mask reg              |
+==========================+
|0xa0                      |
+--------------------------+


+--------------------------+
|SN76496N                  |
+==========================+
|0xc0                      |
+--------------------------+

0xf0-0xff: diskette

0x200: joystick

0x2f8-0x2ff: serial port

0x3d0-0x3df: video subsystem

0x3f8-0x3ff: modem

.. _8259: http://www.brokenthorn.com/Resources/OSDevPic.html