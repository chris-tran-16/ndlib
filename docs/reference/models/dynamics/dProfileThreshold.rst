*********
Threshold
*********

The Profile-Threshold model, introduced by Milli et al. in [#]_, assumes the existence of node profiles that act as preferential schemas for individual tastes but relax the constraints imposed by the Profile model by letting nodes influenceable via peer pressure mechanisms.

The peer pressure is modeled with a threshold. 

The diffusion process starts from a set of nodes that have already adopted a given behaviour S:

- for each of the susceptible node an unbalanced coin is flipped if the percentage of its neighbors that are already infected excedes its threhosld. As in the Profile Model the coin unbalance is given by the personal profile of the susceptible node;
- if a positive result is obtained the susceptible node will adopt the behaviour, thus becoming infected.
- if the **blocked** status is enabled, after having rejected the adoption with probability ``blocked`` a node becomes immune to the infection.
- every iteration ``adopter_rate`` percentage of nodes spontaneous became infected to endogenous effects.

--------
Statuses
--------

During the simulation a node can experience the following statuses:

===========  ====
Name         Code
===========  ====
Susceptible  0
Infected     1
Blocked      -1
===========  ====

----------
Parameters
----------

============  =====  ===============  =======  =========  =====================
Name          Type   Value Type       Default  Mandatory  Description
============  =====  ===============  =======  =========  =====================
threshold     Node   float in [0, 1]   0.1     False      Individual threshold
profile       Node   float in [0, 1]   0.1     False      Node profile
blocked       Model  float in [0, 1]   0       False      Blocked nodes
adopter_rate  Model  float in [0, 1]   0       False      Autonomous adoption
============  =====  ===============  =======  =========  =====================

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
.. autoclass:: ndlib.models.dynamic.DynProfileThresholdModel.DynProfileThresholdModel
.. automethod:: ndlib.models.dynamic.DynProfileThresholdModel.DynProfileThresholdModel.__init__(graph)

.. automethod:: ndlib.models.dynamic.DynProfileThresholdModel.DynProfileThresholdModel.set_initial_status(self, configuration)
.. automethod:: ndlib.models.dynamic.DynProfileThresholdModel.DynProfileThresholdModel.reset(self)

^^^^^^^^
Describe
^^^^^^^^

.. automethod:: ndlib.models.dynamic.DynProfileThresholdModel.DynProfileThresholdModel.get_info(self)
.. automethod:: ndlib.models.dynamic.DynProfileThresholdModel.DynProfileThresholdModel.get_status_map(self)

^^^^^^^^^^^^^^^^^^
Execute Simulation
^^^^^^^^^^^^^^^^^^
.. automethod:: ndlib.models.dynamic.DynProfileThresholdModel.DynProfileThresholdModel.iteration(self)
.. automethod:: ndlib.models.dynamic.DynProfileThresholdModel.DynProfileThresholdModel.execute_snapshots(bunch_size, node_status)

*NB: the ``execute_iterations()`` method is unavailable for this model (along with other thresholded models).*

-------
Example
-------

In the code below is shown an example of instantiation and execution of a Profile Threshold model simulation on a random graph: we set the initial infected node set to the 10% of the overall population, assign a profile of 0.25 and a threshold of 0.15 to all the nodes.

.. code-block:: python

    import networkx as nx
    import dynetx as dn
    import ndlib.models.ModelConfig as mc
    import ndlib.models.dynamic.DynProfileThresholdModel as prTr
    from past.builtins import xrange

    # Dynamic Network topology
    dg = dn.DynGraph()

    for t in xrange(0, 3):
        g = nx.erdos_renyi_graph(200, 0.05)
        dg.add_interactions_from(g.edges(), t)

    # Model selection
    model = prTr.DynProfileThresholdModel(dg)
    config = mc.Configuration()
    config.add_model_parameter('blocked', 0)
    config.add_model_parameter('adopter_rate', 0)
    config.add_model_parameter('fraction_infected', 0.1)

    # Setting nodes parameters
    threshold = 0.15
    profile = 0.25
    for i in g.nodes():
        config.add_node_configuration("threshold", i, threshold)
        config.add_node_configuration("profile", i, profile)
    
    model.set_initial_status(config)


    # Simulate snapshot based execution
    iterations = model.execute_snapshots()

.. [#] Milli, L., Rossetti, G., Pedreschi, D., & Giannotti, F. (2018). Active and passive diffusion processes in complex networks. Applied network science, 3(1), 42.
