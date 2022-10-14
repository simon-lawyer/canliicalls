Call CanLii
================

<!-- WARNING: THIS FILE WAS AUTOGENERATED! DO NOT EDIT! -->

[CanLii](www.canlii.org) is an indispensable research tool for lawyers,
law students, judges, adjudicators, and researchers. Largely funded by
Canadian lawyers, it is the international gold standard when it comes to
the free and open access to law.

With an intuitive and beautiful interface, researchers use CanLii to
look up an individual case or statute, read it, see what cites it, and
see what it cites.

<div class="column-margin">

<figure>
<img src="images/computer.jpg"
data-fig-alt="&quot;Computer calls a database,&quot; AI artist (2022)"
alt="“Computer calls a database,” AI artist (2022)" />
<figcaption aria-hidden="true">“Computer calls a database,” AI artist
(2022)</figcaption>
</figure>

</div>

Some legal researchers and social scientists require bulk access to
legal data for their projects. Fortunately, CanLii provides an
[API](https://github.com/canlii/API_documentation/blob/master/EN.md)
that allows (limited) programmatic access to its databases. Too few
legal researchers, however, know how to use and interact with APIs.

This library’s purpose is simple: make it easier to use CanLii’s APIs.
More than that, I demonstrate how easy it is to use code to facilitate
aspects of legal research. Use the library, but look at its source code
and see if it inspires you to do more. **You can do this!**

## Install

``` sh
pip install canliicalls
```

or

``` sh
conda install canliicalls
```

then:

``` sh
from canliicalls.caller import *
```

## Usage

### Get and use your secret API key

To use CanLii’s API, you need your own secret access key. Applying for a
key is simple. Just send a request through the [Canlii feedback
form](https://www.canlii.org/en/feedback/feedback.html).

Once you have your API key, the rest is easy. First, enter your secret
API key and, second, enter your preferred language (‘en’ or ‘fr’). Then
call the object.

``` python
api = 'MY SECRET API KEY' #this will look like lots of numbers and letters
language = 'en'

my_caller = Caller(api, language)
```

## A sample research project

To see how to use the library, let’s do a simple research project and
figure out what statutes a recent Supreme Court of Canada case cites to.
To do this, we need to figure out where CanLii stores the case and the
case name.

### Lookup CanLii database names

First, let’s get a list of CanLii database names.

``` python
my_caller.list_tribunals()
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
      <th>databaseId</th>
      <th>jurisdiction</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>qccdoooq</td>
      <td>qc</td>
      <td>Conseil de discipline de l'Ordre des opticiens...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>qcoaciq</td>
      <td>qc</td>
      <td>Comité de discipline de l'organisme d'autorégl...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>skqb</td>
      <td>sk</td>
      <td>Court of King's Bench for Saskatchewan</td>
    </tr>
    <tr>
      <th>3</th>
      <td>onsc</td>
      <td>on</td>
      <td>Superior Court of Justice</td>
    </tr>
    <tr>
      <th>4</th>
      <td>abmgb</td>
      <td>ab</td>
      <td>Alberta Municipal Government Board</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>357</th>
      <td>ytrto</td>
      <td>yk</td>
      <td>Yukon Residential Tenancies Office</td>
    </tr>
    <tr>
      <th>358</th>
      <td>nbsec</td>
      <td>nb</td>
      <td>Financial and Consumer Services Tribunal</td>
    </tr>
    <tr>
      <th>359</th>
      <td>onbcc</td>
      <td>on</td>
      <td>Building Code Commission</td>
    </tr>
    <tr>
      <th>360</th>
      <td>exchc-cech</td>
      <td>ca</td>
      <td>Exchequer Court of Canada</td>
    </tr>
    <tr>
      <th>361</th>
      <td>nttc</td>
      <td>nt</td>
      <td>Territorial Court of the Northwest Territories</td>
    </tr>
  </tbody>
</table>
<p>362 rows × 3 columns</p>
</div>

We can do a few things from here. First, if you want to save this as an
excel or a csv file, that’s easy!

``` python
my_caller.list_tribunals().to_csv('CanLii_tribunal_list.csv')
```

Or you can search directly for the Supreme Court of Canada’s database
ID.

``` python
df = my_caller.list_tribunals()
df[df['name'] == 'Supreme Court of Canada']
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
      <th>databaseId</th>
      <th>jurisdiction</th>
      <th>name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>90</th>
      <td>csc-scc</td>
      <td>ca</td>
      <td>Supreme Court of Canada</td>
    </tr>
  </tbody>
</table>
</div>

### Find recent decisions by the SCC

Great! Now that we have the database ID for SCC cases, we can zero in on
a recent case. To list individual cases from the database, we call a
different function.

This function has a few paramaters. You can decide whether you want the
results in ascending or descending chronological order (defaults to
descending) and how many results you want the API to return (defaults to
100).

``` python
my_caller.list_decisions(databaseId='csc-scc', offset=0, resultCount=10)
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
      <th>caseId</th>
      <th>title</th>
      <th>citation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2022scc34</td>
      <td>R. v. Schneider</td>
      <td>2022 SCC 34 (CanLII)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2022scc33</td>
      <td>R. v. Kirkpatrick</td>
      <td>2022 SCC 33 (CanLII)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022scc32</td>
      <td>R. v. Lafrance</td>
      <td>2022 SCC 32 (CanLII)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2022scc31</td>
      <td>R. v. Sundman</td>
      <td>2022 SCC 31 (CanLII)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2022scc30</td>
      <td>Society of Composers, Authors and Music Publis...</td>
      <td>2022 SCC 30 (CanLII)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2022scc29</td>
      <td>Law Society of Saskatchewan v. Abrametz</td>
      <td>2022 SCC 29 (CanLII)</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2022scc28</td>
      <td>R. v. J.J.</td>
      <td>2022 SCC 28 (CanLII)</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2022scc27</td>
      <td>British Columbia (Attorney General) v. Council...</td>
      <td>2022 SCC 27 (CanLII)</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2022scc26</td>
      <td>Canada (Attorney General) v. Collins Family Trust</td>
      <td>2022 SCC 26 (CanLII)</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2022scc25</td>
      <td>R. v. Goforth</td>
      <td>2022 SCC 25 (CanLII)</td>
    </tr>
  </tbody>
</table>
</div>

### Search for case metadata

We can look up the metadata for any case. Let’s see what the fifth case
down is about.

``` python
my_caller.case_metadata(databaseId='csc-scc', caseId='2022scc30')
```

    databaseId                                                  csc-scc
    caseId                                                    2022scc30
    url                                       https://canlii.ca/t/jqgw0
    title             Society of Composers, Authors and Music Publis...
    citation                                       2022 SCC 30 (CanLII)
    language                                                         en
    docketNumber                                                  39418
    decisionDate                                             2022-07-15
    keywords          technological neutrality — available for on-de...
    topics                                                             
    concatenatedId                                        2022csc-scc30
    dtype: object

### Check to see what legislation the Court cites

Interesting! This case is interesting, I wonder what statutory
provisions in cites?

``` python
my_caller.case_cites_of_legislation(databaseId='csc-scc', caseId='2022scc30')
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
      <th>databaseId</th>
      <th>legislationId</th>
      <th>title</th>
      <th>citation</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>cas</td>
      <td>rsc-1985-c-c-42</td>
      <td>Copyright Act</td>
      <td>RSC 1985, c C-42</td>
      <td>STATUTE</td>
    </tr>
    <tr>
      <th>1</th>
      <td>caa</td>
      <td>sc-2012-c-20</td>
      <td>Copyright Modernization Act</td>
      <td>SC 2012, c 20</td>
      <td>ANNUAL_STATUTE</td>
    </tr>
  </tbody>
</table>
</div>

Only two! Even though the [full
judgement](https://www.canlii.org/en/ca/scc/doc/2022/2022scc30/2022scc30.html?autocompleteStr=2022%20SCC%2030&autocompletePos=1)
is long, this looks right.

## Using loops

This program is designed to support queries at scale. You can, for
example, retrieve all of the keywords for the last ten SCC decisions.

First, request a dataframe of the last ten decisions.

``` python
df = my_caller.list_decisions(databaseId='csc-scc', offset=0, resultCount=10)
df
```

Second, loop over the dataframe and make a separate call for the
keywords of each case.

``` python
for index,row in df.iterrows():
    case = df.loc[index,'caseId']
    print(f'Keywords for {case}.')
    print(my_caller.case_metadata(databaseId='csc-scc', caseId=case)['keywords'])
    print('---')
```

    Keywords for 2022scc34.
    brother — overheard — jury — evidence — probative value
    ---
    Keywords for 2022scc33.
    stare decisis — sexual activity — precedent — without a condom — sex
    ---
    Keywords for 2022scc32.
    police — detainee — detention — interview — encounter
    ---
    Keywords for 2022scc31.
    unlawful confinement — degree murder — domination — truck — temporal-causal connection
    ---
    Keywords for 2022scc30.
    technological neutrality — available for on-demand streaming — work — royalties — works
    ---
    Keywords for 2022scc29.
    abuse — inordinate delay — process — stay — prejudice
    ---
    Keywords for 2022scc28.
    record screening regime — complainants — privacy — evidence — defence
    ---
    Keywords for 2022scc27.
    well-developed factual setting — public interest standing — access to justice — disabilities — legality
    ---
    Keywords for 2022scc26.
    tax — rescission — taxpayer — rectification — mistake
    ---
    Keywords for 2022scc25.
    jury — unlawfully causing bodily harm — necessaries — mens rea requirement — marked departure
    ---
