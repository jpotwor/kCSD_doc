=====================================================================
Quick Start
=====================================================================

Setting the path
=====================================================================
Once you downloaded the folder `kcsd2d_class` you should add it to your
matlab path. It might be easiest to copy the folder to your current matlab
folder and add it (along with all the subfolders) using ``genpath()``::

    >>setpath(genpath('kcsd2d_class'))

Loading data
=====================================================================
Let's examine an examplary data set with measured potentials.
Import the mat-file `data_01.mat`::

    >>load('data_01.mat')


Now variables ``el_pos`` and ``pots`` defining the electrode positions and measured
potentials have been loaded to the workspace. Check out the elements of the
``el_pos`` variable::

    >>el_pos
    
    el_pos =
    
        0.1980         0
        0.3960         0
        0.5940         0
        0.7920         0
        0.0990    0.0990
        0.2970    0.0990
        ...
    

and it's size::

     >>size(el_pos)

        ans =
        
            60     2

It's a two-column matrice, whoose rows denote the `(y, x)` coordinates
(`y`-vertical, `x`-horizontal) of the electrode positions. Here the variable
describes 60 electrodes. 

The ``pots`` variable here  is a column vector containing the potentials measured
at the correspondind positions in ``el_pos``::

    >>pots =
    
    0.0279
    0.0314
    0.0267
    ...
    
    >>size(pots)
    
    ans =
    
    60     1

``pots`` does not have to be a single column vector. Potentials come often in time-series.
In general ``pots`` can be an `n x m` matrix, where `n` still  denotes the number of
electrodes and the consecutive `m` columns contain the evolution the potentials in
time. 
    
Introduction to the kCSD-class
=====================================================================
Methods for carrying out the kCSD method have been embedded in a Matlab class.
Those not familiar with object oriented programming can imagine 
classes as definitions of machines that provided some input can perform different actions.
Like a washing machine given clothes and some washing powder let's us
choose a program of washing the clothes, an instance of the  kCSD class  given 
information about the electrode positions and the signals recorded can perform several
variants of the kCSD method.

Having the ``el_pos`` and ``pots`` variables we can call an instance of the kCSD
class::

    >> k = kcsd2d(el_pos, pots);
    calculating b_pot_matrix & K_pot ...
    calculating interp_cross ...
 
In the above instruction we created an object ``k`` (an instance of the ``kcsd2d`` class) 
and provided it with information about the electrode positions and measured
potentials. We did that by calling the ``kcsd2d()`` function, called a `class
constructor`. The `class constructor` can be provided with several other
variables representing our prior knowledge about the conditions of the
experiment. :doc:`Click here <constructor>` for a full description of the class consructor.

But already the ``k`` object can show us it's estimate of the CSD profile::
    
    >>k.plot_CSD

Now the class object `k` runs it's built in viewer, a GUI that presents the most
up to date estimation. ``k.plot_CSD`` is a method of the ``kcsd2d`` class. 
:doc:`Click here <methods>` for a complete overview of the ``kcsd2d`` class methods.

The estimated CSD values can be extracted from `k` to the Matlab workspace::

    [X, Y, CSD_est] = [k.X, k.Y, k.CSD_est]

``X, Y`` and ``CSD_est`` are properties of the ``kcsd2d`` class. :doc:`Click here <properties>`
for an overview of all the ``kcsd2d`` class properties
