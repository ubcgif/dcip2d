.. _overview:

Package overview
================

Background
----------

 is a program library for carrying out forward modelling and inversion
of DC resistivity and induced polarization data in two dimensions.
Version 1.0 of the code was first developed in 1992. That code, or a
modified subsequent version, has been extensively used in mining and
geotechnical communities and it has also been valuable as instructional
software for learning about practical inverse theory. Improvements to
the code however, have not kept pace with advances in optimization
theory, scientific computing and computer architecture such as the
prevalence of multi-core machines. To incorporate these advances, and
renew  to a state-of-the-art inversion code, we are releasing Version ,
which has been modified and fully parallelized using OpenMP to be used
on computers with hyper-threading capabilities. The following is a list
of modifications implemented in :

-  **Electrodes can be located anywhere in the mesh**: Previous versions
   of the code required electrodes to be on mesh nodes. While convenient
   for simple arrays and few data, this added complications for working
   in regions if high topography and with non-uniform surveys such as
   Schlumberger.


-  |**Arbitrary electrode arrays**: All linear surface array data, or
   combinations thereof, can be inverted. For instance Wenner data and
   pole-dipole data can now be inverted.

-  **Borehole data can be inverted**: In the new version of DCIP2D we
   have added the capability to incorporate borehole data. These can be
   inverted individually or in conjunction with surface array data.

-  **Parallelization using OpenMP**: When many transmitters are used the
   computations can be distributed over multiple processors. The
   parallelization is invoked whenever a forward modeling is carried out
   or when **J**, the sensitivity matrix, is multiplied onto a vector
   which is required in the Conjugate Gradient (CG) solution.

-  **Handling spatially large data sets**: Solution of the numerical
   optimization problem in the previous codes was carried out using a
   subspace technique. This worked well for typical exploration surveys.
   In some cases where the number of transmitters was very large, the
   subspace method had difficulties. In Version we overcome this by
   solving the Gauss-Newton equations using a Conjugate Gradient (CG)
   method. The CG formulation is the suggested method of solution but we
   have left the original subspace solution methodology in the code.
   This will allow users to reproduce results obtained with the older
   codes. Many of the new features of the codes, for example the Huber
   and Ekblom norms, bound constraints, active and inactive cells work
   only with the CG approach.

-  **Huber norm for data misfit**: Outliers in the data will negatively
   impact the performance of the inversion algorithm. This can be
   ameliorated by working with robust norms for data misfit. The Huber
   norm, which can range from :math:`l_1` to :math:`l_2` has been
   implemented here.

-  **Incorporation of geologic information via bound constraints**: A
   priori knowledge about the background conductivity or chargeability
   can be incorporated via bound constraints. That is, each cell is
   restricted to have a model value
   :math:`m_j^{\mbox{low}} < m_j < m_j^{\mbox{high}}`, where the bounds
   :math:`m_j^{\mbox{low}}` and :math:`m_j^{\mbox{high}}` are prescribed
   by the user. The CG solution implements this through projected
   gradient techniques. This is faster than the interior point
   methodology used in the subspace method.

-  **Incorporation of geologic information using the derivative terms
   in the model objective function**: The derivative terms in the
   model objective function can take the form:
    
    .. math::
      \psi_x &= \int \limits_\Gamma \left|\frac{\partial(m-m_o)}{\partial x}\right|^2 dv \\
      :label: phimxx0Summary
    
    .. math::
      \psi_x &= \int \limits_\Gamma \left|\frac{\partial m}{\partial x}\right|^2 dv \\
      :label: phimxSummary

   Similar equations can be written for the :math:`z` derivatives.
   The difference is subtle but it has an important consequence in the
   inversion output. Consider a case where the a priori knowledge that
   a conductive overburden exists and a corresponding reference model
   has been generated. If the thickness of the overburden is known
   then the objective function with equation :eq:`phimxx0Summary`    
   should be used. The resultant model will have a boundary at the
   designated location. If the thickness is not known then equation
   :eq:`phimxSummary` should be used. When this is done the reference
   model will appear only in the smallest model component
   :math:`\psi_s` and the final model may show the discontinuity at a
   location different from that in the reference model. The capability
   of using the reference model in the derivative terms in these two
   ways is incorporated into Version . The choice of which objective
   function to use also arises with reference models generated from
   borehole information.

-  **Incorporation of a priori information using active and inactive
   cells**: Considerable flexibility in constructing different types of
   solutions is afforded by being able to have certain cells “inactive.”
   This can reduce the size of the inverse problem and also be used to
   incorporate a priori knowledge about the model. Inactive cells are
   used in the forward modeling but they are held fixed in the
   inversion. Again, there are choices to be made regarding whether the
   values of the inactive cells should, or should not, affect the
   neighboring cells. Different models are obtained from the two
   options.

-  **Mesh builder**: The forward modelling requires that an underlying
   mesh be built and evaluated. An updated GUI is provided to help
   accomplish this job. The validity of the mesh is evaluated by
   computing the apparent resistivity for a homogenous halfspace.

-  **Sensitivity Matrix**: A cumulative sensitivity matrix is generated.
   This is insightful for survey design and also it can be used as proxy
   for depth of investigation.

-  **Depth of investigation**: The forward problem is numerically solved
   on a large domain but the data are only sensitive to a portion of
   that volume. Structure appearing at depth and outside the lateral
   extent of the data array is controlled only by the model objective
   function. As such it is best removed for final presentation. The
   useful region of investigation can be estimated using the DOI process
   :ref:`OldenburgLi99` or by using the cumulative
   sensitivity analysis.

-  **Selecting wave numbers**: Working in 2D requires that Maxwell’s
   equations are solved in the wave number domain and a cosine transform
   is applied to obtain data in space. In previous codes the number and
   value of wave numbers were hardwired into the code. Default values
   are still generated but for highly unusual data sets there may be a
   reason to explore the dependence of solution accuracy with wave
   number selection.

 capabilities and limitations
-----------------------------

The package that can be licensed includes the following components:

Array types
~~~~~~~~~~~

All linear survey surface-array types, including non-standard or un-even
arrays, as well as their combinations can be inverted. The GUI is mainly
designed to handle the most commonly used array types and therefore it
works well with dipole-dipole, pole-dipole, pole-pole and Wenner or
RealSection arrays. Borehole data can be inverted but the user interface
does not yet support borehole array types.

The Earth model
~~~~~~~~~~~~~~~

Inversion for a 2D model of the earth implies that data were gathered
along a survey line at the surface.  considers the subsurface in terms
of a mesh of rectangular cells. Numerical accuracy in increased by using
smaller cells but this also increases the size of the problem. here are
usually at least three cells between electrode positions and the
discretized volume extends well beyond the data area.

 program library content
------------------------

#. **Executable programs** for performing forward modelling and
   inversion of 2D DC resistivity or induced polarization surveys. The
   DCIP2D library (Windows or Linux platforms) consists of the programs:

   -  : calculates DC resistivity and/or IP data based on a given model
      of the earth.

   -  : inverts 2D DC resistivity data

   -  : inverts 2D IP resistivity data

#. **Graphical user interfaces** are supplied for the Windows platforms
   *only*. Facilities include:

   -  : a primary interface for setting up the inversion and monitoring
      progress.

   -  : a utility for viewing raw data, error distributions, and for
      comparing observed to predicted data directly or as difference
      maps.

   -  : a utility for displaying resulting 2D models, and graphs of
      convergence behaviour.

   -  : a separate utility for building synthetic 2D models and then
      running forward modelling to produce a synthetic data set.

#. **Example data sets and excercises** can be provided upon request.
