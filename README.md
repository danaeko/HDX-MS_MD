# Simulation Validation Against Experimental Data

This repository contains the methodology and results for validating **MD simulations** using **HDX-MS experimental data**. The validation was conducted using the **Python package HDXer** and the **BestVendruscolo** method to compute protection factors for the residues.

---

To compute protection factors for the residues, the following predictive model was used:

```math
\ln P_i = (\beta_C N_{C,i} + \beta_H N_{H,i})
```

### HDXer: Backbone Amide Protection Factor Estimation

This model estimates **𝑃𝑖** (protection factor) for each backbone amide as an ensemble average function of:

- **Heavy atom contacts**
- **H bonds** formed by the amide NH group.

The number of H bonds and heavy atom contacts formed by each amide NH group determines the protection factor for each backbone amide.

By default, HDXer evaluates H bonds and contacts similarly to the originally-parameterized model:

- **H bonds** are counted if there is a protein oxygen atom within **2.4 Å** of the amide H atom.
- **Heavy-atom contacts** are computed as the total number of **nearby protein non-hydrogen atoms** within **6.5 Å** of the amide N atom.
- **Contacts from sequence neighbors** of the amide (residues i-2 to i+2) are **excluded** from the total.

This phenomenological model includes two empirical scaling parameters to reflect the relative contributions of:

- **Heavy atom contacts (𝛽𝐶)**
- **H bonding (𝛽𝐻)**

For this study, the default values were used:

- **𝛽𝐶 = 0.35**
- **𝛽𝐻 = 2.0**

These values were parameterized using a training dataset of **globular, monomeric proteins**.

---

Using the calculated **protection factors**, HDXer computes the **fractional deuteration** of each amide at a nominal deuterium exposure time (**𝐷𝑖,𝑡**) using:
```math
𝐷𝑖,𝑡 = 1 - \exp(-k_{int,i} t / P_i)
```
where:

- **𝑘int,𝑖** = Intrinsic rate of exchange for each amide.
  - Automatically computed by HDXer based on **experimentally-determined reference data**.

---

## Experimental Conditions
The HDX-MS experiments were conducted under the following conditions:

- **Temperature**: 293 K  
- **pD**: 7.9  

These parameters were incorporated into the **predictive model**.

---

## Computational and Experimental Comparison
Three independent replicas for the following systems were analyzed:

1. **Full-length PI3Kα WT**
2. **ΔABD p110α WT**

The **H/D exchange rates** were predicted for all available fragments at:

- **3 seconds**
- **30 seconds**
- **300 seconds**

The computational results were then compared to **experimental data**.

---

#### References
1. P.S. Lee, R.T. Bradshaw, F. Marinelli, K. Kihn, A. Smith, P.L. Wintrode, et al. Interpreting hydrogen-deuterium exchange experiments with molecular simulations: tutorials and applications of the HDXer ensemble reweighting software. Living J Comput Mol Sci (2021); 3: 1521. DOI: 10.33011/livecoms.3.1.1521

2. R.T. Bradshaw, F. Marinelli, J.D. Faraldo-Gómez, L.R. Forrest Interpretation of HDX Data by Maximum-Entropy Reweighting of Simulated Structural Ensembles. Biophys J (2020); 118: 1649-1664. DOI: 10.1016/j.bpj.2020.02.005

3. R.B. Best, M. Vendruscolo Structural interpretation of hydrogen exchange protection factors in proteins: characterization of the native state fluctuations of CI2. Structure (2006); 14: 97-106. DOI: 10.1016/j.str.2005.09.012

4. Y. Bai, J.S. Milne, L. Mayne, S.W. Englander Primary structure effects on peptide group hydrogen exchange. Proteins (1993); 17: 75-86. DOI: 10.1002/prot.340170110

5. D. Nguyen, L. Mayne, M.C. Phillips, S.W. Englander Reference parameters for protein hydrogen exchange rates. J Am Soc Mass Spectrom (2018); 29: 1936-1939. DOI: 10.1007/s13361-018-2021-z





