.. _constructor:
==================================
kcsd2d Class Constructor
==================================

The kcsd2d class constructor is a function that returns an instance of the
kcsd2d class. Through the constructors arguments list we can input the data and
our prior knowledge about the experiment. The syntax of the constructor looks as
follows::

    >>k = kcsd2d(el_pos, pots, ArgName, ArgVal, ...)

Variables ``el_pos`` and ``pots`` containing the electrode positions and
recorded potentials are mandatory arguments and have to provided in the presented order 
for the constructor to run. All other arguments all optional and can be given 
in arbitrary order in `ArgName`, `ArgVal` pairs.  

Providing experimental data
=================================
The first argument (called by us ``el_pos``) must contain the electrode
positions. It should be a `nx2` matrix, where `n` denotes the number of
electrodes and whoose rows denote the `(y, x)` coordintates (`y`-vertical, `x`-horizontal)
of the electrode positions.

The ``pots`` argument has to be a `nxm` matrix containing signals recorded at the
corresponding `n` electrode positions of ``el_pos``. `m` denotes the number of
time frames analysed.

Providing the experimental data is enough for the constructor to run. However keep
in mind that you may exploit several extra information by calling the
constructor with several optional arguments. 

Defining the estimation area
=================================
By the estimation area we understand thr positions at which the CSD is
reconstructed. This area is set by default as::
    
    >>[X, Y] = meshgrid(X_min: gdX: X_max, Y_min: gdY: Y_max)

where ``X_min, Y_min, X_max, Y_max`` are the extreme values of the electrode
positions. ``gdX`` and ``gdY`` have default values::

    >>gdX = 0.001*(X_max-X_min); gdY = 0.001*(Y_max-Y_min);

You may resign from the default values and provide your own values by running
the constructor with optional arguments::

    >>k = kcsd2d(el_pos, pots, 'X_min', my_X_min, 'X_max', my_X_max, ...
      'Y_min', my_Y_min, 'Y_max', my_Y_maxi, 'gdX', my_gdX, 'gdY', my_gdY)

You can also define the estimation area explictly, e.g.::

    >>[my_X, my_Y] = meshgrid(0:0.01:1.4);
    >>k = kcsd2d(el_pos, pots, 'X', my_X, 'Y', my_Y);

Model parameter options
=================================
Below are listed the model parameters that you whose values may also be provided
in the constructor argument list:

===========  =================================================== =============
Parameter    Description                                         Default value
===========  =================================================== =============
``R_init``   Initial value for basis function radiusi. If not    1  
             provided, the default value is set to the minimal 
             inter-electrode distance. Can later be chosen
             via cross-validation.
       
``h``        h parameter of the kCSD2d method.                   1     

``sigma``    Space conductivity.                                 1 
 
``lambda``   Lambda parameter for ridge regression.              0
===========  =================================================== ============= 



Layout of the sources
=================================
By default the base sources are distributed in the estimation area. This can be
changed by choosing the options ``ext_X`` and ``ext_Y`` while calling the
constructor. The overall number of sources can also be chosem  arbitrarily.

===========  =================================================== =============
Parameter    Description                                         Default value
===========  =================================================== =============
``ext_X``    length by which the sources go out beyond the       0 
             estimation area in the X direction.

``ext_Y``    length by which the sources go out beyond the       0 
             estimation area in the Y direction.

``n_src``    initial number of basis elements used.              300
===========  =================================================== =============

Tests
=================================
If you have a test set with a CSD profile that generated the potentials in
``pots``, you may provide it in the constructor. Say you CSD profile: ``CSD_test``
defined on an area ``X, Y``. You may pass it to the constructor::

    >>k = kcsd2d(el_pos, pots, 'X', X, 'Y', Y, 'test', CSD_test);

Data management
=================================
Carrying out the kCSD method involves lot's of calculations. In order to avoid
repeating the same calculations, the ``kcsd2d`` class stores certain results in
mat-files in the `kcsd2d_class/data` directory and manages them in a smart way.
However the user may prefer not to use this feature (e.g. because of memory
reasons). The feature can be turned off while calling the constructor setting
the value of the ``manage_data`` option to 0::

    >>k = kcsd2d(el_pos, pots, 'manage_data', 0);

