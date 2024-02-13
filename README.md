<h1>Evaluating Biases in Bank Fraud Detection</h1>

<h3>Overview</h3>
<p>ML classification tools are becoming more widely used to identify theft, recommend ideal sentence lengths for prisoners, and screen applications for housing, insurance plans, and more. Unfortunately, these projects have been shown to reflect systemic biases. This project uses <a href="https://arxiv.org/pdf/2211.13358.pdf">Feedzai's 2022 paper</a> to analyze statistical bias in six variations of tabular data on bank fraud. By evaluating bias metrics in both ideal and non-ideal conditions, we can understand techniques for reducing discrimination in bias-sensitive production environments.

<!--I screen for bias using six metrics:-->

<h3>Base dataset</h3>
Researchers at the Universidade do Porto compiled this dataset in 2022, citing a need for unique, high-quality tabular datasets in an era oversaturated with unsupervised training data for NLP models. The dataset includes one million transactions with thirty features over ten months, and was automatically generated with CTGAN, a Generative Adversarial Network (GAN). This CTGAN was trained on ten months of real bank data, itself perturbed with a Laplacian noise mechanism, before generating 2.5 million simulated transactions. 
<br />
My random forest model of the base dataset achieved 98.5% accuracy with the specified hyperparameters, which the dataset's creators listed as a target value. My false positive rate, or FPR (which the dataset creators considered a better metric) was < 1.14%.</p>
