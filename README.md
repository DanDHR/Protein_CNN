_This code implements my own CNN model that predicts the subcellular location of a protein based on it's sequence from custom made dataset collected from UniProtKB._<br /> 

# To run the code: <br /> 
1. Clone repo from github. <br />
2. Unzip `data.zip`. Note that the a copy of the input data for the model, generarted from my data cleaning/generation script `Data_filter.Rmd`, has already been saved to `data/x_data.csv` and `data/y_data.csv` for input x and target y respectively. `data/x_data.csv` currently contains 5998 protein sequence samples that have been hot encoded and padded to the same length, and `data/y_data.csv` currently contains 5998 samples with 227 classes.
3. To ensure no dependence clashes, use: <br />
        `python -m venv test_env` - Create a new python environment<br />
        `source test_env/bin/activate` - Activate the new environment<br />
       `pip install -r requirements.txt` - Install the all dependences<br />
4. Run either `cnn_no_regulizer.ipynb` or `cnn_regulizer.ipynb` to choose the version with or without L2 regularization (see explaination below). <br /> 

# Details on data collection and cleaning on proteins from UniProtKB
The experiments used protein sequences taken from UniProtKB by following the steps bellow:<br /> 

1.Search "*" in the search bar.<br /> 
2.In the left hand side select "Human" from "Popular organisms" and "3D structure" from "Proteins with".<br /> 
3.In the "Customize columns" select only "Organism", "Sequence" and "Subcellular location [CC]" for poteintial columns.<br /> 
A potential view of that UniProtKB search might be:<br /> 
![image](https://github.com/user-attachments/assets/de823b37-a6fc-4cf7-8c14-8793195b4e11)

The labels for the protein sequences are taken from UniProtKB from the following link:<br /> 
https://www.uniprot.org/help/subcellular_location
And install the `subcell.txt` from the "Related documents" category.

The next step would then be cleaning the data. The `Data_filter.Rmd` code cleanes the data in the following order:<br /> 
1.Loading protein sequences and cleaning rows and columns containing N/A.<br /> 
2.Loading all possible subcellular locations from `subcell.txt` file.<br /> 
3.<br /> 



