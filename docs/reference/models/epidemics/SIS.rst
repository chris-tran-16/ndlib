***
SIS
***

The SIS model was introduced in 1927 by Kermack [#]_.
 
In this model, during the course of an epidemics, a node is allowed to change its status  from **Susceptible** (S) to **Infected** (I).

The model is instantiated on a graph having a non-empty set of infected nodes.

SIS assumes that if, during a generic iteration, a susceptible node comes into contact with an infected one, it becomes infected with probability beta, than it can be switch again to susceptible with probability lambda (the only transition allowed are S→I→S).


--------
Statuses
--------

During the simulation a node can experience the following statuses:

===========  ====
Name         Code
===========  ====
Susceptible  0
Infected     1
===========  ====

----------
Parameters
----------

======  =====  ===============  =======  =========  =====================
Name    Type   Value Type       Default  Mandatory  Description
======  =====  ===============  =======  =========  =====================
beta    Model  float in [0, 1]           True       Infection probability
lambda  Model  float in [0, 1]           True       Recovery probability
======  =====  ===============  =======  =========  =====================

The initial infection status can be defined via:

    - **fraction_infected**: Model Parameter, float in [0, 1]
    - **Infected**: Status Parameter, set of nodes

The two options are mutually exclusive and the latter takes precedence over the former.

-------
Methods
-------

The following class methods are made available to configure, describe and execute the simulation:

^^^^^^^^^
Configure
^^^^^^^^^
.. autoclass:: ndlib.models.epidemics.SISModel.SISModel
.. automethod:: ndlib.models.epidemics.SISModel.SISModel.__init__(graph)

.. automethod:: ndlib.models.epidemics.SISModel.SISModel.set_initial_status(self, configuration)
.. automethod:: ndlib.models.epidemics.SISModel.SISModel.reset(self)

^^^^^^^^
Describe
^^^^^^^^

.. automethod:: ndlib.models.epidemics.SISModel.SISModel.get_info(self)
.. automethod:: ndlib.models.epidemics.SISModel.SISModel.get_status_map(self)

^^^^^^^^^^^^^^^^^^
Execute Simulation
^^^^^^^^^^^^^^^^^^
.. automethod:: ndlib.models.epidemics.SISModel.SISModel.iteration(self)
.. automethod:: ndlib.models.epidemics.SISModel.SISModel.iteration_bunch(self, bunch_size)


-------
Example
-------

In the code below is shown an example of instantiation and execution of an SIS simulation on a random graph: we set the initial set of infected nodes as 5% of the overall population, a probability of infection of 1%, and a probability of recovery of 0.5%.

.. code-block:: python

    import networkx as nx
    import ndlib.models.ModelConfig as mc
    import ndlib.models.epidemics.SISModel as sis

    # Network topology
    g = nx.erdos_renyi_graph(1000, 0.1)

    # Model selection
    model = sis.SISModel(g)

    # Model Configuration
    cfg = mc.Configuration()
    cfg.add_model_parameter('beta', 0.01)
    cfg.add_model_parameter('lambda', 0.005)
    cfg.add_model_parameter("fraction_infected", 0.05)
    model.set_initial_status(cfg)

    # Simulation execution
    iterations = model.iteration_bunch(200)


.. [#] W. O. Kermack and A. McKendrick, “A Contribution to the Mathematical Theory of Epidemics,” Proceedings of the Royal Society of London. Series A, Containing Papers of a Mathematical and Physical Character, vol. 115, no. 772, pp. 700–721, Aug. 1927
