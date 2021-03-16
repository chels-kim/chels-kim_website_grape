---
layout: post
title: An EDA of Worldwide COVID-19 Clinical Trials
subtitle : A detailed analysis of worldwide clinical trials logged on ClinicalTrials.gov
tags: [Notebook, EDA, Data Science]
author: Chelsea Kim
comments : True
---

# Contents
- [Plan of Attack](#eda-plan-of-attack)
- [Higher-level analysis of data](#higher-level-analysis-of-data)
- [Brainstorming](#brainstorming-questions-id-like-to-ask)
- [Detailed analysis](#exploratory-analysis)
   1. [COVID-related vs. unrelated studies](#1-comparison-of-covid--and-non-covid-related-studies)
   2. [A deeper dive into COVID-related studies](#2-a-deeper-dive-into-covid-related-studies-only
      )
      

<br>  
---

## EDA Plan of Attack
1. **High-level understanding of data**
    - First, I will take down notes about the data from the Kaggle descriptions
    - I might also check out the associated notebook to get a better sense
2. **Brainstorm some questions** I would like to ask and analyze the data for.
    - I will probably also get inspiration from the aforementioned notebook
3. **Learn as I go!** Specifically, my goals for this EDA are:
    - get more comfortable working with the basic packages like Numpy and Pandas
    - get used to graphing using matplotlib library since that's the most common one

### Overall timeline goal
- Today is February 25, 2021; I would like to finish this project in maximum 1 week, so March 4th at the latest.



### Some quick notes on the data

- Maintained by the NIH, the database at [ClinicalTrials.gov](ClinicalTrials.gov) contains information about all privately and publicly funded clinical studies around the world.
- The particular dataset to be used consists of clinical trials related to COVID-19 studies specifically.
    - XML files: each correspond to one study; filename: `NCT########`, where the `#`'s indicate unique numerical identifiers
     of studies.
    - 1 CSV file: not as detailed as above but provides a summary.



<br>

# Higher-level analysis of data



```python

# print the first few rows of studies dataframe and describe it
df_studies.head()
df_studies.describe()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>enrollment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>5020.000000</td>
      <td>4.989000e+03</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2509.500000</td>
      <td>1.981830e+04</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1449.293506</td>
      <td>4.333506e+05</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1254.750000</td>
      <td>6.000000e+01</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>2509.500000</td>
      <td>1.750000e+02</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3764.250000</td>
      <td>5.600000e+02</td>
    </tr>
    <tr>
      <th>max</th>
      <td>5019.000000</td>
      <td>2.000000e+07</td>
    </tr>
  </tbody>
</table>
</div>




```python

# Load the CSV file as well
df_studies_CSV = pd.read_csv( os.path.join(current_folder, 'covid19-clinical-trials-dataset',
                                           'COVID clinical trials.csv'))
df_studies_CSV.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rank</th>
      <th>NCT Number</th>
      <th>Title</th>
      <th>Acronym</th>
      <th>Status</th>
      <th>Study Results</th>
      <th>Conditions</th>
      <th>Interventions</th>
      <th>Outcome Measures</th>
      <th>Sponsor/Collaborators</th>
      <th>...</th>
      <th>Other IDs</th>
      <th>Start Date</th>
      <th>Primary Completion Date</th>
      <th>Completion Date</th>
      <th>First Posted</th>
      <th>Results First Posted</th>
      <th>Last Update Posted</th>
      <th>Locations</th>
      <th>Study Documents</th>
      <th>URL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>NCT04595136</td>
      <td>Study to Evaluate the Efficacy of COVID19-0001...</td>
      <td>COVID-19</td>
      <td>Not yet recruiting</td>
      <td>No Results Available</td>
      <td>SARS-CoV-2 Infection</td>
      <td>Drug: Drug COVID19-0001-USR|Drug: normal saline</td>
      <td>Change on viral load results from baseline aft...</td>
      <td>United Medical Specialties</td>
      <td>...</td>
      <td>COVID19-0001-USR</td>
      <td>November 2, 2020</td>
      <td>December 15, 2020</td>
      <td>January 29, 2021</td>
      <td>October 20, 2020</td>
      <td>NaN</td>
      <td>October 20, 2020</td>
      <td>Cimedical, Barranquilla, Atlantico, Colombia</td>
      <td>NaN</td>
      <td>https://ClinicalTrials.gov/show/NCT04595136</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>NCT04395482</td>
      <td>Lung CT Scan Analysis of SARS-CoV2 Induced Lun...</td>
      <td>TAC-COVID19</td>
      <td>Recruiting</td>
      <td>No Results Available</td>
      <td>covid19</td>
      <td>Other: Lung CT scan analysis in COVID-19 patients</td>
      <td>A qualitative analysis of parenchymal lung dam...</td>
      <td>University of Milano Bicocca</td>
      <td>...</td>
      <td>TAC-COVID19</td>
      <td>May 7, 2020</td>
      <td>June 15, 2021</td>
      <td>June 15, 2021</td>
      <td>May 20, 2020</td>
      <td>NaN</td>
      <td>November 9, 2020</td>
      <td>Ospedale Papa Giovanni XXIII, Bergamo, Italy|P...</td>
      <td>NaN</td>
      <td>https://ClinicalTrials.gov/show/NCT04395482</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>NCT04416061</td>
      <td>The Role of a Private Hospital in Hong Kong Am...</td>
      <td>COVID-19</td>
      <td>Active, not recruiting</td>
      <td>No Results Available</td>
      <td>COVID</td>
      <td>Diagnostic Test: COVID 19 Diagnostic Test</td>
      <td>Proportion of asymptomatic subjects|Proportion...</td>
      <td>Hong Kong Sanatorium &amp; Hospital</td>
      <td>...</td>
      <td>RC-2020-08</td>
      <td>May 25, 2020</td>
      <td>July 31, 2020</td>
      <td>August 31, 2020</td>
      <td>June 4, 2020</td>
      <td>NaN</td>
      <td>June 4, 2020</td>
      <td>Hong Kong Sanatorium &amp; Hospital, Hong Kong, Ho...</td>
      <td>NaN</td>
      <td>https://ClinicalTrials.gov/show/NCT04416061</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>NCT04395924</td>
      <td>Maternal-foetal Transmission of SARS-Cov-2</td>
      <td>TMF-COVID-19</td>
      <td>Recruiting</td>
      <td>No Results Available</td>
      <td>Maternal Fetal Infection Transmission|COVID-19...</td>
      <td>Diagnostic Test: Diagnosis of SARS-Cov2 by RT-...</td>
      <td>COVID-19 by positive PCR in cord blood and / o...</td>
      <td>Centre Hospitalier Régional d'Orléans|Centre d...</td>
      <td>...</td>
      <td>CHRO-2020-10</td>
      <td>May 5, 2020</td>
      <td>May 2021</td>
      <td>May 2021</td>
      <td>May 20, 2020</td>
      <td>NaN</td>
      <td>June 4, 2020</td>
      <td>CHR Orléans, Orléans, France</td>
      <td>NaN</td>
      <td>https://ClinicalTrials.gov/show/NCT04395924</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>NCT04516954</td>
      <td>Convalescent Plasma for COVID-19 Patients</td>
      <td>CPCP</td>
      <td>Enrolling by invitation</td>
      <td>No Results Available</td>
      <td>COVID 19</td>
      <td>Biological: Convalescent COVID 19 Plasma</td>
      <td>Evaluate the safety|Change in requirement for ...</td>
      <td>Vinmec Research Institute of Stem Cell and Gen...</td>
      <td>...</td>
      <td>ISC.20.11.1</td>
      <td>August 1, 2020</td>
      <td>November 30, 2020</td>
      <td>December 30, 2020</td>
      <td>August 18, 2020</td>
      <td>NaN</td>
      <td>August 20, 2020</td>
      <td>Vinmec Research Institute of Stem cell and Gen...</td>
      <td>NaN</td>
      <td>https://ClinicalTrials.gov/show/NCT04516954</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 27 columns</p>
</div>




```python

# What's in the CSV that's not listed in the XML data? Or at least lost in translation to dfs?
print(list(df_studies.columns))
print(list(df_studies_CSV.columns))

```

    ['Unnamed: 0', 'condition', 'date_processed', 'enrollment', 'id', 'intervention', 'location_countries', 'overall_status', 'sponsors', 'start_date', 'study_type', 'title']
    ['Rank', 'NCT Number', 'Title', 'Acronym', 'Status', 'Study Results', 'Conditions', 'Interventions', 'Outcome Measures', 'Sponsor/Collaborators', 'Gender', 'Age', 'Phases', 'Enrollment', 'Funded Bys', 'Study Type', 'Study Designs', 'Other IDs', 'Start Date', 'Primary Completion Date', 'Completion Date', 'First Posted', 'Results First Posted', 'Last Update Posted', 'Locations', 'Study Documents', 'URL']
    


Because we extracted only a few important categories of information from the .xml files, the `df_studies` dataframe is now
more condensed than the .csv file's summary in `df_studies_CSV`.



<br>

# Brainstorming questions I'd like to ask

To start the EDA, let's look at a few of the columns in `df_studies` (i.e. the one that's been summarized better) in
more detail!




```python

list_dfColumns = list(df_studies.columns)
print(list_dfColumns)


```

    ['Unnamed: 0', 'condition', 'date_processed', 'enrollment', 'id', 'intervention', 'location_countries', 'overall_status', 'sponsors', 'start_date', 'study_type', 'title']
    


Based on the preliminary look at the data, there seems to be some data in the dataset that are unrelated to COVID.
With that in mind, here are some questions I would like to ask throughout this analysis:

1. Regarding the entire dataset:
    - What are the conditions (COVID- and non-COVID-related) that are being studied?
    - What status is each study in? Which of those are COVID-related?
        - From the CSV file column description in original dataset post, there is a very small number of studies with results
    available (1%). What percentage is this of the actual studies that have already been completed?
    - In which countries do studies take place? Which of those are COVID-related?
    - What are the study types used? Do COVID-related studies differ from the rest?
        - What's the distribution of enrollment sizes for study types?
  

2. In COVID-related studies:
    - Specific to **observational studies**:
        - length of each study (from CSV)
        - how is the outcome measured?
    - Specific to **interventional studies**:
        - length of each study (also from CSV)
        - what were some types of interventions used?
        - in drug studies, what were the top 5 most popular ones studied?
    - In the small percentage of studies that actually have results posted, were any of them focused on COVID?
        - If so, can we draw any meaningful conclusions?




<br>

# Exploratory analysis

## 1. Comparison of COVID- and non-COVID-related studies

### Conditions being studied
First, I ask what conditions (COVID- and non-COVID-related) are being studied.




This pie chart shows that almost 78% of the current clinical studies available in the dataset are concerned with COVID-19.
However, the **conditions** being studied may not completely represent all studies that are related to COVID. For this,
let's examine the **study titles** to see if the `Unrelated` and `Unrelated, Respiratory` studies contain any COVID-related keywords in
their titles.




As shown here, studies that were not directly concerned with conditions associated with COVID-19 as classified
through `Conditions` category were still relevant to the disease. For example, some focus on psychological and resultant
secondary impacts of COVID-19 (e.g. *"Impact of a Novel Coronavirus (2019-nCoV) Outbreak on Public Anxiety
and Cardiovascular Disease Risk in China"*), while others investigate the side effects of COVID-19 in already vulnerable
cancer patients (e.g. *"TRACERx Renal (TRAcking Renal Cell Carcinoma Evolution Through Therapy (Rx)) CAPTURE: COVID-19 
Antiviral Response in a Pan-tumour Immune Study"*).

What do the remaining titles look like?




Now, we can visualize the changes in an updated pie chart.




As shown, COVID-related studies, whether directly or indirectly related, make up almost 90% of the trials listed
on the [ClinicalTrials.gov](ClinicalTrials.gov) website. Of the remainder of them, about 3% are unrelated but study
respiratory diseases such as the flu or common cold, and about 8% appear to be totally unrelated to COVID-19.





<br>

### Study status
What status is each study currently in? We can start by plotting the `overall_status` column as a simple horizontal bar chart.





As shown, almost half of the studies are currently recruiting for participants! It would be interesting to see what proportion of
these studies are made up by COVID-related ones.

Moreover, while the current chart is displayed in the descending order of counts, we can also further categorize statuses.
Note that the `overall_status` column contains information both about **recruitment status** and **expanded access status**.
Here, "expanded access" refers to a way for patients with serious diseases or conditions who cannot participate in a clinical trial
to gain access to a medical product that has not been approved by the U.S. Food and Drug Administration (FDA).

First, the following are definitions of each category of study status:
- **Not yet recruiting:** The study has not started recruiting participants.
- **Recruiting**: The study is currently recruiting participants.
- **Enrolling by invitation**: The study is selecting its participants from a population, or group of people, decided on by the researchers in advance. These studies are not open to everyone who meets the eligibility criteria but only to people in that particular population, who are specifically invited to participate.
- **Active, not recruiting**: The study is ongoing, and participants are receiving an intervention or being examined, but potential participants are not currently being recruited or enrolled.
- **Suspended**: The study has stopped early but may start again.
- **Terminated**: The study has stopped early and will not start again. Participants are no longer being examined or treated.
- **Completed**: The study has ended normally, and participants are no longer being examined or treated (that is, the last participant's last visit has occurred).
- **Withdrawn**: The study stopped early, before enrolling its first participant.
- **Unknown**: A study on ClinicalTrials.gov whose last known status was recruiting; not yet recruiting; or active, not recruiting but that has passed its completion date, and the status has not been last verified within the past 2 years.

Overall, we could divide the above into four categories depending on the stage of a study:
`Not started`, `In preparation / Active`, `Inactive`, or `Finished`.

Secondly, the following are the definitions of each category of expanded access status; these could altogether be put under the `Expanded access` category:
- **Available**: Expanded access is currently available for this investigational treatment, and patients who are not participants in the clinical study may be able to gain access to the drug, biologic, or medical device being studied.
- **No longer available**: Expanded access was available for this intervention previously but is not currently available and will not be available in the future.
- **Temporarily not available**: Expanded access is not currently available for this intervention but is expected to be available in the future.
- **Approved for marketing**: The intervention has been approved by the U.S. Food and Drug Administration for use by the public.





```python

# TODO: Sort each, and colour each bar according to the groups
df_studies['status_group'] = ['']*len(df_studies.overall_status)
overall_status_list = list(df_studies['overall_status'].value_counts().index)
print(overall_status_list)


```

    ['Recruiting', 'Not yet recruiting', 'Completed', 'Active, not recruiting', 'Enrolling by invitation', 'Withdrawn', 'Terminated', 'Suspended', 'Available', 'No longer available', 'Approved for marketing']
    


```python

# Group the statuses
group_labels = ['Not started', 'In prep / Active', 'Inactive', 'Finished', 'Expanded access']

# TODO: Could refactor this portion; this won't work if the number of studies in each status changes and the
# value count order changes
for i, s in enumerate(overall_status_list):
    if i in [1]:
        idx = 0
    elif i in [0,3,4]:
        idx = 1
    elif i in [5,6,7]:
        idx = 2
    elif i in [2]:
        idx = 3
    elif i in [8,9,10]:
        idx = 4
    elif s == 'Unknown':
        idx = 5
    df_studies.loc[df_studies['overall_status'] == s,'status_group'] = group_labels[idx]

# Create a code for each study topic
df_studies['study_topic_code'] = [''] * len(df_studies['study_topic'])

# Within each group, label the COVID-related ones
df_studies.loc[df_studies.study_topic == 'COVID-related', 'study_topic_code'] = 1
df_studies.loc[df_studies.study_topic == 'COVID_secondary', 'study_topic_code'] = 1
df_studies.loc[df_studies.study_topic == 'Unrelated', 'study_topic_code'] = 0
df_studies.loc[df_studies.study_topic == 'Unrelated, Respiratory', 'study_topic_code'] = 0

# Sort within each group
grouped_status = df_studies.groupby(['status_group','overall_status','study_topic_code'])['study_topic_code'].agg(len)\
    .unstack()

grouped_status['sum'] = grouped_status.sum(axis=1)

dfs = []
indexList = np.asarray(grouped_status.index.get_level_values(0))

for gr_label in group_labels:
    df = grouped_status.iloc[np.where(indexList==gr_label)[0]].sort_values(by='sum',ascending=False)
    # df['status_group'] = [gr_label]*len(df.index)
    dfs.append(df)

grouped_status = pd.concat(dfs)
del grouped_status['sum']

```


```python
def mybarh_stacked_df(df, ylabel, title, colToHighlight = 1, colourmap='plasma',
                      xlabel='Count', legendlabel='', cumsum=False):
    '''
    Plots a stacked horizontal bar graph using an unstacked dataframe.
    If dataframe has multiIndex, for the moment this function only supports two levels of multiIndices.
    DataFrame must contain two columns of data, and the second column's data gets highlighted by default unless specified as parameter.
    :param df: dataframe name
    :param ylabel: vertical axis (categorical) label
    :param title: title of plot
    :param colToHighlight: column to highlight; default second column
    :param colourmap: colourmap of choice; default 'plasma'
    :param xlabel: horizontal axis label; default 'count'
    :return: the stacked horizontal bar plot.
    '''

    plt.rcdefaults()
    ax = df.plot.barh( stacked=True, title=title, legend=False)

    categories = df.index
    if isinstance(categories, pd.MultiIndex):
        cat_labels = categories.unique(level=1).to_list()
        cat_groups = categories.unique(level=0).to_list()
    elif isinstance(categories, pd.Index):
        cat_labels = categories.to_list()
        cat_groups = categories.to_list()
    counts = df.values

    # Set axes and labels
    ax.set_xlabel(xlabel)
    ax.set_ylabel(ylabel)
    ax.set_yticklabels(cat_labels)
    ax.invert_yaxis()  # labels read top-to-bottom
    ax.set_title(title)

    _, xmax = plt.xlim()
    xmax *= 1.24
    plt.xlim(0, xmax)

    # Colours and formatting
    patches = np.array(ax.patches)
    patches = patches.reshape((2, int(len(patches)/2)))

    colours = data_color(len(cat_groups), colourmap)

    # label the column being highlighted
    for i, c in enumerate(df.sum(axis=1).to_list()):
        # need value itself and the percentage
        value = counts[i,colToHighlight]
        pct = value / np.nansum(counts[i,:]) * 100
        if isinstance(categories, pd.MultiIndex):
            color = colours[cat_groups.index(categories.get_level_values(0)[i])]
        else:
            color = 'black'
        ax.text(c + xmax*0.015, i,
            '{:.0f}, {:.1f}%'.format(value, pct),
            color=color, ha='left', va='center')

    newColourInd = []
    colourInd = -100

    for (idx1,idx2), patch in np.ndenumerate(patches):
        if isinstance(categories, pd.MultiIndex): # update colour as you iterate through groups
            multiIndex_firstLevelVal = categories.get_level_values(0)[idx2]
            colourInd_updated = cat_groups.index(multiIndex_firstLevelVal)
            if colourInd_updated != colourInd:
                newColourInd.append(idx2)
            colourInd = colourInd_updated
        else:
            colourInd = -1

        if idx1 == colToHighlight:
            alpha = 1
        else:
            alpha = 0.5

        patch.set_y( patch.get_y()-0.1 )
        patch.set_height(0.7)
        patch.set_facecolor(colours[colourInd] * alpha)

    if isinstance(categories, pd.MultiIndex):
        ax.legend(patches[colToHighlight,newColourInd],cat_groups,
                  title=categories.names[0].replace('_',' ').capitalize())
    else:
        ax.legend(legendlabel)
    return plt.show()

# Plot stacked horizontal bar chart
mybarh_stacked_df(grouped_status, ylabel='Study status',
                  title='Status of clinical studies, with a focus on COVID-related studies\n'
                        'and their percentage-makeup in each status category')

```


    
![png](COVID-19%20Clinical%20Trials_files/COVID-19%20Clinical%20Trials_19_0.png)
    



### Countries of study
In which countries do studies take place? Let's find out.



```python
countries = list(df_studies['location_countries'].value_counts().index)
country_counts = list(df_studies['location_countries'].value_counts().values)

mybarh_df(countries[:15], country_counts[:15],
           title='Top 15 countries with the most clinical trials logged',
           diffColourIdx=(0,1,2))

```


    
![png]({{ site.baseurl }}/assets/img/kaggle-covid-clintri/output_30_0.png)
    


Of the top 15 countries conducting the most clinical trials in the world, United States, France, and United Kingdom each
placed in top three. The percentage values highlighted for these top three countries are percentage values of the total
number of clinical trials conducted by the top 15 countries shown. As can be seen, these three countries' trials make
up about half of the share of clinical trials being conducted by the top 15 countries.

Which of those are COVID-related ones?



```python

# Horizontal bar graph

# list(df_studies.columns)
# df_studies['location_countries'].value_counts()

# Unstack the countries dataframe
countries = df_studies[['location_countries','study_topic_code']]

# sort rows in a descending order of total number of studies in each country
countries_ind = countries.groupby(['location_countries','study_topic_code']).location_countries.size().unstack()\
    .sum(axis=1).sort_values(ascending=False).index

# Sort using above indices
countries = countries.groupby(['location_countries','study_topic_code']).location_countries.size().unstack().reindex(index=countries_ind)

# countries[:20]
mybarh_stacked_df(countries[:15],
                  ylabel='Country',
                  title='Number and percentage of COVID-related studies in \n'
                        'top 15 countries conducting the most clinical trials',
                  colourmap='plasma',
                  legendlabel=['Non-COVID-related \nstudies', 'COVID-related studies'])

# TODO: Make a stacked bar graph with normalized lengths to highligh the percentage values
# TODO: LEVEL-UP -- draw a world map and graph dots on top of them

```


    
![png](COVID-19%20Clinical%20Trials_files/COVID-19%20Clinical%20Trials_23_0.png)
    



As shown here, in all top 15 countries conducting the most clinical trials in the world according to
[ClinicalTrials.gov](https://clinicaltrials.gov), more than 83% of the studies are COVID-related; this demonstrates the
world-wide effort that is being put into researching and learning about the virus and its impact in a variety of ways.



### Study type
What are the study types used? Do COVID-related studies differ from the rest?

First, the following are the study types included in the dataset.

#### Interventional studies (also called clinical trials):
A type of clinical study in which participants are assigned to groups that receive one or more intervention/treatment
(or no intervention) so that researchers can evaluate the effects of the interventions on biomedical or health-related
outcomes. The assignments are determined by the study's protocol. Participants may receive diagnostic, therapeutic, or
other types of interventions.

#### Observational studies (includes patient registries):
A type of clinical study in which participants are identified as belonging to study groups and are assessed for
biomedical or health outcomes. Participants may receive diagnostic, therapeutic, or other types of interventions, but
the investigator does not assign participants to a specific interventions/treatment. A patient registry is a type of
observational study.

#### Expanded access:
As described previously.



```python

def mygroupedbar( data, data_labels, group_labels, group_name, data_name='Count', title='', colormap='plasma'):
    '''
    plot grouped data into a bar chart.
    :param data: 2-d array containing columns of different categories. Number of rows should match with length
        of group_labels.
    :param data_labels: labels for data. Length should match with number of columns in data.
    :param data_name: y axis name; default 'Count'
    :param group_labels: labels for groups.
    :param group_name: x axis name.
    :param title: title of plot.
    :return: grouped bar chart.
    '''
    x = np.arange(len(group_labels))  # the label locations
    width = 0.35  # the width of the bars

    fig, ax = plt.subplots()
    rects_grouped = []
    colours_list = data_color(len(group_labels)+1,colormap)

    for col in range(len(data_labels)):
        x_bar_center = x + width/2 * (col*2 - (len(data_labels)-1))
        rects = ax.bar(x_bar_center, data[:,col], width, label=data_labels[col])
        for bar in rects:
            bar.set_color(colours_list[col])
        rects_grouped.append(rects)

    # Add some text for labels, title and custom x-axis tick labels, etc.
    ax.set_xlabel(group_name)
    ax.set_xticks(x)
    ax.set_xticklabels(group_labels)
    ax.set_ylabel(data_name)
    ax.set_ylim([0, ax.get_ylim()[1]*1.1])
    ax.set_title(title)
    ax.legend()

    def autolabel(rects):
        """Attach a text label above each bar in *rects*, displaying its height."""
        for rect in rects:
            height = rect.get_height()
            ax.annotate('{:.0f}'.format(height),
                        xy=(rect.get_x() + rect.get_width() / 2, height),
                        xytext=(0, 4),  # 3 points vertical offset
                        textcoords="offset points",
                        ha='center', va='bottom')

    for rects in rects_grouped:
        autolabel(rects)

    fig.tight_layout()

    plt.show()

study_types = df_studies[['study_type','study_topic_code']].groupby(['study_type','study_topic_code']).study_type.agg(len).unstack()

study_types_ind = study_types.index
study_types_new = []

for study_type in study_types_ind:
    study_types_new.append(study_type.replace(' ', '\n'))

data = study_types.fillna(0).values

mygroupedbar( data=data, data_labels=['Not COVID-related','COVID-related'],
              data_name='Count', group_labels=study_types_new, group_name='Study type',
              title='Study methods used for \nCOVID- and non-COVID-related studies' )

```


    
![png](COVID-19%20Clinical%20Trials_files/COVID-19%20Clinical%20Trials_26_0.png)
    



Both COVID- and non-COVID-related studies follow a similar trend; in both cases, interventional studies are the most
popular method, observational studies not including patient registry come second, and patient registry come third. Finally,
expanded access studies are the rarest of study types, only seen in a minuscule portion of COVID-related studies and not
at all in non-COVID-related ones. We might reason that expanded access is perhaps unlikely to be approved, unless urgently
necessary in situations such as in a novel viral pandemic, where much about the virus is unknown but lives are in danger.


### Enrollment sizes (per study type and per study topic)
What's the distribution of enrollment sizes across different study methods, and across different study types?
Here, I ignore all enrollment sizes that are not available (such as in the Expanded access cases) or zero.



```python
# df_studies.columns
grouper = ['study_type','study_topic_code','enrollment']
enrollments = df_studies[grouper].dropna()
enrollments = enrollments[enrollments['enrollment'] != 0]
study_types_ind_new = study_types_ind[1:].copy()

# plot distributions of enrollment sizes
ax = plt.subplot()
sns.stripplot(
    x='study_type', y='enrollment',
    hue='study_topic_code',
    data=enrollments,
    order=study_types_ind_new,
    dodge=True,
    jitter=0.2,
    size=4, alpha=0.5,
    palette='plasma',
    ax=ax
)
ax.set_yscale('log')
ax.set_ylabel('Enrollment size')
ax.set_xlabel('Study type')
xticklabels = ax.get_xticklabels()
xticklabels[2] = 'Observational\n[Patient Registry]'
ax.set_xticklabels(xticklabels)

ax.legend(['Not COVID-related','COVID-related'])
ax.set_title('Distribution of enrollment sizes\nacross study types and relevance to COVID')

plt.show()

# TODO: Plot a line showing median; use the box plot function?

```


    
![png](COVID-19%20Clinical%20Trials_files/COVID-19%20Clinical%20Trials_29_0.png)
    



```python

# Compute statistics
enrollment_dist_stats = []

for study_type in study_types_ind_new:
    data = enrollments.loc[enrollments['study_type']==study_type, ['study_topic_code','enrollment']]
    x = data.loc[data['study_topic_code']==0,'enrollment']
    y = data.loc[data['study_topic_code']==1,'enrollment']

    stat, pval = stats.epps_singleton_2samp(x,y)
    enrollment_dist_stats.append((stat,pval))

for i, statistics in enumerate(enrollment_dist_stats):
    if statistics[1] < 0.05:
        print("For {} studies, COVID-related and non-COVID-related studies' enrollment sizes"
              " came from the same distribution. (p-value = {:.3f})"
              .format(study_types_ind_new[i].lower(),statistics[1]))
    else:
        print("For {} studies, COVID-related and non-COVID-related studies' enrollment sizes"
              " did not come from the same distribution. (p-value = {:.3f})"
              .format(study_types_ind_new[i].lower(),statistics[1]))


```

    For interventional studies, COVID-related and non-COVID-related studies' enrollment sizes did not come from the same distribution. (p-value = 0.450)
    For observational studies, COVID-related and non-COVID-related studies' enrollment sizes came from the same distribution. (p-value = 0.019)
    For observational [patient registry] studies, COVID-related and non-COVID-related studies' enrollment sizes came from the same distribution. (p-value = 0.000)
    

### Common measure of outcome being used for each study type?
What methods do each study types use to measure outcome?

### Study results?
From CSV file, what results can we find?


<br>

## 2. A deeper dive into COVID-related studies only

### Observational studies
- length of each study (from CSV)
- how is the outcome measured?



```python
# from CSV, find out about length of each study and categorize into observational vs. interventional
df_studies_COVID = df_studies.loc[df_studies['study_topic_code']==1,
                                  ['condition','enrollment','id','intervention','title', 'study_topic_code']]
# df_studies_CSV.columns
# df_studies.columns

right = df_studies_CSV[['NCT Number','Outcome Measures','Phases',
                        'Start Date','Primary Completion Date','Completion Date']]

df_COVID_joined = pd.merge(df_studies_COVID, right,
                           how="inner", left_on='id',right_on='NCT Number',
                           sort=False,suffixes=("","_CSV"),
                           validate="1:1")

# Define timestamp columns and drop NA values
study_dates_names = ['Start Date','Primary Completion Date','Completion Date']
df_COVID_joined.dropna(axis=0,subset=study_dates_names,inplace=True)

# Take differences between dates to find study durations
df_COVID_joined[study_dates_names] = df_COVID_joined[study_dates_names].astype('datetime64')

df_COVID_joined['study_duration_primary'] = df_COVID_joined['Primary Completion Date'] - df_COVID_joined['Start Date']
df_COVID_joined['study_duration_total'] = df_COVID_joined['Completion Date'] - df_COVID_joined['Start Date']

# df_COVID_joined.head()
```


```python

# np.where(pd.DatetimeIndex(study_dates_timestamp['Start Date']).date == pd.to_datetime((2019,2020)[0]))
# pd.to_datetime((2019,2020))
# hi = (0,1,2,3)
# hi[not 0]
datetime.date(2020,1,1).year
```




    2020




```python
# Sort values in terms of Study start dates, oldes to newest
study_dates_timestamp = df_COVID_joined[study_dates_names].sort_values(by='Start Date')
study_dates_timestamp#.reset_index(drop=True,inplace=True)

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Start Date</th>
      <th>Primary Completion Date</th>
      <th>Completion Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>190</th>
      <td>1998-01-01</td>
      <td>2016-05-31</td>
      <td>2021-03-31</td>
    </tr>
    <tr>
      <th>0</th>
      <td>2007-11-01</td>
      <td>2025-02-01</td>
      <td>2025-02-01</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2012-02-05</td>
      <td>2023-09-01</td>
      <td>2023-09-01</td>
    </tr>
    <tr>
      <th>4062</th>
      <td>2013-01-01</td>
      <td>2020-11-11</td>
      <td>2025-01-01</td>
    </tr>
    <tr>
      <th>2199</th>
      <td>2013-04-05</td>
      <td>2023-12-01</td>
      <td>2023-12-01</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2338</th>
      <td>2021-07-15</td>
      <td>2021-12-15</td>
      <td>2021-12-15</td>
    </tr>
    <tr>
      <th>2703</th>
      <td>2021-09-01</td>
      <td>2024-06-01</td>
      <td>2024-06-01</td>
    </tr>
    <tr>
      <th>382</th>
      <td>2021-09-30</td>
      <td>2021-12-15</td>
      <td>2021-12-31</td>
    </tr>
    <tr>
      <th>2894</th>
      <td>2021-11-01</td>
      <td>2022-11-01</td>
      <td>2022-11-01</td>
    </tr>
    <tr>
      <th>2997</th>
      <td>2021-12-01</td>
      <td>2022-12-01</td>
      <td>2022-12-01</td>
    </tr>
  </tbody>
</table>
<p>4447 rows × 3 columns</p>
</div>




```python
study_dates_timestamp['Start Date'].astype('datetime64[D]')
```




    190    1998-01-01
    0      2007-11-01
    7      2012-02-05
    4062   2013-01-01
    2199   2013-04-05
              ...    
    2338   2021-07-15
    2703   2021-09-01
    382    2021-09-30
    2894   2021-11-01
    2997   2021-12-01
    Name: Start Date, Length: 4447, dtype: datetime64[ns]




```python

def mytimeline(dataframe_orig, startyear_range, endyear = None,
               indexercol=0, increment=1, title='', ylabel='', colormap='plasma'):
    '''
    Plots a timeline for a given dataframe containing timestamp data in its columns.
    :param dataframe: pandas DataFrame containing timestamp in its columns.
    :param startyear_range: low and high limits of the range of timeline start years to be plotted; array-like of ints of size 2.
    :param endyear: higher limit of the time axis. If not provided, will be defined by data; int, default = NaN
    :param indexercol: index of column to sort dataframe rows by, and to select a portion of data to be plotted from; int
    :param increment: increment by which to increase the years plotted as xticks; default 1.
    :param title: title of resulting timeline; default ''
    :param colormap: colormap to choose colours from; each column has a different colour.
    :return: outputs the timeline plot.
    '''

    # sort dataframe
    colnames = dataframe_orig.columns.to_list()
    indexercol_name = colnames[indexercol]
    dataframe = dataframe_orig[colnames].sort_values(by=indexercol_name)
    dataframe.reset_index(drop=True, inplace=True)
    colnames.pop(indexercol)

    # Define the time range of study start dates to plot
    # To do this, I must first define first and last possible start dates
    startdate_first = datetime.date(startyear_range[0], 1, 1)
    startdate_last = datetime.date(startyear_range[1], 1, 1)

    # Then, compare that date with the indexer column
    indStart = np.where(dataframe[indexercol_name] >= datetime.datetime.combine(startdate_first, datetime.datetime.min.time()))[0][0]
    indEnd = np.where(dataframe[indexercol_name] <= datetime.datetime.combine(startdate_first, datetime.datetime.min.time()))[0][-1]

    # Define dataframe range to be plotted
    df_plotted = dataframe.iloc[indStart:indEnd+1]

    if endyear is None :
        enddate = max(max(dataframe.iloc[indStart:indEnd+1].loc[colnames]))
        endyear = enddate.year
    # else:
    #     enddate = datetime.date(endyear,12,31)

    # Plot the studies on a timeline
    ax = plt.subplot()

    # Set plot colours
    colours_list = data_color(4,colormap)
    colours_list[:,-1] = 0.7

    # draw vertical lines every incremental year
    year_incr = [datetime.date( startyear_range[0] + year*increment, 1, 1)
                 for year in range(int( (endyear+1-startyear_range[0]) /increment ))]

    # Label axes and title
    ax.set_xlim(year_incr[0],year_incr[-1])
    ax.set_xticks(year_incr)
    ax.set_xticklabels([incr.year for incr in year_incr])
    ax.set_xlabel('Year')

    # ax.set_ylim([0, indEnd-indStart])
    ax.set_ylabel(ylabel)
    ax.set_title(title)
    ax.grid(b=True, axis='both')

    for col in reversed(range(len(colnames))):
        ax.hlines(y=np.arange(len(df_plotted))[::-1],
                  xmin=df_plotted[indexercol_name],
                  xmax=df_plotted[colnames[col]],
                  colors=colours_list[col],linewidths=2)

    ax.legend(colnames)


mytimeline(study_dates_timestamp,
           startyear_range=[2019, 2021], endyear=None,
           title='Timeline of COVID-related studies',
           ylabel='Study Number')


```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-32-fdea85f39df8> in <module>
         69 
         70 
    ---> 71 mytimeline(study_dates_timestamp,
         72            startyear_range=[2019, 2021], endyear=None,
         73            title='Timeline of COVID-related studies',
    

    <ipython-input-32-fdea85f39df8> in mytimeline(dataframe_orig, startyear_range, endyear, indexercol, increment, title, ylabel, colormap)
         33 
         34     if endyear is None :
    ---> 35         enddate = max(max(dataframe.iloc[indStart:indEnd+1].loc[colnames]))
         36         endyear = enddate.year
         37     # else:
    

    ~\Desktop\GitHub\kaggle-covid-clintri\.venv\lib\site-packages\pandas\core\indexing.py in __getitem__(self, key)
        893 
        894             maybe_callable = com.apply_if_callable(key, self.obj)
    --> 895             return self._getitem_axis(maybe_callable, axis=axis)
        896 
        897     def _is_scalar_access(self, key: Tuple):
    

    ~\Desktop\GitHub\kaggle-covid-clintri\.venv\lib\site-packages\pandas\core\indexing.py in _getitem_axis(self, key, axis)
       1111                     raise ValueError("Cannot index with multidimensional key")
       1112 
    -> 1113                 return self._getitem_iterable(key, axis=axis)
       1114 
       1115             # nested tuple slicing
    

    ~\Desktop\GitHub\kaggle-covid-clintri\.venv\lib\site-packages\pandas\core\indexing.py in _getitem_iterable(self, key, axis)
       1051 
       1052         # A collection of keys
    -> 1053         keyarr, indexer = self._get_listlike_indexer(key, axis, raise_missing=False)
       1054         return self.obj._reindex_with_indexers(
       1055             {axis: [keyarr, indexer]}, copy=True, allow_dups=True
    

    ~\Desktop\GitHub\kaggle-covid-clintri\.venv\lib\site-packages\pandas\core\indexing.py in _get_listlike_indexer(self, key, axis, raise_missing)
       1264             keyarr, indexer, new_indexer = ax._reindex_non_unique(keyarr)
       1265 
    -> 1266         self._validate_read_indexer(keyarr, indexer, axis, raise_missing=raise_missing)
       1267         return keyarr, indexer
       1268 
    

    ~\Desktop\GitHub\kaggle-covid-clintri\.venv\lib\site-packages\pandas\core\indexing.py in _validate_read_indexer(self, key, indexer, axis, raise_missing)
       1306             if missing == len(indexer):
       1307                 axis_name = self.obj._get_axis_name(axis)
    -> 1308                 raise KeyError(f"None of [{key}] are in the [{axis_name}]")
       1309 
       1310             ax = self.obj._get_axis(axis)
    

    KeyError: "None of [Index(['Primary Completion Date', 'Completion Date'], dtype='object')] are in the [index]"


### Interventional studies
- length of each study (also from CSV)
- what were some types of interventions used?
- in drug studies, what were the top 5 most popular ones studied?

### Any meaningful conclusions?
- In the small percentage of studies that actually have results posted, were any of them focused on COVID?
- If so, can we draw any meaningful conclusions?


