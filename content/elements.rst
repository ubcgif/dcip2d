.. _elements:

Elements of the program DCIP2D
==============================

Introduction
------------

The program library consists of the programs:

#. : performs forward modelling for DC and IP data

#. : inverts DC potentials to recover a conductivity model

#. : inverts IP data to recover a chargeability model

Each of the above programs requires input files as well as the
specification of parameters in order to run. However, some files are
used by a number of programs. Before detailing the run procedures for
each of the above programs we first present information about these
general files.

General files for  programs
---------------------------

There are six general files which are used in . These are:

#. **observations file**: specifies the observed measurements and the associated electrode
   locations

#. **electrodes file**: contains the electrode locations for forward modelling

#. **mesh2D file**: contains the finite difference mesh for the 2D modelling and
   inversions. A special type of model file specifies the active cells.

#. **topo2D file**: contains the topographic data

#. **model2D file**: structure to hold cell values for models: conductivity,
   chargeability or active

#. **weights2D file**: file that contains special weightings which alter the type of model
   produced in the inversions

.. toctree::
        :maxdepth: 1

        Observations file<observations>
        Electrodes locations file<locations>
        Mesh2D file<mesh2d>
        Topo2D file<topo2d>
        Model2D file<model2d>
        Weights2D file<weights2d>