.. _runningtheprograms:

Running the programs
====================

The software package  uses five general codes:

- **DCIPF2D**: performs 2D forward modelling for DC and IP data

- **DCINV2D**: inverts DC potentials to recover a 2D conductivity model

- **IPINV2D**: inverts IP data to recover a 2D chargeability model

This section discusses the use of these codes individually.

Introduction
------------

All programs in the package can be executed under Windows or Linux
environments. They can be run by typing the program name followed by a
control file in the “command prompt” (Windows) or “terminal” (Linux).
They can be executed directly on the command line or in a shell script
or batch file. When a program is executed without any arguments, it will
print the usage to screen.

Execution on a single computer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The command format and the control, or input, file format on a single
machine are described below. Within the command prompt or terminal, any
of the programs can be called using:

program :math:`arg_1` [:math:`arg_2` :math:`\cdots` :math:`arg_i`]

where:

-  *program* is the name of the executable

-  :math:`arg_i` is a command line argument, which can be a name of corresponding
   required or optional file. Optional command line arguments are
   specified by brackets: . **NOTE:** Typing as the control file, serves
   as a help function and returns all of the keyword combinations
   allowed for that program.

Each control file contains a formatted list of arguments, parameters,
and file names in a combination specific for the executable, which can
be in any order. Values that are being set by the user are given to each
program through a specific list of keywords (e.g., to specify the type
of weighting). Different control file formats will be explained further
in the document for each executable. All files are in ASCII text format
- they can be read with any text editor. Input and control files can
have any name the user specifies. Details for the format of each file
can be found in Section [Elements]. When inputting a file, the word
should be following the keyword (e.g., ). If a value is being used than
the word will follow a keyword (e.g., ).

DCIPF2D
-------

This program performs forward modelling of DC and IP data. Command line
usage:

dcipf2d dcipf2d.inp

where the input file, , is described below. The options can be in any
order.

Input files
~~~~~~~~~~~

Keywords for the input file are:

+------------------------------------+---------------------------------------+
| FWD [DC \| IP \| IPL]              | ! Type of data to model               |
+------------------------------------+---------------------------------------+
| MESH FILE fileName                 | ! Mesh                                |
+------------------------------------+---------------------------------------+
| LOC [LOC\_X \| LOC\_XZ] fileName   | ! Locations file                      |
+------------------------------------+---------------------------------------+
| TOPO [FILE fileName \| DEFAULT]    | ! Topography                          |
+------------------------------------+---------------------------------------+
| COND [VALUE c \| FILE fileName]    | ! Conductivity value or model file    |
+------------------------------------+---------------------------------------+
| CHG [VALUE c \| FILE fileName]     | ! Chargeability value or model file   |
+------------------------------------+---------------------------------------+
| WAVE w\_min w\_max N               | ! Optional min/max of N wave values   |
+------------------------------------+---------------------------------------+

-  The choices after this keyword are:

   #. for DC forward modelling. The chargeability model and wave, if
      given, is ignored for DC forward modelling only.

   #. for IP forward modelling.

   #. for IP forward modelling using product of the sensitivity matrix
      and chargeability.

-  The 2D mesh file name is followed after these keywords. For example .

-  The observation locations. The choices after this keyword are:

   #. when giving or locations formats.

   #. when using the locations format.

-  The choices for the topography are:

   #. followed by the name of the topography file.

   #. for flat topography at and elevation of 0.

-  The choices for the conductivity model are:

   #. followed by the name of the conductivity file .

   #. followed by a number for the conductivity throughout the mesh.

-  The choices for the chargeability model are:

   #. followed by the name of the chargeability file .

   #. followed by a number for the chargeability throughout the mesh.

-  is followed by 3 constants: . These are the wave numbers used in the
   cosine transform. There will be wave values, log spaced from to in
   time. The default values (if is not given) is , , and .

Example of dcipf2d.inp
~~~~~~~~~~~~~~~~~~~~~~

Example of an input file for to model DC data that are given in format
and a with a topography file:

+-------------------------+--------------------------------------+
| FWD DC                  | ! DC input type                      |
+-------------------------+--------------------------------------+
| MESH FILE myMesh.msh    | ! mesh file                          |
+-------------------------+--------------------------------------+
| LOC LOC\_XZ obs.loc     | ! general formatted locations file   |
+-------------------------+--------------------------------------+
| TOPO FILE myTopo.dat    | ! topography file                    |
+-------------------------+--------------------------------------+
| COND FILE myModel.con   | ! conductivity model file            |
+-------------------------+--------------------------------------+

Output files
~~~~~~~~~~~~

The files created by are:

-  The computed DC potential data.

-  The computed IP data if the option is chosen.

-  The computed IP data if the option is chosen.

DCINV2D
-------

This program performs the inversion of DC resistivity data. Command line
usage:

dcinv2d dcinv2d.inp

where the input file, , is described below. The options can be in any
order. The minimum keywords needed for an inversion are and .

Input Files
~~~~~~~~~~~

Keywords for the input file are:

+----------------------------------------------------+----------------------------------------+
| MESH [DEFAULT \| FILE \| NC\_ASPR n a]             | ! Specify the mesh                     |
+----------------------------------------------------+----------------------------------------+
| OBS [LOC\_X \| LOC\_XZ] fileName                   | ! Observations file follows            |
+----------------------------------------------------+----------------------------------------+
| NITER n                                            | ! Maximum number of iterations         |
+----------------------------------------------------+----------------------------------------+
| CHIFACT [c \| DEFAULT]                             | ! Chifact c or default                 |
+----------------------------------------------------+----------------------------------------+
| TOPO [FILE \| DEFAULT]                             | ! Topography                           |
+----------------------------------------------------+----------------------------------------+
| INIT\_MOD [VALUE \| FILE \| DEFAULT]               | ! Initial conductivity model           |
+----------------------------------------------------+----------------------------------------+
| REF\_MOD [VALUE \| FILE \| DEFAULT]                | ! reference conductivity model         |
+----------------------------------------------------+----------------------------------------+
| ALPHA [VALUE \| LENGTH \| DEFAULT]                 | ! Alphas or length scales              |
+----------------------------------------------------+----------------------------------------+
| WEIGHT [FILE \| FILES \| DEFAULT]                  | ! Alphas or length scales              |
+----------------------------------------------------+----------------------------------------+
| WAVE w\_min w\_max N                               | ! min/max of N wave values             |
+----------------------------------------------------+----------------------------------------+
| STORE\_ALL\_MODELS [TRUE \| FALSE]                 | ! store all models or write to disk    |
+----------------------------------------------------+----------------------------------------+
| INVMODE [CG \| SVD]                                | ! way to solve the system              |
+----------------------------------------------------+----------------------------------------+
| CG\_PARAM maxit tol                                | ! parameters for CG system             |
+----------------------------------------------------+----------------------------------------+
| HUBER c                                            | ! constant for the Huber norm          |
+----------------------------------------------------+----------------------------------------+
| EKBLOM rho\_s rho\_x rho\_z eps\_s eps\_x eps\_z   | ! six constants for the Ekblom norm    |
+----------------------------------------------------+----------------------------------------+
| ACTIVE\_CELLS fileName                             | ! specify file for active cells        |
+----------------------------------------------------+----------------------------------------+
| USE\_MREF [TRUE \| FALSE]                          | ! ref model throughout spatial terms   |
+----------------------------------------------------+----------------------------------------+
| BOUNDS [VALUE \| FILE\_L \| FILE\_U \| NONE]       | ! specify bounds                       |
+----------------------------------------------------+----------------------------------------+

-  The choices after this keyword are:

   #. the programs creates a mesh (output ) with 3 cells between
      electrodes and the aspect ratio of the top cells set to 3.
      **NOTE**: This option assumes that the data are collected by
      commonly used arrays and that the topographic relief is moderate.
      Thus, this option may not be optimal when the data are collected
      with unusual electrode geometry or when data are collected over
      severe surface topography. In such cases, the user should redesign
      the mesh so that it is better suited for the particular needs of
      the data set.

   #. file name of the mesh

   #. creates a mesh (output ) that has cells between the electrodes and
      the aspect ratio of the top cells is set to

-  The observation locations. The choices after this keyword are:

   #. when giving or locations formats

   #. when using the locations format.

-  A value follows this keyword representing the number of maximum
   iterations for the inversion. **NOTE**: The program will terminate
   before the specified maximum number of iterations is reached if the
   expected data misfit is achieved and if the model norm has plateaued.
   However, if the program exits when the maximum iteration is reached,
   the file should be checked to see if the desired (based on the number
   of data and chi factor) has been reached and if the model norm is no
   longer changing. If either of these conditions has not been met then
   the program should be restarted. If the desired misfit level is not
   achieved, but the model norm has plateaued and the model is not
   changing between successive iterations, then the user may want to
   adjust the target misfit to a higher value. Also an investigation as
   to which data are most poorly fit can be informative. It may be that
   the assigned standard deviations to specific data are unrealistically
   small. The program restarts using the information in and .

-  The value at which the program reproduced the data. The choices after
   this keyword are:

   #. where the program will start with 1e-3 initially and then when the
      misfit stop decreasing, the chi factor will be changed by 10%

   #. the value to set the chi factor (1 is when the data misfit equals
      the number of data), or if a value is not there, but is given, the
      program will stop when the data misfit reaches the number of data

-  The choices after this keyword are:

   #. followed by the name of the topography file

   #. for flat topography at an elevation of 0.

-  The choices for the initial model are:

   #. name of the initial conductivity file

   #. the value for the initial conductivity throughout the mesh

   #. for the initial model to be set to the reference model.

-  The choices for the reference model are:

   #. name of the reference conductivity file

   #. the value for the reference conductivity throughout the mesh

   #. the reference model is equal to the best fitting half-space model.

-  is followed by 3 constants: . These are the wave numbers used in the
   cosine transform. There will be wave values, log spaced from to in
   time. The default values (if is not given) is , , and .

-  The choices after this keyword are:

   #. where the program will set :math:`\alpha_s` =
      0.001\*(90\ :math:`/`\ max electrode separation)\ :math:`^2` and
      :math:`\alpha_x = \alpha_z = 1`.

   #. the user gives the coefficients for the each model component for
      the model objective function from equation [eq:intMOF]:
      :math:`\alpha_s` is the smallest model component, :math:`\alpha_x`
      is along line smoothness, and :math:`\alpha_z` is vertical
      smoothness.

   #. the user gives the length scales and the smallest model component
      is calculated accordingly. The conversion from :math:`\alpha`\ ’s
      to length scales can be done by:

      .. math:: L_x = \sqrt{\frac{\alpha_x}{\alpha_s}} ; ~L_z = \sqrt{\frac{\alpha_z}{\alpha_s}}

       where length scales are defined in meters. When user-defined, it
      is preferable to have length scales exceed the corresponding cell
      dimensions.

-  The weighting for the model objective function allows for three
   options:

   #. No weighting is supplied (all values of weights are 1)

   #. The weighting is supplied as a file with all the weights in one
      file

   #. The weighting is supplied as three separate files with the weight
      for the smallest model component in , the :math:`x-`\ component
      written in file and the :math:`z-`\ component written in .

-  There are two choices:

   #. Write all models and predicted data to disk. Each iteration will
      have and files where is the iteration (e.g., 01 for the first
      iteration)

   #. Only the final model and predicted data file are written. These
      files are named and for the conductivity and predicted data,
      respectively.

-  This specifies the way the system is solved:

   #. Solve the system using a subspace method with basis vectors. This
      is the solution methodology of the original code and the default
      if not given.

   #. Solve the system using a subspace method with conjugate gradients
      (CG). This allows additional constraints (i.e., Huber and Ekblom
      norms) to be incorporated into the code.

-  is used when the inversion mode is . The keyword is followed by two
   constants: specifying the maximum number of iterations (default is
   10), and specifying the solution’s accuracy (default is 0.01)

-  The Huber norm is used when evaluating the data misfit. A constant
   follows this keyword and this option is only available when using the
   inversion mode option. The default value is 1e100. The constant is
   from equation [eq:Huber\_phid].

-  Use the Ekblom norm. Six (6) values should follow this keyword:
   representing the constants found in equation [eq:ekblom].

-  followed by the file name of the active cell file.

-  This option is used to decide if the reference model should be in the
   spatial terms of the model objective function (equation [eq:intMOF]).
   There are two options: to include the reference model in the spatial
   terms or to have the reference model only in the smallest model
   component.

-  The bounds options are:

   #. Do not include bounds in the inversion

   #. Give a constant global lower bound of and upper bound of .

   #. The lower bound is given in a file and is in the format.

   #. The upper bound is given in a file and is in the format.

Example of dcinv2d.inp
~~~~~~~~~~~~~~~~~~~~~~

Below is an example of the input file . The code will create a mesh with
4 cell between electrode locations and the aspect ratio of the size top
cells set to 2. This means the reference and initial models will not be
given in a file, but rather set to 0.001 S/m. The length scales will be
5 m in each direction and the Ekblom norm will have exponents of 1.0 in
each direction to emphasize blockiness. It will start from scratch and
stop after 50 iterations if the desired misfit (equal to 90% of the
number of data) is not achieved. Conjugate gradients are used to solve
the system of equations with a maximum number of CG iterations set at
800 and a relative accuracy of 1e-5. There are no bounds in this
inversion.

+-------------------------------------+-----------------------------------------+
| OBS LOC\_XZ obs\_dc.dat             | ! general formatted data                |
+-------------------------------------+-----------------------------------------+
| TOPO FILE topography.txt            | ! topography file                       |
+-------------------------------------+-----------------------------------------+
| MESH NC\_ASPR 4 2                   | ! DCINV2D created mesh                  |
+-------------------------------------+-----------------------------------------+
| ALPHA LENGTH 5 5                    | ! length scales of 5 m                  |
+-------------------------------------+-----------------------------------------+
| CHIFACT 0.9                         | ! data misfit equal to number of data   |
+-------------------------------------+-----------------------------------------+
| INIT\_MOD DEFAULT                   | ! initial model is ref model            |
+-------------------------------------+-----------------------------------------+
| REF\_MOD VALUE 0.001                | ! ref model                             |
+-------------------------------------+-----------------------------------------+
| EKBLOM 1.0 1.0 1.0 1e-5 1e-5 1e-5   | ! Ekblom norm                           |
+-------------------------------------+-----------------------------------------+
| NITER 50                            | ! max iterations                        |
+-------------------------------------+-----------------------------------------+
| INVMODE CG                          | ! use CG solver                         |
+-------------------------------------+-----------------------------------------+
| CG\_PARAM 800 1e-5                  | ! Solver specs                          |
+-------------------------------------+-----------------------------------------+

Output Files
~~~~~~~~~~~~

will create the following files:

#. The log file containing the minimum information for each iteration,
   summary of the inversion, and standard deviations if assigned by .

#. The developers log file containing the values of the model objective
   function value(\ :math:`\psi_m`), trade-off parameter
   (:math:`\beta`), and data misfit (:math:`\psi_d`) at each iteration

#. Conductivity model for each iteration ( defines the iteration step)
   if is used

#. Predicted data for each iteration ( defines the iteration step) if is
   used

#. Predicted data file that is updated after each iteration (will also
   be the predicted data)

#. Conductivity model that matches the predicted data file and is
   updated after each iteration (will also be the recovered model)

#. Model file of average sensitivity values for the mesh

IPINV2D
-------

This program performs the 2D inversion of induced polarization data.
Command line usage:

ipinv2d ipinv2d.inp

for the control file described below. The options can be in any order.
The minimum keywords needed for an inversion are , , and .

Input Files
~~~~~~~~~~~

Keywords for the input file are:

+----------------------------------------------------+----------------------------------------+
| MESH [DEFAULT \| FILE \| NC\_ASPR n a]             | ! Specify the mesh                     |
+----------------------------------------------------+----------------------------------------+
| OBS [LOC\_X \| LOC\_XZ] fileName                   | ! Observations file follows            |
+----------------------------------------------------+----------------------------------------+
| NITER n                                            | ! Maximum number of iterations         |
+----------------------------------------------------+----------------------------------------+
| CHIFACT [c \| DEFAULT]                             | ! Chifact c or default                 |
+----------------------------------------------------+----------------------------------------+
| TOPO [FILE \| DEFAULT]                             | ! Topography                           |
+----------------------------------------------------+----------------------------------------+
| INIT\_MOD [VALUE \| FILE \| DEFAULT]               | ! Initial chargeability model          |
+----------------------------------------------------+----------------------------------------+
| REF\_MOD [VALUE \| FILE \| DEFAULT]                | ! Reference chargeability model        |
+----------------------------------------------------+----------------------------------------+
| COND [VALUE \| FILE ]                              | ! Conductivity model                   |
+----------------------------------------------------+----------------------------------------+
| ALPHA [VALUE \| LENGTH \| DEFAULT]                 | ! Alphas or length scales              |
+----------------------------------------------------+----------------------------------------+
| WEIGHT [FILE \| FILES \| DEFAULT]                  | ! Alphas or length scales              |
+----------------------------------------------------+----------------------------------------+
| WAVE w\_min w\_max N                               | ! min/max of N wave values             |
+----------------------------------------------------+----------------------------------------+
| STORE\_ALL\_MODELS [TRUE \| FALSE]                 | ! store all models or write to disk    |
+----------------------------------------------------+----------------------------------------+
| INVMODE [CG \| SVD]                                | ! way to solve the system              |
+----------------------------------------------------+----------------------------------------+
| CG\_PARAM maxit tol                                | ! parameters for CG system             |
+----------------------------------------------------+----------------------------------------+
| HUBER c                                            | ! constant for the Huber norm          |
+----------------------------------------------------+----------------------------------------+
| EKBLOM rho\_s rho\_x rho\_z eps\_s eps\_x eps\_z   | ! six constants for the Ekblom norm    |
+----------------------------------------------------+----------------------------------------+
| ACTIVE\_CELLS fileName                             | ! specify file for active cells        |
+----------------------------------------------------+----------------------------------------+
| USE\_MREF [TRUE \| FALSE]                          | ! ref model throughout spatial terms   |
+----------------------------------------------------+----------------------------------------+
| BOUNDS [VALUE \| FILE\_L \| FILE\_U \| NONE]       | ! specify bounds                       |
+----------------------------------------------------+----------------------------------------+

-  The choices after this keyword are:

   #. the programs creates a mesh (output ) with 3 cells between
      electrodes and the aspect ratio of the top cells set to 3.
      **NOTE**: This option assumes that the data are collected by
      commonly used arrays and that the topographic relief is moderate.
      Thus, this option may not be optimal when the data are collected
      with unusual electrode geometry or when data are collected over
      severe surface topography. In such cases, the user should redesign
      the mesh so that it is better suited for the particular needs of
      the data set.

   #. file name of the mesh

   #. creates a mesh (output ) that has cells between the electrodes and
      the aspect ratio of the top cells is set to

-  The observation locations. The choices after this keyword are:

   #. when giving or locations formats

   #. when using the locations format.

-  A value follows this keyword representing the number of maximum
   iterations for the inversion. **NOTE**: The program will terminate
   before the specified maximum number of iterations is reached if the
   expected data misfit is achieved and if the model norm has plateaued.
   However, if the program exits when the maximum iteration is reached,
   the file should be checked to see if the desired (based on the number
   of data and chi factor) has been reached and if the model norm is no
   longer changing. If either of these conditions has not been met then
   the program should be restarted. If the desired misfit level is not
   achieved, but the model norm has plateaued and the model is not
   changing between successive iterations, then the user may want to
   adjust the target misfit to a higher value. Also an investigation as
   to which data are most poorly fit can be informative. It may be that
   the assigned standard deviations to specific data are unrealistically
   small. The program restarts using the information in and .

-  The value at which the program reproduced the data. The choices after
   this keyword are:

   #. where the program will start with 1e-3 initially and then when the
      misfit stop decreasing, the chi factor will be changed by 10%

   #. the value to set the chi factor (1 is when the data misfit equals
      the number of data), or if a value is not there, but is given, the
      program will stop when the data misfit reaches the number of data

-  The choices after this keyword are:

   #. followed by the name of the topography file

   #. for flat topography at an elevation of 0.

-  The choices for the initial model are:

   #. name of the initial chargeability file

   #. the value for the initial chargeability throughout the mesh

   #. for the initial model to be set to the reference model.

-  The choices for the reference model are:

   #. name of the reference chargeability file

   #. the value for the reference chargeability throughout the mesh

   #. the reference model is set to zero.

-  The choices for the conductivity model (required) are:

   #. name of the conductivity file

   #. the value for the conductivity throughout the mesh. **NOTE**: The
      conductivity of a uniform half space for IP inversions should only
      be used for preliminary examination of the data. When there is
      little structure in the background conductivity, the inversion
      using this default mode can yield a reasonable chargeability model
      and it is justifiable to fit the data close to the expected misfit
      value. However, when the background conductivity deviates greatly
      from a uniform half space, reproducing the data to within the
      assumed errors will certainly result in over-fitting the data. If
      the half-space conductivity is assumed, then it is prudent to
      assign a value greater than 1.0 for chi factor when the background
      conductivity is structurally complex. The judgment can be made
      based upon the complexity of the apparent resistivity
      pseudo-section.

-  is followed by 3 constants: . These are the wave numbers used in the
   cosine transform. There will be wave values, log spaced from to in
   time. The default values (if is not given) is , , and .

-  The choices after this keyword are:

   #. where the program will set :math:`\alpha_s` =
      0.001\*(90\ :math:`/`\ max electrode separation)\ :math:`^2` and
      :math:`\alpha_x = \alpha_z = 1`.

   #. the user gives the coefficients for the each model component for
      the model objective function from equation [eq:intMOF]:
      :math:`\alpha_s` is the smallest model component, :math:`\alpha_x`
      is along line smoothness, and :math:`\alpha_z` is vertical
      smoothness.

   #. the user gives the length scales and the smallest model component
      is calculated accordingly. The conversion from :math:`\alpha`\ ’s
      to length scales can be done by:

      .. math:: L_x = \sqrt{\frac{\alpha_x}{\alpha_s}} ; ~L_z = \sqrt{\frac{\alpha_z}{\alpha_s}}

       where length scales are defined in meters. When user-defined, it
      is preferable to have length scales exceed the corresponding cell
      dimensions.

-  The weighting for the model objective function allows for three
   options:

   #. No weighting is supplied (all values of weights are 1)

   #. The weighting is supplied as a file with all the weights in one
      file

   #. The weighting is supplied as three separate files with the weight
      for the smallest model component in , the :math:`x-`\ component
      written in file and the :math:`z-`\ component written in .

-  There are two choices:

   #. Write all models and predicted data to disk. Each iteration will
      have and files where is the iteration (e.g., 01 for the first
      iteration)

   #. Only the final model and predicted data file are written. These
      files are named and for the conductivity and predicted data,
      respectively.

-  This specifies the way the system is solved:

   #. Solve the system using a subspace method with basis vectors. This
      is the solution methodology of the original code and the default
      if not given.

   #. Solve the system using a subspace method with conjugate gradients
      (CG). This allows additional constraints (i.e., Huber and Ekblom
      norms) to be incorporated into the code.

-  is used when the inversion mode is . The keyword is followed by two
   constants: specifying the maximum number of iterations (default is
   10), and specifying the solution’s accuracy (default is 0.01)

-  The Huber norm is used when evaluating the data misfit. A constant
   follows this keyword and this option is only available when using the
   inversion mode option. The default value is 1e100.

-  Use the Ekblom norm. Six (6) values should follow this keyword:
   representing the constants found in equation [eq:ekblom].

-  followed by the file name of the active cell file.

-  This option is used to decide if the reference model should be in the
   spatial terms of the model objective function (equation [eq:intMOF]).
   There are two options: to include the reference model in the spatial
   terms or to have the reference model only in the smallest model
   component.

-  The bounds options are:

   #. Do not include bounds in the inversion

   #. Give a constant global lower bound of and upper bound of .

   #. The lower bound is given in a file and is in the format.

   #. The upper bound is given in a file and is in the format.

Example of ipinv2d.inp
~~~~~~~~~~~~~~~~~~~~~~

Below is an example of the input file . The code reads mesh from the
file with topography from . The means the reference and initial models
will be set to one another and equal zero. The conductivity model is
given as the output from . The alpha values have been given for
:math:`\alpha_s=0.001` and :math:`\alpha_x = \alpha_z = 1`. The model
objective function will have an :math:`l_2` norm (which would also be
the same as ). It will start from scratch and stop after 50 iterations
if the desired misfit (equal to the number of data) is not achieved.
Conjugate gradients are used to solve the system of equations and the
bounds are given in two separate files.

+----------------------------+-----------------------------------------+
| OBS LOC\_XZ obs\_ip.dat    | ! general formatted data                |
+----------------------------+-----------------------------------------+
| TOPO FILE topography.txt   | ! topography file                       |
+----------------------------+-----------------------------------------+
| MESH FILE mesh2d.msh       | ! mesh                                  |
+----------------------------+-----------------------------------------+
| COND FILE dcinv2d.con      | ! conductivity model                    |
+----------------------------+-----------------------------------------+
| ALPHA VALUE 0.001 1 1      | ! length scales of 5 m                  |
+----------------------------+-----------------------------------------+
| CHIFACT 1.0                | ! data misfit equal to number of data   |
+----------------------------+-----------------------------------------+
| INIT\_MOD DEFAULT          | ! initial model is ref model            |
+----------------------------+-----------------------------------------+
| REF\_MOD DEFAULT           | ! ref model                             |
+----------------------------+-----------------------------------------+
| NITER 50                   | ! max iterations                        |
+----------------------------+-----------------------------------------+
| INVMODE CG                 | ! use CG solver                         |
+----------------------------+-----------------------------------------+
| BOUNDS FILE\_L lower.bnd   | ! lower bounds                          |
+----------------------------+-----------------------------------------+
| BOUNDS FILE\_U upper.bnd   | ! upper bounds                          |
+----------------------------+-----------------------------------------+

Output Files
~~~~~~~~~~~~

will create the following files:

#. The log file containing the minimum information for each iteration,
   summary of the inversion, and standard deviations if assigned by .

#. The developers log file containing the values of the model objective
   function value(\ :math:`\psi_m`), trade-off parameter
   (:math:`\beta`), and data misfit (:math:`\psi_d`) at each iteration

#. Chargeability model for each iteration ( defines the iteration step)
   if is used

#. Predicted data for each iteration ( defines the iteration step) if is
   used

#. Predicted data file that is updated after each iteration (will also
   be the predicted data)

#. Chargeability model that matches the predicted data file and is
   updated after each iteration (will also be the recovered model)

#. Model file of average sensitivity values for the mesh
