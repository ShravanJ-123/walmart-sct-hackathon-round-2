# walmart-sct-hackathon-round-2
Prediction of hard drive failure using S.M.A.R.T statistics. 

# Problem Understanding
<ul>
<li>Hard disk failures can be catastrophic in large scale data centers leading to potential loss of all data. To alleviate the impact of such failures, companies are actively looking at ways to predict disk failures and take preemptive measures</li>
<li>Traditional threshold-based algorithms were successful in predicting drive failures only 3-10% of the times[1].</li>
<li>Thus, we saw a shift to learning based algorithms that use Self-Monitoring, Analysis and Reporting Technology (S.M.A.R.T) attributes to make predictions.</li>
<li>Hard disk failures can be costly, both in terms of data recovery efforts and the need to replace or repair hardware. </li>
<li>Solving the problem reduces these costs by preventing failures before they occur or by implementing proactive measures that minimize their impact.</li>
</ul>

# Data Processing 
Backblaze owns and operates multiple data centers that have more than a million drives [4] and they regularly releases reports about the performance of these drives.
<ul>
<li>Date: Indicates recording date of the data.</li>
<li>Serial Number: Unique identifier for each hard drive.</li>
<li>Model: Name of the hard drive model.</li>
<li>Capacity Bytes: Total storage capacity of the hard drive in bytes.</li>
<li>Failure: Binary indicator of whether the hard drive has failed (1) or not (0).</li>
<li>SMART X Normalized: Normalized values of SMART attributes indicating various health and operational aspects.</li>
<li>SMART X Raw: Original unprocessed values of SMART attributes providing insight into the hard drive's condition.</li>
</ul>


# Data Cleaning
<ul>
<li><b>Column Dropping:</b></li>
<ol type="1">
<li>Columns with names containing "normalized" are dropped using a filtering mechanism.</li>
<li>Columns with all NaN values are dropped as they don't provide any meaningful information.</li>
</ol>
<li><B>Handling Missing Values:</B></li>
<ol <ol type="1">
<li>Rows containing any NaN values are removed from the dataset. This ensures that the dataset is free from missing values, which could potentially affect the analysis.</li>
</ol>
<li><B>Removing Columns with Constant Values:</B></li> 
<ol <ol type="1">
<li>Columns with the same value for all records are identified and dropped, except for certain columns like 'model' and 'capacity_bytes'.</li>
</ol>
<li><B>Sorting Data:</B></li> 
<ol <ol type="1">
<li>The data is sorted based on 'serial_number' and 'date'. </li>
</ol>
</ul>

# Feature Engineering
Domain knowledge and PCA was used to determine the important features for each of the models. Backblaze suggested the use of raw S.M.A.R.T statistic 5, 187, 188, 197 and 198 for the purpose of analysis [9]. Similar suggestions were made in pioneering research papers in this domain. Hence, we included these 5 statistics along with the other columns that we retained after the data cleaning process. 
<ul>
<li><B>Identifying Last Day Data:<B></li>
<ul><li>The last day's data for each hard drive is extracted. This could be useful for creating features such as the latest values of certain attributes before a failure event.</li></ul>
  
<li><B>Labeling Failure Status:</B></li>
<ul><li>If a hard drive has failed on the last day, all records corresponding to that hard drive within the last 'n' days are labeled as failures. </li></ul>
</ul>

# Model Building
<ul>
<li>We tested performance on 2 classifiers 1) Random Forest and 2) XGBoost so that we could get a better idea of how various transformations to the dataset were improving over all performance.</li>
<li>Random Forest, as an ensemble method built on decision trees, tends to excel in scenarios where the data has simpler relationships and is less prone to overfitting, making it a robust choice for smaller datasets or those with high levels of noise.</li>
<li>On the other hand, XGBoost's optimized gradient boosting framework is adept at capturing complex nonlinear relationships in the data, making it a preferred option when dealing with intricate feature interactions or large-scale datasets.</li>
</ul>

# Model Evaluation
<ul>
<li>Table 1 : Random Forest results on original dataset for the 1st quarter of 2019 </li>
</ul>

<i>Accuracy: 0.9994109693116142</i>
<table>
  <thead>
    <tr>
      <th>Lables</th>
      <th>Precision</th>
      <th>Recall</th>
      <th>F1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
    </tr>
    <tr>
      <td>1</td>
      <td>0.47</td>
      <td>0.05</td>
      <td>0.08</td>
    </tr>
  </tbody>
</table>


<ul>
<li>Table 2 : XGBoost results after limiting to last 10 days for the 1st quarter of 2019
 </li>
</ul>

<i>Accuracy: 0.9951358893008677
</i>
<table>
  <thead>
    <tr>
      <th>Lables</th>
      <th>Precision</th>
      <th>Recall</th>
      <th>F1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1.00</td>
      <td>1.00</td>
      <td>1.00</td>
    </tr>
    <tr>
      <td>1</td>
      <td>0.89</td>
      <td>0.16</td>
      <td>0.28</td>
    </tr>
  </tbody>
</table>








