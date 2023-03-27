# Biologically Informed Neural Network (BINN)

The BINN-package allows you to create a sparse neural network from a pathway and input file. The examples presented in [notebooks](notebooks/) use the [Reactome pathway database](https://reactome.org/) and a proteomic dataset to generate the neural network. It also allows you to train and interpret the network using [SHAP](https://github.com/slundberg/shap). Plotting functions are also available for generating sankey plots.

First, a network is created. This is the network that will be used to create the sparse BINN.

```py
input_data = pd.read_csv("../data/test_data.tsv", sep="\t")
translation = pd.read_csv("../data/translation.tsv", sep="\t")
pathways = pd.read_csv("../data/pathways.tsv", sep="\t")

network = Network(
    input_data=input_data,
    pathways=pathways,
    mapping=translation,
    verbose=True
)
```

The BINN can thereafter be generated using the network:

```py
binn = BINN(
    pathways=network,
    n_layers=4,
    dropout=0.2,
    validate=False,
)
```

An sklearn wrapper is also available:

```py
binn = BINNClassifier(
    pathways=network,
    n_layers=4,
    dropout=0.2,
    validate=True,
    epochs=10,
    threads=10,
    logger=SuperLogger("logs/test")
)
```

This generates the model:

```py
Sequential(
  (Layer_0): Linear(in_features=446, out_features=953, bias=True)
  (BatchNorm_0): BatchNorm1d(953, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (Dropout_0): Dropout(p=0.2, inplace=False)
  (Tanh 0): Tanh()
  (Layer_1): Linear(in_features=953, out_features=455, bias=True)
  (BatchNorm_1): BatchNorm1d(455, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (Dropout_1): Dropout(p=0.2, inplace=False)
  (Tanh 1): Tanh()
  (Layer_2): Linear(in_features=455, out_features=162, bias=True)
  (BatchNorm_2): BatchNorm1d(162, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (Dropout_2): Dropout(p=0.2, inplace=False)
  (Tanh 2): Tanh()
  (Layer_3): Linear(in_features=162, out_features=28, bias=True)
  (BatchNorm_3): BatchNorm1d(28, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (Dropout_3): Dropout(p=0.2, inplace=False)
  (Tanh 3): Tanh()
  (Output layer): Linear(in_features=28, out_features=2, bias=True)
)

```

### Example input

```py

#Test data (quantmatrix or some matrix containing input column - in this case "Protein")
#                        PeptideSequence Protein
# 0  VDRDVAPGTLC(UniMod:4)DVAGWGIVNHAGR  P00746
# 1  VDRDVAPGTLC(UniMod:4)DVAGWGIVNHAGR  P00746
# 2                          VDTVDPPYPR  P04004
# 3                      AVTEQGAELSNEER  P27348
# 4                     VDVIPVNLPGEHGQR  P02751
...
#Pathways file
#           parent          child
# 0    R-BTA-109581   R-BTA-109606
# 1    R-BTA-109581   R-BTA-169911
# 2    R-BTA-109581  R-BTA-5357769
# 3    R-BTA-109581    R-BTA-75153
# 4    R-BTA-109582   R-BTA-140877
...
#Translation file
#           input    translation
# 0    A0A075B6P5   R-HSA-166663
# 1    A0A075B6P5   R-HSA-173623
# 2    A0A075B6P5   R-HSA-198933
# 3    A0A075B6P5   R-HSA-202733
# 4    A0A075B6P5  R-HSA-2029481
...
```

Plotting a subgraph starting from a node generates the plot:
![Pathway sankey!](/img/sankey.png "Pathway sankey")
A compelte sankey may look like this:
![Complete sankey!](/img/test.png "Complete sankey")

---

### Installation

The package can be installed with git.

```
gh repo clone InfectionMedicineProteomics/BINN
```

### Contributors

Erik Hartman
Aaron Scott

### Contact

Erik Hartman - erik.hartman@hotmail.com
