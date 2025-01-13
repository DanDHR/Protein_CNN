_This code implements my own CNN model that predicts the subcellular location of a protein based on it's sequence from custom made dataset collected from UniProtKB._<br /> 



# To run the code: <br /> 
1. Clone repo from github: `git clone https://github.com/DanDHR/Protein_CNN.git` <br />
2. Unzip `data.zip`. Note that the a copy of the input data for the model, generarted from my data cleaning/generation script `Data_filter.Rmd`, has already been saved to `data/x_data.csv` and `data/y_data.csv` for input x and target y respectively. `data/x_data.csv` currently contains 5998 protein sequence samples that have been hot encoded and padded to the same length, and `data/y_data.csv` currently contains 5998 samples with 227 classes.
3. To ensure no dependence clashes, use: <br />
        `python -m venv test_env` - Create a new python environment<br />
        `source test_env/bin/activate` - Activate the new environment<br />
       `pip install -r requirements.txt` - Install the all dependences<br />
4. Run either `cnn_no_regulizer.ipynb` or `cnn_regulizer.ipynb` to choose the version with or without L2 regularization (see explaination below). <br />
5. Note that the plots from the experiments are located in folders `final_plots_no_regularizer` and `final_plots_regularizer`. <br />



# Details on data collection on proteins from UniProtKB
The experiments used protein sequences taken from UniProtKB by following the steps bellow:<br /> 

1. Search "*" in the search bar.<br /> 
2. In the left hand side select "Human" from "Popular organisms" and "3D structure" from "Proteins with".<br /> 
3. In the "Customize columns" select only "Organism", "Sequence" and "Subcellular location [CC]" for poteintial columns.<br /> 
A potential view of that UniProtKB search might be:<br /> 
![image](https://github.com/user-attachments/assets/de823b37-a6fc-4cf7-8c14-8793195b4e11)

The labels for the protein sequences are taken from UniProtKB from the following link:<br /> 
https://www.uniprot.org/help/subcellular_location
And install the `subcell.txt` from the "Related documents" category.



# Details on data cleaning for both protein sequence and protein labels/locations in R
The next step would then be cleaning the data. The `Data_filter.Rmd` code cleanes the data in the following order:<br /> 
1. Loading protein sequences and cleaning rows and columns containing N/A.<br /> 
2. Loading all possible subcellular locations from `subcell.txt` file.<br /> 
3. Combine both the protein sequence and protein locations into a matrix, preparing it for further processing.<br />
4. Remove all of the proteins larger than 75% of the total protein sequence length (great for removing outliers).<br />
6. Remove any missed empty columns and rows.<br />
7. Prepare the protein sequences by applying one-hot encoding and then pad the sequences to match lengths.<br />
8. Prepare the data into matrices and export them as `.csv` files inside a `data/` folder.<br />



# CNN predictor: <br /> 

A convolutional neural network was chosen because of its effieceincy of recognising spatial patterns within the protein sequence. This is crucial since the only information I will be training it is the one-hot encoded table of protein sequences. While other machine learning methods have been considered, e.g RNNs, MLPs etc. CNN was chosen as a compromise between my computer's computational capacity and the data complexity.<br />

In terms of performance and generalisation, the model has been tested at various depths for performance in terms of validation accuracy and the current depth of network was chosen to optimise performance while ensuring that it can be ran on a standard PC within reasonable amount of time, while being able to tackle the complexity commonly associated with biominformatic datasets. Dropout was added as earlier runs of the code where validation was showing strong signs of overfitting through the training and validation losses.<br />

As this is a multi-class classification problem, for any positive class, we can label the said class as 1 (i.e. the correct location of the protein within the cell), and every other class as 0 (absense of the protein within that location). If we concentrate on the model's ability to gain true positives and true negatives (through the confusion matrix), we can see the the model is rarely able to truly predict the correct location of the protein for both classes. While the accuracy was high, the confusion matrix has shown that the true positive predictions were greatly affected by class imbalance (for each protein it is much more common for label to be 0, than 1 in the dataset), causing the model to severely overfit, as consistently predicting 0 leads to a higher accuracy, despite consistently misclassifying most examples with the true label of 1. <br />

This lead to the creation of the second version - identical to the first with the exception of adding L2 regularization in attempt to tackle model overfitting, and in particular, class imbalance, at the expense of overall training and validation accuracy. This is not a valid solution however, as it greatly increased the number of false positive predictions. Nevertheless, the implementation was kept here as it produced a notable, albeit small, improvement in the correctness of predictions in the minority class. Should the scope of the project be broader, more time would have been spent on investigating methods tackling such class imbalance.<br />



















