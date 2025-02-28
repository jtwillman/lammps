.. index:: pair_style rheo/solid

pair_style rheo/solid command
=============================

Syntax
""""""

.. code-block:: LAMMPS

   pair_style rheo/solid

Examples
""""""""

.. code-block:: LAMMPS

   pair_style rheo/solid
   pair_coeff * * 1.0 1.5 1.0

Description
"""""""""""

.. versionadded:: TBD

Style *rheo/solid* is effectively a copy of pair style
:doc:`bpm/spring <pair_bpm_spring>` except it only applies forces
between solid RHEO particles, determined by checking the status of
each pair of neighboring particles before calculating forces.

The style computes pairwise forces with the formula

.. math::

   F = k (r - r_c)

where :math:`k` is a stiffness and :math:`r_c` is the cutoff length.
An additional damping force is also applied to interacting
particles. The force is proportional to the difference in the
normal velocity of particles

.. math::

   F_D = - \gamma w (\hat{r} \bullet \vec{v})

where :math:`\gamma` is the damping strength, :math:`\hat{r}` is the
displacement normal vector, :math:`\vec{v}` is the velocity difference
between the two particles, and :math:`w` is a smoothing factor.
This smoothing factor is constructed such that damping forces go to zero
as particles come out of contact to avoid discontinuities. It is
given by

.. math::

   w = 1.0 - \left( \frac{r}{r_c} \right)^8 .

The following coefficients must be defined for each pair of atom types
via the :doc:`pair_coeff <pair_coeff>` command as in the examples
above, or in the data file or restart files read by the
:doc:`read_data <read_data>` or :doc:`read_restart <read_restart>`
commands, or by mixing as described below:

* :math:`k`             (force/distance units)
* :math:`r_c`           (distance units)
* :math:`\gamma`        (force/velocity units)


----------

Mixing, shift, table, tail correction, restart, rRESPA info
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

For atom type pairs I,J and I != J, the A coefficient and cutoff
distance for this pair style can be mixed.  A is always mixed via a
*geometric* rule.  The cutoff is mixed according to the pair_modify
mix value.  The default mix value is *geometric*\ .  See the
"pair_modify" command for details.

This pair style does not support the :doc:`pair_modify <pair_modify>`
shift option, since the pair interaction goes to 0.0 at the cutoff.

The :doc:`pair_modify <pair_modify>` table and tail options are not
relevant for this pair style.

This pair style writes its information to :doc:`binary restart files
<restart>`, so pair_style and pair_coeff commands do not need to be
specified in an input script that reads a restart file.

This pair style can only be used via the *pair* keyword of the
:doc:`run_style respa <run_style>` command.  It does not support the
*inner*, *middle*, *outer* keywords.

----------

Restrictions
""""""""""""

This pair style is part of the RHEO package.  It is only enabled if
LAMMPS was built with that package.  See the :doc:`Build package
<Build_package>` page for more info.

Related commands
""""""""""""""""

:doc:`fix rheo <fix_rheo>`,
:doc:`fix rheo/thermal <fix_rheo_thermal>`,
:doc:`pair bpm/spring <pair_bpm_spring>`

Default
"""""""

none
