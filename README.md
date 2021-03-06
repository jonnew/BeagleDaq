BeagleDaq
=========

Fork of BeagleDaq project (Goldner lab, UMass)

=========

BeagleDAQ
                A data-acquisition platform for the BeagleBoard


BeagleDAQ is a data acquisition daughterboard for the BeagleBoard single-board
computer. The board offers 32 ADC channels and 32 DAC channels (all with 16-bit
resolution), in addition to eight level-shifted GPIO pins.

Getting Started:
=================
The designs are drawn using the gEDA[1] electronic design suite. See the
accompanying Makefile for common tasks.

The Board is designed to work with a BeagleBoard XM-style expansion connector
(female connector on the bottom of the BeagleBoard).

Board Overview:
================
Like many converters, the ADCs and DACs on the BeagleDAQ use a simple serial
interface known as SPI (Serial Peripheral Interface). The board is built on
Texas Instruments' 16-bit converters,

  DAC8568: 8-channel 16-bit ADC
  ADS8344: 8-channel 16-bit DAC (100 ksamples/second)

These are supported by,
  TI TXB0108: 8-bit level shifter
  TI SN74AHC139: Dual 2-Line to 4-Line Demultiplexer

The BeagleBoard exposes two of the OMAP's four SPI controllers on the its
expansion header. These controllers, McSPI3 and McSPI4, support two and one
chip select lines, respectively. This would limit us to only three converters
on our board.

To work around this limitation, the BeagleDAQ passes the McSPI interfaces' chip
select lines through a demultiplexer. This demultiplexer is controlled using
GPIO pins. Thus, when programming the SPI controllers, one must always use chip
select 0 and use the chip select lines to choose between the four converters.

Since the DACs and ADCs have different clocking requirements (ADCs can be
clocked no higher than 2 MHz, DACs can go up to 20 MHz) the BeagleDAQ
partitions these two types of devices on to two separate SPI buses. The DACs
sit on McSPI3 while the ADCs sit on McSPI4. Each of these buses its own chip
select multiplexer.

Technical Details:
===================

I2C bus:
  0x50: AT24C01
  0x20: TCA6416


License:
=================
The board designs are licensed under the GNU General Public License. See
COPYING for more information.


[1] http://www.gpleda.org/
