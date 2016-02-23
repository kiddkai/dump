Nomad
=====

Glossary
--------

- Job - describe things that should be running, not where should things get deployed to.
- Task Group - a group that task should run together
- Driver - The basic means of execcuting Tasks(Docker, Java, binaries)
- Task - smallestt unit of work in Nomad. Executed by *drivers*. Task specified driver, configuration
  of the driver, constrains, and resources required.
- Client - a machine that task can be run on. All client must runs nomad agent.
- Allocation - An Allocation is a mapping between a task group in a job and a client node. 
- Evaluation - Evaluations are the mechanism by which Nomad makes scheduling decisions.
- Server - Nomad servers are the brains of the cluster.
- Regions and Datacenters - Nomad models infrastructure as regions and datacenters. Regions may contain multiple datacenters.
- Bin Packing - Bin Packing is the process of filling bins with items in a way that maximizes the utilization of bins.
