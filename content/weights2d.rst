.. _weights2d:

Weighting file
==============

This is the file used to define the smallness and spatial weights to be
incorporated into the inversion codes **DCINV2D** or **IPINV2D**. These weights are given in
the model objective function (equation [eq:intMOF]). As with the *Model* file,
the weights are stored in a row format. Weights with the value of 1.0 do
contribute extra information to model objective function. Deviating from
1.0 will increase the relative importance (or not) of the cell value or
interface. One has the choice of a single file containing all the
weights or a file for each weight.

An example of the file structure *containing all weights* is as follows:

.. figure:: ../images/weightsfile_format.png
   :figwidth: 50%
   :align: center
   :name: weightsfile_format

#. :math:`N_x`: Number of cells in the horizontal direction. This number is the same
   as found in Model and Mesh files.

#. :math:`N_z`: Number of cells in the vertical direction. This number is the same as
   found in Model and Mesh files.

#. :math:`Ws_{i,k}`: Weight given to the smallest model component. :math:`W_s` has size of
   :math:`N_x \times N_z`. The values of :math:`i` and :math:`k` count
   consistent with the Model format.

#. :math:`Wx_{i,k}`: Weight given across the faces of cells in the :math:`x`-direction.
   :math:`W_x` has size of :math:`N_x-1 \times N_z`. The values of :math:`i` and
   :math:`k` count consistent with the Model format.

#. :math:`Wy_{i,k}`: Weight given across the faces of cells in the :math:`z`-direction.
   :math:`W_y`has size of :math:`N_x \times N_z-1`. The values of :math:`i` and
   :math:`k` count consistent with the Model format.

An example of a file structure *containing only one weight* is as
follows:

.. figure:: ../images/weightsfile_OneWeightformat.png
   :figwidth: 50%
   :align: center
   :name: weightsfile_OneWeightformat

#. :math:`N^W_x`: Number of cells in the horizontal direction for that weight (:math:`N_x` for W\ :math:`_s` and W\ :math:`_z` and :math:`N_x-1` for W\ :math:`_x`).

#. :math:N^W_z`: Number of cells in the vertical direction for that weight (:math:`N_z` for W\ :math:`_s` and W\ :math:`_x` and :math:`N_z-1` for W\ :math:`_z`).

#. :math:`W_{i,k}`: Weight value

Example of a weight file
------------------------

The following is an example of a weight file *with all of the weights*:

.. figure:: ../images/weightsfile_format_example.png
   :figwidth: 50%
   :align: center
   :name: weightsfile_format_example

In the above example, there are 5 horizontal cells and 3 vertical cell
associated with the mesh. Therefore, W\ :math:`_s` is
:math:`5 \times 3`, W\ :math:`_x` is :math:`4 \times 3`, and
W\ :math:`_z` is :math:`5 \times 2`. The is no additional emphasis
placed on the smallest model component. Large emphasis on minimizing the
derivatives has been place on two cell faces in the
:math:`x-`\ direction in all three vertical rows. Small values have been
placed at two vertical faces to allow for large jumps. This weighting
file would be analogous to driving the model toward a vertical dyke.

Note on weight design
---------------------

When the weighting is supplied by the user, care should be taken so that
the final weighting matrix constructed by the program is positive
definite. This property can be destroyed when the combination of the
cell weighting coefficients and component coefficients (see
the description of special weighting w.dat file) makes the diagonal
elements too small. Under such circumstances, the program will usually
print in the log file the indices of the rows which are not diagonally
dominant. In the most severe situation, the program will output an error
message indicating that the matrix may not be positive definite. When
these messages appear, the user should stop the program execution and
redesign the special weighting by increasing the value of s or
decreasing the dynamic range of WS .
