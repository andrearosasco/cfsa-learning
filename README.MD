## Directory Structure 
This is the current structure of the project. Note that the project structure is likely to change in the future.
```python
├── cfsa         #  package containing the proposed few shot algorithm
├── comparisons  #  other few-shot algorithms used for comparisons (when possible directly cloned from GitHub)
├── data         #  training and evaluation datasets
│   ├── coco   
│   └── lvis
├── scripts      #  the only files that contains a main (training, evaluation, etc.)
└── utils
    ├── dataset  #  dataset utilities (dataset classes, data pre/post processing) 
    └── model    #  torch.nn.Modules that are not directly part of the algorithm (e.g. feature extractors)
```
