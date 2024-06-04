---
author: Kohulan Rajan
date: 2024-06-05
title-block-banner: PaCh/cover_pach.webp
bibliography: ../references.bib
---

# PaCh (Packed Chemicals): Computationally Effective Binary Format for Chemical Structure Encoding

## Why discuss this paper? 

I chose the PaCh (Packed Chemicals) paper [@Nugmanov2024] current topics in the cheminformatics seminar because 

- It aims to combine compact local stereochemistry encoding (in SMILES) with explicit bond connectivity and 2D layout (like in MDL MOL files).
- This is an interesting approach to package these two pieces of information together, which makes our group think about possible real-world applications.
- Benchmarking shows PaCh outperforms SMILES, MOL, and CML in encoding/decoding speed while maintaining compact size.
- Introduces a computationally effective format tailored for cheminformatics applications, deep learning tasks, and efficient data storage/processing.

## Context

This paper proposes a new binary format called PaCh (Packed Chemicals) that aims to combine the advantages of compact string-based SMILES and explicit bond connectivity of MDL MOL files while addressing their shortcomings. PaCh introduces a condensed representation of packing atom properties, 2D coordinates, bidirectional connectivity, and relative stereochemistry coding in an efficient manner to enable fast processing for modern data analytics workflows.

## Key Features of PaCh

- Binary Format: Utilizes a binary format for compact and efficient storage of molecular data.
- Explicit Atom Connectivity: Stores atom connectivity explicitly, including implicit hydrogens, to ensure accurate representation.
- Electronic State and Stereochemistry: Encodes electronic states, stereochemistry, and other essential molecular attributes.
- 2D Layout: Includes 2D layout information, similar to MOL files, for visualization and analysis.
- Reaction Encoding: Supports the encoding of chemical reactions, facilitating reaction analysis and retrosynthesis.
- Zlib Compression: Employs Zlib compression to further reduce storage size.

## Available representations

### SMILES
- Compact linear string representation  
- Encodes local stereochemistry and implicit hydrogens
- Limitations:
   - Representing radicals, 2D/3D data, metal complexes
   - Ring closures beyond 100
   - Explicit bonds

### INCHI  
- Comprehensive format endorsed by IUPAC
- Encodes connectivity, stereochemistry, tautomers
- Issues:
   - Flexible hydrogen mapping
   - Atomic charge determination
- Lacks 2D/3D encoding in main part

### MDL MOL/RXN
- Encodes 2D/3D coordinates, explicit radicals/charges  
- Limited stereochemistry support
- Ambiguity in implicit H counting
- Line length restrictions and optional zero fields hamper parsing

### CML/MRV
- XML-based, easier parsing than MDL
- Encodes 2D/3D
- Implicit H and stereochemistry inherit MOL ambiguities

Note: The paper highlights the limitations of existing formats, motivating the need for a new format like PaCh that combines advantages while mitigating drawbacks.

### Binary Formats

Previous attempts at binary encoding of chemical structures, such as the Chemical JSON format, have aimed to improve efficiency but often fall short in terms of compression and speed. Nugmanov et al. [@Nugmanov2024] aim to overcome these limitations with Pach.

## Problem setting

- Existing chemical structure encoding formats are not optimized for computational efficiency.
- There is a need for a format that balances compression, speed, and ease of use.

## Advantages over Existing Formats

- Addresses SMILES Limitations: Overcomes issues in SMILES related to representing radicals, complex stereochemistry, and disconnected components.
- Resolves MOL Challenges: Avoids problems with implicit hydrogen counting and parsing difficulties associated with MOL files.
- Combines Strengths: Integrates the best features of SMILES (e.g., local stereo- and implicit hydrogen encoding) and MOL (e.g., explicit bond encoding and 2D layout).

Additionally, the authors propose that the previous representations might not be "rich" enough to capture the complexity of chemical reactions.

## Approach

### PaCh Format Overview

- **Header Byte (8 bit)**:
  - Differentiates format version and data type (molecular or reaction data).
  - Molecule header byte: 2 (0x02).
  - Reaction header byte: 1 (0x01).
  - The remaining 253 values are reserved for future extensions.
  - Hard declaration of format version in the header to avoid compatibility issues seen in SMILES or MDL formats.
  - Changes to specifications for 0x01 and 0x02 require a different header byte to maintain compatibility and parsing efficiency.

### Molecule Encoding

- **Header Byte**: Starts with a byte equal to 2 (0x02).
- **12-bit Atom Count**: Encodes the number of atoms (max 4095 atoms).
- **12-bit Chiral Cis–Trans Bond Count**: Encodes the number of chiral cis-trans bonds.
- **4-byte Header Block**: Consists of the above-mentioned data.
- **Atom Blocks**: Each atom block is 9 bytes, repeated for each atom.
- **Atom Connectivity Coding**: Bidirectional coding of atom connectivity, preserving neighbour order essential for stereochemistry coding.
    - Flattened adjacency matrix without zero connections and atom indices, coded bidirectionally for space efficiency.
- **Bond Orders**:  Stored without duplicates, representing the upper triangle of the adjacency matrix without zero connections.
    - Single bond: 0b000, double bond: 0b001, triple bond: 0b010, aromatic bond: 0b011, coordinate bond: 0b111.
- **Cis–Trans Bonds**: Optional repeated 4-byte blocks for cis-trans bonds.
    - Codes 12-bit numbers of cumulene terminal atoms and bytes with states 0x00 and 0x01 for trans- and cis-forms respectively.

### Key Points

- **Header Byte Values**: 2 for molecules, 1 for reactions.
- **Future-Proofing**: 253 values reserved for extensions.
- **Compatibility**: Hard declaration of version to avoid issues.
- **Efficiency**: Simplified parsing and implementation due to structured coding.
- **Connectivity Block**: Bidirectional, ensuring consistency in neighbour order.
- **Bond Orders**: Stored efficiently without duplicates, utilizing up to 7 bits for alignment to full byte.
- **Cis–Trans Block**: Codes cumulene terminal atoms and cis/trans states for bonds.


![Figure 1: Encoded molecule example from the paper by Nugmanov et al.:](https://pubs.acs.org/cms/10.1021/acs.jcim.3c01720/asset/images/large/ci3c01720_0001.jpeg)

- **Figure 1 Overview**:
  - **(a) Block overview**: General structure of the molecule PaCh.
  - **(b) Atom (#2) block structure**: Detailed structure of a specific atom block.
  - **(c) Adjacency matrix**: Bond orders with connected atom indices and flattened connectivity.
  - **(d) Flattened bond orders for acetic acid**: Uses 2 bytes with padding.
  - **(e) Ccis/Ttrans block**: Bytes representation with white-gray row indicating the bytes.

- **Atom Block Structure (Figure 1b)**:
  - **12-bit Atom ID**: Unique label (1–4095) used in the connectivity block. Important for tasks like reaction analytics, database management, and property prediction.
  - **4-bit Atom Neighbor Count**: Ranges from 0 (e.g., halides) to 15 (hypervalent complexes like ferrocenes).
  - **2-bit Tetrahedron Stereo Sign**: Encodes one of four possible states: not chiral, unknown label, chirality sign.
  - **2-bit Allene Stereo Sign**: Same notation as for tetrahedrons.
  - **5-bit Isotope Information**: Difference from the standard isotope number +16 to avoid negative values. Special case for unspecified isotope. Covers a range from -15 to +15 relative to the standard.
  - **7-bit Atomic Number**: Maximum value of 127, with the current last element being Og (118).
  - **4 Bytes for Coordinates**: x,y coordinates of the atom in float16 format, sufficient for small molecules and saves space.
  - **3-bit Implicit Hydrogen Count**: Ranges from 0 to 6, with a special case for unknown hydrogen count. Important for accurate analysis in cheminformatics. Proper representation requires Kekule form for structures with aromatic bonds.
  - **4-bit Atomic Charge**: Ranges from -4 to 4, shifted by 4 to avoid negative numbers (e.g., 4 for neutral).
  - **1-bit Radical State**: Binary coding for radical state (0b0 for no radical, 0b1 for radical). Suitable for most organic molecules with a single unpaired electron. Limitation in coding carbenes: 0 for singlet, 1 for triplet.


### Reaction Encoding

- **Header Byte**: Reaction PaCh begins with a header byte of 1 (0x01).
- **Byte Counts**: Followed by 3 bytes representing the counts of reactants, reagents, and products.
- **Limitation**:Maximum of 255 molecules per category, totalling 765 molecules.
- **Continuous Addition**: Molecules are added continuously to the first 4 bytes without separators.
- **MDL RXN Format**: Similar to PaCh but differs in the order of molecules; reagents come last.
- **SMILES Order**: Uses the order of reactants-reagents-products with a ">" as a separator.
- **Size Information**: Packed molecules must contain byte size information for proper restoration.

![Figure 2: Structure of the reaction PaCh with one reactant and one product from the paper by Nugmanov et al.:](https://pubs.acs.org/cms/10.1021/acs.jcim.3c01720/asset/images/large/ci3c01720_0002.jpeg)

Note: PaCh is using Zlib compression for increasing memory effectiveness.

### Stereochemistry Coding

- The stereochemistry coding in PaCh is relative, meaning it depends on neighbours’ connectivity order. This is similar to SMILES notation and helps avoid misinterpretations compared to absolute stereochemistry coding.

Tetrahedron: The chirality of tetrahedral centers is determined by calculating the sign of the volume of a pyramid formed by the chiral atom and its three neighbouring atoms. A positive volume indicates clockwise rotation and is coded as 0b11, while a negative volume indicates counterclockwise rotation and is coded as 0b10.
Cumulene cis/trans: The cis/trans configuration of cumulenes with an odd number of double bonds is determined by the relative orientation of the first neighbour of each terminal atom. If the first neighbours are on the same side of the double bond chain, it's cis (0x01), and if they are on opposite sides, it's trans (0x00).
Allenes: The stereochemistry of allenes is determined by the rotation of vectors corresponding to the first neighbours of the terminal atoms across the double bond axis. Clockwise rotation is coded as 0b10, and counterclockwise rotation is coded as 0b11.

## Benchmarking

Benchmarking Results for Reading/Writing Operations on a 4200 Molecule Dataset in Milliseconds Measured in a Single Thread on Intel Xeon W-10885M CPU
This is a direct comparison with RDKit to compare the speed of each tool.

| Format         | Read (Chython) | Write (Chython) | Read (RDKit) | Write (RDKit) | Size (kB) |
|----------------|----------------|-----------------|--------------|---------------|-----------|
| MOL            | 2446          | 573             | 404          | 1256          | 9634     |
| SMILES         | 2475          | 515             | 1995         | 268           | 195      |
| Python pickle  | 243           | 72              | 210          | 123           | 1857     |
| (RDKit) INCHI  | 3760          | 1720            | 1060         | 580           | -        |
| CML            | 1710          | 15894           | -            | -             | -        |
| PaCh           | 107 (6)       | 51              | -            | -             | 1424     |
| PaCh + Zlib    | 124           | 122             | -            | -             | 853      |
| compressed MOL | 2454          | -               | -            | -             | -        |

## Feature Comparison of Formats. 
* – 2D Layout is Required; 
** – Can be Calculated from a 2D Layout, Which Leads to False Labeling for Unknown Stereo

| Feature             | SMILE | MOL | CML/MRV | PaCh |
|---------------------|-------|-----|--------|------|
| charge              | Yes   | Yes | Yes    | Yes  |
| radical state       | No    | Yes | Yes    | Yes  |
| implicit hydrogens  | No    | Yes | Yes    | Yes  |
| isotopes           | Yes   | Yes | Yes    | Yes  |
| 2D layout           | No    | Yes | Yes    | Yes  |
| coordinate bond     | No    | Yes | Yes    | Yes  |
| tetrahedron stereo  | Yes   | Yes | Yes*  | Yes  |
| allene stereo       | Yes   | Yes | Yes*  | Yes  |
| cis–trans stereo    | Yes   | Yes | Yes** | Yes  |

## Example of Usage

The PaCh format is implemented in a Chython–Python-based framework for molecule and reaction processing. Below is how to read and write PaCh using Chython.

- https://github.com/chython/chython
```python
from chython import MoleculeContainer, ReactionContainer, smiles
# reaction example
r = smiles('CC=O>>CCO')
p = r.pack()
u = ReactionContainer.unpack(p)
print(r == u)


# generate uncompressed PaCh
m = smiles('CCN')
p = m.pack(compressed=False)
u = MoleculeContainer.unpack(p, compressed=False)
print(m == u)

```


## Takeaways 

- A new binary format, PaCh (Packed Chemicals), is introduced for encoding molecular and reaction.
- This may be useful for deep learning where we have to use 2D coordinates (instead of CXSMILES) but have to test it in real-world scenarios.
- Designed to be computationally efficient, combining the strengths of existing formats like SMILES and MDL MOL while addressing their limitations.
- PaCh is implemented in the Chython library and is used in the Chytorch deep learning framework for efficient handling of chemical data.

## References
