Deploy espaloma 0.3.1 force field to parametrize your MM system
===============================================================
Pretrained espaloma force field could be deployed on arbitrary small molecule
systems in a few lines::

    # imports
    import os
    import torch
    import espaloma as esp
    
    # define or load a molecule of interest via the Open Force Field toolkit
    from openff.toolkit.topology import Molecule
    molecule = Molecule.from_smiles("CN1C=NC2=C1C(=O)N(C(=O)N2C)C")
    
    # create an Espaloma Graph object to represent the molecule of interest
    molecule_graph = esp.Graph(molecule)
    
    # load pretrained model
    espaloma_model = esp.get_model("latest")
    
    # apply a trained espaloma model to assign parameters
    espaloma_model(molecule_graph.heterograph)
    
    # create an OpenMM System for the specified molecule
    openmm_system = esp.graphs.deploy.openmm_system_from_graph(molecule_graph)

If using espaloma from a local ``.pt`` file, say for example ``espaloma-0.3.1.pt``,
then you would need to run the ``eval`` method of the model to get the correct
inference/predictions, as follows::

    # load local pretrained model
    espaloma_model = torch.load("espaloma-0.3.1.pt")
    espaloma_model.eval()

The rest of the code should be the same as in the previous example.
