.. _locations:

Electrodes file
===============

This file contains the electrode locations for . The electrodes file
follows the same formats at the files, but without data and standard
deviations. Thus, an electrode file can have three different formats:
the , , or format. Only a single format is allowed in a data file. **The
general format is the only format that will allow the use of borehole
locations**. The type of format chosen for forward modelling does not
make any difference to the and is determined only by the user’s
preference. At the beginning of execution, the programs will determine
the format and the output files will be written in the same format.

General format
--------------

The forward modelling code can handle arbitrary electrode
configurations, and a mixture of different configurations can be present
in the data file. This is accomplished by specifying the locations of
four electrodes for each location. Whenever the two current electrodes,
or two potential electrodes, are given the identical location, that
particular pair is considered to be a single pole with the negative
electrode being at infinity. The format consists of a line with the
current electrode location and number of potential electrode locations
associated with it. Each location has :math:`x` and :math:`z`
coordinates. An example of the format file structure is as follows:

| \|ccccc\|
| :math:`A_1^x` & :math:`A_1^z` & :math:`B_1^x` & :math:`B_1^z` &
  :math:`n_1`
| :math:`M_1^x` & :math:`M_1^z` & :math:`N_1^x` & :math:`N_1^z` &
| :math:`\vdots` & :math:`\vdots` & :math:`\vdots` & :math:`\vdots` &
| :math:`M_{n_1}^x` & :math:`M_{n_1}^z` & :math:`N_{n_1}^x` &
  :math:`N_{n_1}^z` &
| :math:`A_2^x` & :math:`A_2^z` & :math:`B_2^x` & :math:`B_2^z` &
  :math:`n_2`
| :math:`M_1^x` & :math:`M_1^z` & :math:`N_1^x` & :math:`N_1^z` &
| :math:`\vdots` & :math:`\vdots` & :math:`\vdots` & :math:`\vdots` &
| :math:`M_{n_2}^x` & :math:`M_{n_2}^z` & :math:`N_{n_2}^x` &
  :math:`N_{n_2}^z` &
| :math:`A_3^x` & :math:`A_3^z` & :math:`B_3^x` & :math:`B_3^z` &
  :math:`n_3`
| :math:`\vdots` & :math:`\vdots` & :math:`\vdots` & :math:`\vdots` &

#. Any comments can go here. This line is ignored by

#. This flag must go prior to in order to let the code know the file is
   of general format

#. | Only used for IP inversion and not required if only using DC
     inversion. NOTE: If omitted from IP inversion, the program will
     choose .
   | , Type of IP data is apparent chargeability
   | , Type of IP data is secondary potentials

#. i\ :math:`^{th}` horizontal position along line of current electrode
   A

#. i\ :math:`^{th}` elevation of current electrode A

#. i\ :math:`^{th}` horizontal position along line of current electrode
   B

#. i\ :math:`^{th}` elevation of current electrode B

#. j\ :math:`^{th}` horizontal position along line of potential
   electrode M associated with the i\ :math:`^{th}` current pair

#. j\ :math:`^{th}` elevation of potential electrode M associated with
   the i\ :math:`^{th}` current pair

#. j\ :math:`^{th}` horizontal position along line of potential
   electrode N associated with the i\ :math:`^{th}` current pair

#. j\ :math:`^{th}` elevation of potential electrode N associated with
   the i\ :math:`^{th}` current pair

Example of general format
`````````````````````````

The following is an example of locations for DC data (e.g., no IPTYPE):

| \|ccccc\|
| 221 & -45 & 221 & -45 & 6
| 50 & 250 & 100 & 25 &
| 100 & 250 & 150 & 50 &
| 150 & 500 & 200 & 75 &
| 200 & 75 & 250 & 100 &
| 250 & 100 & 300 & 125 &
| 300 & 125 & 350 & 150 &
| 221 & -45 & 600 & -55 & 2
| 100 & 25 & 150 & 500 &
| 150 & 500 & 200 & 75.0 &

In the above example, there are two current electrode locations, the
first with six potential electrodes and the second with two potential
electrode data. The line “IPTYPE=2” would be added if this file were IP
data of second potentials.

Surface format
--------------

The surface format is similar to the general format with difference that
the elevation data is not given. Instead, the program places the
electrodes on top of the discretized topographic surface. Accordingly,
this format **cannot be used with borehole data** and if no topography
is given, assumes the data are on top of the mesh at an elevation of 0.
Whenever the two current electrodes, or two potential electrodes, are
given the identical location, that particular pair is considered to be a
single pole with the negative electrode being at infinity. The format
consists of a line with the current electrode location and number of
potential electrode locations associated with it. An example of the
format file structure is as follows:

| \|ccc\|
| :math:`A_1^x` & :math:`B_1^x` & :math:`n_1`
| :math:`M_1^x` & :math:`N_1^x` &
| :math:`\vdots` & :math:`\vdots` &
| :math:`M_{n_1}^x` & :math:`N_{n_1}^x` &
| :math:`A_2^x` & :math:`B_2^x` & :math:`n_2`
| :math:`M_1^x` & :math:`N_1^x` &
| :math:`\vdots` & :math:`\vdots` &
| :math:`M_{n_2}^x` & :math:`N_{n_2}^x` &
| :math:`A_3^x` & :math:`B_3^x` & :math:`n_3`
| :math:`\vdots` & :math:`\vdots` &

The following are detailed summaries of components of the surface-format
observations file:

#. Any comments can go here. This line is ignored by

#. | Only used for IP inversion and not required if only using DC
     inversion. NOTE: If omitted from IP inversion, the program will
     choose .
   | , Type of IP data is apparent chargeability
   | , Type of IP data is secondary potentials

#. i\ :math:`^{th}` horizontal position along line of current electrode
   A

#. i\ :math:`^{th}` horizontal position along line of current electrode
   B

#. j\ :math:`^{th}` horizontal position along line of potential
   electrode M associated with the i\ :math:`^{th}` current pair

#. j\ :math:`^{th}` horizontal position along line of potential
   electrode N associated with the i\ :math:`^{th}` current pair

Example of surface format
`````````````````````````

The following is an example of IP data in units of apparent
chargeability:

| \|ccc\|
| 221 & -45 & 4
| 50 & 25 &
| 100 & 50 &
| 250 & 125 &
| 300 & 150 &
| 221 & -55 & 2
| 100 & 150 &
| 150 & 200 &

In the above example, there are two current electrode locations, the
first with four potential electrodes and the second with two potential
electrode data. The line “IPTYPE=1” would be absent if this file were DC
data.

Simple format
-------------

The simple format is the most straightforward, but also most restrictive
of the three formats. The elevations are not given similar to the
surface format with difference that the elevation data is not given.
Instead, the program places the electrodes on top of the discretized
topographic surface. Accordingly, this format **cannot be used with
borehole data** and if no topography is given, assumes the locations are
on top of the mesh at an elevation of 0. Whenever the two current
electrodes, or two potential electrodes, are given the identical
location, that particular pair is considered to be a single pole with
the negative electrode being at infinity. The format consists of a line
with the current electrode pair location and potential electrode
location pair. An example of the format file structure is as follows:

| \|cccc\|
| :math:`A_1^x` & :math:`B_1^x` & :math:`M_1^x` & :math:`N_1^x`
| :math:`A_2^x` & :math:`B_2^x` & :math:`M_2^x` & :math:`N_2^x`
| :math:`\vdots` & :math:`\vdots` & :math:`\vdots` & :math:`\vdots`
| :math:`A_n^x` & :math:`B_n^x` & :math:`M_n^x` & :math:`N_n^x`

The following are detailed summaries of components of the simple-format
observations file:

#. Any comments can go here. This line is ignored by

#. | Only used for IP inversion and not required if only using DC
     inversion. NOTE: If omitted from IP inversion, the program will
     choose .
   | , Type of IP data is apparent chargeability
   | , Type of IP data is secondary potentials

#. i\ :math:`^{th}` horizontal position along line of current electrode
   A

#. i\ :math:`^{th}` horizontal position along line of current electrode
   B

#. i\ :math:`^{th}` horizontal position along line of potential
   electrode M

#. i\ :math:`^{th}` horizontal position along line of potential
   electrode N

Example of simple format
````````````````````````

The following is an example of the simple format. The data are the same
as given in the surface format example; IP data in units of apparent
chargeability:

| \|cccc\|
| 221 & -45 & 50 & 25
| 221 & -45 & 100 & 50
| 221 & -45 & 250 & 125
| 221 & -45 & 300 & 150
| 221 & -55 & 100 & 150
| 221 & -55 & 150 & 200
