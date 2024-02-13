<h1>Evaluating Biases in Bank Fraud Detection</h1>

<h3>Overview</h3>
<p>ML classification tools are becoming more widely used to identify shoplifters, recommend ideal sentence lengths for prisoners, and screen applications for housing, insurance plans, etc Unfortunately, such algorithms have been shown to reflect systemic biases. My project uses <a href="https://arxiv.org/pdf/2211.13358.pdf">Feedzai's 2022 paper</a> to analyze statistical bias in six variations of tabular data on bank fraud. By evaluating bias metrics in both ideal and non-ideal conditions, we can understand techniques for reducing discrimination in bias-sensitive production environments.</p>
<p>I screen for bias using six metrics (<a href="https://arxiv.org/abs/2106.00467v3">Castelnovo et al., 2021</a>):</p>
<ul>
  <li>Group biases</li>
  <ul>
    <li><b>Disparate Impact:</b> The ratio of rate of favorable outcomes for the unprivileged group to that of the privileged group.</li>
    <li><b>Statistical parity difference:</b> Difference of favorable outcomes received by the privileged group to the unprivileged group</li>
    <li><b>Equal Opportunity Difference:</b> The difference of true positive rates between the unprivileged and the privileged groups</li>
    <li><b>Average Odds Difference:</b> The average difference of false positive rate (false positives / negatives) and true positive rate (true positives / positives) between unprivileged and privileged groups</li>
  </ul>
  <li>Individual biases</li>
  <ul>
    <li><b>Theil Index:</b> The generalized entropy of benefit for all individuals in the dataset, with alpha = 1.</li>
    <li><b>Consistency: </b>The similarity of predictions among a row's k-nearest neighbors.</li>
  </ul>
</ul>

<h3>Base dataset</h3>
<p>Researchers at the Universidade do Porto compiled this dataset in 2022, citing a need for unique, high-quality tabular datasets in an era oversaturated with unsupervised training data for NLP models. The dataset includes one million transactions with thirty features over ten months, and was automatically generated with CTGAN, a Generative Adversarial Network (GAN). This CTGAN was trained on ten months of real bank data, itself perturbed with a Laplacian noise mechanism, before generating 2.5 million simulated transactions.</p>

<p>The researchers developed six variations of this dataset to help data scientists identify biases that could arise in a production environment. These variants take "customer_age" as a protected attribute _A_ (i.e. an attribute tied to discriminatory fraud detection) and differentiate customers under 50 (the privileged group) from customers over fifty (the unprivileged group). The variants are designed to reflect the following data biases:</p>
<ul>
  <li>Group size disparity. Only 10% of customers are over fifty, leading to less accurate predictions for this age group.</li>
  <li>Prevalence disparity (Variant II). Age (or directly correlating factors) were used to calculate labels. Training on these labels will therefore amplify statistical bias through a positive feedback loop. </li>
  <li>Separability disparity (Variant II). Each age group has a tendency to self-segregate in ways that can either heighten or lower risk of fraud. Feedzai's paper gives the example of younger people being more willing to use ATMs at night in poorly-lit areas, heightening their use of fraud.</li>
</ul>

<p>My random forest model of the dataset achieved ~98.5% accuracy with the specified hyperparameters, which the dataset's creators listed as a target value. My false positive rate, or FPR (which the dataset creators considered a better metric) was ~1.14%.</p>

<h3>Evaluating Group Bias</h3>
<p>Group bias is immediately apparent from my model's disparate impact and statistical parity difference. These consider the ratio and difference, respectively, of each group's number of positive predictions. With disparate_impact_ratio = 0.14 and statistical_parity_difference = 49/57, it is clear that we need a metric independent of total population size. However, this does raise immediate questions about whether the lack of training data for our unprivileged group affects its accuracy. Clearly, steps should be taken to equalize each group's number of inputs for similar cases in production environments--whether by removing training data on our privileged group or making a more active commitment to collecting data on our underprivileged group.</p>

<p><img src="https://github.com/bfitzgerald3132/BankFraudDetection/blob/main/Screenshot%202024-02-13%20105644.png?raw=true" /></p>
