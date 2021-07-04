![test workflow](https://github.com/reymond-group/drfp/actions/workflows/tests.yml/badge.svg)
# DRFP


An NLP-inspired chemical reaction fingerprint based on basic set arithmetic.


## Description

Predicting the nature and outcome of reactions using computational methods is an important tool to accelerate chemical research. The recent application of deep learning-based learned fingerprints to reaction classification and reaction yield prediction has shown an impressive increase in performance compared to previous methods such as DFT- and structure-based fingerprints. However, learned fingerprints require large training data sets, are inherently biased, and are based on complex deep learning architectures. Here we present the differential reaction fingerprint *DRFP*. The *DRFP* algorithm takes a reaction SMILES as an input and creates a binary fingerprint based on the symmetric difference of two sets containing the circular molecular n-grams generated from the molecules listed left and right from the reaction arrow, respectively, without the need for distinguishing between reactants and reagents. We show that *DRFP* outperforms DFT-based fingerprints in reaction yield prediction and other structure-based fingerprints in reaction classification, and reaching the performance of state-of-the-art learned fingerprints in both tasks while being data-independent.

## Getting Started

*DRFP* can be installed from pypi using `pip install drfp`. However, it depends on [RDKit](https://www.rdkit.org/) which is best [installed using conda](https://www.rdkit.org/docs/Install.html).

Once DRFP is installed, there are two ways you can use it. You can use the cli app `drfp` or the library provided by the package.

### CLI
```bash
drfp my_rxn_smiles.txt my_rxn_fps.pkl -d 512
```

This will create a pickle dump containing an numpy ndarray containing DRFP fingerprints with a dimensionality of 512. To also export the mapping, use the flag `--mapping`. This will create the additional file `my_rxn_fps.map.pkl`. You can call `drfp --help` to show all available flags and options.

### Library
Following is a basic exmple of how to use DRFP from a Python script.
```python
from drfp import DrfpEncoder

rxn_smiles = [
    "CO.O[C@@H]1CCNC1.[C-]#[N+]CC(=O)OC>>[C-]#[N+]CC(=O)N1CC[C@@H](O)C1",
    "CCOC(=O)C(CC)c1cccnc1.Cl.O>>CCC(C(=O)O)c1cccnc1",
]

fps = DrfpEncoder.encode(smiles)
```

The variable `fps` now points to a list containing the fingerprints for the two reaction SMILES as numpy arrays.


For more examples, see the two notebooks [here](), and [here]().
## Documentation

| `DrfpEncoder.encode()` | Description | Type | Default |
|-|-|-|-|
| `X` | An iterable (e.g. a list) of reaction SMILES or a single reaction SMILES to be encoded | `Iterable` or `str` |  |
| `n_folded_length` | The folded length of the fingerprint (the parameter for the modulo hashing) | `int` | `2048` |
| `min_radius` | The minimum radius of a substructure (0 includes single atoms) | `int` | `0` |
| `radius` | The maximum radius of a substructure | `int` | `3` |
| `rings` | Whether to include full rings as substructures | `bool` | `True` |
| `mapping` |  Return a feature to substructure mapping in addition to the fingerprints. If true, the return signature of this method is `Tuple[List[np.ndarray], Dict[int, Set[str]]]` | `bool` | `False` |