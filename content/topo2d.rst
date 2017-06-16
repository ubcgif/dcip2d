.. _topo2d:

Topography file
===============

This is the file used to define the surface topography along the
traverse of the DC/IP experiment by specifying the elevations of
selected points. It should be noted that the file is linked to the
topography. The specification of the top elevation, , sets the top of
the mesh. In other words, :math:`\codeName{elev0}` in the topography is
the top of the mesh and where :math:`z=0`. This point is assumed to be
the highest point on the topographic surface. Locations above on the
surface are set to . The elevation is positive up similar to the file
and thus it can be given in relative values. The topography file must
cover the core portion of the mesh where electrodes are placed. The
coverage should ideally extend to both ends of the mesh, otherwise the
remaining portion towards the ends will be assumed to have the same
surface elevation in each direction as given at the end points within
the file. The topographic surface is discretized onto the mesh using the
elevations at the horizontal nodes that are obtained by linear
interpolation from this file.

An example of the file structure is as follows:

+------------------+--------------------+
| N                | elev0              |
+------------------+--------------------+
| X\ :math:`_1`    | elev\ :math:`_1`   |
+------------------+--------------------+
| X\ :math:`_2`    | elev\ :math:`_2`   |
+------------------+--------------------+
| :math:`\vdots`   | :math:`\vdots`     |
+------------------+--------------------+
| X\ :math:`_N`    | elev\ :math:`_N`   |
+------------------+--------------------+

#. Number of locations defining the topographic profile.

#. The elevation of the top of the 2D . See the introductory paragraph
   in this section for details.

#. i\ :math:`^{th}` horizontal location.

#. i\ :math:`^{th}` elevation at X\ :math:`_i`.

Example of topography
---------------------

The following is an example of topography:

+--------+------+
| 10     | 40   |
+--------+------+
| -250   | 10   |
+--------+------+
| -180   | 20   |
+--------+------+
| -130   | 30   |
+--------+------+
| -110   | 40   |
+--------+------+
| -50    | 50   |
+--------+------+
| 50     | 50   |
+--------+------+
| 110    | 40   |
+--------+------+
| 130    | 30   |
+--------+------+
| 180    | 20   |
+--------+------+
| 250    | 10   |
+--------+------+

In the above example, there are 10 locations that vary in elevation from
10 to 50 m. The two locations above 40 m () will be represented as 40 m
of elevation within . The top of the mesh will also be placed at 40 m.
