*****************
Kertesz Threshold
*****************

The Kertesz Threshold model was introduced in 2015 by Ruan et al. [#]_ and it is an extension of the Watts threshold model [#]_. 

The authors extend the classical model introducing a density **r** of blocked nodes -- nodes which are immune to social influence -- and a probability of spontaneous adoption **p** to capture external influence. 

Thus, the model distinguishes three kinds of node: **Blocked** (B), **Susceptible** (S) and **Adoptiong** (A). The latter class breaks into two categories: vulnerable and stable nodes. A node can adopt either under its neighbors’ influence, or spontaneously, due to endogenous effects.


--------
Statuses
--------

During the simulation a node can experience the following statuses:

===========  ====
Name         Code
===========  ====
Susceptible   0
Infected      1
Blocked      -1
===========  ====


----------
Parameters
----------

==================  =====  ===============  =======  =========  =======================
Name                Type   Value Type       Default  Mandatory  Description
==================  =====  ===============  =======  =========  =======================
adopter_rate        Model  float in [0, 1]   0       False      Exogenous adoption rate
percentage_blocked  Model  float in [0, 1]   0.1     False      Blocked nodes
threshold           Node   float in [0, 1]   0.1     False      Individual threshold
==================  =====  ===============  =======  =========  =======================

The initial infection status can be defined via:

    - **fraction_infected**: Model Parameter, float in [0, 1]
    - **Infected**: Status Parameter, set of nodes

The initial blocked nodes can be defined via:

    - **percentage_blocked**: Model Parameter, float in [0, 1]
    - **Blocked**: Status Parameter, set of nodes

In both cases, the two options are mutually exclusive and the latter takes precedence over the former.

-------
Methods
-------

The following class methods are made available to configure, describe and execute the simulation:

^^^^^^^^^
Configure
^^^^^^^^^
.. autoclass:: ndlib.models.epidemics.KerteszThresholdModel.KerteszThresholdModel
.. automethod:: ndlib.models.epidemics.KerteszThresholdModel.KerteszThresholdModel.__init__(graph)

.. automethod:: ndlib.models.epidemics.KerteszThresholdModel.KerteszThresholdModel.set_initial_status(self, configuration)
.. automethod:: ndlib.models.epidemics.KerteszThresholdModel.KerteszThresholdModel.reset(self)

^^^^^^^^
Describe
^^^^^^^^

.. automethod:: ndlib.models.epidemics.KerteszThresholdModel.KerteszThresholdModel.get_info(self)
.. automethod:: ndlib.models.epidemics.KerteszThresholdModel.KerteszThresholdModel.get_status_map(self)

^^^^^^^^^^^^^^^^^^
Execute Simulation
^^^^^^^^^^^^^^^^^^
.. automethod:: ndlib.models.epidemics.KerteszThresholdModel.KerteszThresholdModel.iteration(self)
.. automethod:: ndlib.models.epidemics.KerteszThresholdModel.KerteszThresholdModel.iteration_bunch(self, bunch_size)

-------
Example
-------

In the code below is shown an example of instantiation and execution of a Kertesz Threshold model simulation on a random graph: we set the initial infected as well blocked node sets equals to the 10% of the overall population, assign a threshold of 0.25 to all the nodes and impose an probability of spontaneous adoptions of 40%.


.. code-block:: python

    import networkx as nx
    import ndlib.models.ModelConfig as mc
    import ndlib.models.epidemics.KerteszThresholdModel as ks

    # Network topology
    g = nx.erdos_renyi_graph(1000, 0.1)

    # Model selection
    model = ks.KerteszThresholdModel(g)
        
    # Model Configuration
    config = mc.Configuration()
    config.add_model_parameter('adopter_rate', 0.4)
    config.add_model_parameter('percentage_blocked', 0.1)
    config.add_model_parameter('fraction_infected', 0.1)

    # Setting node parameters
    threshold = 0.25
    for i in g.nodes():
        config.add_node_configuration("threshold", i, threshold)

    model.set_initial_status(config)

    # Simulation execution
    iterations = model.iteration_bunch(200)


.. [#] Z. Ruan, G. In ̃iguez, M. Karsai, and J. Kertész, “Kinetics of social contagion,” Phys. Rev. Lett., vol. 115, p. 218702, Nov 2015.
.. [#] D. J. Watts, “A simple model of global cascades on random networks,” Proceedings of the National Academy of Sciences, vol. 99, no. 9, pp. 5766–5771, 2002.
