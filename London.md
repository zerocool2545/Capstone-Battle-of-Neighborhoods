
## Import Libraries


```python
import pandas as pd # library for data analsysis
import numpy as np # library to handle data in a vectorized manner
from bs4 import BeautifulSoup
import requests
import csv
print('Libraries imported.')
print('Beautiful Soup imported')
```

    Libraries imported.
    Beautiful Soup imported


## Built London DF from 8 sources


```python
source = requests.get('https://en.wikipedia.org/wiki/N_postcode_area').text
soup = BeautifulSoup(source, 'lxml')
table = soup.find('table', class_='wikitable sortable').text 
table=table.split('\n')
table
a = list(filter(lambda x: x!= '', table)) #The List with our data#
v1 = np.array(a)
north = pd.DataFrame(v1.reshape(-1, 4))
north.columns = ['PostCode','Town','Neighborhood','Area']
north.columns=list(map(str,north.columns))
north.drop([0],inplace=True)
north.set_index('PostCode',inplace=True)
north.reset_index(inplace=True)
north['Area Category']='North'
```


```python
source2 = requests.get('https://en.wikipedia.org/wiki/SE_postcode_area').text
soup2 = BeautifulSoup(source2, 'lxml')
table2 = soup2.find('table', class_='wikitable sortable').text 
table2=table2.split('\n')
b = list(filter(lambda x: x!= '', table2)) #The List with our data#
indexes = [8, 9, 10]
for index in sorted(indexes, reverse=True):
    del b[index]
len(b)
v2 = np.array(b)
se = pd.DataFrame(v2.reshape(-1, 4))
se.columns = ['PostCode','Town','Neighborhood','Area']
se.columns=list(map(str,se.columns))
se.drop([0],inplace=True)
se.set_index('PostCode',inplace=True)
se.reset_index(inplace=True)
se['Area Category']='South East'
```


```python
source3 = requests.get('https://en.wikipedia.org/wiki/E_postcode_area').text
soup3 = BeautifulSoup(source3, 'lxml')
table3 = soup3.find('table', class_='wikitable sortable').text 
table3=table3.split('\n')
c = list(filter(lambda x: x!= '', table3)) #The List with our data#
v3 = np.array(c)
east = pd.DataFrame(v3.reshape(-1, 4))
east.columns = ['PostCode','Town','Neighborhood','Area']
east.columns=list(map(str,east.columns))
east.drop([0],inplace=True)
east.set_index('PostCode',inplace=True)
east.reset_index(inplace=True)
east['Area Category']='East'
```


```python
source4 = requests.get('https://en.wikipedia.org/wiki/EC_postcode_area').text
soup4 = BeautifulSoup(source4, 'lxml')
table4 = soup4.find('table', class_='wikitable sortable').text 
table4=table4.split('\n')
d = list(filter(lambda x: x!= '', table4)) #The List with our data#
indexes = [16, 17, 18,43,44,45,70,71,72,93,94,95,108,109,110]
for index in sorted(indexes, reverse=True):
    del d[index]
v4 = np.array(d)
eastc = pd.DataFrame(v4.reshape(-1, 4))
eastc.columns = ['PostCode','Town','Neighborhood','Area']
eastc.columns=list(map(str,eastc.columns))
eastc.drop([0],inplace=True)
eastc.set_index('PostCode',inplace=True)
eastc.reset_index(inplace=True)
eastc['Area Category']='East Central'
```


```python
source5 = requests.get('https://en.wikipedia.org/wiki/NW_postcode_area').text
soup5 = BeautifulSoup(source5, 'lxml')
table5 = soup5.find('table', class_='wikitable sortable').text 
table5=table5.split('\n')
e = list(filter(lambda x: x!= '', table5)) #The List with our data#
v5 = np.array(e)
nw = pd.DataFrame(v5.reshape(-1, 4))
nw.columns = ['PostCode','Town','Neighborhood','Area']
nw.columns=list(map(str,nw.columns))
nw.drop([0],inplace=True)
nw.set_index('PostCode',inplace=True)
nw.reset_index(inplace=True)
nw['Area Category']='North West'
```


```python
source6 = requests.get('https://en.wikipedia.org/wiki/SW_postcode_area').text
soup6 = BeautifulSoup(source6, 'lxml')
table6 = soup6.find('table', class_='wikitable sortable').text 
table6=table6.split('\n')
f = list(filter(lambda x: x!= '', table6)) #The List with our data#
v6 = np.array(f)
sw = pd.DataFrame(v6.reshape(-1, 4))
sw.columns = ['PostCode','Town','Neighborhood','Area']
sw.columns=list(map(str,sw.columns))
sw.drop([0],inplace=True)
sw.set_index('PostCode',inplace=True)
sw.reset_index(inplace=True)
sw['Area Category']='South West'
```


```python
source7 = requests.get('https://en.wikipedia.org/wiki/W_postcode_area').text
soup7 = BeautifulSoup(source7, 'lxml')
table7 = soup7.find('table', class_='wikitable sortable').text 
table7=table7.split('\n')
g = list(filter(lambda x: x!= '', table7)) #The List with our data#
v7 = np.array(g)
west = pd.DataFrame(v7.reshape(-1, 4))
west.columns = ['PostCode','Town','Neighborhood','Area']
west.columns=list(map(str,west.columns))
west.drop([0],inplace=True)
west.set_index('PostCode',inplace=True)
west.reset_index(inplace=True)
west['Area Category']='West'
```


```python
source8 = requests.get('https://en.wikipedia.org/wiki/WC_postcode_area').text
soup8 = BeautifulSoup(source8, 'lxml')
table8 = soup8.find('table', class_='wikitable sortable').text 
table8=table8.split('\n')
h = list(filter(lambda x: x!= '', table8)) #The List with our data#
v8 = np.array(h)
westc = pd.DataFrame(v8.reshape(-1, 4))
westc.columns = ['PostCode','Town','Neighborhood','Area']
westc.columns=list(map(str,westc.columns))
westc.drop([0],inplace=True)
westc.set_index('PostCode',inplace=True)
westc.reset_index(inplace=True)
westc['Area Category']='West Central'
```


```python
London = north.append([se,east,eastc,nw,sw,west,westc])
London.set_index('PostCode',inplace=True)
London.reset_index(inplace=True) #The London dataframe is ready
```

## Checks & Data Cleansing


```python
London['Area Category'].value_counts()
```




    South East      28
    South West      27
    West            26
    North           25
    East Central    23
    East            22
    West Central    14
    North West      13
    Name: Area Category, dtype: int64




```python
London['Area'].value_counts()
```




    Westminster                                                           20
    City of London                                                        12
    Camden                                                                 8
    Barnet                                                                 6
    non-geographic                                                         5
    Enfield                                                                4
    Tower Hamlets                                                          4
    Lambeth, Wandsworth                                                    4
    Bexley, Greenwich                                                      3
    Lewisham, Southwark                                                    3
    Islington, Camden                                                      3
    Camden, Westminster                                                    3
    Lewisham                                                               3
    Kensington and Chelsea, Westminster                                    3
    Kensington and Chelsea                                                 3
    Lambeth, Southwark                                                     3
    Lambeth                                                                3
    Greenwich, Lewisham                                                    3
    Hammersmith and Fulham                                                 2
    Merton, Wandsworth                                                     2
    City of London, Westminster                                            2
    Ealing, Hounslow                                                       2
    Tower Hamlets, City of London                                          2
    Richmond upon Thames, Hounslow                                         2
    Newham                                                                 2
    Hammersmith and Fulham, Kensington and Chelsea                         2
    Camden, City of London                                                 2
    Newham, Waltham Forest, Hackney, Tower Hamlets                         2
    Southwark                                                              2
    Camden, Islington                                                      2
                                                                          ..
    Redbridge                                                              1
    Hackney                                                                1
    Ealing, Hounslow, Hammersmith and Fulham                               1
    Camden, Haringey, Islington                                            1
    Westminster, Kensington and Chelsea                                    1
    Croydon, Lambeth, Merton, Wandsworth                                   1
    Hackney, Tower Hamlets                                                 1
    Barnet, Haringey                                                       1
    Islington, City of London                                              1
    Hackney, Waltham Forest                                                1
    Newham, Waltham Forest                                                 1
    Islington, Hackney (part)                                              1
    Waltham Forest                                                         1
    Kensington and Chelsea, Westminster, Hammersmith and Fulham            1
    Haringey, Islington, Hackney                                           1
    Greenwich                                                              1
    Greenwich, Lewisham, Bromley                                           1
    Enfield, Barnet, Haringey                                              1
    Barnet, Brent, Camden                                                  1
    Camden, Barnet                                                         1
    Bexley, Bromley, Greenwich, Lewisham                                   1
    Hammersmith and Fulham, Hounslow                                       1
    Hounslow, Ealing, Hammersmith and Fulham                               1
    City of London, Islington                                              1
    Bromley                                                                1
    Waltham Forest, Hackney                                                1
    Greenwich, Lewisham, Southwark                                         1
    Southwark, Lambeth                                                     1
    Tower Hamlets, Hackney                                                 1
    Kensington and Chelsea, Westminster, Hammersmith and Fulham, Brent     1
    Name: Area, Length: 90, dtype: int64




```python
Distr = pd.DataFrame(London.Neighborhood.str.split(':',1).tolist(),columns = ['District','Neighborhood2'])
London['District']=Distr['District']
London['Neighborhood']=Distr['Neighborhood2']
London=London[London.Area != 'non-geographic']
London=London[London.Area != 'Non-geographic'] #Exclude from DF Non Geographic areas
```


```python
London['Neighborhood'].value_counts()
```




     Homerton, Hackney Wick, South Hackney, Hackney Marshes, Cambridge Heath (part)                                                                                      1
     Kennington, Lambeth (part), Vauxhall (part)                                                                                                                         1
     Herne Hill, Tulse Hill (part), Dulwich (part)                                                                                                                       1
     Wood Green, Bounds Green (part), Bowes Park                                                                                                                         1
     Upper Holloway, Archway, Tufnell Park (part)                                                                                                                        1
     South Kensington, Knightsbridge (part)                                                                                                                              1
     Chelsea, Brompton, Knightsbridge (part)                                                                                                                             1
     Notting Hill, Ladbroke Grove (south), Holland Park (part)                                                                                                           1
     Streatham, Streatham Common, Norbury, Streatham Park, Furzedown, Streatham Vale, Mitcham Common, Pollards Hill, Eastfields                                          1
     Willesden, Harlesden, Kensal Green, Brent Park, College Park, Stonebridge, North Acton (part), West Twyford, Neasden (south), Old Oak Common, Park Royal (north)    1
     Hendon, Brent Cross (part)                                                                                                                                          1
     Bankside, South Bank, Lambeth (part), Southwark, Bermondsey (part), Vauxhall (part), Old Kent Road (part)                                                           1
     Finsbury Park, Manor House, Harringay (part), Stroud Green                                                                                                          1
     New Southgate, Friern Barnet, Bounds Green, Arnos Grove (part)                                                                                                      1
     Raynes Park, Lower Morden, Merton Park, Wimbledon Chase                                                                                                             1
     Hammersmith, Ravenscourt Park, Stamford Brook (part)                                                                                                                1
     Barnes, Barnes Bridge                                                                                                                                               1
     Stratford, West Ham (part), Maryland, Leyton (part), Leytonstone (part) Temple Mills (part), Hackney Wick (part), Bow (part)                                        1
     Forest Hill, Honor Oak, Crofton Park (part)                                                                                                                         1
     Cricklewood, Dollis Hill, Childs Hill, Golders Green (part), Brent Cross (part), Willesden (north), Neasden (north)                                                 1
     East Ham, Beckton, Upton Park (part), Barking (part)                                                                                                                1
     West Brompton, Chelsea (west)                                                                                                                                       1
     Lee, Mottingham, Grove Park, Chinbrook, Hither Green (part), Eltham (part), Horn Park, Blackheath (part)                                                            1
     Plaistow, West Ham (part), Upton Park (part)                                                                                                                        1
     Kensington, Holland Park (part)                                                                                                                                     1
     Clapham, Stockwell (part)                                                                                                                                           1
     Upper Edmonton, Edmonton (part)                                                                                                                                     1
     Woodford, South Woodford                                                                                                                                            1
     Dulwich, Dulwich Village, West Dulwich, Tulse Hill (part)                                                                                                           1
     East Dulwich, Dulwich Village (part), Peckham Rye, Loughborough Junction, Herne Hill                                                                                1
                                                                                                                                                                        ..
     The Hyde, Colindale, Kingsbury, West Hendon, Wembley Park (part), Queensbury (part)                                                                                 1
     West Ealing, Northfields (north and west)                                                                                                                           1
     Paddington, Bayswater, Hyde Park, Westbourne Green, Little Venice (part), Notting Hill (part)                                                                       1
     Leytonstone, Wanstead, Aldersbrook (part), Snaresbrook, Cann Hall                                                                                                   1
     Brixton, Stockwell, Clapham (part), Oval                                                                                                                            1
     Brockley, Crofton Park                                                                                                                                              1
     Acton, West Acton, North Acton (part), South Acton, East Acton (west), Park Royal (south), Hanger Hill Garden Estate, Gunnersbury Park                              1
     Muswell Hill, South Friern                                                                                                                                          1
     South Lambeth, Wandsworth Road, Vauxhall, Battersea:Nine Elms (part), Clapham (part), NW area of Stockwell, Oval                                                    1
     Putney, Roehampton, Kingston Vale, Putney Heath, Putney Vale, Richmond Park, Roehampton Vale                                                                        1
     Hornsey, Crouch End, Harringay (part)                                                                                                                               1
     Deptford, Evelyn, Rotherhithe (part)                                                                                                                                1
     Hampstead, Belsize Park, Frognal, Childs Hill (east), South Hampstead (north), Swiss Cottage (east), Primrose Hill (north), Chalk Farm (west), Gospel Oak           1
     Palmers Green                                                                                                                                                       1
     Peckham, Nunhead, South Bermondsey (part), Old Kent Road (part)                                                                                                     1
     Maida Hill, Maida Vale, Little Venice (part)                                                                                                                        1
     Sydenham, Crystal Palace (part)                                                                                                                                     1
     Poplar, Limehouse, Canary Wharf, Millwall, Blackwall, Cubitt Town, South Bromley, North Greenwich, Leamouth                                                         1
     New Cross                                                                                                                                                           1
     Forest Gate, Leytonstone (Part)                                                                                                                                     1
     Earls Court                                                                                                                                                         1
     Balham, Clapham South, Wandsworth Common (part)                                                                                                                     1
     Greenwich, Maze Hill, Greenwich Peninsula                                                                                                                           1
     Hackney Central, Dalston, London Fields, Stoke Newington (part), Cambridge Heath (part)                                                                             1
     Thamesmead                                                                                                                                                          1
     North Finchley, Woodside Park, Friern Barnet (part)                                                                                                                 1
     Highgate, Hampstead Heath (part)                                                                                                                                    1
     Canning Town, Silvertown, Royal Docks, North Woolwich, Beckton (part)                                                                                               1
     Bow, Bow Common, Bromley-by-Bow, Old Ford, Mile End (mostly), Fish Island,Mill Meads (part), Stratford (part), West Ham (part)                                      1
     Southgate, Oakwood, Arnos Grove (part)                                                                                                                              1
    Name: Neighborhood, Length: 112, dtype: int64




```python
London.set_index('PostCode',inplace=True)
London.reset_index(inplace=True)
London.tail()
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
      <th>PostCode</th>
      <th>Town</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>Area Category</th>
      <th>District</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>168</th>
      <td>WC2B</td>
      <td>LONDON</td>
      <td>None</td>
      <td>Camden, Westminster</td>
      <td>West Central</td>
      <td>Drury Lane, Kingsway, Aldwych</td>
    </tr>
    <tr>
      <th>169</th>
      <td>WC2E</td>
      <td>LONDON</td>
      <td>None</td>
      <td>Westminster</td>
      <td>West Central</td>
      <td>Covent Garden</td>
    </tr>
    <tr>
      <th>170</th>
      <td>WC2H</td>
      <td>LONDON</td>
      <td>None</td>
      <td>Camden, Westminster</td>
      <td>West Central</td>
      <td>Leicester Square, St. Giles</td>
    </tr>
    <tr>
      <th>171</th>
      <td>WC2N</td>
      <td>LONDON</td>
      <td>None</td>
      <td>Westminster</td>
      <td>West Central</td>
      <td>Charing Cross</td>
    </tr>
    <tr>
      <th>172</th>
      <td>WC2R</td>
      <td>LONDON</td>
      <td>None</td>
      <td>Westminster</td>
      <td>West Central</td>
      <td>Somerset House, Temple (west)</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Fill empty cells in neighborhood variable
for i in London.index:
    if (London['Neighborhood'].iloc[[i]].isnull().values.any()) :
        London['Neighborhood'].iloc[[i]]=London['District'].iloc[[i]]
    else:
        "data science rocks" 
```


```python
London=London[London.Neighborhood != 'Non-geographic postcode district']
London=London[London.Area != 'Non-geographic postcode district']
London=London[London.PostCode!= 'E77']
London=London[London.PostCode!= 'E98']
London=London[London.PostCode!= 'Non-geographic postcode district'] #Exclude non geographic areas
```


```python
London.set_index('PostCode',inplace=True)
London.reset_index(inplace=True)
London.tail()
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
      <th>PostCode</th>
      <th>Town</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>Area Category</th>
      <th>District</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>166</th>
      <td>WC2B</td>
      <td>LONDON</td>
      <td>Drury Lane, Kingsway, Aldwych</td>
      <td>Camden, Westminster</td>
      <td>West Central</td>
      <td>Drury Lane, Kingsway, Aldwych</td>
    </tr>
    <tr>
      <th>167</th>
      <td>WC2E</td>
      <td>LONDON</td>
      <td>Covent Garden</td>
      <td>Westminster</td>
      <td>West Central</td>
      <td>Covent Garden</td>
    </tr>
    <tr>
      <th>168</th>
      <td>WC2H</td>
      <td>LONDON</td>
      <td>Leicester Square, St. Giles</td>
      <td>Camden, Westminster</td>
      <td>West Central</td>
      <td>Leicester Square, St. Giles</td>
    </tr>
    <tr>
      <th>169</th>
      <td>WC2N</td>
      <td>LONDON</td>
      <td>Charing Cross</td>
      <td>Westminster</td>
      <td>West Central</td>
      <td>Charing Cross</td>
    </tr>
    <tr>
      <th>170</th>
      <td>WC2R</td>
      <td>LONDON</td>
      <td>Somerset House, Temple (west)</td>
      <td>Westminster</td>
      <td>West Central</td>
      <td>Somerset House, Temple (west)</td>
    </tr>
  </tbody>
</table>
</div>




```python
London=London.drop(['Town'],axis=1) #drop Town variable
```


```python
Distr2 = pd.DataFrame(London.District.str.split(' district',1).tolist(),columns = ['District2','New'])
London['District']=Distr2['District2']
London.head()
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
      <th>PostCode</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>Area Category</th>
      <th>District</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>N1</td>
      <td>Barnsbury (part), Canonbury, Kings Cross, Isl...</td>
      <td>Hackney, Islington, Camden</td>
      <td>North</td>
      <td>Northern head</td>
    </tr>
    <tr>
      <th>1</th>
      <td>N1C</td>
      <td>Kings Cross Central</td>
      <td>Camden</td>
      <td>North</td>
      <td>Kings Cross Central</td>
    </tr>
    <tr>
      <th>2</th>
      <td>N2</td>
      <td>East Finchley, Fortis Green, Hampstead Garden...</td>
      <td>Barnet, Haringey</td>
      <td>North</td>
      <td>East Finchley</td>
    </tr>
    <tr>
      <th>3</th>
      <td>N3</td>
      <td>Finchley, Church End, Finchley Central</td>
      <td>Barnet</td>
      <td>North</td>
      <td>Finchley</td>
    </tr>
    <tr>
      <th>4</th>
      <td>N4</td>
      <td>Finsbury Park, Manor House, Harringay (part),...</td>
      <td>Haringey, Islington, Hackney</td>
      <td>North</td>
      <td>Finsbury Park</td>
    </tr>
  </tbody>
</table>
</div>



## Find Latitude & Longitude of London Neighborhoods


```python
!conda install -c conda-forge geocoder --yes
import geocoder
print('Done')
```

    Solving environment: done
    
    ## Package Plan ##
    
      environment location: /home/jupyterlab/conda
    
      added / updated specs: 
        - geocoder
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        orderedset-2.0             |           py36_0         231 KB  conda-forge
        geocoder-1.38.1            |             py_0          52 KB  conda-forge
        ratelim-0.1.6              |           py36_0           5 KB  conda-forge
        certifi-2018.8.24          |        py36_1001         139 KB  conda-forge
        ------------------------------------------------------------
                                               Total:         427 KB
    
    The following NEW packages will be INSTALLED:
    
        geocoder:   1.38.1-py_0      conda-forge
        orderedset: 2.0-py36_0       conda-forge
        ratelim:    0.1.6-py36_0     conda-forge
    
    The following packages will be UPDATED:
    
        certifi:    2018.8.24-py36_1 conda-forge --> 2018.8.24-py36_1001 conda-forge
    
    
    Downloading and Extracting Packages
    orderedset-2.0       | 231 KB    | ##################################### | 100% 
    geocoder-1.38.1      | 52 KB     | ##################################### | 100% 
    ratelim-0.1.6        | 5 KB      | ##################################### | 100% 
    certifi-2018.8.24    | 139 KB    | ##################################### | 100% 
    Preparing transaction: done
    Verifying transaction: done
    Executing transaction: done
    Done



```python
frame=London
frame['Latitude']=0
frame['Longitude']=0
frame.loc[0,'Latitude']
frame['PostCode']
frame.loc[0,'PostCode']
frame.index
frame #create our new dataframe
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
      <th>PostCode</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>Area Category</th>
      <th>District</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>N1</td>
      <td>Barnsbury (part), Canonbury, Kings Cross, Isl...</td>
      <td>Hackney, Islington, Camden</td>
      <td>North</td>
      <td>Northern head</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>N1C</td>
      <td>Kings Cross Central</td>
      <td>Camden</td>
      <td>North</td>
      <td>Kings Cross Central</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>N2</td>
      <td>East Finchley, Fortis Green, Hampstead Garden...</td>
      <td>Barnet, Haringey</td>
      <td>North</td>
      <td>East Finchley</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>N3</td>
      <td>Finchley, Church End, Finchley Central</td>
      <td>Barnet</td>
      <td>North</td>
      <td>Finchley</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>N4</td>
      <td>Finsbury Park, Manor House, Harringay (part),...</td>
      <td>Haringey, Islington, Hackney</td>
      <td>North</td>
      <td>Finsbury Park</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>N5</td>
      <td>Highbury, Highbury Fields</td>
      <td>Islington, Hackney (part)</td>
      <td>North</td>
      <td>Highbury</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>N6</td>
      <td>Highgate, Hampstead Heath (part)</td>
      <td>Camden, Haringey, Islington</td>
      <td>North</td>
      <td>Highgate</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>N7</td>
      <td>Holloway, Barnsbury (part), Islington (part),...</td>
      <td>Islington, Camden</td>
      <td>North</td>
      <td>Holloway</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>N8</td>
      <td>Hornsey, Crouch End, Harringay (part)</td>
      <td>Haringey, Islington</td>
      <td>North</td>
      <td>Hornsey</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>N9</td>
      <td>Lower Edmonton, Edmonton (part)</td>
      <td>Enfield</td>
      <td>North</td>
      <td>Lower Edmonton</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>N10</td>
      <td>Muswell Hill, South Friern</td>
      <td>Haringey, Barnet</td>
      <td>North</td>
      <td>Muswell Hill</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>N11</td>
      <td>New Southgate, Friern Barnet, Bounds Green, A...</td>
      <td>Enfield, Barnet, Haringey</td>
      <td>North</td>
      <td>New Southgate</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>N12</td>
      <td>North Finchley, Woodside Park, Friern Barnet ...</td>
      <td>Barnet</td>
      <td>North</td>
      <td>North Finchley</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>N13</td>
      <td>Palmers Green</td>
      <td>Enfield</td>
      <td>North</td>
      <td>Palmers Green</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>N14</td>
      <td>Southgate, Oakwood, Arnos Grove (part)</td>
      <td>Enfield, Barnet</td>
      <td>North</td>
      <td>Southgate</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>N15</td>
      <td>South Tottenham, Harringay (part), West Green...</td>
      <td>Hackney, Haringey</td>
      <td>North</td>
      <td>South Tottenham</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>N16</td>
      <td>Stoke Newington, Stamford Hill (part), Shackl...</td>
      <td>Islington, Hackney</td>
      <td>North</td>
      <td>Stoke Newington</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>N17</td>
      <td>Tottenham, Wood Green (part)</td>
      <td>Haringey</td>
      <td>North</td>
      <td>Tottenham</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>N18</td>
      <td>Upper Edmonton, Edmonton (part)</td>
      <td>Enfield</td>
      <td>North</td>
      <td>Upper Edmonton</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>N19</td>
      <td>Upper Holloway, Archway, Tufnell Park (part)</td>
      <td>Islington, Camden</td>
      <td>North</td>
      <td>Upper Holloway</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>N20</td>
      <td>Whetstone, Totteridge, Oakleigh Park</td>
      <td>Barnet</td>
      <td>North</td>
      <td>Whetstone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>N21</td>
      <td>Winchmore Hill, Bush Hill, Grange Park</td>
      <td>Enfield</td>
      <td>North</td>
      <td>Winchmore Hill</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>N22</td>
      <td>Wood Green, Bounds Green (part), Bowes Park</td>
      <td>Haringey, Enfield</td>
      <td>North</td>
      <td>Wood Green</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>SE1</td>
      <td>Bankside, South Bank, Lambeth (part), Southwa...</td>
      <td>Lambeth, Southwark, City of London (part)</td>
      <td>South East</td>
      <td>South Eastern head</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>SE2</td>
      <td>Abbey Wood, West Heath, Crossness, Thamesmead...</td>
      <td>Bexley, Greenwich</td>
      <td>South East</td>
      <td>Abbey Wood</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>SE3</td>
      <td>Blackheath, Kidbrooke, Westcombe Park</td>
      <td>Greenwich, Lewisham</td>
      <td>South East</td>
      <td>Blackheath</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>SE4</td>
      <td>Brockley, Crofton Park</td>
      <td>Lewisham</td>
      <td>South East</td>
      <td>Brockley</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>SE5</td>
      <td>Camberwell, Denmark Hill, Peckham, Brixton (p...</td>
      <td>Southwark, Lambeth</td>
      <td>South East</td>
      <td>Camberwell</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>SE6</td>
      <td>Catford, Bellingham, Hither Green (part)</td>
      <td>Lewisham</td>
      <td>South East</td>
      <td>Catford</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29</th>
      <td>SE7</td>
      <td>Charlton</td>
      <td>Greenwich</td>
      <td>South East</td>
      <td>Charlton</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>141</th>
      <td>W1T</td>
      <td>Fitzrovia, Tottenham Court Road</td>
      <td>Camden</td>
      <td>West</td>
      <td>Fitzrovia, Tottenham Court Road</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>142</th>
      <td>W1U</td>
      <td>Marylebone</td>
      <td>Westminster</td>
      <td>West</td>
      <td>Marylebone</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>143</th>
      <td>W1W</td>
      <td>Great Portland Street, Fitzrovia</td>
      <td>Westminster</td>
      <td>West</td>
      <td>Great Portland Street, Fitzrovia</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>144</th>
      <td>W2</td>
      <td>Paddington, Bayswater, Hyde Park, Westbourne ...</td>
      <td>Westminster, Kensington and Chelsea</td>
      <td>West</td>
      <td>Paddington head</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>145</th>
      <td>W3</td>
      <td>Acton, West Acton, North Acton (part), South ...</td>
      <td>Ealing, Hounslow, Hammersmith and Fulham</td>
      <td>West</td>
      <td>Acton</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>146</th>
      <td>W4</td>
      <td>Chiswick, Gunnersbury, Turnham Green, Acton G...</td>
      <td>Hounslow, Ealing, Hammersmith and Fulham</td>
      <td>West</td>
      <td>Chiswick</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>147</th>
      <td>W5</td>
      <td>Ealing, South Ealing, Ealing Common, North Ea...</td>
      <td>Ealing, Hounslow</td>
      <td>West</td>
      <td>Ealing</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>148</th>
      <td>W6</td>
      <td>Hammersmith, Ravenscourt Park, Stamford Brook...</td>
      <td>Hammersmith and Fulham, Hounslow</td>
      <td>West</td>
      <td>Hammersmith</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>149</th>
      <td>W7</td>
      <td>Hanwell, Boston Manor (part)</td>
      <td>Ealing, Hounslow</td>
      <td>West</td>
      <td>Hanwell</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>150</th>
      <td>W8</td>
      <td>Kensington, Holland Park (part)</td>
      <td>Kensington and Chelsea</td>
      <td>West</td>
      <td>Kensington</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>151</th>
      <td>W9</td>
      <td>Maida Hill, Maida Vale, Little Venice (part)</td>
      <td>Westminster, Brent, Camden</td>
      <td>West</td>
      <td>Maida Hill</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>152</th>
      <td>W10</td>
      <td>North Kensington, Kensal Town, Ladbroke Grove...</td>
      <td>Kensington and Chelsea, Westminster, Hammersmi...</td>
      <td>West</td>
      <td>North Kensington</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>153</th>
      <td>W11</td>
      <td>Notting Hill, Ladbroke Grove (south), Holland...</td>
      <td>Kensington and Chelsea, Westminster, Hammersmi...</td>
      <td>West</td>
      <td>Notting Hill</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>154</th>
      <td>W12</td>
      <td>Shepherds Bush, White City, Wormwood Scrubs, ...</td>
      <td>Hammersmith and Fulham</td>
      <td>West</td>
      <td>Shepherds Bush</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>155</th>
      <td>W13</td>
      <td>West Ealing, Northfields (north and west)</td>
      <td>Ealing</td>
      <td>West</td>
      <td>West Ealing</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>156</th>
      <td>W14</td>
      <td>West Kensington, Kensington Olympia, Holland ...</td>
      <td>Hammersmith and Fulham, Kensington and Chelsea</td>
      <td>West</td>
      <td>West Kensington</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>157</th>
      <td>WC1A</td>
      <td>New Oxford Street</td>
      <td>Camden</td>
      <td>West Central</td>
      <td>New Oxford Street</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>158</th>
      <td>WC1B</td>
      <td>Bloomsbury, British Museum, Southampton Row</td>
      <td>Camden</td>
      <td>West Central</td>
      <td>Bloomsbury, British Museum, Southampton Row</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>159</th>
      <td>WC1E</td>
      <td>University College London</td>
      <td>Camden</td>
      <td>West Central</td>
      <td>University College London</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>160</th>
      <td>WC1H</td>
      <td>St Pancras</td>
      <td>Camden</td>
      <td>West Central</td>
      <td>St Pancras</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>161</th>
      <td>WC1N</td>
      <td>Russell Square, Great Ormond Street</td>
      <td>Camden</td>
      <td>West Central</td>
      <td>Russell Square, Great Ormond Street</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>162</th>
      <td>WC1R</td>
      <td>Gray's Inn</td>
      <td>Camden</td>
      <td>West Central</td>
      <td>Gray's Inn</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>163</th>
      <td>WC1V</td>
      <td>High Holborn</td>
      <td>Camden, City of London</td>
      <td>West Central</td>
      <td>High Holborn</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>164</th>
      <td>WC1X</td>
      <td>Kings Cross, Finsbury (west), Clerkenwell (north)</td>
      <td>Camden, Islington</td>
      <td>West Central</td>
      <td>Kings Cross, Finsbury (west), Clerkenwell (north)</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>165</th>
      <td>WC2A</td>
      <td>Lincoln's Inn Fields, Royal Courts of Justice,...</td>
      <td>Camden, Westminster, City of London</td>
      <td>West Central</td>
      <td>Lincoln's Inn Fields, Royal Courts of Justice,...</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>166</th>
      <td>WC2B</td>
      <td>Drury Lane, Kingsway, Aldwych</td>
      <td>Camden, Westminster</td>
      <td>West Central</td>
      <td>Drury Lane, Kingsway, Aldwych</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>167</th>
      <td>WC2E</td>
      <td>Covent Garden</td>
      <td>Westminster</td>
      <td>West Central</td>
      <td>Covent Garden</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>168</th>
      <td>WC2H</td>
      <td>Leicester Square, St. Giles</td>
      <td>Camden, Westminster</td>
      <td>West Central</td>
      <td>Leicester Square, St. Giles</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>169</th>
      <td>WC2N</td>
      <td>Charing Cross</td>
      <td>Westminster</td>
      <td>West Central</td>
      <td>Charing Cross</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>170</th>
      <td>WC2R</td>
      <td>Somerset House, Temple (west)</td>
      <td>Westminster</td>
      <td>West Central</td>
      <td>Somerset House, Temple (west)</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>171 rows × 7 columns</p>
</div>




```python
for i in frame.index:
    lat_lng_coords = (None)
    while lat_lng_coords is None:
        g  = geocoder.google('{}, London'.format(frame.loc[i,'PostCode']))
        lat_lng_coords = g.latlng
    frame.loc[i,'Latitude']= lat_lng_coords[0]
    frame.loc[i,'Longitude'] = lat_lng_coords[1] #import Latitude & Longitude to our new dataframe
```


```python
frame['Area Category'].value_counts()
```




    South East      28
    South West      27
    West            25
    North           23
    East Central    23
    East            20
    West Central    14
    North West      11
    Name: Area Category, dtype: int64




```python
!conda install -c conda-forge folium=0.5.0 --yes
import folium 
```

    Solving environment: done
    
    ## Package Plan ##
    
      environment location: /home/jupyterlab/conda
    
      added / updated specs: 
        - folium=0.5.0
    
    
    The following packages will be downloaded:
    
        package                    |            build
        ---------------------------|-----------------
        folium-0.5.0               |             py_0          45 KB  conda-forge
        altair-2.2.2               |           py36_1         461 KB  conda-forge
        branca-0.3.0               |             py_0          24 KB  conda-forge
        vincent-0.4.4              |             py_1          28 KB  conda-forge
        ------------------------------------------------------------
                                               Total:         558 KB
    
    The following NEW packages will be INSTALLED:
    
        altair:  2.2.2-py36_1 conda-forge
        branca:  0.3.0-py_0   conda-forge
        folium:  0.5.0-py_0   conda-forge
        vincent: 0.4.4-py_1   conda-forge
    
    
    Downloading and Extracting Packages
    folium-0.5.0         | 45 KB     | ##################################### | 100% 
    altair-2.2.2         | 461 KB    | ##################################### | 100% 
    branca-0.3.0         | 24 KB     | ##################################### | 100% 
    vincent-0.4.4        | 28 KB     | ##################################### | 100% 
    Preparing transaction: done
    Verifying transaction: done



```python
latitude=51.509865
longitude=-0.118092

# create map of London with markers for each Neighborhood
map_London = folium.Map(location=[latitude, longitude], zoom_start=11)


 
for lat, lng, label,cat in zip(frame['Latitude'], frame['Longitude'], frame['Neighborhood'],frame['Area Category']):
    if (cat == 'North'):
        label = folium.Popup(label, parse_html=True)
        folium.CircleMarker(
        [lat, lng],
        radius=6,
        popup=label,
        color='green',
        fill=False,
        fill_color='#3186cc',
        fill_opacity=0.7,
        parse_html=False).add_to(map_London)  
    elif (cat== 'South East'):
            label = folium.Popup(label, parse_html=True)
            folium.CircleMarker(
            [lat, lng],
            radius=6,
            popup=label,
            color='red',
            fill=False,
            fill_color='#3186cc',
            fill_opacity=0.7,
            parse_html=False).add_to(map_London)  
    elif (cat== 'South West'):
            label = folium.Popup(label, parse_html=True)
            folium.CircleMarker(
            [lat, lng],
            radius=6,
            popup=label,
            color='blue',
            fill=False,
            fill_color='#3186cc',
            fill_opacity=0.7,
            parse_html=False).add_to(map_London)
    elif (cat== 'East Central'):
            label = folium.Popup(label, parse_html=True)
            folium.CircleMarker(
            [lat, lng],
            radius=6,
            popup=label,
            color='orange',
            fill=False,
            fill_color='#3186cc',
            fill_opacity=0.7,
            parse_html=False).add_to(map_London)   
    elif (cat== 'West'):
            label = folium.Popup(label, parse_html=True)
            folium.CircleMarker(
            [lat, lng],
            radius=6,
            popup=label,
            color='#2ecc71',
            fill=False,
            fill_color='#3186cc',
            fill_opacity=0.7,
            parse_html=False).add_to(map_London)   
    elif (cat== 'East'):
            label = folium.Popup(label, parse_html=True)
            folium.CircleMarker(
            [lat, lng],
            radius=6,
            popup=label,
            color='#9b59b6',
            fill=False,
            fill_color='#3186cc',
            fill_opacity=0.7,
            parse_html=False).add_to(map_London)   
    elif (cat== 'West Central'):
            label = folium.Popup(label, parse_html=True)
            folium.CircleMarker(
            [lat, lng],
            radius=6,
            popup=label,
            color='#3498db',
            fill=False,
            fill_color='#3186cc',
            fill_opacity=0.7,
            parse_html=False).add_to(map_London)    
    else:
            label = folium.Popup(label, parse_html=True)
            folium.CircleMarker(
            [lat, lng],
            radius=6,
            popup=label,
            color='#3498db',
            fill=False,
            fill_color='#34495e',
            fill_opacity=0.7,
            parse_html=False).add_to(map_London)   
map_London

```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiIiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYiA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYicsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNTEuNTA5ODY1LC0wLjExODA5Ml0sCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB6b29tOiAxMSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbGF5ZXJzOiBbXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHdvcmxkQ29weUp1bXA6IGZhbHNlLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NwogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB9KTsKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfYzYyYjU3NDI0YjZlNDZiMGFhNTg2ZTM4NWM1Zjg5M2UgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICdodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZycsCiAgICAgICAgICAgICAgICB7CiAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgIm1heFpvb20iOiAxOCwKICAibWluWm9vbSI6IDEsCiAgIm5vV3JhcCI6IGZhbHNlLAogICJzdWJkb21haW5zIjogImFiYyIKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQ1ZWI4M2M5YmQ2ZTQ4MjZhMTM2ZmY4Zjg0NjZjYmFhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTQxMjYyMSwtMC4wODgxMzg3OTk5OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJncmVlbiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzJkYzRiM2U2YzE1YjQ4NmFiZTVlYzc0YWFmMGQ1ZmFmID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2NlZGM1YmUwZmVmNjQ4OTlhM2I3NzI0NjIzMzkzNWQ5ID0gJCgnPGRpdiBpZD0iaHRtbF9jZWRjNWJlMGZlZjY0ODk5YTNiNzcyNDYyMzM5MzVkOSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEJhcm5zYnVyeSAocGFydCksIENhbm9uYnVyeSwgS2luZ3MgQ3Jvc3MsIElzbGluZ3RvbiwgUGVudG9udmlsbGUsIERlIEJlYXV2b2lyIFRvd24sIEhveHRvbi4gU2hvcmVkaXRjaCAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzJkYzRiM2U2YzE1YjQ4NmFiZTVlYzc0YWFmMGQ1ZmFmLnNldENvbnRlbnQoaHRtbF9jZWRjNWJlMGZlZjY0ODk5YTNiNzcyNDYyMzM5MzVkOSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80NWViODNjOWJkNmU0ODI2YTEzNmZmOGY4NDY2Y2JhYS5iaW5kUG9wdXAocG9wdXBfMmRjNGIzZTZjMTViNDg2YWJlNWVjNzRhYWYwZDVmYWYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTNkN2Y4YWRhZjJmNDFmMmFlZTJmODk2Yzk5MzY5ZjIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MzcyODUxLC0wLjEyNDg5MzFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiZ3JlZW4iLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81YmYzMDdlODg3NGI0OGE0YjYzMjc5ZmFhMDMyOTJmMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF81NDgwNWExNTAzZGU0YTQ1YmQ3Y2IyNDU0MGI1YjUyZCA9ICQoJzxkaXYgaWQ9Imh0bWxfNTQ4MDVhMTUwM2RlNGE0NWJkN2NiMjQ1NDBiNWI1MmQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPktpbmdzIENyb3NzIENlbnRyYWw8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzViZjMwN2U4ODc0YjQ4YTRiNjMyNzlmYWEwMzI5MmYwLnNldENvbnRlbnQoaHRtbF81NDgwNWExNTAzZGU0YTQ1YmQ3Y2IyNDU0MGI1YjUyZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lM2Q3ZjhhZGFmMmY0MWYyYWVlMmY4OTZjOTkzNjlmMi5iaW5kUG9wdXAocG9wdXBfNWJmMzA3ZTg4NzRiNDhhNGI2MzI3OWZhYTAzMjkyZjApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMWVlMThkMTQ1Mzc1NGM3OWI1N2E3MDM3YWY0ZWQzMDkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41OTExNTAzLC0wLjE2NzkwMTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiZ3JlZW4iLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81MTIwZDBiYWNmY2Y0ZWMxOGZkZTZiZjBiNjMyM2NlOSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85ZmJlMTkwZjBjNWY0MDdlODk0ZjgxZTNkZDg3ZTQwNCA9ICQoJzxkaXYgaWQ9Imh0bWxfOWZiZTE5MGYwYzVmNDA3ZTg5NGY4MWUzZGQ4N2U0MDQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBFYXN0IEZpbmNobGV5LCBGb3J0aXMgR3JlZW4sIEhhbXBzdGVhZCBHYXJkZW4gU3VidXJiIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNTEyMGQwYmFjZmNmNGVjMThmZGU2YmYwYjYzMjNjZTkuc2V0Q29udGVudChodG1sXzlmYmUxOTBmMGM1ZjQwN2U4OTRmODFlM2RkODdlNDA0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzFlZTE4ZDE0NTM3NTRjNzliNTdhNzAzN2FmNGVkMzA5LmJpbmRQb3B1cChwb3B1cF81MTIwZDBiYWNmY2Y0ZWMxOGZkZTZiZjBiNjMyM2NlOSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85MzcxNTRjNDY1YTY0OTgzOWMyYjlhZjQ4NTliNWM3OSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjYwMTY0LC0wLjE5MTU4NTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiZ3JlZW4iLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83YmEzYjQxYmU0MjM0ZWViYWVhYzY3ZDk1ZDRjZGFiMyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9lYTZmYmU3NzQzZTg0ZDU5YmQxMzA4MWM4MDNkZGI4MiA9ICQoJzxkaXYgaWQ9Imh0bWxfZWE2ZmJlNzc0M2U4NGQ1OWJkMTMwODFjODAzZGRiODIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBGaW5jaGxleSwgQ2h1cmNoIEVuZCwgRmluY2hsZXkgQ2VudHJhbDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfN2JhM2I0MWJlNDIzNGVlYmFlYWM2N2Q5NWQ0Y2RhYjMuc2V0Q29udGVudChodG1sX2VhNmZiZTc3NDNlODRkNTliZDEzMDgxYzgwM2RkYjgyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzkzNzE1NGM0NjVhNjQ5ODM5YzJiOWFmNDg1OWI1Yzc5LmJpbmRQb3B1cChwb3B1cF83YmEzYjQxYmU0MjM0ZWViYWVhYzY3ZDk1ZDRjZGFiMyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lOTU5ODdiZmJhOGY0ZWFiOWZmYmU1NGJmZDAwOWIxMiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU3MjgxMTQsLTAuMTAwMDE1OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJncmVlbiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzFmZWViMzdkMjNhNTRiMjY5YjdiYzljNzE5NjQ1MWRlID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzYxZWIzMmJhYTU3ZjQxNTY4NmFhYzI4OGI4NTgzMDhiID0gJCgnPGRpdiBpZD0iaHRtbF82MWViMzJiYWE1N2Y0MTU2ODZhYWMyODhiODU4MzA4YiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEZpbnNidXJ5IFBhcmssIE1hbm9yIEhvdXNlLCBIYXJyaW5nYXkgKHBhcnQpLCBTdHJvdWQgR3JlZW48L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzFmZWViMzdkMjNhNTRiMjY5YjdiYzljNzE5NjQ1MWRlLnNldENvbnRlbnQoaHRtbF82MWViMzJiYWE1N2Y0MTU2ODZhYWMyODhiODU4MzA4Yik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lOTU5ODdiZmJhOGY0ZWFiOWZmYmU1NGJmZDAwOWIxMi5iaW5kUG9wdXAocG9wdXBfMWZlZWIzN2QyM2E1NGIyNjliN2JjOWM3MTk2NDUxZGUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYjMzYjc5OGQyNGQ2NDI1Njk2OWU5ZGI0OTM0MmM0NDIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41NTQ0MDI4LC0wLjA5NzAwNzJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiZ3JlZW4iLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80OTFjZDA5MWYzZDA0YTI4ODM0ODNhODBlNjNlMjNkYiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84ZGViMjUwYTlkMmI0ZTUxYWNkZGNiY2I0ZTYyMTA1OCA9ICQoJzxkaXYgaWQ9Imh0bWxfOGRlYjI1MGE5ZDJiNGU1MWFjZGRjYmNiNGU2MjEwNTgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBIaWdoYnVyeSwgSGlnaGJ1cnkgRmllbGRzPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80OTFjZDA5MWYzZDA0YTI4ODM0ODNhODBlNjNlMjNkYi5zZXRDb250ZW50KGh0bWxfOGRlYjI1MGE5ZDJiNGU1MWFjZGRjYmNiNGU2MjEwNTgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYjMzYjc5OGQyNGQ2NDI1Njk2OWU5ZGI0OTM0MmM0NDIuYmluZFBvcHVwKHBvcHVwXzQ5MWNkMDkxZjNkMDRhMjg4MzQ4M2E4MGU2M2UyM2RiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2E4YTRhMWU3NDgzOTQxMjViM2UxYmY4ZDRlOGQxNzQ1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTc1Mzg4NywtMC4xNTAxMTU1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImdyZWVuIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNWQ5MDJmYTE0MWJlNGMyMzlmNjg2NzJmMzQyNjIzYjEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYmY3NGQ0NzI1ZWJhNGViZDlkMTI3NDgwYTU4ZjI2MWMgPSAkKCc8ZGl2IGlkPSJodG1sX2JmNzRkNDcyNWViYTRlYmQ5ZDEyNzQ4MGE1OGYyNjFjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gSGlnaGdhdGUsIEhhbXBzdGVhZCBIZWF0aCAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzVkOTAyZmExNDFiZTRjMjM5ZjY4NjcyZjM0MjYyM2IxLnNldENvbnRlbnQoaHRtbF9iZjc0ZDQ3MjVlYmE0ZWJkOWQxMjc0ODBhNThmMjYxYyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9hOGE0YTFlNzQ4Mzk0MTI1YjNlMWJmOGQ0ZThkMTc0NS5iaW5kUG9wdXAocG9wdXBfNWQ5MDJmYTE0MWJlNGMyMzlmNjg2NzJmMzQyNjIzYjEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzVjMDhhNDFlMTY3NGI5M2JmMDU5ODRhNDRkZGM3ZDggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41NTQzODY5LC0wLjExNDY2NThdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiZ3JlZW4iLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83NDRkZTNiM2IwNDE0NjU3YjQ2ZjU3NTMwYzA4MDNlNiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9iZjkyODhkZTE1MGY0ZGQzYTA3MDRjZGI3MGEzN2NmYiA9ICQoJzxkaXYgaWQ9Imh0bWxfYmY5Mjg4ZGUxNTBmNGRkM2EwNzA0Y2RiNzBhMzdjZmIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBIb2xsb3dheSwgQmFybnNidXJ5IChwYXJ0KSwgSXNsaW5ndG9uIChwYXJ0KSwgVHVmbmVsbCBQYXJrIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNzQ0ZGUzYjNiMDQxNDY1N2I0NmY1NzUzMGMwODAzZTYuc2V0Q29udGVudChodG1sX2JmOTI4OGRlMTUwZjRkZDNhMDcwNGNkYjcwYTM3Y2ZiKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzM1YzA4YTQxZTE2NzRiOTNiZjA1OTg0YTQ0ZGRjN2Q4LmJpbmRQb3B1cChwb3B1cF83NDRkZTNiM2IwNDE0NjU3YjQ2ZjU3NTMwYzA4MDNlNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iZDg4MjljNjc0YTk0ODM2YjFlZjIzYzAzY2I0MGE1NiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU4MzMxMTgsLTAuMTIzNjI1N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJncmVlbiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzViYmI3NTQ0YjYzYjQ3ZTI5MzRiYmMyZGUxN2U2OTFjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2UyMjJjYzEzZmUyOTQ5NzQ4ODRlZDcwMzU5NmViZTU3ID0gJCgnPGRpdiBpZD0iaHRtbF9lMjIyY2MxM2ZlMjk0OTc0ODg0ZWQ3MDM1OTZlYmU1NyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEhvcm5zZXksIENyb3VjaCBFbmQsIEhhcnJpbmdheSAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzViYmI3NTQ0YjYzYjQ3ZTI5MzRiYmMyZGUxN2U2OTFjLnNldENvbnRlbnQoaHRtbF9lMjIyY2MxM2ZlMjk0OTc0ODg0ZWQ3MDM1OTZlYmU1Nyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iZDg4MjljNjc0YTk0ODM2YjFlZjIzYzAzY2I0MGE1Ni5iaW5kUG9wdXAocG9wdXBfNWJiYjc1NDRiNjNiNDdlMjkzNGJiYzJkZTE3ZTY5MWMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNGE1ZDY3YmZmNmQ1NDY4N2I4NDViZThlMTVlZjFmNWIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS42MjgxMDQ0MDAwMDAwMSwtMC4wNTU5NzY1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImdyZWVuIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfY2Y3OTY3NzkzMzFkNGJjNGI5NjljODk4YmZkNDNhM2UgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZGIzZmE2MzhkZGEwNDYzN2E5OTkwZGZmOWEzYThkNTggPSAkKCc8ZGl2IGlkPSJodG1sX2RiM2ZhNjM4ZGRhMDQ2MzdhOTk5MGRmZjlhM2E4ZDU4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gTG93ZXIgRWRtb250b24sIEVkbW9udG9uIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfY2Y3OTY3NzkzMzFkNGJjNGI5NjljODk4YmZkNDNhM2Uuc2V0Q29udGVudChodG1sX2RiM2ZhNjM4ZGRhMDQ2MzdhOTk5MGRmZjlhM2E4ZDU4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzRhNWQ2N2JmZjZkNTQ2ODdiODQ1YmU4ZTE1ZWYxZjViLmJpbmRQb3B1cChwb3B1cF9jZjc5Njc3OTMzMWQ0YmM0Yjk2OWM4OThiZmQ0M2EzZSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hYTUwMjVhNzkwNjU0ZGYzYmYyMzMxN2E3YTM2YmQ4ZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU5NjQ0NDI5OTk5OTk5LC0wLjE0NDMyODddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiZ3JlZW4iLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wOTA1YzU0ZGQ1NDE0MDNhYTEzMzBkMTYyZjdjZGI0NSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82MTI0NjEzNzUzN2Y0MmYzYWE3MTEyNWJlYTEyNjhjMCA9ICQoJzxkaXYgaWQ9Imh0bWxfNjEyNDYxMzc1MzdmNDJmM2FhNzExMjViZWExMjY4YzAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBNdXN3ZWxsIEhpbGwsIFNvdXRoIEZyaWVybjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMDkwNWM1NGRkNTQxNDAzYWExMzMwZDE2MmY3Y2RiNDUuc2V0Q29udGVudChodG1sXzYxMjQ2MTM3NTM3ZjQyZjNhYTcxMTI1YmVhMTI2OGMwKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2FhNTAyNWE3OTA2NTRkZjNiZjIzMzE3YTdhMzZiZDhlLmJpbmRQb3B1cChwb3B1cF8wOTA1YzU0ZGQ1NDE0MDNhYTEzMzBkMTYyZjdjZGI0NSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wZGI1NGUyZjkzZjM0ZTQ3YWI1NzkxNTczZTk4MjZhMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjYxNzUwNjIsLTAuMTM4NTMzOV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJncmVlbiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzY1ZTIxNTZkNjM1YTRjNTI4MGEzYmY0MzhkOGE4NzI5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzk1NWM4MjZlNGY1YzQ5Yjc5ZTUyNTQ5MTY0ZTMwNzY3ID0gJCgnPGRpdiBpZD0iaHRtbF85NTVjODI2ZTRmNWM0OWI3OWU1MjU0OTE2NGUzMDc2NyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IE5ldyBTb3V0aGdhdGUsIEZyaWVybiBCYXJuZXQsIEJvdW5kcyBHcmVlbiwgQXJub3MgR3JvdmUgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82NWUyMTU2ZDYzNWE0YzUyODBhM2JmNDM4ZDhhODcyOS5zZXRDb250ZW50KGh0bWxfOTU1YzgyNmU0ZjVjNDliNzllNTI1NDkxNjRlMzA3NjcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMGRiNTRlMmY5M2YzNGU0N2FiNTc5MTU3M2U5ODI2YTEuYmluZFBvcHVwKHBvcHVwXzY1ZTIxNTZkNjM1YTRjNTI4MGEzYmY0MzhkOGE4NzI5KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzM0MmE2ZDJiMmRlMDQ1ZTI4NGM0MzYzOWUzNWMxNGFkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNjE3NDUwMjk5OTk5OTksLTAuMTc5ODc1OF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJncmVlbiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzIzNGFhZTU1ZjgwMTRhMmY4ZjVhNDFkM2RmZWE5NWQ4ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2EzOWQzY2E5OWU0OTRiYjk5NDhkOTNiNWJlNmZkMWFlID0gJCgnPGRpdiBpZD0iaHRtbF9hMzlkM2NhOTllNDk0YmI5OTQ4ZDkzYjViZTZmZDFhZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IE5vcnRoIEZpbmNobGV5LCBXb29kc2lkZSBQYXJrLCBGcmllcm4gQmFybmV0IChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMjM0YWFlNTVmODAxNGEyZjhmNWE0MWQzZGZlYTk1ZDguc2V0Q29udGVudChodG1sX2EzOWQzY2E5OWU0OTRiYjk5NDhkOTNiNWJlNmZkMWFlKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzM0MmE2ZDJiMmRlMDQ1ZTI4NGM0MzYzOWUzNWMxNGFkLmJpbmRQb3B1cChwb3B1cF8yMzRhYWU1NWY4MDE0YTJmOGY1YTQxZDNkZmVhOTVkOCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl81NTVlOGE4YmRmNTI0Nzc3OGU4ZWViM2JkNzIzOTAzMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjYxNzU0MjYsLTAuMTAzMTI1OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJncmVlbiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzllMjA2ZTg3ZDU0NDQ4MDk4YjMwMDcwMjg5Yzk1ZjcxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzY5NGNiMTNlNDIyNDRiMTE5MjI1NjQyYzY4OTIwODlmID0gJCgnPGRpdiBpZD0iaHRtbF82OTRjYjEzZTQyMjQ0YjExOTIyNTY0MmM2ODkyMDg5ZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFBhbG1lcnMgR3JlZW48L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzllMjA2ZTg3ZDU0NDQ4MDk4YjMwMDcwMjg5Yzk1ZjcxLnNldENvbnRlbnQoaHRtbF82OTRjYjEzZTQyMjQ0YjExOTIyNTY0MmM2ODkyMDg5Zik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81NTVlOGE4YmRmNTI0Nzc3OGU4ZWViM2JkNzIzOTAzMC5iaW5kUG9wdXAocG9wdXBfOWUyMDZlODdkNTQ0NDgwOThiMzAwNzAyODljOTVmNzEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZmM3NDI3MTFlNWRkNGZiZDhjOGU2ZTg5ZDE2Mjg3ZjIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS42MzU5NTEyLC0wLjEyMzg1OTNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiZ3JlZW4iLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81MmZmMzE3OTIxOWI0NWQ2YWE3ZWZiODc5Mzk5MGY2ZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84M2U4MmZiMjEzNWY0YzY2ODljM2M0MjJmYTY0MWNmOCA9ICQoJzxkaXYgaWQ9Imh0bWxfODNlODJmYjIxMzVmNGM2Njg5YzNjNDIyZmE2NDFjZjgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBTb3V0aGdhdGUsIE9ha3dvb2QsIEFybm9zIEdyb3ZlIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNTJmZjMxNzkyMTliNDVkNmFhN2VmYjg3OTM5OTBmNmYuc2V0Q29udGVudChodG1sXzgzZTgyZmIyMTM1ZjRjNjY4OWMzYzQyMmZhNjQxY2Y4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2ZjNzQyNzExZTVkZDRmYmQ4YzhlNmU4OWQxNjI4N2YyLmJpbmRQb3B1cChwb3B1cF81MmZmMzE3OTIxOWI0NWQ2YWE3ZWZiODc5Mzk5MGY2Zik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yNDYwNTE2Y2Y3OWQ0OTM5YjIxYWIxYTY5YzgxNTM3NiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU4MzM1MTksLTAuMDc2NDkyOTk5OTk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiZ3JlZW4iLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9iYWM1YmI5NjBiN2U0ZTczYmQzOTQ4NTFmYjU5MzgwNyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83MzM2ZWMzZWE2MDI0ZGUzODZlOWIwZTY0NTZhNGVhOCA9ICQoJzxkaXYgaWQ9Imh0bWxfNzMzNmVjM2VhNjAyNGRlMzg2ZTliMGU2NDU2YTRlYTgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBTb3V0aCBUb3R0ZW5oYW0sIEhhcnJpbmdheSAocGFydCksIFdlc3QgR3JlZW4sIFNldmVuIFNpc3RlcnMsIFN0YW1mb3JkIEhpbGwgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iYWM1YmI5NjBiN2U0ZTczYmQzOTQ4NTFmYjU5MzgwNy5zZXRDb250ZW50KGh0bWxfNzMzNmVjM2VhNjAyNGRlMzg2ZTliMGU2NDU2YTRlYTgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMjQ2MDUxNmNmNzlkNDkzOWIyMWFiMWE2OWM4MTUzNzYuYmluZFBvcHVwKHBvcHVwX2JhYzViYjk2MGI3ZTRlNzNiZDM5NDg1MWZiNTkzODA3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzRkNzc1ODFjMDBmOTQxZjU4ZmM3NDcyMGQwZTIwM2U4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTYyMzA3OCwtMC4wNzY0MzUzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImdyZWVuIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfN2FkNjk4Y2FjMzI3NGE0ODk5ODE3YzJlYzE4ZjkwMmYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZGRlMTBmMTlmN2Q4NDA0OTk3MzE3ZWNiNjY1OGM0OTcgPSAkKCc8ZGl2IGlkPSJodG1sX2RkZTEwZjE5ZjdkODQwNDk5NzMxN2VjYjY2NThjNDk3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gU3Rva2UgTmV3aW5ndG9uLCBTdGFtZm9yZCBIaWxsIChwYXJ0KSwgU2hhY2tsZXdlbGwsIERhbHN0b24gKHBhcnQpLCBOZXdpbmd0b24gR3JlZW4gKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83YWQ2OThjYWMzMjc0YTQ4OTk4MTdjMmVjMThmOTAyZi5zZXRDb250ZW50KGh0bWxfZGRlMTBmMTlmN2Q4NDA0OTk3MzE3ZWNiNjY1OGM0OTcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNGQ3NzU4MWMwMGY5NDFmNThmYzc0NzIwZDBlMjAzZTguYmluZFBvcHVwKHBvcHVwXzdhZDY5OGNhYzMyNzRhNDg5OTgxN2MyZWMxOGY5MDJmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2I0MzI4ZDdkYjljZjRkZDNhMGUwNjIyNTJmNWI4OWFiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTkzODg5Mzk5OTk5OTksLTAuMDUyOTYzN10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJncmVlbiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzJhYjgwNTYzNTU5NjRjNmFhYjg3YjQ3NjZkYzExYzkyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzA5OGQ5OGI5ODhiZjQzNGViYzQ1ZDViNDQyZGMxMTViID0gJCgnPGRpdiBpZD0iaHRtbF8wOThkOThiOTg4YmY0MzRlYmM0NWQ1YjQ0MmRjMTE1YiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFRvdHRlbmhhbSwgV29vZCBHcmVlbiAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzJhYjgwNTYzNTU5NjRjNmFhYjg3YjQ3NjZkYzExYzkyLnNldENvbnRlbnQoaHRtbF8wOThkOThiOTg4YmY0MzRlYmM0NWQ1YjQ0MmRjMTE1Yik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iNDMyOGQ3ZGI5Y2Y0ZGQzYTBlMDYyMjUyZjViODlhYi5iaW5kUG9wdXAocG9wdXBfMmFiODA1NjM1NTk2NGM2YWFiODdiNDc2NmRjMTFjOTIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNGRiZGQxMmM1ZWEyNGE5NDk2NGQ5ZjlkYzM2MjBkNGMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS42MTQ5MzA2MDAwMDAwMSwtMC4wNzY1Nzk2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImdyZWVuIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfM2I1ZWQwYjg1ZTFiNGQxZDgzZDk4ZDMyMGRjYjI2YWIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOGZmMDI0MzhhNzU2NGJmODk0Y2RiZWM0MmZiZDZkOTkgPSAkKCc8ZGl2IGlkPSJodG1sXzhmZjAyNDM4YTc1NjRiZjg5NGNkYmVjNDJmYmQ2ZDk5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gVXBwZXIgRWRtb250b24sIEVkbW9udG9uIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfM2I1ZWQwYjg1ZTFiNGQxZDgzZDk4ZDMyMGRjYjI2YWIuc2V0Q29udGVudChodG1sXzhmZjAyNDM4YTc1NjRiZjg5NGNkYmVjNDJmYmQ2ZDk5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzRkYmRkMTJjNWVhMjRhOTQ5NjRkOWY5ZGMzNjIwZDRjLmJpbmRQb3B1cChwb3B1cF8zYjVlZDBiODVlMWI0ZDFkODNkOThkMzIwZGNiMjZhYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83MTk0NzMxYmY4NGE0NWIxYmUzZjEyOGZkN2UzMjc0MCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU2NDg4ODMsLTAuMTMyMzgwOF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJncmVlbiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2YwN2M0NGExMTdhMDQzZTQ4N2ZlMWY2ZWVkZjU2MTUzID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2Y1NmNkZDU5OTJiNjQwOTU5ZjMzOWNiYWQ2ZDhlNWI3ID0gJCgnPGRpdiBpZD0iaHRtbF9mNTZjZGQ1OTkyYjY0MDk1OWYzMzljYmFkNmQ4ZTViNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFVwcGVyIEhvbGxvd2F5LCBBcmNod2F5LCBUdWZuZWxsIFBhcmsgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mMDdjNDRhMTE3YTA0M2U0ODdmZTFmNmVlZGY1NjE1My5zZXRDb250ZW50KGh0bWxfZjU2Y2RkNTk5MmI2NDA5NTlmMzM5Y2JhZDZkOGU1YjcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNzE5NDczMWJmODRhNDViMWJlM2YxMjhmZDdlMzI3NDAuYmluZFBvcHVwKHBvcHVwX2YwN2M0NGExMTdhMDQzZTQ4N2ZlMWY2ZWVkZjU2MTUzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQwYjdjNTBhY2E5NTQzMGY5MGVmMzg1MmE2ZWExNWFlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNjM1ODU1MTk5OTk5OTksLTAuMTk0Nzc3OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJncmVlbiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2VkMjMyZThjMGQ1YTRhODU4ODRjYzgyZWVmYzYzMmEwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzVmY2JkNjM1MjAxNTQ3ODFhNGZlZDg5ZjhhYzBkNGY3ID0gJCgnPGRpdiBpZD0iaHRtbF81ZmNiZDYzNTIwMTU0NzgxYTRmZWQ4OWY4YWMwZDRmNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFdoZXRzdG9uZSwgVG90dGVyaWRnZSwgT2FrbGVpZ2ggUGFyazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZWQyMzJlOGMwZDVhNGE4NTg4NGNjODJlZWZjNjMyYTAuc2V0Q29udGVudChodG1sXzVmY2JkNjM1MjAxNTQ3ODFhNGZlZDg5ZjhhYzBkNGY3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzQwYjdjNTBhY2E5NTQzMGY5MGVmMzg1MmE2ZWExNWFlLmJpbmRQb3B1cChwb3B1cF9lZDIzMmU4YzBkNWE0YTg1ODg0Y2M4MmVlZmM2MzJhMCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83Y2U4OTcxMmY4MWI0Yjk1YjZjMThjZmU1YjJlZTIyMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjYzNTk3MzcsLTAuMTAwMjQyNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJncmVlbiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVmYzg2ZmE2Y2NkYjRiNWZiNTAwZjAxZmI2YzE3ZmI5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2ExMzM5YzliNDM3MzQzZDRhM2Q3NDExZWNmZTUyMGIwID0gJCgnPGRpdiBpZD0iaHRtbF9hMTMzOWM5YjQzNzM0M2Q0YTNkNzQxMWVjZmU1MjBiMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFdpbmNobW9yZSBIaWxsLCBCdXNoIEhpbGwsIEdyYW5nZSBQYXJrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81ZmM4NmZhNmNjZGI0YjVmYjUwMGYwMWZiNmMxN2ZiOS5zZXRDb250ZW50KGh0bWxfYTEzMzljOWI0MzczNDNkNGEzZDc0MTFlY2ZlNTIwYjApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfN2NlODk3MTJmODFiNGI5NWI2YzE4Y2ZlNWIyZWUyMjMuYmluZFBvcHVwKHBvcHVwXzVmYzg2ZmE2Y2NkYjRiNWZiNTAwZjAxZmI2YzE3ZmI5KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzI3OWJjOTExNjZmYjQ2ZjE4ZGQwNDhkMmQwNGU1MmY2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNjAxNzM5OCwtMC4xMTQ4NjA3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImdyZWVuIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZmU1Yjc4ODk3MmFlNDZhM2JlZjQzYzQ3MGQ4ZmU2MDggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMzJiMmNmOGNmNDNmNDczZGFlNTQ4MTZlOGZmZDM4YWEgPSAkKCc8ZGl2IGlkPSJodG1sXzMyYjJjZjhjZjQzZjQ3M2RhZTU0ODE2ZThmZmQzOGFhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gV29vZCBHcmVlbiwgQm91bmRzIEdyZWVuIChwYXJ0KSwgQm93ZXMgUGFyazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZmU1Yjc4ODk3MmFlNDZhM2JlZjQzYzQ3MGQ4ZmU2MDguc2V0Q29udGVudChodG1sXzMyYjJjZjhjZjQzZjQ3M2RhZTU0ODE2ZThmZmQzOGFhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzI3OWJjOTExNjZmYjQ2ZjE4ZGQwNDhkMmQwNGU1MmY2LmJpbmRQb3B1cChwb3B1cF9mZTViNzg4OTcyYWU0NmEzYmVmNDNjNDcwZDhmZTYwOCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82ZjBhM2E4NmE3ZDg0NGQ2YWNhMWNlZGQ1Yjk0ZGM0OSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUwMTgzMjY5OTk5OTk5LC0wLjA5MDk1MDk5OTk5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2JmMmE3OGQzZWU5ZDQwMmI4NmFkMTUyYTY4OWMzYzA1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2JjNGVmY2NhZjNjOTQyYTJiNTFiZGI2MzYyY2YyMDM3ID0gJCgnPGRpdiBpZD0iaHRtbF9iYzRlZmNjYWYzYzk0MmEyYjUxYmRiNjM2MmNmMjAzNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEJhbmtzaWRlLCBTb3V0aCBCYW5rLCBMYW1iZXRoIChwYXJ0KSwgU291dGh3YXJrLCBCZXJtb25kc2V5IChwYXJ0KSwgVmF1eGhhbGwgKHBhcnQpLCBPbGQgS2VudCBSb2FkIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYmYyYTc4ZDNlZTlkNDAyYjg2YWQxNTJhNjg5YzNjMDUuc2V0Q29udGVudChodG1sX2JjNGVmY2NhZjNjOTQyYTJiNTFiZGI2MzYyY2YyMDM3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzZmMGEzYTg2YTdkODQ0ZDZhY2ExY2VkZDViOTRkYzQ5LmJpbmRQb3B1cChwb3B1cF9iZjJhNzhkM2VlOWQ0MDJiODZhZDE1MmE2ODljM2MwNSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82MDEyNzY1Y2VhMjg0NzIwOGY5NzM5YjhiNWFkZmExNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ4NjA0MjEsMC4xMjAyNTg2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2YyOTYxYTkyNDFkZDQ2MjhhMDFhYmQ4ZWExYTk1Y2U2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzhlZTM1ZTlhZTk5ODQ0YzM4ZGMyM2Q3MDE2NjRkMjVkID0gJCgnPGRpdiBpZD0iaHRtbF84ZWUzNWU5YWU5OTg0NGMzOGRjMjNkNzAxNjY0ZDI1ZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEFiYmV5IFdvb2QsIFdlc3QgSGVhdGgsIENyb3NzbmVzcywgVGhhbWVzbWVhZCAocGFydCksIFBsdW1zdGVhZCAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2YyOTYxYTkyNDFkZDQ2MjhhMDFhYmQ4ZWExYTk1Y2U2LnNldENvbnRlbnQoaHRtbF84ZWUzNWU5YWU5OTg0NGMzOGRjMjNkNzAxNjY0ZDI1ZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82MDEyNzY1Y2VhMjg0NzIwOGY5NzM5YjhiNWFkZmExNi5iaW5kUG9wdXAocG9wdXBfZjI5NjFhOTI0MWRkNDYyOGEwMWFiZDhlYTFhOTVjZTYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOTk1MmVkOWJiYzg2NDI1ODg0Yjg4Y2JmODM1ZjIyMjQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40Njc3MTUyOTk5OTk5OSwwLjAxNzU2ODVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZGI5NmRhN2IzZWFjNDA0NDhkZjNhYzhkMmEzMzA2MGIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMGQzNDcwMmQ1OTM1NDRkNGIxYjU2ZjEyZWU3MjQ4ZWQgPSAkKCc8ZGl2IGlkPSJodG1sXzBkMzQ3MDJkNTkzNTQ0ZDRiMWI1NmYxMmVlNzI0OGVkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQmxhY2toZWF0aCwgS2lkYnJvb2tlLCBXZXN0Y29tYmUgUGFyazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZGI5NmRhN2IzZWFjNDA0NDhkZjNhYzhkMmEzMzA2MGIuc2V0Q29udGVudChodG1sXzBkMzQ3MDJkNTkzNTQ0ZDRiMWI1NmYxMmVlNzI0OGVkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzk5NTJlZDliYmM4NjQyNTg4NGI4OGNiZjgzNWYyMjI0LmJpbmRQb3B1cChwb3B1cF9kYjk2ZGE3YjNlYWM0MDQ0OGRmM2FjOGQyYTMzMDYwYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hZTRhNGQ0NGQwNzE0YjBiYWExNDA1ODk4NWM2YTNiNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ1OTgzMzcsLTAuMDMyMjA0N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81NTA5YmQ1NDNhYTc0MTNiYWQxMzc2ZmVjMDM2YTY3MCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85MjBkMWYxZGQxZWQ0MTRjOWFlNGExZjc1OWRiOWJmYyA9ICQoJzxkaXYgaWQ9Imh0bWxfOTIwZDFmMWRkMWVkNDE0YzlhZTRhMWY3NTlkYjliZmMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBCcm9ja2xleSwgQ3JvZnRvbiBQYXJrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81NTA5YmQ1NDNhYTc0MTNiYWQxMzc2ZmVjMDM2YTY3MC5zZXRDb250ZW50KGh0bWxfOTIwZDFmMWRkMWVkNDE0YzlhZTRhMWY3NTlkYjliZmMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYWU0YTRkNDRkMDcxNGIwYmFhMTQwNTg5ODVjNmEzYjYuYmluZFBvcHVwKHBvcHVwXzU1MDliZDU0M2FhNzQxM2JhZDEzNzZmZWMwMzZhNjcwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzIwMTBhYzNkMmIzZTQwMGRiNzYzMTFmOGZlNmExMDk5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDc1NTYxLC0wLjA5MDg2NTRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfODc5MmMwNDA2MWE3NDI5MzkwYWJhYjNlMDFhMGI3MDggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMWQ3NDAwZDViY2FjNDI4MDgzNDNlYjMwNTE2NWEyZWMgPSAkKCc8ZGl2IGlkPSJodG1sXzFkNzQwMGQ1YmNhYzQyODA4MzQzZWIzMDUxNjVhMmVjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQ2FtYmVyd2VsbCwgRGVubWFyayBIaWxsLCBQZWNraGFtLCBCcml4dG9uIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfODc5MmMwNDA2MWE3NDI5MzkwYWJhYjNlMDFhMGI3MDguc2V0Q29udGVudChodG1sXzFkNzQwMGQ1YmNhYzQyODA4MzQzZWIzMDUxNjVhMmVjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzIwMTBhYzNkMmIzZTQwMGRiNzYzMTFmOGZlNmExMDk5LmJpbmRQb3B1cChwb3B1cF84NzkyYzA0MDYxYTc0MjkzOTBhYmFiM2UwMWEwYjcwOCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zMTJiNzM5NGQ4NjU0YmFhOTMzZmQ4NzQ1MDhkOWIyYiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQzNjIwNjgsLTAuMDE3NTQ4N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80NjQyODc0MzUyYTE0OWFkYjkyZGVkZGJmYjQ5YzA5YiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hODdkY2I1YjhiMTA0NmJiYWIzOTc4MWJkODlhYjJhZCA9ICQoJzxkaXYgaWQ9Imh0bWxfYTg3ZGNiNWI4YjEwNDZiYmFiMzk3ODFiZDg5YWIyYWQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBDYXRmb3JkLCBCZWxsaW5naGFtLCBIaXRoZXIgR3JlZW4gKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80NjQyODc0MzUyYTE0OWFkYjkyZGVkZGJmYjQ5YzA5Yi5zZXRDb250ZW50KGh0bWxfYTg3ZGNiNWI4YjEwNDZiYmFiMzk3ODFiZDg5YWIyYWQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMzEyYjczOTRkODY1NGJhYTkzM2ZkODc0NTA4ZDliMmIuYmluZFBvcHVwKHBvcHVwXzQ2NDI4NzQzNTJhMTQ5YWRiOTJkZWRkYmZiNDljMDliKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzczOWUyZTdjZGU1NTQ1MmY5ZTIzZjNiODM2ZTBjMWEwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDg2MDk3NCwwLjAzODA5ODNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOGM5OTI2OGEwNDI2NDZjZWJkNTFkNmZhMTA4Y2I4MjQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfY2QzMTZjMzhhNjRmNDA3ZTg4MjA3NTA3NWU1NzYzODAgPSAkKCc8ZGl2IGlkPSJodG1sX2NkMzE2YzM4YTY0ZjQwN2U4ODIwNzUwNzVlNTc2MzgwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQ2hhcmx0b248L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzhjOTkyNjhhMDQyNjQ2Y2ViZDUxZDZmYTEwOGNiODI0LnNldENvbnRlbnQoaHRtbF9jZDMxNmMzOGE2NGY0MDdlODgyMDc1MDc1ZTU3NjM4MCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83MzllMmU3Y2RlNTU0NTJmOWUyM2YzYjgzNmUwYzFhMC5iaW5kUG9wdXAocG9wdXBfOGM5OTI2OGEwNDI2NDZjZWJkNTFkNmZhMTA4Y2I4MjQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMDJmMzc4OTc4N2YzNGIxNTkyZGI0NmUxMzMxYTI3NWIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40ODA4NDY3LC0wLjAyNjM2NzZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNGNhMDc0NjRiNDM5NGI5NGFmZmFlNTU3NDIwYjgwMGIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNDBlNDE3NDUxYzVmNDUxYjgxNTU5MTJhMTcxM2YyOTAgPSAkKCc8ZGl2IGlkPSJodG1sXzQwZTQxNzQ1MWM1ZjQ1MWI4MTU1OTEyYTE3MTNmMjkwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gRGVwdGZvcmQsIEV2ZWx5biwgUm90aGVyaGl0aGUgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80Y2EwNzQ2NGI0Mzk0Yjk0YWZmYWU1NTc0MjBiODAwYi5zZXRDb250ZW50KGh0bWxfNDBlNDE3NDUxYzVmNDUxYjgxNTU5MTJhMTcxM2YyOTApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDJmMzc4OTc4N2YzNGIxNTkyZGI0NmUxMzMxYTI3NWIuYmluZFBvcHVwKHBvcHVwXzRjYTA3NDY0YjQzOTRiOTRhZmZhZTU1NzQyMGI4MDBiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzJmZDRhNDIzM2I3ODQ4NmE5ZWUxNDc5MDAzMTJiZDJkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDQxNDQzOSwwLjA1ODUzMTU5OTk5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzg0YmViNjUwNmJhNzRiMmU4OGM1ODJkOTAzZTM0MDgwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzA3YWVjYjZlMGNiMTQyOGQ5ZjRhMTQ1NjNlNjQwMmJlID0gJCgnPGRpdiBpZD0iaHRtbF8wN2FlY2I2ZTBjYjE0MjhkOWY0YTE0NTYzZTY0MDJiZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEVsdGhhbSwgTW90dGluZ2hhbSwgTmV3IEVsdGhhbSwgV2VsbCBIYWxsLCBBdmVyeSBIaWxsIChwYXJ0KSwgRmFsY29ud29vZCAocGFydCksIFNpZGN1cCAocGFydCksIENoaW5icm9vayAocGFydCksIExvbmdsYW5kcyAocGFydCkgS2lkYnJvb2tlIChwYXJ0KSwgU2hvb3RlciYjMzk7cyBIaWxsIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfODRiZWI2NTA2YmE3NGIyZTg4YzU4MmQ5MDNlMzQwODAuc2V0Q29udGVudChodG1sXzA3YWVjYjZlMGNiMTQyOGQ5ZjRhMTQ1NjNlNjQwMmJlKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzJmZDRhNDIzM2I3ODQ4NmE5ZWUxNDc5MDAzMTJiZDJkLmJpbmRQb3B1cChwb3B1cF84NGJlYjY1MDZiYTc0YjJlODhjNTgyZDkwM2UzNDA4MCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9hNDM4YWU1OWY5YWU0YWM3OWFjMWRkOTFmZGMzNWY3NSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ4MDg0OTYsLTAuMDAyOTI5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzRmODY4MTgxNDVmMDQyYzc5OTE3Njc1YTJhNjczMDNjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzUwN2E3NDNiNzA4MjQ0NGQ4NjUwNDhhODUyMTM3MWI2ID0gJCgnPGRpdiBpZD0iaHRtbF81MDdhNzQzYjcwODI0NDRkODY1MDQ4YTg1MjEzNzFiNiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEdyZWVud2ljaCwgTWF6ZSBIaWxsLCBHcmVlbndpY2ggUGVuaW5zdWxhPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80Zjg2ODE4MTQ1ZjA0MmM3OTkxNzY3NWEyYTY3MzAzYy5zZXRDb250ZW50KGh0bWxfNTA3YTc0M2I3MDgyNDQ0ZDg2NTA0OGE4NTIxMzcxYjYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYTQzOGFlNTlmOWFlNGFjNzlhYzFkZDkxZmRjMzVmNzUuYmluZFBvcHVwKHBvcHVwXzRmODY4MTgxNDVmMDQyYzc5OTE3Njc1YTJhNjczMDNjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2VhMTcyNWNkNDdkMTQ0ZDViNjdiNTRjN2Y2ZmQ2ZDJhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDkxMzA3OSwtMC4xMDg1MzMzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzI4MDE4NDMwMjNjNTRiM2NiZjgyZjEzMjc4ZmRmYTk3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzE0ZWFhZjBmMmM4ZjRjN2U4MzgwOWZiZWVhN2JlYTJkID0gJCgnPGRpdiBpZD0iaHRtbF8xNGVhYWYwZjJjOGY0YzdlODM4MDlmYmVlYTdiZWEyZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEtlbm5pbmd0b24sIExhbWJldGggKHBhcnQpLCBWYXV4aGFsbCAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzI4MDE4NDMwMjNjNTRiM2NiZjgyZjEzMjc4ZmRmYTk3LnNldENvbnRlbnQoaHRtbF8xNGVhYWYwZjJjOGY0YzdlODM4MDlmYmVlYTdiZWEyZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lYTE3MjVjZDQ3ZDE0NGQ1YjY3YjU0YzdmNmZkNmQyYS5iaW5kUG9wdXAocG9wdXBfMjgwMTg0MzAyM2M1NGIzY2JmODJmMTMyNzhmZGZhOTcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNzc3NjA5OGZjOWY1NDc4YzhhZjczNDNmOWY5MDZjY2QgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40NDY3MDgsMC4wMTc1NTUzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2M2ZjA1NjdiNTMzNzQyZGM5ZjkxNTdiYTQ5YzViYmQ2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzFkZTVkNmU4N2UzMzQzMTliZDM3MGQzNmRkYzY5OTI3ID0gJCgnPGRpdiBpZD0iaHRtbF8xZGU1ZDZlODdlMzM0MzE5YmQzNzBkMzZkZGM2OTkyNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IExlZSwgTW90dGluZ2hhbSwgR3JvdmUgUGFyaywgQ2hpbmJyb29rLCBIaXRoZXIgR3JlZW4gKHBhcnQpLCBFbHRoYW0gKHBhcnQpLCBIb3JuIFBhcmssIEJsYWNraGVhdGggKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jNmYwNTY3YjUzMzc0MmRjOWY5MTU3YmE0OWM1YmJkNi5zZXRDb250ZW50KGh0bWxfMWRlNWQ2ZTg3ZTMzNDMxOWJkMzcwZDM2ZGRjNjk5MjcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNzc3NjA5OGZjOWY1NDc4YzhhZjczNDNmOWY5MDZjY2QuYmluZFBvcHVwKHBvcHVwX2M2ZjA1NjdiNTMzNzQyZGM5ZjkxNTdiYTQ5YzViYmQ2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2JlY2ViYzU3ZGY0NzQ2OGQ4YzFlOTQ2ZmM0M2FhMDQzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDU3MjEyLC0wLjAwNTg1MzNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMzcxNzFkOGQyMzI0NDA1MjhhNDgwZjNlNjZmMmQzM2MgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYjU4NmE2ZDExOTEzNDVhYjgxYjA2YmNlOTU2Y2RkYzggPSAkKCc8ZGl2IGlkPSJodG1sX2I1ODZhNmQxMTkxMzQ1YWI4MWIwNmJjZTk1NmNkZGM4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gTGV3aXNoYW0sIEhpdGhlciBHcmVlbiwgTGFkeXdlbGw8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzM3MTcxZDhkMjMyNDQwNTI4YTQ4MGYzZTY2ZjJkMzNjLnNldENvbnRlbnQoaHRtbF9iNTg2YTZkMTE5MTM0NWFiODFiMDZiY2U5NTZjZGRjOCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iZWNlYmM1N2RmNDc0NjhkOGMxZTk0NmZjNDNhYTA0My5iaW5kUG9wdXAocG9wdXBfMzcxNzFkOGQyMzI0NDA1MjhhNDgwZjNlNjZmMmQzM2MpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNjAxMGZmODY1NThjNDIzYmI3NjNjYjJhZmU5NDA5MTYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40NzU1OSwtMC4wMzgwODRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMzEzZjMzNDNlOTcxNDU0ZThkNjcwOTM5NTAyZWMyMGUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZmFkN2FkYmMwOGUxNDFlYjg2NjUyZmM2NWM0NTlkN2QgPSAkKCc8ZGl2IGlkPSJodG1sX2ZhZDdhZGJjMDhlMTQxZWI4NjY1MmZjNjVjNDU5ZDdkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gTmV3IENyb3NzPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8zMTNmMzM0M2U5NzE0NTRlOGQ2NzA5Mzk1MDJlYzIwZS5zZXRDb250ZW50KGh0bWxfZmFkN2FkYmMwOGUxNDFlYjg2NjUyZmM2NWM0NTlkN2QpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNjAxMGZmODY1NThjNDIzYmI3NjNjYjJhZmU5NDA5MTYuYmluZFBvcHVwKHBvcHVwXzMxM2YzMzQzZTk3MTQ1NGU4ZDY3MDkzOTUwMmVjMjBlKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2QwNjNmYmVhMDhhOTRiNzdiYTAyOTM4NTAyNmVhZGI1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDcwMzI2OSwtMC4wNjE1MjM3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzNjY2I0Mzk5MmNhMDRlNTE4YzM3OTlhNjNhOTRlNjhjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzcwODM1ZDljNDFlNzRmOGZhYWFhYWFiYmE1YTY4MjlkID0gJCgnPGRpdiBpZD0iaHRtbF83MDgzNWQ5YzQxZTc0ZjhmYWFhYWFhYmJhNWE2ODI5ZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFBlY2toYW0sIE51bmhlYWQsIFNvdXRoIEJlcm1vbmRzZXkgKHBhcnQpLCBPbGQgS2VudCBSb2FkIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfM2NjYjQzOTkyY2EwNGU1MThjMzc5OWE2M2E5NGU2OGMuc2V0Q29udGVudChodG1sXzcwODM1ZDljNDFlNzRmOGZhYWFhYWFiYmE1YTY4MjlkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2QwNjNmYmVhMDhhOTRiNzdiYTAyOTM4NTAyNmVhZGI1LmJpbmRQb3B1cChwb3B1cF8zY2NiNDM5OTJjYTA0ZTUxOGMzNzk5YTYzYTk0ZTY4Yyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iODk5NTY2ZGI3NDc0ZjM1YTVmNTAzYzk0MDk4ZmZjNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ5NjYwMjIsLTAuMDQ5ODQ1N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kOWFlNmY4NzUxN2U0YTIzYTBmMDQ4NWYwNjM5YTgyOSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84ODY5ZDc4YTlhYWI0YmJkYmU3MTRkNjY1NDZjMmQ2NiA9ICQoJzxkaXYgaWQ9Imh0bWxfODg2OWQ3OGE5YWFiNGJiZGJlNzE0ZDY2NTQ2YzJkNjYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBSb3RoZXJoaXRoZSAocGFydCksIFN1cnJleSBRdWF5cywgU291dGggQmVybW9uZHNleSAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Q5YWU2Zjg3NTE3ZTRhMjNhMGYwNDg1ZjA2MzlhODI5LnNldENvbnRlbnQoaHRtbF84ODY5ZDc4YTlhYWI0YmJkYmU3MTRkNjY1NDZjMmQ2Nik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iODk5NTY2ZGI3NDc0ZjM1YTVmNTAzYzk0MDk4ZmZjNC5iaW5kUG9wdXAocG9wdXBfZDlhZTZmODc1MTdlNGEyM2EwZjA0ODVmMDYzOWE4MjkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZGFhOGM3OGQ0YWE5NDU2ZGEyNDZlNmJkMDdmNmJhYjkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40ODczODA5LC0wLjA5MjM3MTUwMDAwMDAwMDAxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzAwMzMzOTlhODk5YTQxYmFhMDNhMjdjMDBmZWM2MWNjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2QzOTkyZDQ3NmUyMTRkODFhYjQzZWVlM2RiYjRlY2IyID0gJCgnPGRpdiBpZD0iaHRtbF9kMzk5MmQ0NzZlMjE0ZDgxYWI0M2VlZTNkYmI0ZWNiMiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFdhbHdvcnRoLCBLZW5uaW5ndG9uIChwYXJ0KSwgTmV3aW5ndG9uPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wMDMzMzk5YTg5OWE0MWJhYTAzYTI3YzAwZmVjNjFjYy5zZXRDb250ZW50KGh0bWxfZDM5OTJkNDc2ZTIxNGQ4MWFiNDNlZWUzZGJiNGVjYjIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZGFhOGM3OGQ0YWE5NDU2ZGEyNDZlNmJkMDdmNmJhYjkuYmluZFBvcHVwKHBvcHVwXzAwMzMzOTlhODk5YTQxYmFhMDNhMjdjMDBmZWM2MWNjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2NhNjU1NTcxNDllYTRmYzQ5Yjk2OWQ2NTczNDRmNTAzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDc4MTk4MTk5OTk5OTksMC4wNzYyMDUyOTk5OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xNjUxZmI0ZGQ1ZmE0ZjU5OGY5NDRhODQwZTg4MjExNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hMWIxYWU1NzA3ZDk0YWQ5YjNmMTk0ZjVhMGZiYjAzZiA9ICQoJzxkaXYgaWQ9Imh0bWxfYTFiMWFlNTcwN2Q5NGFkOWIzZjE5NGY1YTBmYmIwM2YiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBXb29sd2ljaCwgUm95YWwgQXJzZW5hbCwgUGx1bXN0ZWFkLCBTaG9vdGVyJiMzOTtzIEhpbGw8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzE2NTFmYjRkZDVmYTRmNTk4Zjk0NGE4NDBlODgyMTE1LnNldENvbnRlbnQoaHRtbF9hMWIxYWU1NzA3ZDk0YWQ5YjNmMTk0ZjVhMGZiYjAzZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9jYTY1NTU3MTQ5ZWE0ZmM0OWI5NjlkNjU3MzQ0ZjUwMy5iaW5kUG9wdXAocG9wdXBfMTY1MWZiNGRkNWZhNGY1OThmOTQ0YTg0MGU4ODIxMTUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMTkwOTE4NzVjODE0NDMyZGEzY2NlYjA0NDA4M2ZjNTcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40MTc4MDQ1LC0wLjA4NDgyMjI5OTk5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVlZDBhNjBlMzBlYzQ3YWU5MGI2MWQ1YWVhM2E1NTI0ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzUwYjhkMTkyZWQ3MTRkNzhiNDJjZTJkYmU1ODA0MzYzID0gJCgnPGRpdiBpZD0iaHRtbF81MGI4ZDE5MmVkNzE0ZDc4YjQyY2UyZGJlNTgwNDM2MyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFVwcGVyIE5vcndvb2QsIENyeXN0YWwgUGFsYWNlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81ZWQwYTYwZTMwZWM0N2FlOTBiNjFkNWFlYTNhNTUyNC5zZXRDb250ZW50KGh0bWxfNTBiOGQxOTJlZDcxNGQ3OGI0MmNlMmRiZTU4MDQzNjMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMTkwOTE4NzVjODE0NDMyZGEzY2NlYjA0NDA4M2ZjNTcuYmluZFBvcHVwKHBvcHVwXzVlZDBhNjBlMzBlYzQ3YWU5MGI2MWQ1YWVhM2E1NTI0KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzU5OWJiMDg3YjIwZjQxZGI4ZGY1NGRjMWI2YTE2MTEyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDEyNTcwNiwtMC4wNjEzOTY2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzczNGYyYTY1N2JhYzQ2MzA4MTk2MzQxOGNlMzNiNzU5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2QwNGMwMTc3ZTU4YjRmYTk5YmYxYTY4YzNhOWY2NWEyID0gJCgnPGRpdiBpZD0iaHRtbF9kMDRjMDE3N2U1OGI0ZmE5OWJmMWE2OGMzYTlmNjVhMiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEFuZXJsZXksIENyeXN0YWwgUGFsYWNlIChwYXJ0KSwgUGVuZ2UsIEJlY2tlbmhhbSAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzczNGYyYTY1N2JhYzQ2MzA4MTk2MzQxOGNlMzNiNzU5LnNldENvbnRlbnQoaHRtbF9kMDRjMDE3N2U1OGI0ZmE5OWJmMWE2OGMzYTlmNjVhMik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81OTliYjA4N2IyMGY0MWRiOGRmNTRkYzFiNmExNjExMi5iaW5kUG9wdXAocG9wdXBfNzM0ZjJhNjU3YmFjNDYzMDgxOTYzNDE4Y2UzM2I3NTkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZmJkYTQzZjBmZWZiNDJmYmIzYTNhNzA3NGQzYTZjOGEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40Mzg4MDI2LC0wLjA4NDg4NjA5OTk5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2Y0NDYyMjU0OTZlYzQ4OGI5ZWQwNWU1YWM2YmFjN2NiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2M5ZDc1ZWE1MGNlMjQ5ZGY5NjIzNjdlYWVkZDJiZmUzID0gJCgnPGRpdiBpZD0iaHRtbF9jOWQ3NWVhNTBjZTI0OWRmOTYyMzY3ZWFlZGQyYmZlMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IER1bHdpY2gsIER1bHdpY2ggVmlsbGFnZSwgV2VzdCBEdWx3aWNoLCBUdWxzZSBIaWxsIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZjQ0NjIyNTQ5NmVjNDg4YjllZDA1ZTVhYzZiYWM3Y2Iuc2V0Q29udGVudChodG1sX2M5ZDc1ZWE1MGNlMjQ5ZGY5NjIzNjdlYWVkZDJiZmUzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2ZiZGE0M2YwZmVmYjQyZmJiM2EzYTcwNzRkM2E2YzhhLmJpbmRQb3B1cChwb3B1cF9mNDQ2MjI1NDk2ZWM0ODhiOWVkMDVlNWFjNmJhYzdjYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iNDY4MzU5MWMyY2U0MTljOTYwNGMyYWRmZjU1M2RiOCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ1NDU2MzUsLTAuMDczMjEwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80NGJjMDc5ODExOTQ0YmI4YTM0MmIyNjczOWE5ODdiYiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9iMTBjZjMzYjA2NzQ0Njg2ODE2OGJjNmQyN2FhNzU5OSA9ICQoJzxkaXYgaWQ9Imh0bWxfYjEwY2YzM2IwNjc0NDY4NjgxNjhiYzZkMjdhYTc1OTkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBFYXN0IER1bHdpY2gsIER1bHdpY2ggVmlsbGFnZSAocGFydCksIFBlY2toYW0gUnllLCBMb3VnaGJvcm91Z2ggSnVuY3Rpb24sIEhlcm5lIEhpbGw8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzQ0YmMwNzk4MTE5NDRiYjhhMzQyYjI2NzM5YTk4N2JiLnNldENvbnRlbnQoaHRtbF9iMTBjZjMzYjA2NzQ0Njg2ODE2OGJjNmQyN2FhNzU5OSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iNDY4MzU5MWMyY2U0MTljOTYwNGMyYWRmZjU1M2RiOC5iaW5kUG9wdXAocG9wdXBfNDRiYzA3OTgxMTk0NGJiOGEzNDJiMjY3MzlhOTg3YmIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMDc1ZjFlNmVhYjA5NGE2YWIwMjU2YTlkNmM0MWYzZjQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40NDQwNzMzLC0wLjA0OTc1Ml0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kMTNkNGFkZjM1ODA0NTg1OWUyNTk0NjEyNGM4ZTg5MSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85NDM3NDU3YTdkOTQ0NmExYTZjZmNkYTJkYWVjZDYyMiA9ICQoJzxkaXYgaWQ9Imh0bWxfOTQzNzQ1N2E3ZDk0NDZhMWE2Y2ZjZGEyZGFlY2Q2MjIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBGb3Jlc3QgSGlsbCwgSG9ub3IgT2FrLCBDcm9mdG9uIFBhcmsgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9kMTNkNGFkZjM1ODA0NTg1OWUyNTk0NjEyNGM4ZTg5MS5zZXRDb250ZW50KGh0bWxfOTQzNzQ1N2E3ZDk0NDZhMWE2Y2ZjZGEyZGFlY2Q2MjIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDc1ZjFlNmVhYjA5NGE2YWIwMjU2YTlkNmM0MWYzZjQuYmluZFBvcHVwKHBvcHVwX2QxM2Q0YWRmMzU4MDQ1ODU5ZTI1OTQ2MTI0YzhlODkxKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzBjM2Y1Y2EwZTk3MTRhMDViODhlOGM4YmM3N2ZlZWJiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDU0NTQ2NSwtMC4wOTY2NjA3OTk5OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wMDEyNWVmMDdkZmM0NDg4YWUwMmI0ZTU2YTIzY2EyYiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80NDM2NTgxNTVkNzY0YTUwODUyOTAwOTMwMmM0ZjFkYiA9ICQoJzxkaXYgaWQ9Imh0bWxfNDQzNjU4MTU1ZDc2NGE1MDg1MjkwMDkzMDJjNGYxZGIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBIZXJuZSBIaWxsLCBUdWxzZSBIaWxsIChwYXJ0KSwgRHVsd2ljaCAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzAwMTI1ZWYwN2RmYzQ0ODhhZTAyYjRlNTZhMjNjYTJiLnNldENvbnRlbnQoaHRtbF80NDM2NTgxNTVkNzY0YTUwODUyOTAwOTMwMmM0ZjFkYik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8wYzNmNWNhMGU5NzE0YTA1Yjg4ZThjOGJjNzdmZWViYi5iaW5kUG9wdXAocG9wdXBfMDAxMjVlZjA3ZGZjNDQ4OGFlMDJiNGU1NmEyM2NhMmIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNmM1NDlhOWFhMTFhNDA0NWI0Y2YyZTc2N2NkMDkyNTMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS4zOTY4MjEsLTAuMDczMDU4OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80MTQ0M2Y5MDFhNTM0NWUxOTllOTJkMzkyMTYzNjM0ZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9iMzAwNmQwMGUwMGE0OTc2YTk1ODE1ZDdkNGI3MTZmOCA9ICQoJzxkaXYgaWQ9Imh0bWxfYjMwMDZkMDBlMDBhNDk3NmE5NTgxNWQ3ZDRiNzE2ZjgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBTb3V0aCBOb3J3b29kLCBTZWxodXJzdCAocGFydCksIFRob3JudG9uIEhlYXRoIChwYXJ0KSwgV29vZHNpZGUgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80MTQ0M2Y5MDFhNTM0NWUxOTllOTJkMzkyMTYzNjM0ZS5zZXRDb250ZW50KGh0bWxfYjMwMDZkMDBlMDBhNDk3NmE5NTgxNWQ3ZDRiNzE2ZjgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNmM1NDlhOWFhMTFhNDA0NWI0Y2YyZTc2N2NkMDkyNTMuYmluZFBvcHVwKHBvcHVwXzQxNDQzZjkwMWE1MzQ1ZTE5OWU5MmQzOTIxNjM2MzRlKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzJhYTM3OGM4Y2MzNzRmODU4OTQ1MTNhNjFiYjNiYTViID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDI4MzIwMTk5OTk5OTksLTAuMDU1NTc3Ml0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJyZWQiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yZDFhMzlmZGZhNjA0NDVkYmE1N2VmMTViMjczN2M1MiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82NDJlZmY4ZTk1NGU0NmUyODc2YjQzNzUwMmE2NWNiZSA9ICQoJzxkaXYgaWQ9Imh0bWxfNjQyZWZmOGU5NTRlNDZlMjg3NmI0Mzc1MDJhNjVjYmUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBTeWRlbmhhbSwgQ3J5c3RhbCBQYWxhY2UgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yZDFhMzlmZGZhNjA0NDVkYmE1N2VmMTViMjczN2M1Mi5zZXRDb250ZW50KGh0bWxfNjQyZWZmOGU5NTRlNDZlMjg3NmI0Mzc1MDJhNjVjYmUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMmFhMzc4YzhjYzM3NGY4NTg5NDUxM2E2MWJiM2JhNWIuYmluZFBvcHVwKHBvcHVwXzJkMWEzOWZkZmE2MDQ0NWRiYTU3ZWYxNWIyNzM3YzUyKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2E5ZWY4MzViN2I3ODRiYzVhOGQ5YWE0NTdmYjg1Nzk2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDI4Mjg4NywtMC4xMDI0MjldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAicmVkIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYmI0MDY1MTdmODEzNDNmMTg4YzZlY2FmNmQzZDNjOTggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZTAyNDYxODQ3Y2VjNDI5YzhlMmMwZmUxOWI1MTRiYjQgPSAkKCc8ZGl2IGlkPSJodG1sX2UwMjQ2MTg0N2NlYzQyOWM4ZTJjMGZlMTliNTE0YmI0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gV2VzdCBOb3J3b29kLCBHaXBzeSBIaWxsIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYmI0MDY1MTdmODEzNDNmMTg4YzZlY2FmNmQzZDNjOTguc2V0Q29udGVudChodG1sX2UwMjQ2MTg0N2NlYzQyOWM4ZTJjMGZlMTliNTE0YmI0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2E5ZWY4MzViN2I3ODRiYzVhOGQ5YWE0NTdmYjg1Nzk2LmJpbmRQb3B1cChwb3B1cF9iYjQwNjUxN2Y4MTM0M2YxODhjNmVjYWY2ZDNkM2M5OCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wYjhlZmI0ODhmZTY0ZTZkYjRlOTUyZDM1YmNlMjg0ZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUwMTgxNzgsMC4xMDg1NzQyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogInJlZCIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2Q0NDg3ZDc5ZGI3NTQ5ZWI5NDI5OTA5MWU5YWNhNGI1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2M1MmU4NDAxMWZkODQ1YjVhODE4YjVjYWM2YmFlMzBmID0gJCgnPGRpdiBpZD0iaHRtbF9jNTJlODQwMTFmZDg0NWI1YTgxOGI1Y2FjNmJhZTMwZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFRoYW1lc21lYWQ8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Q0NDg3ZDc5ZGI3NTQ5ZWI5NDI5OTA5MWU5YWNhNGI1LnNldENvbnRlbnQoaHRtbF9jNTJlODQwMTFmZDg0NWI1YTgxOGI1Y2FjNmJhZTMwZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8wYjhlZmI0ODhmZTY0ZTZkYjRlOTUyZDM1YmNlMjg0ZS5iaW5kUG9wdXAocG9wdXBfZDQ0ODdkNzlkYjc1NDllYjk0Mjk5MDkxZTlhY2E0YjUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMDUzYWY0YzU4NDU1NDg1NjlkMTQ1MWFiODE1OWIzNjMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MDg4MDU4LC0wLjA2MTU1MjFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzliNTliNiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzY2MzQ2MjdmMzAzMjRiOGVhYzYwMzA4NTViMWEwNDZlID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzlhMDZmYTJiMGZlNzQ0OWVhNGJiOGMwZmNmNDkyOWRjID0gJCgnPGRpdiBpZD0iaHRtbF85YTA2ZmEyYjBmZTc0NDllYTRiYjhjMGZjZjQ5MjlkYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEFsZGdhdGUgKHBhcnQpLCBCaXNob3BzZ2F0ZSAocGFydCksIFdoaXRlY2hhcGVsIChtb3N0bHkpLCBTaG9yZWRpdGNoIChwYXJ0KSwgU3BpdGFsZmllbGRzLCBTaGFkd2VsbCwgU3RlcG5leSAobW9zdGx5KSwgTWlsZSBFbmQgKHBhcnQpLCBQb3J0c29rZW48L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzY2MzQ2MjdmMzAzMjRiOGVhYzYwMzA4NTViMWEwNDZlLnNldENvbnRlbnQoaHRtbF85YTA2ZmEyYjBmZTc0NDllYTRiYjhjMGZjZjQ5MjlkYyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8wNTNhZjRjNTg0NTU0ODU2OWQxNDUxYWI4MTU5YjM2My5iaW5kUG9wdXAocG9wdXBfNjYzNDYyN2YzMDMyNGI4ZWFjNjAzMDg1NWIxYTA0NmUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTQxYTM1MzgyZDdkNDhjZTljZDE5YzA2NTIyNzE3MzcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MDU3OTU3LC0wLjA1NzE5OTFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzliNTliNiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2Y5YzNhMzRhNjkyMTQzMzRhMThjNjAzMDlkNTFlODI4ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzgyMWYzNTJhODI5ZDQ4OGRiMzlkYTA3NzFiZmFhYWM3ID0gJCgnPGRpdiBpZD0iaHRtbF84MjFmMzUyYTgyOWQ0ODhkYjM5ZGEwNzcxYmZhYWFjNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2FwcGluZywgU3QgS2F0aGFyaW5lIERvY2tzLCBTdGVwbmV5IChwYXJ0KSwgU2hhZHdlbGwgKHBhcnQpLCBXaGl0ZWNoYXBlbCAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2Y5YzNhMzRhNjkyMTQzMzRhMThjNjAzMDlkNTFlODI4LnNldENvbnRlbnQoaHRtbF84MjFmMzUyYTgyOWQ0ODhkYjM5ZGEwNzcxYmZhYWFjNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lNDFhMzUzODJkN2Q0OGNlOWNkMTljMDY1MjI3MTczNy5iaW5kUG9wdXAocG9wdXBfZjljM2EzNGE2OTIxNDMzNGExOGM2MDMwOWQ1MWU4MjgpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZWVmYTA3MWNmNzBjNDQ3M2EwN2JkYzA2N2NiYzZmMTMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MjgxMzM5OTk5OTk5OSwtMC4wNjE2NTExOTk5OTk5OTk5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjOWI1OWI2IiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNTJjYWFkOGJkYzU0NDcwM2E5MWYzOGI1NDliOTcwZTcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYzZmZGIyODgxMTljNDVkMzk3MDg0MGMwMDYyODBmZTQgPSAkKCc8ZGl2IGlkPSJodG1sX2M2ZmRiMjg4MTE5YzQ1ZDM5NzA4NDBjMDA2MjgwZmU0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQmV0aG5hbCBHcmVlbiAobW9zdGx5KSwgSGFnZ2Vyc3RvbiwgSG94dG9uIChwYXJ0KSwgU2hvcmVkaXRjaCAocGFydCksIENhbWJyaWRnZSBIZWF0aCAobW9zdGx5KSwgR2xvYmUgVG93bjwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNTJjYWFkOGJkYzU0NDcwM2E5MWYzOGI1NDliOTcwZTcuc2V0Q29udGVudChodG1sX2M2ZmRiMjg4MTE5YzQ1ZDM5NzA4NDBjMDA2MjgwZmU0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2VlZmEwNzFjZjcwYzQ0NzNhMDdiZGMwNjdjYmM2ZjEzLmJpbmRQb3B1cChwb3B1cF81MmNhYWQ4YmRjNTQ0NzAzYTkxZjM4YjU0OWI5NzBlNyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wY2YwMzgyYzIxMmY0OWRlODQyNTMxNjg0YjM4ZjA1MiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUyODE0ODMsLTAuMDIwNTQxNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjOWI1OWI2IiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNTg1NzkwZTlkOTU3NDMyNTk0N2EzOWRmMmFlOWI0MDYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYzgyMmY5MmU2ZjNlNDY1NGE5MDMxZjE1NWNhNzJiMzkgPSAkKCc8ZGl2IGlkPSJodG1sX2M4MjJmOTJlNmYzZTQ2NTRhOTAzMWYxNTVjYTcyYjM5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQm93LCBCb3cgQ29tbW9uLCBCcm9tbGV5LWJ5LUJvdywgT2xkIEZvcmQsIE1pbGUgRW5kIChtb3N0bHkpLCBGaXNoIElzbGFuZCxNaWxsIE1lYWRzIChwYXJ0KSwgU3RyYXRmb3JkIChwYXJ0KSwgV2VzdCBIYW0gKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81ODU3OTBlOWQ5NTc0MzI1OTQ3YTM5ZGYyYWU5YjQwNi5zZXRDb250ZW50KGh0bWxfYzgyMmY5MmU2ZjNlNDY1NGE5MDMxZjE1NWNhNzJiMzkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMGNmMDM4MmMyMTJmNDlkZTg0MjUzMTY4NGIzOGYwNTIuYmluZFBvcHVwKHBvcHVwXzU4NTc5MGU5ZDk1NzQzMjU5NDdhMzlkZjJhZTliNDA2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzIwMTkwNzdkMmY2NzRhYTE5MmE4MTAyODNiNDgxMjNjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNjMwNzQ5OSwtMC4wMTE3ODAzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM5YjU5YjYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9iMzIyMTFlN2JiOGY0YzkwYmEwOGVlNDM2ZGZmZDhjNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jM2M1NDhmMzJkYjE0MGZlYjAzYjc2ZTRlYzI0NzcwZiA9ICQoJzxkaXYgaWQ9Imh0bWxfYzNjNTQ4ZjMyZGIxNDBmZWIwM2I3NmU0ZWMyNDc3MGYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBDaGluZ2ZvcmQsIFNld2FyZHN0b25lLCBIaWdoYW1zIFBhcmssIFVwcGVyIEVkbW9udG9uIChwYXJ0KSwgV29vZGZvcmQgR3JlZW4gKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iMzIyMTFlN2JiOGY0YzkwYmEwOGVlNDM2ZGZmZDhjNS5zZXRDb250ZW50KGh0bWxfYzNjNTQ4ZjMyZGIxNDBmZWIwM2I3NmU0ZWMyNDc3MGYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMjAxOTA3N2QyZjY3NGFhMTkyYTgxMDI4M2I0ODEyM2MuYmluZFBvcHVwKHBvcHVwX2IzMjIxMWU3YmI4ZjRjOTBiYTA4ZWU0MzZkZmZkOGM1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U4ODkyYjE3ZmUwMDRmMzRiZTkzOTQ3ZmZhZGYzOGU4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTU5NjkyMDAwMDAwMDEsLTAuMDQ5OTU4NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjOWI1OWI2IiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMGIwYjg1ZDY3YzRjNDQ0MjlkYWM2ODlmZDA4YTQzZWYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYTc1NTY3OTA4NDEwNDcyODkyOGUzNzQxMWE4OWI3ZGYgPSAkKCc8ZGl2IGlkPSJodG1sX2E3NTU2NzkwODQxMDQ3Mjg5MjhlMzc0MTFhODliN2RmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gTGV5dG9uIChQYXJ0KSwgVXBwZXIgQ2xhcHRvbiwgTG93ZXIgQ2xhcHRvbiwgU3Rva2UgTmV3aW5ndG9uIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMGIwYjg1ZDY3YzRjNDQ0MjlkYWM2ODlmZDA4YTQzZWYuc2V0Q29udGVudChodG1sX2E3NTU2NzkwODQxMDQ3Mjg5MjhlMzc0MTFhODliN2RmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2U4ODkyYjE3ZmUwMDRmMzRiZTkzOTQ3ZmZhZGYzOGU4LmJpbmRQb3B1cChwb3B1cF8wYjBiODVkNjdjNGM0NDQyOWRhYzY4OWZkMDhhNDNlZik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85YWQ3NzIxNzA5ZmY0NWIwOGZmZjY2YzlhMDkwNGM5NyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUyNTUwNjgsMC4wNTg3MDgxMDAwMDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjOWI1OWI2IiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfY2FmYjRiZDIwNWUzNGIyOGE2NWEzOTVmY2UwNzVkMzEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNTVhMjU0MjI0Y2U2NDY3ZWJiYTRiNDViYzgwN2U0Y2IgPSAkKCc8ZGl2IGlkPSJodG1sXzU1YTI1NDIyNGNlNjQ2N2ViYmE0YjQ1YmM4MDdlNGNiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gRWFzdCBIYW0sIEJlY2t0b24sIFVwdG9uIFBhcmsgKHBhcnQpLCBCYXJraW5nIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfY2FmYjRiZDIwNWUzNGIyOGE2NWEzOTVmY2UwNzVkMzEuc2V0Q29udGVudChodG1sXzU1YTI1NDIyNGNlNjQ2N2ViYmE0YjQ1YmM4MDdlNGNiKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzlhZDc3MjE3MDlmZjQ1YjA4ZmZmNjZjOWEwOTA0Yzk3LmJpbmRQb3B1cChwb3B1cF9jYWZiNGJkMjA1ZTM0YjI4YTY1YTM5NWZjZTA3NWQzMSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9jYTFjZWIxYmQzNGM0MGNkYWNmZDMyYzllMzBhNzM1MiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU1MTgwOTQsMC4wMjkzNzI4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM5YjU5YjYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hODJiYjNiNzEzZGE0ODJhOWJhODdiZWNjZThmYmQ2YiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jZGZlMjQ0YTIyNTY0ZTVkODg5M2FkMDYxMDA5MzMwMiA9ICQoJzxkaXYgaWQ9Imh0bWxfY2RmZTI0NGEyMjU2NGU1ZDg4OTNhZDA2MTAwOTMzMDIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBGb3Jlc3QgR2F0ZSwgTGV5dG9uc3RvbmUgKFBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hODJiYjNiNzEzZGE0ODJhOWJhODdiZWNjZThmYmQ2Yi5zZXRDb250ZW50KGh0bWxfY2RmZTI0NGEyMjU2NGU1ZDg4OTNhZDA2MTAwOTMzMDIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfY2ExY2ViMWJkMzRjNDBjZGFjZmQzMmM5ZTMwYTczNTIuYmluZFBvcHVwKHBvcHVwX2E4MmJiM2I3MTNkYTQ4MmE5YmE4N2JlY2NlOGZiZDZiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q2NTVhYjgyMzhkNTRjNjVhOTdlMDEzYTA1MDI0ZjUzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTQzOTA4MywtMC4wNjE2ODYxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM5YjU5YjYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85MGM3MTliNDRhNTY0MzMwOWY5YzVmNTFjNjkwYjk0MiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83NGQ3ZGE2MmZlNjA0YjQ1OGU5MmJlMGVmNTY1YzFmNCA9ICQoJzxkaXYgaWQ9Imh0bWxfNzRkN2RhNjJmZTYwNGI0NThlOTJiZTBlZjU2NWMxZjQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBIYWNrbmV5IENlbnRyYWwsIERhbHN0b24sIExvbmRvbiBGaWVsZHMsIFN0b2tlIE5ld2luZ3RvbiAocGFydCksIENhbWJyaWRnZSBIZWF0aCAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzkwYzcxOWI0NGE1NjQzMzA5ZjljNWY1MWM2OTBiOTQyLnNldENvbnRlbnQoaHRtbF83NGQ3ZGE2MmZlNjA0YjQ1OGU5MmJlMGVmNTY1YzFmNCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kNjU1YWI4MjM4ZDU0YzY1YTk3ZTAxM2EwNTAyNGY1My5iaW5kUG9wdXAocG9wdXBfOTBjNzE5YjQ0YTU2NDMzMDlmOWM1ZjUxYzY5MGI5NDIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMDhkNmEzNzQxYmFjNGU2M2JjMDBkN2Q3NmMxYTE2N2IgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41NDEyODc5OTk5OTk5OSwtMC4wNDExMTE0MDAwMDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjOWI1OWI2IiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjdhYmExMTIyOWMzNDI3ODkzOTJkOTUyNDE4YjExZWUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNjBmOTViZDJmNjNkNDFjMjljZjczZjkxYjYyMDk5OTkgPSAkKCc8ZGl2IGlkPSJodG1sXzYwZjk1YmQyZjYzZDQxYzI5Y2Y3M2Y5MWI2MjA5OTk5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gSG9tZXJ0b24sIEhhY2tuZXkgV2ljaywgU291dGggSGFja25leSwgSGFja25leSBNYXJzaGVzLCBDYW1icmlkZ2UgSGVhdGggKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iN2FiYTExMjI5YzM0Mjc4OTM5MmQ5NTI0MThiMTFlZS5zZXRDb250ZW50KGh0bWxfNjBmOTViZDJmNjNkNDFjMjljZjczZjkxYjYyMDk5OTkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDhkNmEzNzQxYmFjNGU2M2JjMDBkN2Q3NmMxYTE2N2IuYmluZFBvcHVwKHBvcHVwX2I3YWJhMTEyMjljMzQyNzg5MzkyZDk1MjQxOGIxMWVlKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzVmNjU1NTgzZjg1MDQ1M2ZiYmQyM2FlZGJjY2JhZWFiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTY0OTYxOSwtMC4wMTQ2OTExXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM5YjU5YjYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lMTIwYjhiMzczODc0ZmE2OTQyMTRhOTk0M2Q4MTY5ZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF81YWE2ZjFiYmU0YmY0ODI5YTY1ZWUyZTQyOGU4OTU3ZSA9ICQoJzxkaXYgaWQ9Imh0bWxfNWFhNmYxYmJlNGJmNDgyOWE2NWVlMmU0MjhlODk1N2UiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBMZXl0b24sIFRlbXBsZSBNaWxscywgSGFja25leSBNYXJzaGVzIChwYXJ0KSBVcHBlciBDbGFwdG9uIChwYXJ0KSwgV2FsdGhhbXN0b3cgTWFyc2hlczwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZTEyMGI4YjM3Mzg3NGZhNjk0MjE0YTk5NDNkODE2OWQuc2V0Q29udGVudChodG1sXzVhYTZmMWJiZTRiZjQ4MjlhNjVlZTJlNDI4ZTg5NTdlKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzVmNjU1NTgzZjg1MDQ1M2ZiYmQyM2FlZGJjY2JhZWFiLmJpbmRQb3B1cChwb3B1cF9lMTIwYjhiMzczODc0ZmE2OTQyMTRhOTk0M2Q4MTY5ZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82NGEwNGUyNDg5ODI0Nzc0ODZjYzNlZjM3NTJkNjNkZCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU3Mjg1MjUsMC4wMTc2MzQ4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM5YjU5YjYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9iNTc2NDY1ZjUyZjE0NmQ1OGUzODhkOTcxZmExODJjNyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9lMmEzOWMwYzIxZDQ0ZmZhOTdlNDk1NDQyMWRlOGJhZCA9ICQoJzxkaXYgaWQ9Imh0bWxfZTJhMzljMGMyMWQ0NGZmYTk3ZTQ5NTQ0MjFkZThiYWQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBMZXl0b25zdG9uZSwgV2Fuc3RlYWQsIEFsZGVyc2Jyb29rIChwYXJ0KSwgU25hcmVzYnJvb2ssIENhbm4gSGFsbDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYjU3NjQ2NWY1MmYxNDZkNThlMzg4ZDk3MWZhMTgyYzcuc2V0Q29udGVudChodG1sX2UyYTM5YzBjMjFkNDRmZmE5N2U0OTU0NDIxZGU4YmFkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzY0YTA0ZTI0ODk4MjQ3NzQ4NmNjM2VmMzc1MmQ2M2RkLmJpbmRQb3B1cChwb3B1cF9iNTc2NDY1ZjUyZjE0NmQ1OGUzODhkOTcxZmExODJjNyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82NjM2YjhlMGYyNDQ0YTkyYmY2ZDIzYWI4ZGE0NTNkZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU1NDQyOTUsMC4wNTU4Mjg4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM5YjU5YjYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83ZjQ1Y2UzOTRkZWQ0OTU0YWYyNzBkMzgzZDc4YzU1NyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84YmY2NDJiYzViYmI0Mjc4YmIzNTE2YzQ5YjIyZmQzMCA9ICQoJzxkaXYgaWQ9Imh0bWxfOGJmNjQyYmM1YmJiNDI3OGJiMzUxNmM0OWIyMmZkMzAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBNYW5vciBQYXJrLCBMaXR0bGUgSWxmb3JkLCBBbGRlcnNicm9vayAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzdmNDVjZTM5NGRlZDQ5NTRhZjI3MGQzODNkNzhjNTU3LnNldENvbnRlbnQoaHRtbF84YmY2NDJiYzViYmI0Mjc4YmIzNTE2YzQ5YjIyZmQzMCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82NjM2YjhlMGYyNDQ0YTkyYmY2ZDIzYWI4ZGE0NTNkZi5iaW5kUG9wdXAocG9wdXBfN2Y0NWNlMzk0ZGVkNDk1NGFmMjcwZDM4M2Q3OGM1NTcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWY2NjVmYTI2NjI4NDJmMTgxMWNmOTJmZmE2Yzg3MWMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MzA3NzUyOTk5OTk5OSwwLjAyOTM1MDZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzliNTliNiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzFkMzY4NTE3ZjE1ZTRiYWI5Y2M4NzhkOTRmNzMyMGRiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzUzYzY3ODM3NDgwZjQ1MzlhMDdlZTMxZTM3OTlmYjRhID0gJCgnPGRpdiBpZD0iaHRtbF81M2M2NzgzNzQ4MGY0NTM5YTA3ZWUzMWUzNzk5ZmI0YSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFBsYWlzdG93LCBXZXN0IEhhbSAocGFydCksIFVwdG9uIFBhcmsgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xZDM2ODUxN2YxNWU0YmFiOWNjODc4ZDk0ZjczMjBkYi5zZXRDb250ZW50KGh0bWxfNTNjNjc4Mzc0ODBmNDUzOWEwN2VlMzFlMzc5OWZiNGEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOWY2NjVmYTI2NjI4NDJmMTgxMWNmOTJmZmE2Yzg3MWMuYmluZFBvcHVwKHBvcHVwXzFkMzY4NTE3ZjE1ZTRiYWI5Y2M4NzhkOTRmNzMyMGRiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q0OGJmN2M4Y2QzMzRhZjk5NWQ0ZTU2ZDNjM2ZlYmViID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA5NzUwMiwtMC4wMTc1OTVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzliNTliNiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2FlMzNkNjhmNWNjYzQ0YmZhYzI0ZDk3ZDRjZjZiNTg3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2JhOGYxMTk0OWUxYTRiZTM5MWE1MDQ1ZmUwNjE5NzAxID0gJCgnPGRpdiBpZD0iaHRtbF9iYThmMTE5NDllMWE0YmUzOTFhNTA0NWZlMDYxOTcwMSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFBvcGxhciwgTGltZWhvdXNlLCBDYW5hcnkgV2hhcmYsIE1pbGx3YWxsLCBCbGFja3dhbGwsIEN1Yml0dCBUb3duLCBTb3V0aCBCcm9tbGV5LCBOb3J0aCBHcmVlbndpY2gsIExlYW1vdXRoPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hZTMzZDY4ZjVjY2M0NGJmYWMyNGQ5N2Q0Y2Y2YjU4Ny5zZXRDb250ZW50KGh0bWxfYmE4ZjExOTQ5ZTFhNGJlMzkxYTUwNDVmZTA2MTk3MDEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZDQ4YmY3YzhjZDMzNGFmOTk1ZDRlNTZkM2MzZmViZWIuYmluZFBvcHVwKHBvcHVwX2FlMzNkNjhmNWNjYzQ0YmZhYzI0ZDk3ZDRjZjZiNTg3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2E1YjY0YzRmYzBkODRiZTNiMzZlMmFmODFhZTk4MGQxID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTQxMjk1LDAuMDA1ODcwOV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjOWI1OWI2IiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZTM0N2YyZTdjNDg4NDQ3MGJiODQyNjU3NTZkZmI4ZTIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNzhkYjc4NWVlN2E2NDY3NDk3NTIwYjFiNGJiZTQzNzUgPSAkKCc8ZGl2IGlkPSJodG1sXzc4ZGI3ODVlZTdhNjQ2NzQ5NzUyMGIxYjRiYmU0Mzc1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gU3RyYXRmb3JkLCBXZXN0IEhhbSAocGFydCksIE1hcnlsYW5kLCBMZXl0b24gKHBhcnQpLCBMZXl0b25zdG9uZSAocGFydCkgVGVtcGxlIE1pbGxzIChwYXJ0KSwgSGFja25leSBXaWNrIChwYXJ0KSwgQm93IChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZTM0N2YyZTdjNDg4NDQ3MGJiODQyNjU3NTZkZmI4ZTIuc2V0Q29udGVudChodG1sXzc4ZGI3ODVlZTdhNjQ2NzQ5NzUyMGIxYjRiYmU0Mzc1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2E1YjY0YzRmYzBkODRiZTNiMzZlMmFmODFhZTk4MGQxLmJpbmRQb3B1cChwb3B1cF9lMzQ3ZjJlN2M0ODg0NDcwYmI4NDI2NTc1NmRmYjhlMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zMmY4YzliYTViM2I0NTdlYTJhNjA4ZjMzOTA1YTQ4NSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUwOTc0NzgsMC4wMjkzMjg1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM5YjU5YjYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xZGZlMjM4ZmJkYWQ0OWVhOGE1ODhlNWIxMTBhYzQ1NiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hNGMyNTNhODdiMTg0ZThiYjg4Mjk1MzkyNjc5YWRlOSA9ICQoJzxkaXYgaWQ9Imh0bWxfYTRjMjUzYTg3YjE4NGU4YmI4ODI5NTM5MjY3OWFkZTkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBDYW5uaW5nIFRvd24sIFNpbHZlcnRvd24sIFJveWFsIERvY2tzLCBOb3J0aCBXb29sd2ljaCwgQmVja3RvbiAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzFkZmUyMzhmYmRhZDQ5ZWE4YTU4OGU1YjExMGFjNDU2LnNldENvbnRlbnQoaHRtbF9hNGMyNTNhODdiMTg0ZThiYjg4Mjk1MzkyNjc5YWRlOSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zMmY4YzliYTViM2I0NTdlYTJhNjA4ZjMzOTA1YTQ4NS5iaW5kUG9wdXAocG9wdXBfMWRmZTIzOGZiZGFkNDllYThhNTg4ZTViMTEwYWM0NTYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYjY4YWM3OTA5YjZjNGZkYTlkZTllYzE4ZjVjODAwZGQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41OTAxNzY5LC0wLjAxNzM0MzddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzliNTliNiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzdlOWQ5MDU1OGE5NTQxMDdiNzAwMThkOThmYjhkMzExID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzZlOGM5MzY0MmVkZjQ3YjNhNjJmMDEzY2Q1OWJlNjA2ID0gJCgnPGRpdiBpZD0iaHRtbF82ZThjOTM2NDJlZGY0N2IzYTYyZjAxM2NkNTliZTYwNiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFdhbHRoYW1zdG93LCBVcHBlciBXYWx0aGFtc3RvdywgTGV5dG9uIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfN2U5ZDkwNTU4YTk1NDEwN2I3MDAxOGQ5OGZiOGQzMTEuc2V0Q29udGVudChodG1sXzZlOGM5MzY0MmVkZjQ3YjNhNjJmMDEzY2Q1OWJlNjA2KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2I2OGFjNzkwOWI2YzRmZGE5ZGU5ZWMxOGY1YzgwMGRkLmJpbmRQb3B1cChwb3B1cF83ZTlkOTA1NThhOTU0MTA3YjcwMDE4ZDk4ZmI4ZDMxMSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85Mjg4MTkwZTNhODU0MjI5ODY2ZWM2OTc3ODA2YmNmNSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU5MTI2NzEsMC4wMjY0NzIxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM5YjU5YjYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8zODYwYzhmZDE1YzQ0MzUyYTgxMzkwMmRkZjg0MTIwNCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83ZmNiYTZlNmRkZmI0NmE5YmIwM2Y5ZTQyMGMxMDVhNSA9ICQoJzxkaXYgaWQ9Imh0bWxfN2ZjYmE2ZTZkZGZiNDZhOWJiMDNmOWU0MjBjMTA1YTUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBXb29kZm9yZCwgU291dGggV29vZGZvcmQ8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzM4NjBjOGZkMTVjNDQzNTJhODEzOTAyZGRmODQxMjA0LnNldENvbnRlbnQoaHRtbF83ZmNiYTZlNmRkZmI0NmE5YmIwM2Y5ZTQyMGMxMDVhNSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85Mjg4MTkwZTNhODU0MjI5ODY2ZWM2OTc3ODA2YmNmNS5iaW5kUG9wdXAocG9wdXBfMzg2MGM4ZmQxNWM0NDM1MmE4MTM5MDJkZGY4NDEyMDQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNWNjODk3OTc2NDFiNGY1ZmFjN2QzZDQ3YmY5NjU1OTEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41NDM5MjQxLC0wLjAwODgwNzVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzliNTliNiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzJlYWUwYTU0NTQyODRjOWU5YjM1ZDZhZjk0YjhiNmRiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzUyNzRkMjQyODNmMzQzYTQ4NGRmNTEyN2NmZGRmNzBhID0gJCgnPGRpdiBpZD0iaHRtbF81Mjc0ZDI0MjgzZjM0M2E0ODRkZjUxMjdjZmRkZjcwYSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IE9seW1waWMgUGFyaywgJmFtcDsgcGFydHMgb2YgU3RyYXRmb3JkLCBIb21lcnRvbiwgTGV5dG9uLCBCb3c7PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yZWFlMGE1NDU0Mjg0YzllOWIzNWQ2YWY5NGI4YjZkYi5zZXRDb250ZW50KGh0bWxfNTI3NGQyNDI4M2YzNDNhNDg0ZGY1MTI3Y2ZkZGY3MGEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNWNjODk3OTc2NDFiNGY1ZmFjN2QzZDQ3YmY5NjU1OTEuYmluZFBvcHVwKHBvcHVwXzJlYWUwYTU0NTQyODRjOWU5YjM1ZDZhZjk0YjhiNmRiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2M0ZTM1MzFjOWJkNTQwODQ4NDI2OGQ5MWZlODMyMzhkID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTE4MjUxMywtMC4wOTkwODU4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIm9yYW5nZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzk4ZDU1M2VjZDlhMDRkZTY5YmNkZDk5OTdhZDg1NDk5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2VkZjVhNWYxN2U1MTQ5ZTU5YTUxNjkxZWI3YzAyYTQ5ID0gJCgnPGRpdiBpZD0iaHRtbF9lZGY1YTVmMTdlNTE0OWU1OWE1MTY5MWViN2MwMmE0OSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3QgQmFydGhvbG9tZXcmIzM5O3MgSG9zcGl0YWw8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzk4ZDU1M2VjZDlhMDRkZTY5YmNkZDk5OTdhZDg1NDk5LnNldENvbnRlbnQoaHRtbF9lZGY1YTVmMTdlNTE0OWU1OWE1MTY5MWViN2MwMmE0OSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9jNGUzNTMxYzliZDU0MDg0ODQyNjhkOTFmZTgzMjM4ZC5iaW5kUG9wdXAocG9wdXBfOThkNTUzZWNkOWEwNGRlNjliY2RkOTk5N2FkODU0OTkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNzZjODI1MDFlZjY2NDEwM2E1MmMwZWQ1YjhjYTczYmEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MjA4Nzg0OTk5OTk5OSwtMC4xMDA1NjQ3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIm9yYW5nZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzExYWU4MjYyMzQ2NTRiNDZiMDlmOGZmZjQxN2Q2ZTExID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzYxNjMwMTdkY2ViODQ0ODU4NWViMzU0YjU5ZjZkNGU4ID0gJCgnPGRpdiBpZD0iaHRtbF82MTYzMDE3ZGNlYjg0NDg1ODVlYjM1NGI1OWY2ZDRlOCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2xlcmtlbndlbGwsIEZhcnJpbmdkb248L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzExYWU4MjYyMzQ2NTRiNDZiMDlmOGZmZjQxN2Q2ZTExLnNldENvbnRlbnQoaHRtbF82MTYzMDE3ZGNlYjg0NDg1ODVlYjM1NGI1OWY2ZDRlOCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83NmM4MjUwMWVmNjY0MTAzYTUyYzBlZDViOGNhNzNiYS5iaW5kUG9wdXAocG9wdXBfMTFhZTgyNjIzNDY1NGI0NmIwOWY4ZmZmNDE3ZDZlMTEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNDY1ZDNlODZhMWRlNDU5MWI3NDg0MjI5NTE2YzRiYzkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MTk1NTc4LC0wLjEwNzkwODNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAib3JhbmdlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMjc1ZWM2MDQ1YWFmNDZkZmExMmFjNDcwYjY3MDI4MGYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNmI5NjcxY2VjODU4NGZmMDgzNjZjMDJlZjRhNjI1MzMgPSAkKCc8ZGl2IGlkPSJodG1sXzZiOTY3MWNlYzg1ODRmZjA4MzY2YzAyZWY0YTYyNTMzIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5IYXR0b24gR2FyZGVuPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yNzVlYzYwNDVhYWY0NmRmYTEyYWM0NzBiNjcwMjgwZi5zZXRDb250ZW50KGh0bWxfNmI5NjcxY2VjODU4NGZmMDgzNjZjMDJlZjRhNjI1MzMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDY1ZDNlODZhMWRlNDU5MWI3NDg0MjI5NTE2YzRiYzkuYmluZFBvcHVwKHBvcHVwXzI3NWVjNjA0NWFhZjQ2ZGZhMTJhYzQ3MGI2NzAyODBmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzk3YjYyYmQ0ZDFlMDQxZjg4ZTBiOWE0ZWI0ODZiZThhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTI0MTU4MywtMC4xMDcxOTExXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIm9yYW5nZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2EyZThkMjE1YzBjMzQzNTY5ZGZiYWZhZTdmNDE4ODIxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2MyZGMzMTc3OTUxMjRjMzRhMzUyZjBmMjVmODIzMDQyID0gJCgnPGRpdiBpZD0iaHRtbF9jMmRjMzE3Nzk1MTI0YzM0YTM1MmYwZjI1ZjgyMzA0MiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Rmluc2J1cnksIEZpbnNidXJ5IEVzdGF0ZSAod2VzdCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2EyZThkMjE1YzBjMzQzNTY5ZGZiYWZhZTdmNDE4ODIxLnNldENvbnRlbnQoaHRtbF9jMmRjMzE3Nzk1MTI0YzM0YTM1MmYwZjI1ZjgyMzA0Mik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85N2I2MmJkNGQxZTA0MWY4OGUwYjlhNGViNDg2YmU4YS5iaW5kUG9wdXAocG9wdXBfYTJlOGQyMTVjMGMzNDM1NjlkZmJhZmFlN2Y0MTg4MjEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOTBkYThkM2ZjMTY2NGUzZTg5ODcyZjEwNDg0OWJmZDkgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MjY3OTY5OTk5OTk5OSwtMC4wOTU0NDE1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIm9yYW5nZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2FmZjNkNzlhZWEwYTQ4ZmFiNTBjOGJkZTFjMDNkMGI3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzUxMDVmMmE1ZjVkYzQ4MDY4NjI2MWEzMzEzNDAyMzIzID0gJCgnPGRpdiBpZD0iaHRtbF81MTA1ZjJhNWY1ZGM0ODA2ODYyNjFhMzMxMzQwMjMyMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Rmluc2J1cnkgKGVhc3QpLCBNb29yZmllbGRzIEV5ZSBIb3NwaXRhbDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYWZmM2Q3OWFlYTBhNDhmYWI1MGM4YmRlMWMwM2QwYjcuc2V0Q29udGVudChodG1sXzUxMDVmMmE1ZjVkYzQ4MDY4NjI2MWEzMzEzNDAyMzIzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzkwZGE4ZDNmYzE2NjRlM2U4OTg3MmYxMDQ4NDliZmQ5LmJpbmRQb3B1cChwb3B1cF9hZmYzZDc5YWVhMGE0OGZhYjUwYzhiZGUxYzAzZDBiNyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84MjZmY2Y3MGNmNDk0ODE5OWE1ZmQ2Njk2ZjZmMjM1OSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUyMzUxNTMsLTAuMDkwMjg2ODk5OTk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAib3JhbmdlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZmJiNjY0ZDVmM2RkNDc4MmIxNGIxZTQxYTU4NWU3OTIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfODQwMWM0ODZmMDhiNGYyMGI2NGI0MjEyMDI5NjE4ZTMgPSAkKCc8ZGl2IGlkPSJodG1sXzg0MDFjNDg2ZjA4YjRmMjBiNjRiNDIxMjAyOTYxOGUzIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TdCBMdWtlJiMzOTtzLCBCdW5oaWxsIEZpZWxkczwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZmJiNjY0ZDVmM2RkNDc4MmIxNGIxZTQxYTU4NWU3OTIuc2V0Q29udGVudChodG1sXzg0MDFjNDg2ZjA4YjRmMjBiNjRiNDIxMjAyOTYxOGUzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzgyNmZjZjcwY2Y0OTQ4MTk5YTVmZDY2OTZmNmYyMzU5LmJpbmRQb3B1cChwb3B1cF9mYmI2NjRkNWYzZGQ0NzgyYjE0YjFlNDFhNTg1ZTc5Mik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yNmUyMzRhYmM5YjI0NjlhOTI2ZjU3Mjg5NTc4OTUxYiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUyNDE3OTM5OTk5OTk5LC0wLjA4MDczODNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAib3JhbmdlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjA5ODliZjQ3NWM1NGQ5YWE3NzczN2MwNWRmMjcyMjEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZjIwNTgwMjBmMTAwNGUxYTkzNDdiODYwNzE2NGNhMTIgPSAkKCc8ZGl2IGlkPSJodG1sX2YyMDU4MDIwZjEwMDRlMWE5MzQ3Yjg2MDcxNjRjYTEyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TaG9yZWRpdGNoPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iMDk4OWJmNDc1YzU0ZDlhYTc3NzM3YzA1ZGYyNzIyMS5zZXRDb250ZW50KGh0bWxfZjIwNTgwMjBmMTAwNGUxYTkzNDdiODYwNzE2NGNhMTIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMjZlMjM0YWJjOWIyNDY5YTkyNmY1NzI4OTU3ODk1MWIuYmluZFBvcHVwKHBvcHVwX2IwOTg5YmY0NzVjNTRkOWFhNzc3MzdjMDVkZjI3MjIxKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2M3ZmU3NjM0YzQwYjQ3MjBiYzg2OTk0YzQ0Y2RkNzllID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTE4MjY0OSwtMC4wODE0NTU2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIm9yYW5nZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzViNzhmNTE1ZTZhYzRmMTdiZGI2YTJkNzUzZDc5ODg1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzJjNzU2MWE2NDk0NzQ2ZmM5MTRkN2EzMDc4NjVkODc4ID0gJCgnPGRpdiBpZD0iaHRtbF8yYzc1NjFhNjQ5NDc0NmZjOTE0ZDdhMzA3ODY1ZDg3OCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QnJvYWRnYXRlLCBMaXZlcnBvb2wgU3RyZWV0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81Yjc4ZjUxNWU2YWM0ZjE3YmRiNmEyZDc1M2Q3OTg4NS5zZXRDb250ZW50KGh0bWxfMmM3NTYxYTY0OTQ3NDZmYzkxNGQ3YTMwNzg2NWQ4NzgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYzdmZTc2MzRjNDBiNDcyMGJjODY5OTRjNDRjZGQ3OWUuYmluZFBvcHVwKHBvcHVwXzViNzhmNTE1ZTZhYzRmMTdiZGI2YTJkNzUzZDc5ODg1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzJlMDhiMWRjZTJmOTRlOTI4YTZhOTIyNTg1MWIxM2MwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTE1OTY0MywtMC4wODI1NTA1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIm9yYW5nZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzQyYWIxOWNlYzQwMDQ0NTU5MmRlYWIyOTFlNDMwMWRmID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzczM2QxZjEyNTMxMTQ5ZTZiMzBhZWY1ZWRhNDY2ZWNiID0gJCgnPGRpdiBpZD0iaHRtbF83MzNkMWYxMjUzMTE0OWU2YjMwYWVmNWVkYTQ2NmVjYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+T2xkIEJyb2FkIFN0cmVldCwgVG93ZXIgNDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzQyYWIxOWNlYzQwMDQ0NTU5MmRlYWIyOTFlNDMwMWRmLnNldENvbnRlbnQoaHRtbF83MzNkMWYxMjUzMTE0OWU2YjMwYWVmNWVkYTQ2NmVjYik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yZTA4YjFkY2UyZjk0ZTkyOGE2YTkyMjU4NTFiMTNjMC5iaW5kUG9wdXAocG9wdXBfNDJhYjE5Y2VjNDAwNDQ1NTkyZGVhYjI5MWU0MzAxZGYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzgxOGE2OThiMDFjNGVmODkxNTAyNjI3OWY2OTcxOTEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MTU2MzI0MDAwMDAwMSwtMC4wODczMjM0XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIm9yYW5nZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzNjOTA3ZDEyOThmMTQ2ZGViNjJmNGU0Njk5N2UwZDNhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzkxMTVkZjhlMWM5ZjQ3YWM5NDE4ZWI5M2Q2OTRjMThmID0gJCgnPGRpdiBpZD0iaHRtbF85MTE1ZGY4ZTFjOWY0N2FjOTQxOGViOTNkNjk0YzE4ZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QmFuayBvZiBFbmdsYW5kPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8zYzkwN2QxMjk4ZjE0NmRlYjYyZjRlNDY5OTdlMGQzYS5zZXRDb250ZW50KGh0bWxfOTExNWRmOGUxYzlmNDdhYzk0MThlYjkzZDY5NGMxOGYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMzgxOGE2OThiMDFjNGVmODkxNTAyNjI3OWY2OTcxOTEuYmluZFBvcHVwKHBvcHVwXzNjOTA3ZDEyOThmMTQ2ZGViNjJmNGU0Njk5N2UwZDNhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzM5NzYyMzY1ZmE1MzQ3OTlhYjE1NzQ5MTM4MTMyYzViID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTE1NjI5LC0wLjA5MTczMDQ5OTk5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIm9yYW5nZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2UzZDcxZDgzODQ4NzQwYWFiNTE3NGExMTE5OGZjM2ViID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzgwYTY2MjRlNjcyNTQ2NWM4Yjc1OTZkYTlmM2EyMDY0ID0gJCgnPGRpdiBpZD0iaHRtbF84MGE2NjI0ZTY3MjU0NjVjOGI3NTk2ZGE5ZjNhMjA2NCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+R3VpbGRoYWxsPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lM2Q3MWQ4Mzg0ODc0MGFhYjUxNzRhMTExOThmYzNlYi5zZXRDb250ZW50KGh0bWxfODBhNjYyNGU2NzI1NDY1YzhiNzU5NmRhOWYzYTIwNjQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMzk3NjIzNjVmYTUzNDc5OWFiMTU3NDkxMzgxMzJjNWIuYmluZFBvcHVwKHBvcHVwX2UzZDcxZDgzODQ4NzQwYWFiNTE3NGExMTE5OGZjM2ViKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzcyNjhkMDk0ZjUyMDQ5YzhiYzQxYWIzYmUwZmY5ZjZiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTE5NTcxNSwtMC4wOTE3NDM0XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIm9yYW5nZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzkzNjhmNTVhZmM4ZDQzNGE5YWJkM2RkZGM1ODhjYzc3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzBlZWVlNmI1ZDljZTQ2MmRiYTgzMWNlODU3NDcwMmI4ID0gJCgnPGRpdiBpZD0iaHRtbF8wZWVlZTZiNWQ5Y2U0NjJkYmE4MzFjZTg1NzQ3MDJiOCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QmFyYmljYW48L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzkzNjhmNTVhZmM4ZDQzNGE5YWJkM2RkZGM1ODhjYzc3LnNldENvbnRlbnQoaHRtbF8wZWVlZTZiNWQ5Y2U0NjJkYmE4MzFjZTg1NzQ3MDJiOCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83MjY4ZDA5NGY1MjA0OWM4YmM0MWFiM2JlMGZmOWY2Yi5iaW5kUG9wdXAocG9wdXBfOTM2OGY1NWFmYzhkNDM0YTlhYmQzZGRkYzU4OGNjNzcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMmFlODBhNGZmZjE2NGNmODkxOTI5ZmE3NzE2ZTBjYjcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MTQzMjQ1LC0wLjA3ODUwNjhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAib3JhbmdlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNDU2NDMzOWI2OTFiNDVkOWFlNDM5ZmFkOTUyNzg5YzUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZjIxYTIwMmUxZTk0NDczOGExYzRhNWUwNjMzMDc0MmQgPSAkKCc8ZGl2IGlkPSJodG1sX2YyMWEyMDJlMWU5NDQ3MzhhMWM0YTVlMDYzMzA3NDJkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TdCBNYXJ5IEF4ZSwgQWxkZ2F0ZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNDU2NDMzOWI2OTFiNDVkOWFlNDM5ZmFkOTUyNzg5YzUuc2V0Q29udGVudChodG1sX2YyMWEyMDJlMWU5NDQ3MzhhMWM0YTVlMDYzMzA3NDJkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzJhZTgwYTRmZmYxNjRjZjg5MTkyOWZhNzcxNmUwY2I3LmJpbmRQb3B1cChwb3B1cF80NTY0MzM5YjY5MWI0NWQ5YWU0MzlmYWQ5NTI3ODljNSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85YTAxNTE5NjZmNDk0ODM4ODI5YjY1ZGVkNzllNTM4NCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxMjAyMzYsLTAuMDgwMzM1OTk5OTk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAib3JhbmdlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNTZmOWFlY2ZhM2U5NDUyMTgxMDVkNjA5NWQxYjVjMjkgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfM2M3NzE0MmEzNTNkNGZmMGE3ZTUxMjhhNzliY2QwODggPSAkKCc8ZGl2IGlkPSJodG1sXzNjNzcxNDJhMzUzZDRmZjBhN2U1MTI4YTc5YmNkMDg4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5MbG95ZCYjMzk7cyBvZiBMb25kb24sIEZlbmNodXJjaCBTdHJlZXQ8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzU2ZjlhZWNmYTNlOTQ1MjE4MTA1ZDYwOTVkMWI1YzI5LnNldENvbnRlbnQoaHRtbF8zYzc3MTQyYTM1M2Q0ZmYwYTdlNTEyOGE3OWJjZDA4OCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85YTAxNTE5NjZmNDk0ODM4ODI5YjY1ZGVkNzllNTM4NC5iaW5kUG9wdXAocG9wdXBfNTZmOWFlY2ZhM2U5NDUyMTgxMDVkNjA5NWQxYjVjMjkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNjFhYmM2ZDIxNWI0NDVjODhlMGM3ZjVkNGYxNTk5ZjYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MTAzODUxLC0wLjA3NDA5MDddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAib3JhbmdlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNmM2NjhjZjEwYjMyNGM0MzgxNTRkZmZhMjQwNTJkYWQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZjk5YzgyMzI2OWY0NGUzMGJmNWI0YTE3NjBmZThlOWYgPSAkKCc8ZGl2IGlkPSJodG1sX2Y5OWM4MjMyNjlmNDRlMzBiZjViNGExNzYwZmU4ZTlmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Ub3dlciBIaWxsLCBUb3dlciBvZiBMb25kb248L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzZjNjY4Y2YxMGIzMjRjNDM4MTU0ZGZmYTI0MDUyZGFkLnNldENvbnRlbnQoaHRtbF9mOTljODIzMjY5ZjQ0ZTMwYmY1YjRhMTc2MGZlOGU5Zik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82MWFiYzZkMjE1YjQ0NWM4OGUwYzdmNWQ0ZjE1OTlmNi5iaW5kUG9wdXAocG9wdXBfNmM2NjhjZjEwYjMyNGM0MzgxNTRkZmZhMjQwNTJkYWQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfODAxZmU4MTE4MDA0NGZkYmExNzNjNjE1MDdjZjg5NTggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MDkwNjgzLC0wLjA3ODQ5MTk5OTk5OTk5OTk5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIm9yYW5nZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzBiMGRmYTBjZGU4YTQyYTg5MTAzOGUxOWFlMGE0ZjIyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2MxNWIzMGM2ZDA4MzQyZjY4OWRhYjZiYmIxMGQwNGU0ID0gJCgnPGRpdiBpZD0iaHRtbF9jMTViMzBjNmQwODM0MmY2ODlkYWI2YmJiMTBkMDRlNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TW9udW1lbnQsIEJpbGxpbmdzZ2F0ZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMGIwZGZhMGNkZThhNDJhODkxMDM4ZTE5YWUwYTRmMjIuc2V0Q29udGVudChodG1sX2MxNWIzMGM2ZDA4MzQyZjY4OWRhYjZiYmIxMGQwNGU0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzgwMWZlODExODAwNDRmZGJhMTczYzYxNTA3Y2Y4OTU4LmJpbmRQb3B1cChwb3B1cF8wYjBkZmEwY2RlOGE0MmE4OTEwMzhlMTlhZTBhNGYyMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84ZGJkNmI5ZWJmMDE0ZThiYjUyZjFlY2U5MGI5MWE4MyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxMzAwNjMsLTAuMDg0Mzc3N10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJvcmFuZ2UiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF82OWI1Mzk1NTI3ZTg0MDM3OGRlMzI5NjhhY2ZmZWI5ZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83ODQ2MmQ0OGVhMzY0YzJmYTEyNjg3ZjkzMDdlY2ZlMCA9ICQoJzxkaXYgaWQ9Imh0bWxfNzg0NjJkNDhlYTM2NGMyZmExMjY4N2Y5MzA3ZWNmZTAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNvcm5oaWxsLCBHcmFjZWNodXJjaCBTdHJlZXQsIExvbWJhcmQgU3RyZWV0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82OWI1Mzk1NTI3ZTg0MDM3OGRlMzI5NjhhY2ZmZWI5ZC5zZXRDb250ZW50KGh0bWxfNzg0NjJkNDhlYTM2NGMyZmExMjY4N2Y5MzA3ZWNmZTApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOGRiZDZiOWViZjAxNGU4YmI1MmYxZWNlOTBiOTFhODMuYmluZFBvcHVwKHBvcHVwXzY5YjUzOTU1MjdlODQwMzc4ZGUzMjk2OGFjZmZlYjlkKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q4OTlmZTIyYjU4YzRkZWJhZGE1ZWIyODA3MzVkMjI5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTE1NjE2NTk5OTk5OTksLTAuMTA2NDIzNV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJvcmFuZ2UiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xMzBkNjc4ZDc4MmY0MjE4ODBmOTIyMTQzOTU5ZjIyMyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF82ZDE2NDhhNzM0NzM0NGE4YWY4ZGFkMjI5ZjYxMTUzOSA9ICQoJzxkaXYgaWQ9Imh0bWxfNmQxNjQ4YTczNDczNDRhOGFmOGRhZDIyOWY2MTE1MzkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkZldHRlciBMYW5lPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xMzBkNjc4ZDc4MmY0MjE4ODBmOTIyMTQzOTU5ZjIyMy5zZXRDb250ZW50KGh0bWxfNmQxNjQ4YTczNDczNDRhOGFmOGRhZDIyOWY2MTE1MzkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZDg5OWZlMjJiNThjNGRlYmFkYTVlYjI4MDczNWQyMjkuYmluZFBvcHVwKHBvcHVwXzEzMGQ2NzhkNzgyZjQyMTg4MGY5MjIxNDM5NTlmMjIzKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQxM2FlYzZhOGQ1NTQ2NTg5MzdlYjU2YTBjOWZlMjMzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTE0MzEwMiwtMC4wOTc2MDI2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIm9yYW5nZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzAzYTk2YjY5NGIyODRiODg4MTBiNGRlMTgxZTk0Yjk0ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzRkY2EyMmZlMzIzZTRjNWY5ZmVmYmNiOGU5NjAzZmM0ID0gJCgnPGRpdiBpZD0iaHRtbF80ZGNhMjJmZTMyM2U0YzVmOWZlZmJjYjhlOTYwM2ZjNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3QgUGF1bCYjMzk7czwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMDNhOTZiNjk0YjI4NGI4ODgxMGI0ZGUxODFlOTRiOTQuc2V0Q29udGVudChodG1sXzRkY2EyMmZlMzIzZTRjNWY5ZmVmYmNiOGU5NjAzZmM0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzQxM2FlYzZhOGQ1NTQ2NTg5MzdlYjU2YTBjOWZlMjMzLmJpbmRQb3B1cChwb3B1cF8wM2E5NmI2OTRiMjg0Yjg4ODEwYjRkZTE4MWU5NGI5NCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl83NTI1MDVjYTU3Yzk0NDJiYjE3NjRhYTE5OTA1M2Q0MSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxMjAxNzgsLTAuMDg4NDEzN10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJvcmFuZ2UiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF82ZDRhODIyZjY4ZDI0MThkYmY0M2YxNWJmMjg1MzU1MSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mYTA2OTQwMDE0ZDY0NTI1OTY3NTE4Mjk0YzM0YTA4MiA9ICQoJzxkaXYgaWQ9Imh0bWxfZmEwNjk0MDAxNGQ2NDUyNTk2NzUxODI5NGMzNGEwODIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk1hbnNpb24gSG91c2U8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzZkNGE4MjJmNjhkMjQxOGRiZjQzZjE1YmYyODUzNTUxLnNldENvbnRlbnQoaHRtbF9mYTA2OTQwMDE0ZDY0NTI1OTY3NTE4Mjk0YzM0YTA4Mik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83NTI1MDVjYTU3Yzk0NDJiYjE3NjRhYTE5OTA1M2Q0MS5iaW5kUG9wdXAocG9wdXBfNmQ0YTgyMmY2OGQyNDE4ZGJmNDNmMTViZjI4NTM1NTEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMGM3ZTNlYzRkMWNiNDBhYmFhYjhiYmJkMmI0YjIzZDcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MTAzNzM5LC0wLjA5MDI0NDRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAib3JhbmdlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNmM5ZGRiN2QxODcyNGYzYzgzZjc4MWMyNWJjODU1MTYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfN2MyOGJlMzQ3ODc2NGMzZTlmMDIxMWQ0ZTVlNWJkNTYgPSAkKCc8ZGl2IGlkPSJodG1sXzdjMjhiZTM0Nzg3NjRjM2U5ZjAyMTFkNGU1ZTViZDU2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5DYW5ub24gU3RyZWV0PC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82YzlkZGI3ZDE4NzI0ZjNjODNmNzgxYzI1YmM4NTUxNi5zZXRDb250ZW50KGh0bWxfN2MyOGJlMzQ3ODc2NGMzZTlmMDIxMWQ0ZTVlNWJkNTYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMGM3ZTNlYzRkMWNiNDBhYmFhYjhiYmJkMmI0YjIzZDcuYmluZFBvcHVwKHBvcHVwXzZjOWRkYjdkMTg3MjRmM2M4M2Y3ODFjMjViYzg1NTE2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzFkYmJiOGIwZjA4MzQzYzg4YWM1YTlkNTIyNjZhMjM3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTExNjgyLC0wLjA5NzU5MzRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAib3JhbmdlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMDFiNWFlNmZlNmYzNDFlYzkyMWI3YTU2ZTI0MDllZTIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZGUyNWMwMDAxNTRhNDI5Nzk3M2ZhNTU3YWU0NWU5MGQgPSAkKCc8ZGl2IGlkPSJodG1sX2RlMjVjMDAwMTU0YTQyOTc5NzNmYTU1N2FlNDVlOTBkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5CbGFja2ZyaWFyczwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMDFiNWFlNmZlNmYzNDFlYzkyMWI3YTU2ZTI0MDllZTIuc2V0Q29udGVudChodG1sX2RlMjVjMDAwMTU0YTQyOTc5NzNmYTU1N2FlNDVlOTBkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzFkYmJiOGIwZjA4MzQzYzg4YWM1YTlkNTIyNjZhMjM3LmJpbmRQb3B1cChwb3B1cF8wMWI1YWU2ZmU2ZjM0MWVjOTIxYjdhNTZlMjQwOWVlMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yNTc0MTQ5NGQwMzE0MjViYjI0ZmIxNDI1ZDc4OTg5YSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxMTY3NDQsLTAuMTA2NDA4NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJvcmFuZ2UiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF82MDgzNjdkODk0ZGM0NzEzYTZmNDMwZTQ5NTAwYjQxOCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8xZWU3NDA3YzY0ZTg0YTUxYWIxM2QyMmE5YmIyOWQxNCA9ICQoJzxkaXYgaWQ9Imh0bWxfMWVlNzQwN2M2NGU4NGE1MWFiMTNkMjJhOWJiMjlkMTQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlRlbXBsZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNjA4MzY3ZDg5NGRjNDcxM2E2ZjQzMGU0OTUwMGI0MTguc2V0Q29udGVudChodG1sXzFlZTc0MDdjNjRlODRhNTFhYjEzZDIyYTliYjI5ZDE0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzI1NzQxNDk0ZDAzMTQyNWJiMjRmYjE0MjVkNzg5ODlhLmJpbmRQb3B1cChwb3B1cF82MDgzNjdkODk0ZGM0NzEzYTZmNDMwZTQ5NTAwYjQxOCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iODRkMTYxOGZlMzg0MmQwYTFmYWZiYzRmYjcwZjA2ZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUzMDY4NzIsLTAuMTQ2OTMyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMzNDk4ZGIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzM0NDk1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lYzdkNDgyMmExYzA0N2NkODU1MTNkYWU0ZTNhNDdhZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8xOWYxYzdhZjZlNTE0NTZlYjM0YTI2M2JjMDRiOTM1ZCA9ICQoJzxkaXYgaWQ9Imh0bWxfMTlmMWM3YWY2ZTUxNDU2ZWIzNGEyNjNiYzA0YjkzNWQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBNYXJ5bGVib25lIChwYXJ0KSwgRXVzdG9uLCBSZWdlbnQmIzM5O3MgUGFyaywgQmFrZXIgU3RyZWV0LCBDYW1kZW4gVG93biwgIFNvbWVycyBUb3duLCBQcmltcm9zZSBIaWxsIChwYXJ0KSBhbmQgTGlzc29uIEdyb3ZlIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZWM3ZDQ4MjJhMWMwNDdjZDg1NTEzZGFlNGUzYTQ3YWQuc2V0Q29udGVudChodG1sXzE5ZjFjN2FmNmU1MTQ1NmViMzRhMjYzYmMwNGI5MzVkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2I4NGQxNjE4ZmUzODQyZDBhMWZhZmJjNGZiNzBmMDZlLmJpbmRQb3B1cChwb3B1cF9lYzdkNDgyMmExYzA0N2NkODU1MTNkYWU0ZTNhNDdhZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wNDkyMzExMDI2ZGM0ZDk3YmVlMGUxZWIxZWE5MDhiMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU2MjEwODUsLTAuMjI5NjY4M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMzQ5OGRiIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzNDQ5NWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZWYyZGYwZmYzNTk4NDI5ZmFkMTdmOTEyMmI0MGZiOGIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZmQ3ZjdjYjFmMDhiNGI1ODkxN2Q2NjM5MmM1ZDJhNWMgPSAkKCc8ZGl2IGlkPSJodG1sX2ZkN2Y3Y2IxZjA4YjRiNTg5MTdkNjYzOTJjNWQyYTVjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQ3JpY2tsZXdvb2QsIERvbGxpcyBIaWxsLCBDaGlsZHMgSGlsbCwgR29sZGVycyBHcmVlbiAocGFydCksIEJyZW50IENyb3NzIChwYXJ0KSwgV2lsbGVzZGVuIChub3J0aCksIE5lYXNkZW4gKG5vcnRoKTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZWYyZGYwZmYzNTk4NDI5ZmFkMTdmOTEyMmI0MGZiOGIuc2V0Q29udGVudChodG1sX2ZkN2Y3Y2IxZjA4YjRiNTg5MTdkNjYzOTJjNWQyYTVjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzA0OTIzMTEwMjZkYzRkOTdiZWUwZTFlYjFlYTkwOGIzLmJpbmRQb3B1cChwb3B1cF9lZjJkZjBmZjM1OTg0MjlmYWQxN2Y5MTIyYjQwZmI4Yik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84YWFjNjBlZDU3Yjk0MGNkOWUxNWRlMjk3OGMwZmViZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU1MTY4OTQsLTAuMTcwNjExMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMzQ5OGRiIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzNDQ5NWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMTkxMDIzYTk4OTQ1NDY5Zjg5MjJiOGRjODdmY2Y0ZWUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNDE4YWJkMGUzMmFhNGI0NGJiZTdkMDdjOTRiYWEzMjYgPSAkKCc8ZGl2IGlkPSJodG1sXzQxOGFiZDBlMzJhYTRiNDRiYmU3ZDA3Yzk0YmFhMzI2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gSGFtcHN0ZWFkLCBCZWxzaXplIFBhcmssIEZyb2duYWwsIENoaWxkcyBIaWxsIChlYXN0KSwgU291dGggSGFtcHN0ZWFkIChub3J0aCksIFN3aXNzIENvdHRhZ2UgKGVhc3QpLCBQcmltcm9zZSBIaWxsIChub3J0aCksIENoYWxrIEZhcm0gKHdlc3QpLCBHb3NwZWwgT2FrPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xOTEwMjNhOTg5NDU0NjlmODkyMmI4ZGM4N2ZjZjRlZS5zZXRDb250ZW50KGh0bWxfNDE4YWJkMGUzMmFhNGI0NGJiZTdkMDdjOTRiYWEzMjYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOGFhYzYwZWQ1N2I5NDBjZDllMTVkZTI5NzhjMGZlYmUuYmluZFBvcHVwKHBvcHVwXzE5MTAyM2E5ODk0NTQ2OWY4OTIyYjhkYzg3ZmNmNGVlKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzRhMjVkZWRiMzBhNTQyZGJiZWI1Njk3MzYyYTRmNzRlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTkzNjk5MywtMC4yMTgxMTA3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMzNDk4ZGIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzM0NDk1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yODljZjkyM2IyMjg0MDU5YTViOTExZGQ1NmRiOWNhMCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80ZTQxNjJiODZjMjA0MTMzYWI3ODc5ZGNjMDdhM2I4YyA9ICQoJzxkaXYgaWQ9Imh0bWxfNGU0MTYyYjg2YzIwNDEzM2FiNzg3OWRjYzA3YTNiOGMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBIZW5kb24sIEJyZW50IENyb3NzIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMjg5Y2Y5MjNiMjI4NDA1OWE1YjkxMWRkNTZkYjljYTAuc2V0Q29udGVudChodG1sXzRlNDE2MmI4NmMyMDQxMzNhYjc4NzlkY2MwN2EzYjhjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzRhMjVkZWRiMzBhNTQyZGJiZWI1Njk3MzYyYTRmNzRlLmJpbmRQb3B1cChwb3B1cF8yODljZjkyM2IyMjg0MDU5YTViOTExZGQ1NmRiOWNhMCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9jMzZjZTAyOTE2MmE0MTgxOGEwODJiM2Q4ZDYzY2I4ZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU1NDM1NDUsLTAuMTQ0MTExMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMzQ5OGRiIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzNDQ5NWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNzgxYzhmZmM0ZDc4NDlmYWFiM2M0YWY5MDJiNDQxMDQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYTk5OWRlNTY5MmYyNGFmZWJkNDM3MDg4ZTZmZjRlMmIgPSAkKCc8ZGl2IGlkPSJodG1sX2E5OTlkZTU2OTJmMjRhZmViZDQzNzA4OGU2ZmY0ZTJiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gS2VudGlzaCBUb3duLCBDYW1kZW4gVG93biAocGFydCksIEdvc3BlbCBPYWsgKHBhcnQpLCBEYXJ0bW91dGggUGFyaywgQ2hhbGsgRmFybSAoZWFzdCksIFR1Zm5lbGwgUGFyayAod2VzdCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzc4MWM4ZmZjNGQ3ODQ5ZmFhYjNjNGFmOTAyYjQ0MTA0LnNldENvbnRlbnQoaHRtbF9hOTk5ZGU1NjkyZjI0YWZlYmQ0MzcwODhlNmZmNGUyYik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9jMzZjZTAyOTE2MmE0MTgxOGEwODJiM2Q4ZDYzY2I4ZS5iaW5kUG9wdXAocG9wdXBfNzgxYzhmZmM0ZDc4NDlmYWFiM2M0YWY5MDJiNDQxMDQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOThhMTQ4ZDZhYjU4NDM4YzgzNzljMWJlNDZkMGFlMTAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41NDM3NTk0LC0wLjE5NzA4MzNdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzM0OThkYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzQ0OTVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzdmNmRlNTc1ZmVlYTQ0MDE4Y2ZjNjJlMzUzYjJlYWQ3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzZiMGUzM2MwMjA5ZDRiZWFhNmZlNDc2ODUzMGE4MWUzID0gJCgnPGRpdiBpZD0iaHRtbF82YjBlMzNjMDIwOWQ0YmVhYTZmZTQ3Njg1MzBhODFlMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEtpbGJ1cm4sIEJyb25kZXNidXJ5LCBXZXN0IEhhbXBzdGVhZCwgUXVlZW4mIzM5O3MgUGFyaywgS2Vuc2FsIEdyZWVuIChwYXJ0KSwgU291dGggSGFtcHN0ZWFkIChzb3V0aCksIFN3aXNzIENvdHRhZ2UgKHdlc3QpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83ZjZkZTU3NWZlZWE0NDAxOGNmYzYyZTM1M2IyZWFkNy5zZXRDb250ZW50KGh0bWxfNmIwZTMzYzAyMDlkNGJlYWE2ZmU0NzY4NTMwYTgxZTMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOThhMTQ4ZDZhYjU4NDM4YzgzNzljMWJlNDZkMGFlMTAuYmluZFBvcHVwKHBvcHVwXzdmNmRlNTc1ZmVlYTQ0MDE4Y2ZjNjJlMzUzYjJlYWQ3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzg4ZTA5ODY4MDIxODQ4MDdiZmNkN2VmOTdmZWQwYzUzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNjE0NzMwNTk5OTk5OTksLTAuMjMwMTAyXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMzNDk4ZGIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzM0NDk1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81MTlhYWI0MTA5ODE0NjVlODk2YzU0MDc1ODg2MDY1NiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83NjU3OTYyMzdlN2Q0NWEyOWUzODM4Yjk4NjJmZGYzMiA9ICQoJzxkaXYgaWQ9Imh0bWxfNzY1Nzk2MjM3ZTdkNDVhMjllMzgzOGI5ODYyZmRmMzIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBNaWxsIEhpbGwsIEFya2xleSAocGFydCksIEVkZ3dhcmUgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81MTlhYWI0MTA5ODE0NjVlODk2YzU0MDc1ODg2MDY1Ni5zZXRDb250ZW50KGh0bWxfNzY1Nzk2MjM3ZTdkNDVhMjllMzgzOGI5ODYyZmRmMzIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfODhlMDk4NjgwMjE4NDgwN2JmY2Q3ZWY5N2ZlZDBjNTMuYmluZFBvcHVwKHBvcHVwXzUxOWFhYjQxMDk4MTQ2NWU4OTZjNTQwNzU4ODYwNjU2KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQyZDViNTVlMDZiNTQzOWY5OWEwMmEyOTViYWQ0ODdmID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTMzMjgsLTAuMTczNDQzNV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMzQ5OGRiIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzNDQ5NWUiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMjY0MGMzY2UzZjRjNDI3Nzg2NGNlODZiZTg3ZDZhMGQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMjZlODcwMThjODcwNDhmNzgzOTAwOGU4OGM3Mjk3ZDggPSAkKCc8ZGl2IGlkPSJodG1sXzI2ZTg3MDE4Yzg3MDQ4Zjc4MzkwMDhlODhjNzI5N2Q4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gU3QgSm9obiYjMzk7cyBXb29kLCBQcmltcm9zZSBIaWxsIChzb3V0aCksIExpc3NvbiBHcm92ZSAobm9ydGgpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yNjQwYzNjZTNmNGM0Mjc3ODY0Y2U4NmJlODdkNmEwZC5zZXRDb250ZW50KGh0bWxfMjZlODcwMThjODcwNDhmNzgzOTAwOGU4OGM3Mjk3ZDgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDJkNWI1NWUwNmI1NDM5Zjk5YTAyYTI5NWJhZDQ4N2YuYmluZFBvcHVwKHBvcHVwXzI2NDBjM2NlM2Y0YzQyNzc4NjRjZTg2YmU4N2Q2YTBkKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Q3MTM5OThlMzU3ZjQ3Mzg4NmNmZjEzYjQ2YWJhNzNjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTgzMTAzOCwtMC4yNTM0NzY2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMzNDk4ZGIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzM0NDk1ZSIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8zZWE5ZjNhNjMwMWY0ZGRhYjc1YWUxNzU1MDVjZDY5OSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85OTQwMzM2MGM2OTg0ZDVhOWRjZWNkNjJiY2Y1OTUzZCA9ICQoJzxkaXYgaWQ9Imh0bWxfOTk0MDMzNjBjNjk4NGQ1YTlkY2VjZDYyYmNmNTk1M2QiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBUaGUgSHlkZSwgQ29saW5kYWxlLCBLaW5nc2J1cnksIFdlc3QgSGVuZG9uLCBXZW1ibGV5IFBhcmsgKHBhcnQpLCBRdWVlbnNidXJ5IChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfM2VhOWYzYTYzMDFmNGRkYWI3NWFlMTc1NTA1Y2Q2OTkuc2V0Q29udGVudChodG1sXzk5NDAzMzYwYzY5ODRkNWE5ZGNlY2Q2MmJjZjU5NTNkKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Q3MTM5OThlMzU3ZjQ3Mzg4NmNmZjEzYjQ2YWJhNzNjLmJpbmRQb3B1cChwb3B1cF8zZWE5ZjNhNjMwMWY0ZGRhYjc1YWUxNzU1MDVjZDY5OSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xZWYwYjQwNzg4OTY0ZDI3YWFiZDY3YmNjMzIxZTZjNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU0MTAyMjg5OTk5OTk5LC0wLjI1MzA5NDVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzM0OThkYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzQ0OTVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2JjOTBlM2NhZGI1YjRjMDhhNDM1OWJhY2QwN2E0OWM3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzUyNDA3YTMxYWFiZTQ4NGViZmRkODQ5ZTNhYWM3YmIwID0gJCgnPGRpdiBpZD0iaHRtbF81MjQwN2EzMWFhYmU0ODRlYmZkZDg0OWUzYWFjN2JiMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFdpbGxlc2RlbiwgSGFybGVzZGVuLCBLZW5zYWwgR3JlZW4sIEJyZW50IFBhcmssIENvbGxlZ2UgUGFyaywgU3RvbmVicmlkZ2UsIE5vcnRoIEFjdG9uIChwYXJ0KSwgV2VzdCBUd3lmb3JkLCBOZWFzZGVuIChzb3V0aCksIE9sZCBPYWsgQ29tbW9uLCBQYXJrIFJveWFsIChub3J0aCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2JjOTBlM2NhZGI1YjRjMDhhNDM1OWJhY2QwN2E0OWM3LnNldENvbnRlbnQoaHRtbF81MjQwN2EzMWFhYmU0ODRlYmZkZDg0OWUzYWFjN2JiMCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8xZWYwYjQwNzg4OTY0ZDI3YWFiZDY3YmNjMzIxZTZjNi5iaW5kUG9wdXAocG9wdXBfYmM5MGUzY2FkYjViNGMwOGE0MzU5YmFjZDA3YTQ5YzcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTBmNzBhMWNhMDYxNDQyNzgyNDJjZGY0N2UyMTk4MTcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41ODMyMTYyLC0wLjE5NDQxMDZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzM0OThkYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzQ0OTVlIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzgzYzFlM2UxZTg4YzRjYjhhYTI4M2E1MDA0Y2FjMTRhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzQwY2EwNDk3MzdlNjQ2MjNhNGRiZWM3ZmY4NGE4NDhhID0gJCgnPGRpdiBpZD0iaHRtbF80MGNhMDQ5NzM3ZTY0NjIzYTRkYmVjN2ZmODRhODQ4YSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEdvbGRlcnMgR3JlZW4sIFRlbXBsZSBGb3J0dW5lLCBIYW1wc3RlYWQgR2FyZGVuIFN1YnVyYiAod2VzdCksIEhlbmRvbiAocGFydCksIEJyZW50IENyb3NzIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfODNjMWUzZTFlODhjNGNiOGFhMjgzYTUwMDRjYWMxNGEuc2V0Q29udGVudChodG1sXzQwY2EwNDk3MzdlNjQ2MjNhNGRiZWM3ZmY4NGE4NDhhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2UwZjcwYTFjYTA2MTQ0Mjc4MjQyY2RmNDdlMjE5ODE3LmJpbmRQb3B1cChwb3B1cF84M2MxZTNlMWU4OGM0Y2I4YWEyODNhNTAwNGNhYzE0YSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lNTI1MDI0ZWQ5MjI0NDQ0ODE4Y2JkMTM5MTBlYTFlNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUwMzEwOTIsLTAuMTMwNjE4NF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMmI5NzUxNzVkMTk5NGI2MjgzYmYyY2FmYjY0YzRlMzAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZTYxY2JjMjc4NmE3NDRlZmEwNzk4OTQzNTQ3NzYyMGEgPSAkKCc8ZGl2IGlkPSJodG1sX2U2MWNiYzI3ODZhNzQ0ZWZhMDc5ODk0MzU0Nzc2MjBhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5XaGl0ZWhhbGwgYW5kIEJ1Y2tpbmdoYW0gUGFsYWNlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yYjk3NTE3NWQxOTk0YjYyODNiZjJjYWZiNjRjNGUzMC5zZXRDb250ZW50KGh0bWxfZTYxY2JjMjc4NmE3NDRlZmEwNzk4OTQzNTQ3NzYyMGEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTUyNTAyNGVkOTIyNDQ0NDgxOGNiZDEzOTEwZWExZTYuYmluZFBvcHVwKHBvcHVwXzJiOTc1MTc1ZDE5OTRiNjI4M2JmMmNhZmI2NGM0ZTMwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U1MDI1MjQ4OGVkZDQ4YjA5YTcwYjA0N2VlNjM1ODJjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDk4NTAxNiwtMC4xMzg2NzkxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xMmZjYWM1NjE5MmQ0Mjk5ODhlYTJjODc3MTk1NTYwMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9kMTEyNTY0OTlhMWM0OWZlOWNjOGNkZWMxOThmZTIyNCA9ICQoJzxkaXYgaWQ9Imh0bWxfZDExMjU2NDk5YTFjNDlmZTljYzhjZGVjMTk4ZmUyMjQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPmJldHdlZW4gQnVja2luZ2hhbSBHYXRlIGFuZCBWaWN0b3JpYSBTdGF0aW9uPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xMmZjYWM1NjE5MmQ0Mjk5ODhlYTJjODc3MTk1NTYwMi5zZXRDb250ZW50KGh0bWxfZDExMjU2NDk5YTFjNDlmZTljYzhjZGVjMTk4ZmUyMjQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTUwMjUyNDg4ZWRkNDhiMDlhNzBiMDQ3ZWU2MzU4MmMuYmluZFBvcHVwKHBvcHVwXzEyZmNhYzU2MTkyZDQyOTk4OGVhMmM4NzcxOTU1NjAyKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzZmNjQ3MmQ5ZDQ4MTQwMzM5NGRkMDhlNTAwMDBjMDM2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDk5ODIzOCwtMC4xMzEzMzc4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wNjhmNTUwZGJhNTI0Y2ViODZhMTAxNTcxNjAwMDc4OCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8yMDVjYzMzNGJlZTI0MzE5YTI5MDhmN2Y3ZDk4MWMxMCA9ICQoJzxkaXYgaWQ9Imh0bWxfMjA1Y2MzMzRiZWUyNDMxOWEyOTA4ZjdmN2Q5ODFjMTAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPmVhc3Qgb2YgQnVja2luZ2hhbSBHYXRlPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wNjhmNTUwZGJhNTI0Y2ViODZhMTAxNTcxNjAwMDc4OC5zZXRDb250ZW50KGh0bWxfMjA1Y2MzMzRiZWUyNDMxOWEyOTA4ZjdmN2Q5ODFjMTApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNmY2NDcyZDlkNDgxNDAzMzk0ZGQwOGU1MDAwMGMwMzYuYmluZFBvcHVwKHBvcHVwXzA2OGY1NTBkYmE1MjRjZWI4NmExMDE1NzE2MDAwNzg4KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzRmZjVlODczYjRhYjQzOTliNDQzM2MyZTg3YTNjMGQ2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDk1MjI2NSwtMC4xMzA1ODE1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9kMzdhMjA5YzRmOGE0NzZkYmY1OWJhMzIwY2NkMWViOCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85Y2UxNmQ2OTZiNjg0OGNmYTIxMjFjMWI0MmIyNmNiNSA9ICQoJzxkaXYgaWQ9Imh0bWxfOWNlMTZkNjk2YjY4NDhjZmEyMTIxYzFiNDJiMjZjYjUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPnRyaWFuZ3VsYXIgYXJlYSBiZXR3ZWVuIFZpY3RvcmlhIFN0YXRpb24sIHRoZSBIb3VzZXMgb2YgUGFybGlhbWVudCwgYW5kIFZhdXhoYWxsIEJyaWRnZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDM3YTIwOWM0ZjhhNDc2ZGJmNTliYTMyMGNjZDFlYjguc2V0Q29udGVudChodG1sXzljZTE2ZDY5NmI2ODQ4Y2ZhMjEyMWMxYjQyYjI2Y2I1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzRmZjVlODczYjRhYjQzOTliNDQzM2MyZTg3YTNjMGQ2LmJpbmRQb3B1cChwb3B1cF9kMzdhMjA5YzRmOGE0NzZkYmY1OWJhMzIwY2NkMWViOCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yYzYyZjU0ZmUyY2I0MDdjOTkwY2U4ZTk1MjJlNmMxZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ4OTk2MTY5OTk5OTk5LC0wLjEzOTM3MTJdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVmNjY4YzA3NGE0ZTRmMDBhY2FkYTkzMDBhZjYyZTBiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzIxMjAxNzUxZmZmNTRjYWZiYzE2NDc0Y2Q2NDcyMTFjID0gJCgnPGRpdiBpZD0iaHRtbF8yMTIwMTc1MWZmZjU0Y2FmYmMxNjQ3NGNkNjQ3MjExYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+dHJpYW5ndWxhciBhcmVhIGJldHdlZW4gVmF1eGhhbGwgQnJpZGdlLCBDaGVsc2VhIEJyaWRnZSwgYW5kIFZpY3RvcmlhIFN0YXRpb247IFBpbWxpY28gcHJvcGVyPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81ZjY2OGMwNzRhNGU0ZjAwYWNhZGE5MzAwYWY2MmUwYi5zZXRDb250ZW50KGh0bWxfMjEyMDE3NTFmZmY1NGNhZmJjMTY0NzRjZDY0NzIxMWMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMmM2MmY1NGZlMmNiNDA3Yzk5MGNlOGU5NTIyZTZjMWYuYmluZFBvcHVwKHBvcHVwXzVmNjY4YzA3NGE0ZTRmMDBhY2FkYTkzMDBhZjYyZTBiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzA3NzRhZTk4NmRjYTRkOGVhZThmMGUwOWQwN2YzMDkwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDkyNTc4MiwtMC4xNDgyMDExXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8zMGY1NDBlOWI5ZTI0NDZlYjA2MDg4YzFjNTQzMjMxYSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mM2FlZDFhYTk2OTU0YWUzYTc5MDcyZmQ2OWRkNzNjNCA9ICQoJzxkaXYgaWQ9Imh0bWxfZjNhZWQxYWE5Njk1NGFlM2E3OTA3MmZkNjlkZDczYzQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkJlbGdyYXZpYSwgQ2hlbHNlYSAocGFydCksIGFyZWEgYmV0d2VlbiBTbG9hbmUgU3F1YXJlIGFuZCBWaWN0b3JpYSBTdGF0aW9uLCBzb3V0aCBvZiBLaW5ncyBSb2FkPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8zMGY1NDBlOWI5ZTI0NDZlYjA2MDg4YzFjNTQzMjMxYS5zZXRDb250ZW50KGh0bWxfZjNhZWQxYWE5Njk1NGFlM2E3OTA3MmZkNjlkZDczYzQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDc3NGFlOTg2ZGNhNGQ4ZWFlOGYwZTA5ZDA3ZjMwOTAuYmluZFBvcHVwKHBvcHVwXzMwZjU0MGU5YjllMjQ0NmViMDYwODhjMWM1NDMyMzFhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2U2ZDdlYzhlM2YxMTQ4ODk5NTg0MDM1NjA3ZTJhN2E4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA3MzUwOSwtMC4xMjc3NTgzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85OTM0YmEyYjg5MTE0ZDAzYjc0YWRlZjI3ZWY5NGVkNCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8wN2Q0OGU0ODNkYWM0Yzc4ODRlNTUwN2NiZDY3ZDg5MyA9ICQoJzxkaXYgaWQ9Imh0bWxfMDdkNDhlNDgzZGFjNGM3ODg0ZTU1MDdjYmQ2N2Q4OTMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkJlbGdyYXZpYSwgbm9ydGggb2YgRWF0b24gU3F1YXJlLCBLbmlnaHRzYnJpZGdlIChwYXJ0KSwgQ2hlbHNlYSAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzk5MzRiYTJiODkxMTRkMDNiNzRhZGVmMjdlZjk0ZWQ0LnNldENvbnRlbnQoaHRtbF8wN2Q0OGU0ODNkYWM0Yzc4ODRlNTUwN2NiZDY3ZDg5Myk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lNmQ3ZWM4ZTNmMTE0ODg5OTU4NDAzNTYwN2UyYTdhOC5iaW5kUG9wdXAocG9wdXBfOTkzNGJhMmI4OTExNGQwM2I3NGFkZWYyN2VmOTRlZDQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNzA4MTlkNzE0Y2Q2NGVjYjhhNWFkYTRmZDdhNWZiZDAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MDc3MDU1LC0wLjEzMjg0NDhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzE5MjI1OTU2NmI0YTRmMGRhM2Y3YzdmYjc4YjlhNmYyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzJhZTBlZjNiMjljZDQ2NTE4YzNhYTc0ZTNlOWU2YTYyID0gJCgnPGRpdiBpZD0iaHRtbF8yYWUwZWYzYjI5Y2Q0NjUxOGMzYWE3NGUzZTllNmE2MiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U3QgSmFtZXMmIzM5O3M8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzE5MjI1OTU2NmI0YTRmMGRhM2Y3YzdmYjc4YjlhNmYyLnNldENvbnRlbnQoaHRtbF8yYWUwZWYzYjI5Y2Q0NjUxOGMzYWE3NGUzZTllNmE2Mik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83MDgxOWQ3MTRjZDY0ZWNiOGE1YWRhNGZkN2E1ZmJkMC5iaW5kUG9wdXAocG9wdXBfMTkyMjU5NTY2YjRhNGYwZGEzZjdjN2ZiNzhiOWE2ZjIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNTc5MGE1YjQxMjJhNDFhN2JiMDljNzEwMzY3YWE0ZGQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40NDkyNzM1LC0wLjEyMDEwMDRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVkMjM3YWJmYTkxMDQ1Yjg4ZGU4MGE4MThjNzU5MTFkID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzczN2I4NDI4ZDJkODQyZGRhMmYwYTI3ZDIzYjFjNDgxID0gJCgnPGRpdiBpZD0iaHRtbF83MzdiODQyOGQyZDg0MmRkYTJmMGEyN2QyM2IxYzQ4MSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEJyaXh0b24gSGlsbCwgVHVsc2UgSGlsbCwgQnJpeHRvbiwgU3RyZWF0aGFtIEhpbGwsIENsYXBoYW0gKHBhcnQpLCBFYXN0ZXJuIHBhcnRzIG9mIEJhbGhhbSwgTGFtYmV0aDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNWQyMzdhYmZhOTEwNDViODhkZTgwYTgxOGM3NTkxMWQuc2V0Q29udGVudChodG1sXzczN2I4NDI4ZDJkODQyZGRhMmYwYTI3ZDIzYjFjNDgxKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzU3OTBhNWI0MTIyYTQxYTdiYjA5YzcxMDM2N2FhNGRkLmJpbmRQb3B1cChwb3B1cF81ZDIzN2FiZmE5MTA0NWI4OGRlODBhODE4Yzc1OTExZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl82YWVkOTEzOGMxZWU0NTg5ODVmYjBlZDk2YWEyYjVmNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ5MTI0NzIsLTAuMTYxNDIxNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNWM0N2U1OWYyMDRiNDI2MTk0NDg2OGUyN2U5M2RhNmMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOTMwMDE0NDJkNTI3NDU4OWJjYWY4Y2ZmMWViNzM1NTcgPSAkKCc8ZGl2IGlkPSJodG1sXzkzMDAxNDQyZDUyNzQ1ODliY2FmOGNmZjFlYjczNTU3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQ2hlbHNlYSwgQnJvbXB0b24sIEtuaWdodHNicmlkZ2UgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81YzQ3ZTU5ZjIwNGI0MjYxOTQ0ODY4ZTI3ZTkzZGE2Yy5zZXRDb250ZW50KGh0bWxfOTMwMDE0NDJkNTI3NDU4OWJjYWY4Y2ZmMWViNzM1NTcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNmFlZDkxMzhjMWVlNDU4OTg1ZmIwZWQ5NmFhMmI1ZjYuYmluZFBvcHVwKHBvcHVwXzVjNDdlNTlmMjA0YjQyNjE5NDQ4NjhlMjdlOTNkYTZjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzdlZDA2M2M5M2FmMTRmZWE5MWQ2YzhhZWNmOTRmNTQ4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA3MzUwOSwtMC4xMjc3NTgzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81YzZhYjIxYTQyYzM0M2Y3ODBkY2UxZTFkZjQzN2IwNCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8wYzA5NGM5YmMxODI0MDMzOTY0NjViN2JkZDU4MzViMCA9ICQoJzxkaXYgaWQ9Imh0bWxfMGMwOTRjOWJjMTgyNDAzMzk2NDY1YjdiZGQ1ODM1YjAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBDbGFwaGFtLCBTdG9ja3dlbGwgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81YzZhYjIxYTQyYzM0M2Y3ODBkY2UxZTFkZjQzN2IwNC5zZXRDb250ZW50KGh0bWxfMGMwOTRjOWJjMTgyNDAzMzk2NDY1YjdiZGQ1ODM1YjApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfN2VkMDYzYzkzYWYxNGZlYTkxZDZjOGFlY2Y5NGY1NDguYmluZFBvcHVwKHBvcHVwXzVjNmFiMjFhNDJjMzQzZjc4MGRjZTFlMWRmNDM3YjA0KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzdmMTI4YTljZjFiNTQ1MDViMzAyMDQ2NTMxMDIyZmEzID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDkyNTE0NCwtMC4xOTIzMDg4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8zYTdjMzYwYzMyN2E0OTY1ODliODE2NjYwMGRhNmYzMSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8yOTcyZmFmZTMyNDA0MmRlOTFjYTQ0ODNjN2M5NTVjMCA9ICQoJzxkaXYgaWQ9Imh0bWxfMjk3MmZhZmUzMjQwNDJkZTkxY2E0NDgzYzdjOTU1YzAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBFYXJscyBDb3VydDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfM2E3YzM2MGMzMjdhNDk2NTg5YjgxNjY2MDBkYTZmMzEuc2V0Q29udGVudChodG1sXzI5NzJmYWZlMzI0MDQyZGU5MWNhNDQ4M2M3Yzk1NWMwKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzdmMTI4YTljZjFiNTQ1MDViMzAyMDQ2NTMxMDIyZmEzLmJpbmRQb3B1cChwb3B1cF8zYTdjMzYwYzMyN2E0OTY1ODliODE2NjYwMGRhNmYzMSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zN2Y0ODBhMWEwNjM0YjhiYjA5NmQ2NDhlZjE4MDkwOCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ3NTQyMTgsLTAuMjAyNDgyNV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZmM1ZTIxZmQ1MDUwNGI3NTk3YzY4MWNjYWUzY2FmZmQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNGNmMjEyZTVjYzQwNDUyZjlmNjY1ZGM4YTRlODE2NjUgPSAkKCc8ZGl2IGlkPSJodG1sXzRjZjIxMmU1Y2M0MDQ1MmY5ZjY2NWRjOGE0ZTgxNjY1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gRnVsaGFtLCBQYXJzb25zIEdyZWVuPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mYzVlMjFmZDUwNTA0Yjc1OTdjNjgxY2NhZTNjYWZmZC5zZXRDb250ZW50KGh0bWxfNGNmMjEyZTVjYzQwNDUyZjlmNjY1ZGM4YTRlODE2NjUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMzdmNDgwYTFhMDYzNGI4YmIwOTZkNjQ4ZWYxODA5MDguYmluZFBvcHVwKHBvcHVwX2ZjNWUyMWZkNTA1MDRiNzU5N2M2ODFjY2FlM2NhZmZkKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzU2MDBkNDMxOGFhMzQwYjE4MGUxYjY0Mzg2M2ZmMTg4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDk2NDg1MiwtMC4xNzMyMTVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2YxOWQ4YjJmZmMxMjQ5Nzk5N2VhODIwZmRlOWE5YTY1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2UwNmNiNTM0MWY3NTRjNTNiMWJiZjIyNzQ4ZWNiMjAyID0gJCgnPGRpdiBpZD0iaHRtbF9lMDZjYjUzNDFmNzU0YzUzYjFiYmYyMjc0OGVjYjIwMiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFNvdXRoIEtlbnNpbmd0b24sIEtuaWdodHNicmlkZ2UgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mMTlkOGIyZmZjMTI0OTc5OTdlYTgyMGZkZTlhOWE2NS5zZXRDb250ZW50KGh0bWxfZTA2Y2I1MzQxZjc1NGM1M2IxYmJmMjI3NDhlY2IyMDIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNTYwMGQ0MzE4YWEzNDBiMTgwZTFiNjQzODYzZmYxODguYmluZFBvcHVwKHBvcHVwX2YxOWQ4YjJmZmMxMjQ5Nzk5N2VhODIwZmRlOWE5YTY1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzY1N2E1MGIwYjMxMjQzN2NiODVlMzJiZDk4NTE3OTdjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDc1NTIyMSwtMC4xMzE5NTc2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85YWUxOTI2NDhkZDc0MmM4YjE4OTQwZWM2OTFiMjg1YyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8xZDZjNTlmMWJjNTk0MzhiYTA5YzA3YjYwMTIwNDg5YiA9ICQoJzxkaXYgaWQ9Imh0bWxfMWQ2YzU5ZjFiYzU5NDM4YmEwOWMwN2I2MDEyMDQ4OWIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBTb3V0aCBMYW1iZXRoLCBXYW5kc3dvcnRoIFJvYWQsIFZhdXhoYWxsLCBCYXR0ZXJzZWE6TmluZSBFbG1zIChwYXJ0KSwgQ2xhcGhhbSAocGFydCksIE5XIGFyZWEgb2YgU3RvY2t3ZWxsLCBPdmFsPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF85YWUxOTI2NDhkZDc0MmM4YjE4OTQwZWM2OTFiMjg1Yy5zZXRDb250ZW50KGh0bWxfMWQ2YzU5ZjFiYzU5NDM4YmEwOWMwN2I2MDEyMDQ4OWIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNjU3YTUwYjBiMzEyNDM3Y2I4NWUzMmJkOTg1MTc5N2MuYmluZFBvcHVwKHBvcHVwXzlhZTE5MjY0OGRkNzQyYzhiMTg5NDBlYzY5MWIyODVjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzdmOWQ5ZTU0NDhiMjQwNTY5NTZlM2Y2YjFhNWRkNzc5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDY3NjYzOSwtMC4xMTEzNzU3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83OTA4OGRmZjIyZjY0ZDhlYTg4MmFiZWU5YTE1N2Y2OCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83YjM3MDlmMmIzOGY0NzE5YjYzNjVhYTc0NzE0ZDcyNCA9ICQoJzxkaXYgaWQ9Imh0bWxfN2IzNzA5ZjJiMzhmNDcxOWI2MzY1YWE3NDcxNGQ3MjQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBCcml4dG9uLCBTdG9ja3dlbGwsIENsYXBoYW0gKHBhcnQpLCBPdmFsPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83OTA4OGRmZjIyZjY0ZDhlYTg4MmFiZWU5YTE1N2Y2OC5zZXRDb250ZW50KGh0bWxfN2IzNzA5ZjJiMzhmNDcxOWI2MzY1YWE3NDcxNGQ3MjQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfN2Y5ZDllNTQ0OGIyNDA1Njk1NmUzZjZiMWE1ZGQ3NzkuYmluZFBvcHVwKHBvcHVwXzc5MDg4ZGZmMjJmNjRkOGVhODgyYWJlZTlhMTU3ZjY4KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2Y5NDNmMzAyMGY4OTRjNjBiMDE1MDBhMGIwNjUyMWVlID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDgwNzEzNDAwMDAwMDEsLTAuMTc4OTk2NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNGQ2ZDRlYjFlMWU3NGRjNGFhZmJlYjcyMjYzMmMzZGMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNGUxM2VkZmZiN2UzNDk1M2I4ZmY4MTVjMjE4ZmY5YTYgPSAkKCc8ZGl2IGlkPSJodG1sXzRlMTNlZGZmYjdlMzQ5NTNiOGZmODE1YzIxOGZmOWE2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gV2VzdCBCcm9tcHRvbiwgQ2hlbHNlYSAod2VzdCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzRkNmQ0ZWIxZTFlNzRkYzRhYWZiZWI3MjI2MzJjM2RjLnNldENvbnRlbnQoaHRtbF80ZTEzZWRmZmI3ZTM0OTUzYjhmZjgxNWMyMThmZjlhNik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9mOTQzZjMwMjBmODk0YzYwYjAxNTAwYTBiMDY1MjFlZS5iaW5kUG9wdXAocG9wdXBfNGQ2ZDRlYjFlMWU3NGRjNGFhZmJlYjcyMjYzMmMzZGMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNGUwOGUxY2MwMjllNGUyNGFlOTNkMjVmZDc3YjU4MmQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40Njc2MSwtMC4xNTgzNDc3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lZDNiMTNlMGJkOGM0MGMzOGFiY2I0YmExZmEwMTc4ZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83ODhmN2RmZDFiOTQ0NzdkYWZmYjQ4MDBmOTFlYWZmZiA9ICQoJzxkaXYgaWQ9Imh0bWxfNzg4ZjdkZmQxYjk0NDc3ZGFmZmI0ODAwZjkxZWFmZmYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBCYXR0ZXJzZWEobW9zdGx5KSwgQ2xhcGhhbSBTb3V0aDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZWQzYjEzZTBiZDhjNDBjMzhhYmNiNGJhMWZhMDE3OGQuc2V0Q29udGVudChodG1sXzc4OGY3ZGZkMWI5NDQ3N2RhZmZiNDgwMGY5MWVhZmZmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzRlMDhlMWNjMDI5ZTRlMjRhZTkzZDI1ZmQ3N2I1ODJkLmJpbmRQb3B1cChwb3B1cF9lZDNiMTNlMGJkOGM0MGMzOGFiY2I0YmExZmEwMTc4ZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84ZjBiZTc1ZjRiYTM0MTdjYjI2NzVjOGQwZmNhZTI0MyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ0Mzk4ODksLTAuMTQ5NDEwNV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfY2QwZTdhZjFmZDNhNDU0MzljMDM4MzY4NmVmMzVkYjAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMDUyOTk0YTNmYjdjNDQwNGJiNDYwMmNmZWQ0YjdkODEgPSAkKCc8ZGl2IGlkPSJodG1sXzA1Mjk5NGEzZmI3YzQ0MDRiYjQ2MDJjZmVkNGI3ZDgxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQmFsaGFtLCBDbGFwaGFtIFNvdXRoLCBXYW5kc3dvcnRoIENvbW1vbiAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NkMGU3YWYxZmQzYTQ1NDM5YzAzODM2ODZlZjM1ZGIwLnNldENvbnRlbnQoaHRtbF8wNTI5OTRhM2ZiN2M0NDA0YmI0NjAyY2ZlZDRiN2Q4MSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84ZjBiZTc1ZjRiYTM0MTdjYjI2NzVjOGQwZmNhZTI0My5iaW5kUG9wdXAocG9wdXBfY2QwZTdhZjFmZDNhNDU0MzljMDM4MzY4NmVmMzVkYjApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMmVmZmY5MDQ1YjA1NDU3OTlhZmYyYjBmODFiN2I4M2MgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40Nzc5NzY0LC0wLjI0MDc0ODhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2Q1OTI5MTIyYTU1YjRlYWFhYzMyZGVmNTc5NWZkNTcxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2FkNjg4NTQ1YWE3NDRmYTI4N2I1YjE4ZWMxZjBjOWI5ID0gJCgnPGRpdiBpZD0iaHRtbF9hZDY4ODU0NWFhNzQ0ZmEyODdiNWIxOGVjMWYwYzliOSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEJhcm5lcywgQmFybmVzIEJyaWRnZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDU5MjkxMjJhNTViNGVhYWFjMzJkZWY1Nzk1ZmQ1NzEuc2V0Q29udGVudChodG1sX2FkNjg4NTQ1YWE3NDRmYTI4N2I1YjE4ZWMxZjBjOWI5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzJlZmZmOTA0NWIwNTQ1Nzk5YWZmMmIwZjgxYjdiODNjLmJpbmRQb3B1cChwb3B1cF9kNTkyOTEyMmE1NWI0ZWFhYWMzMmRlZjU3OTVmZDU3MSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mOGJlZGMyZjQzYTU0OGFmYjUxOWQwZDQ0YmM2NjA1MSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ2NDc4NjksLTAuMjY3MTE5Nl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZWMzODcxY2VlNTVmNDJhOWJlZjAxMWZmODNiOTFmNWYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNGM5ZjZiY2Q2OTc2NGU5ZjkzOTJhZTE0ZTA3ZDNhNDkgPSAkKCc8ZGl2IGlkPSJodG1sXzRjOWY2YmNkNjk3NjRlOWY5MzkyYWUxNGUwN2QzYTQ5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gTW9ydGxha2UsIEVhc3QgU2hlZW4sIENoaXN3aWNrIEJyaWRnZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZWMzODcxY2VlNTVmNDJhOWJlZjAxMWZmODNiOTFmNWYuc2V0Q29udGVudChodG1sXzRjOWY2YmNkNjk3NjRlOWY5MzkyYWUxNGUwN2QzYTQ5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Y4YmVkYzJmNDNhNTQ4YWZiNTE5ZDBkNDRiYzY2MDUxLmJpbmRQb3B1cChwb3B1cF9lYzM4NzFjZWU1NWY0MmE5YmVmMDExZmY4M2I5MWY1Zik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9jYTRlNTZlNzhlNjQ0NGFiOWZjZjE5YzM0NTA4OGIyMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ1Njk4OTUsLTAuMjI4ODA1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF84YTYwODI5NWQ3NWQ0NmQ1OWY5MjQ0NTQwYThjYTI0MiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jZGUwYmIyY2U3Njc0ZWUwYTM1NWU1NmFkMGU2NWRkMiA9ICQoJzxkaXYgaWQ9Imh0bWxfY2RlMGJiMmNlNzY3NGVlMGEzNTVlNTZhZDBlNjVkZDIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBQdXRuZXksIFJvZWhhbXB0b24sIEtpbmdzdG9uIFZhbGUsIFB1dG5leSBIZWF0aCwgUHV0bmV5IFZhbGUsIFJpY2htb25kIFBhcmssIFJvZWhhbXB0b24gVmFsZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOGE2MDgyOTVkNzVkNDZkNTlmOTI0NDU0MGE4Y2EyNDIuc2V0Q29udGVudChodG1sX2NkZTBiYjJjZTc2NzRlZTBhMzU1ZTU2YWQwZTY1ZGQyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2NhNGU1NmU3OGU2NDQ0YWI5ZmNmMTljMzQ1MDg4YjIxLmJpbmRQb3B1cChwb3B1cF84YTYwODI5NWQ3NWQ0NmQ1OWY5MjQ0NTQwYThjYTI0Mik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85Yzg3NjdmMmQ2YzE0NDAyYjI1NTZiMmRlNzczMmQyZCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQyMDM4OSwtMC4xMjg3NjY4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogImJsdWUiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hZDU3ODEyOTNjZmI0N2EyODRjOGZhZmYwNjJjMWY2ZiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84MDg2OWI3MDJkY2I0ODAwOGQ4OGU5M2M5N2FiMDhlMyA9ICQoJzxkaXYgaWQ9Imh0bWxfODA4NjliNzAyZGNiNDgwMDhkODhlOTNjOTdhYjA4ZTMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBTdHJlYXRoYW0sIFN0cmVhdGhhbSBDb21tb24sIE5vcmJ1cnksIFN0cmVhdGhhbSBQYXJrLCBGdXJ6ZWRvd24sIFN0cmVhdGhhbSBWYWxlLCBNaXRjaGFtIENvbW1vbiwgUG9sbGFyZHMgSGlsbCwgRWFzdGZpZWxkczwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYWQ1NzgxMjkzY2ZiNDdhMjg0YzhmYWZmMDYyYzFmNmYuc2V0Q29udGVudChodG1sXzgwODY5YjcwMmRjYjQ4MDA4ZDg4ZTkzYzk3YWIwOGUzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzljODc2N2YyZDZjMTQ0MDJiMjU1NmIyZGU3NzMyZDJkLmJpbmRQb3B1cChwb3B1cF9hZDU3ODEyOTNjZmI0N2EyODRjOGZhZmYwNjJjMWY2Zik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84NTBhOWU5MzJiZDM0MGM0OGY1NzM5OWUxYTQ1ODFkYyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQyNTYwMjI5OTk5OTk5LC0wLjE1ODEwOTddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiYmx1ZSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2ZhNTFkMzZkMGU5ODQ4MzA4NTFkOTdkMjY5NzU1OTgxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzRkMDQyYzRjYWEyZTRmNjc5ZDUzMzJhNzE3MmQ1Njk4ID0gJCgnPGRpdiBpZD0iaHRtbF80ZDA0MmM0Y2FhMmU0ZjY3OWQ1MzMyYTcxNzJkNTY5OCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFRvb3RpbmcsIE1pdGNoYW0gKHBhcnQpIEJhbGhhbSAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2ZhNTFkMzZkMGU5ODQ4MzA4NTFkOTdkMjY5NzU1OTgxLnNldENvbnRlbnQoaHRtbF80ZDA0MmM0Y2FhMmU0ZjY3OWQ1MzMyYTcxNzJkNTY5OCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84NTBhOWU5MzJiZDM0MGM0OGY1NzM5OWUxYTQ1ODFkYy5iaW5kUG9wdXAocG9wdXBfZmE1MWQzNmQwZTk4NDgzMDg1MWQ5N2QyNjk3NTU5ODEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTgxYTNhOTE2N2NiNDU2MTk0NjgzYzhlNGYzYzFhZWEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40NDY1NTAxLC0wLjE5MzQ2MV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNzA5MDhiZDFlMDkxNDM5OTg3ZDQ4YzFlNGFmMzNjODQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMDljMWE3NzU3NDE2NDliMmFkMmMyODI3ODlmOGE1ZjIgPSAkKCc8ZGl2IGlkPSJodG1sXzA5YzFhNzc1NzQxNjQ5YjJhZDJjMjgyNzg5ZjhhNWYyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gV2FuZHN3b3J0aCBUb3duLCBTb3V0aGZpZWxkcywgRWFybHNmaWVsZDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNzA5MDhiZDFlMDkxNDM5OTg3ZDQ4YzFlNGFmMzNjODQuc2V0Q29udGVudChodG1sXzA5YzFhNzc1NzQxNjQ5YjJhZDJjMjgyNzg5ZjhhNWYyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2U4MWEzYTkxNjdjYjQ1NjE5NDY4M2M4ZTRmM2MxYWVhLmJpbmRQb3B1cChwb3B1cF83MDkwOGJkMWUwOTE0Mzk5ODdkNDhjMWU0YWYzM2M4NCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zNjg2ZDE5NTQyYjk0YTgzYjAwMWQ3NDg3ODQ3NTNmYyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQyNTUyOTcsLTAuMjA1MDU2Nl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNmNhMGM2MDgxMmRhNGZhNGIzZDk4NjEyNjg5ZGNkMDEgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMmNhZjk3MmRlZDJmNGQwMmFkNzM4ZWZkNDZjZjZmYTMgPSAkKCc8ZGl2IGlkPSJodG1sXzJjYWY5NzJkZWQyZjRkMDJhZDczOGVmZDQ2Y2Y2ZmEzIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gV2ltYmxlZG9uLCBDb2xsaWVycyBXb29kLCBNZXJ0b24gUGFyaywgTWVydG9uIEFiYmV5LCBTb3V0aGZpZWxkcywgTW9yZGVuIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNmNhMGM2MDgxMmRhNGZhNGIzZDk4NjEyNjg5ZGNkMDEuc2V0Q29udGVudChodG1sXzJjYWY5NzJkZWQyZjRkMDJhZDczOGVmZDQ2Y2Y2ZmEzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzM2ODZkMTk1NDJiOTRhODNiMDAxZDc0ODc4NDc1M2ZjLmJpbmRQb3B1cChwb3B1cF82Y2EwYzYwODEyZGE0ZmE0YjNkOTg2MTI2ODlkY2QwMSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85MzI1Njc3NDE1ZGU0MGUzYWIxY2MxMmJlYjExZDFmMiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQwNzEyMjMsLTAuMjI1NDYxOF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICJibHVlIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZDJiODRmNzZiN2QzNDUyZjhjYWVkZjFmZTMyZWE4MDUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfY2MwN2UyNTU5NzI1NDY3ZmEzZDgyODlmNDFiMjQzZTggPSAkKCc8ZGl2IGlkPSJodG1sX2NjMDdlMjU1OTcyNTQ2N2ZhM2Q4Mjg5ZjQxYjI0M2U4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gUmF5bmVzIFBhcmssIExvd2VyIE1vcmRlbiwgTWVydG9uIFBhcmssIFdpbWJsZWRvbiBDaGFzZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDJiODRmNzZiN2QzNDUyZjhjYWVkZjFmZTMyZWE4MDUuc2V0Q29udGVudChodG1sX2NjMDdlMjU1OTcyNTQ2N2ZhM2Q4Mjg5ZjQxYjI0M2U4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzkzMjU2Nzc0MTVkZTQwZTNhYjFjYzEyYmViMTFkMWYyLmJpbmRQb3B1cChwb3B1cF9kMmI4NGY3NmI3ZDM0NTJmOGNhZWRmMWZlMzJlYTgwNSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wODcyZWE3Nzg0MzQ0MDEzOWNmMjU2OWZmYWYzNWU5MiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxODUzNTgsLTAuMTQyMDg3OF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfM2VmMWJlZjA3ZGZkNDNmZWJiODg3YmI2NDVkMmQ3MWYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMjkzZTc4YTExNmFkNDY5MGI3NWFlYWUzZDVhOGU1NTYgPSAkKCc8ZGl2IGlkPSJodG1sXzI5M2U3OGExMTZhZDQ2OTBiNzVhZWFlM2Q1YThlNTU2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Qb3J0bGFuZCBQbGFjZSwgUmVnZW50IFN0cmVldDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfM2VmMWJlZjA3ZGZkNDNmZWJiODg3YmI2NDVkMmQ3MWYuc2V0Q29udGVudChodG1sXzI5M2U3OGExMTZhZDQ2OTBiNzVhZWFlM2Q1YThlNTU2KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzA4NzJlYTc3ODQzNDQwMTM5Y2YyNTY5ZmZhZjM1ZTkyLmJpbmRQb3B1cChwb3B1cF8zZWYxYmVmMDdkZmQ0M2ZlYmI4ODdiYjY0NWQyZDcxZik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xNmY2NzNmNzcwNTM0ZWQxYmJhOTEwMzE5MWFkZjZjMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxNDc1MDYsLTAuMTQ3NzY3M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZjc4ZDA0YjQwZTkzNDk0NTk5YTUwMGEzZjExMjI0MzYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNWNkYzg1YTllNTFmNGJlMjliNDAyMjc4NjY5MjgzZDggPSAkKCc8ZGl2IGlkPSJodG1sXzVjZGM4NWE5ZTUxZjRiZTI5YjQwMjI3ODY2OTI4M2Q4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5PeGZvcmQgU3RyZWV0ICh3ZXN0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZjc4ZDA0YjQwZTkzNDk0NTk5YTUwMGEzZjExMjI0MzYuc2V0Q29udGVudChodG1sXzVjZGM4NWE5ZTUxZjRiZTI5YjQwMjI3ODY2OTI4M2Q4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzE2ZjY3M2Y3NzA1MzRlZDFiYmE5MTAzMTkxYWRmNmMxLmJpbmRQb3B1cChwb3B1cF9mNzhkMDRiNDBlOTM0OTQ1OTlhNTAwYTNmMTEyMjQzNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl81YmJhMjRmNGUyNjA0YTM5YWFkZjg5NDA4NTA4OWMxNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxNDI3ODksLTAuMTI5OTM1Nl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOTVlMTIxMzE1YTIyNDQ4ODg1NTA4ZjM0OGI4ZjE4NWIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNTUzY2RkMTc1MzE5NDJiOWE2ZWQxNjZhMmYwMGRiMjggPSAkKCc8ZGl2IGlkPSJodG1sXzU1M2NkZDE3NTMxOTQyYjlhNmVkMTY2YTJmMDBkYjI4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Tb2hvIChzb3V0aCBlYXN0KTsgQ2hpbmF0b3duLCBTb2hvIFNxdWFyZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOTVlMTIxMzE1YTIyNDQ4ODg1NTA4ZjM0OGI4ZjE4NWIuc2V0Q29udGVudChodG1sXzU1M2NkZDE3NTMxOTQyYjlhNmVkMTY2YTJmMDBkYjI4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzViYmEyNGY0ZTI2MDRhMzlhYWRmODk0MDg1MDg5YzE0LmJpbmRQb3B1cChwb3B1cF85NWUxMjEzMTVhMjI0NDg4ODU1MDhmMzQ4YjhmMTg1Yik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl81NDM4NmU4YmRhOTY0NmY1OWZiZGY2NTM3MDExYzgxNSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxNDI3MjIsLTAuMTM1ODE2Nl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMzc1ZjVmM2EyYTJlNDkzZTgxZmI1ZTUyOWZkNzhjMjIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYTdjZDhjMWY1NGIxNDY1ZjkwYzA4YjA4NmUxZmUwMGEgPSAkKCc8ZGl2IGlkPSJodG1sX2E3Y2Q4YzFmNTRiMTQ2NWY5MGMwOGIwODZlMWZlMDBhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Tb2hvIChub3J0aCB3ZXN0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMzc1ZjVmM2EyYTJlNDkzZTgxZmI1ZTUyOWZkNzhjMjIuc2V0Q29udGVudChodG1sX2E3Y2Q4YzFmNTRiMTQ2NWY5MGMwOGIwODZlMWZlMDBhKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzU0Mzg2ZThiZGE5NjQ2ZjU5ZmJkZjY1MzcwMTFjODE1LmJpbmRQb3B1cChwb3B1cF8zNzVmNWYzYTJhMmU0OTNlODFmYjVlNTI5ZmQ3OGMyMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xY2Y0YmEwNmY3Yzk0ZmUyOTVlZDBmY2RiOWM3MDk2MyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxOTUxNjQ5OTk5OTk5LC0wLjE0NjEzNzddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzJlY2M3MSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2RmMGU4MDBhY2RiYzQ0YjM5NzI2N2M3NjVjZDY1ZDI2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzI2YzY5ODE0MzlhMzQ1NzViZTZlMDUxY2Q2OWVkNTI1ID0gJCgnPGRpdiBpZD0iaHRtbF8yNmM2OTgxNDM5YTM0NTc1YmU2ZTA1MWNkNjllZDUyNSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SGFybGV5IFN0cmVldDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZGYwZTgwMGFjZGJjNDRiMzk3MjY3Yzc2NWNkNjVkMjYuc2V0Q29udGVudChodG1sXzI2YzY5ODE0MzlhMzQ1NzViZTZlMDUxY2Q2OWVkNTI1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzFjZjRiYTA2ZjdjOTRmZTI5NWVkMGZjZGI5YzcwOTYzLmJpbmRQb3B1cChwb3B1cF9kZjBlODAwYWNkYmM0NGIzOTcyNjdjNzY1Y2Q2NWQyNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yMjFmZTZiNzFiNGY0YWJhYjZkMThmMWZjMTdiNmViZCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxODg0MTIsLTAuMTYwMTA5OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjE1NjBkYjY1NDkwNGNiYzk4MDYyZDYzYzVkZDQ2MDIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMTA2YjYxZjNjMDExNDI5NDlkYzIzYjYzM2U4Mzc5YzYgPSAkKCc8ZGl2IGlkPSJodG1sXzEwNmI2MWYzYzAxMTQyOTQ5ZGMyM2I2MzNlODM3OWM2IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5NYXJ5bGVib25lPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9iMTU2MGRiNjU0OTA0Y2JjOTgwNjJkNjNjNWRkNDYwMi5zZXRDb250ZW50KGh0bWxfMTA2YjYxZjNjMDExNDI5NDlkYzIzYjYzM2U4Mzc5YzYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMjIxZmU2YjcxYjRmNGFiYWI2ZDE4ZjFmYzE3YjZlYmQuYmluZFBvcHVwKHBvcHVwX2IxNTYwZGI2NTQ5MDRjYmM5ODA2MmQ2M2M1ZGQ0NjAyKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzM2OTk5MGRlOWE5MTRmZjliNDgwY2Q0MmI2MmExMzA1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA4MzQ3Njk5OTk5OTksLTAuMTQ1MzQ0MV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMTRjMzU5MmJiZmNmNDU4Y2JkMTg0NmY2YmQ3YTExOTcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOGFhN2NjMjM0ZmRlNDEzM2E5ZjUwZjg0MDY0NWUzMjggPSAkKCc8ZGl2IGlkPSJodG1sXzhhYTdjYzIzNGZkZTQxMzNhOWY1MGY4NDA2NDVlMzI4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5NYXlmYWlyIChzb3V0aCksIFBpY2NhZGlsbHksIFJveWFsIEFjYWRlbXk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzE0YzM1OTJiYmZjZjQ1OGNiZDE4NDZmNmJkN2ExMTk3LnNldENvbnRlbnQoaHRtbF84YWE3Y2MyMzRmZGU0MTMzYTlmNTBmODQwNjQ1ZTMyOCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zNjk5OTBkZTlhOTE0ZmY5YjQ4MGNkNDJiNjJhMTMwNS5iaW5kUG9wdXAocG9wdXBfMTRjMzU5MmJiZmNmNDU4Y2JkMTg0NmY2YmQ3YTExOTcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMGVhZTdlNTM2NzQ2NGNmMDg3OWQ2MjVkZGNlNTJhZmIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MTA5NjgyOTk5OTk5OSwtMC4xNTEyNF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfODgzZTg0MWRlZDk5NGVkNmFhMjUzNmU5OGQwMjdmYTIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNGVkODYzNzQ2M2YxNGJjODg0N2M3NjE3NzRjZjlhMGQgPSAkKCc8ZGl2IGlkPSJodG1sXzRlZDg2Mzc0NjNmMTRiYzg4NDdjNzYxNzc0Y2Y5YTBkIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5NYXlmYWlyIChub3J0aCksIEdyb3N2ZW5vciBTcXVhcmU8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzg4M2U4NDFkZWQ5OTRlZDZhYTI1MzZlOThkMDI3ZmEyLnNldENvbnRlbnQoaHRtbF80ZWQ4NjM3NDYzZjE0YmM4ODQ3Yzc2MTc3NGNmOWEwZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8wZWFlN2U1MzY3NDY0Y2YwODc5ZDYyNWRkY2U1MmFmYi5iaW5kUG9wdXAocG9wdXBfODgzZTg0MWRlZDk5NGVkNmFhMjUzNmU5OGQwMjdmYTIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOThmZDVmZDI2ZjY5NGY1MDlmMDhlNDk5MGZjMGU0YzAgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MTE2Mzg4OTk5OTk5OSwtMC4xNDAyMTQ3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMyZWNjNzEiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80NjUyNTlhMzQ1MjA0MjgxYjQ4NzE2OWJiNzc2YmU0ZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9kOTY3MDJlYWY2ZDE0OGQ2OTBmMjBhOWFhZmU2ZTI1MSA9ICQoJzxkaXYgaWQ9Imh0bWxfZDk2NzAyZWFmNmQxNDhkNjkwZjIwYTlhYWZlNmUyNTEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk1heWZhaXIgKGVhc3QpLCBIYW5vdmVyIFNxdWFyZSwgU2F2aWxlIFJvdzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNDY1MjU5YTM0NTIwNDI4MWI0ODcxNjliYjc3NmJlNGQuc2V0Q29udGVudChodG1sX2Q5NjcwMmVhZjZkMTQ4ZDY5MGYyMGE5YWFmZTZlMjUxKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzk4ZmQ1ZmQyNmY2OTRmNTA5ZjA4ZTQ5OTBmYzBlNGMwLmJpbmRQb3B1cChwb3B1cF80NjUyNTlhMzQ1MjA0MjgxYjQ4NzE2OWJiNzc2YmU0ZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85MWU5ZmI1NDQ4ZjM0MWQxYmZhMDRlYzZlNTE5OWMyMSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxODg3NDIsLTAuMTMzNjMzMl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfM2YxODhhMjlmMzczNGUyNDlhODlhNWYxN2QwMmM3N2MgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNjBlMjY1YmQ4MDIwNGQ1NzljNGRjNWExMTY2MzBkZDEgPSAkKCc8ZGl2IGlkPSJodG1sXzYwZTI2NWJkODAyMDRkNTc5YzRkYzVhMTE2NjMwZGQxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5GaXR6cm92aWEsIFRvdHRlbmhhbSBDb3VydCBSb2FkPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8zZjE4OGEyOWYzNzM0ZTI0OWE4OWE1ZjE3ZDAyYzc3Yy5zZXRDb250ZW50KGh0bWxfNjBlMjY1YmQ4MDIwNGQ1NzljNGRjNWExMTY2MzBkZDEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOTFlOWZiNTQ0OGYzNDFkMWJmYTA0ZWM2ZTUxOTljMjEuYmluZFBvcHVwKHBvcHVwXzNmMTg4YTI5ZjM3MzRlMjQ5YTg5YTVmMTdkMDJjNzdjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzM0NzQwZDBjODFhNDQ5ZGRiMTRhZWEwOWYyMTI1Zjk4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTE4ODQ5LC0wLjE1NDIyNDldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzJlY2M3MSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2E1ODVjMDI1ODJlNzQ0MjM4NTBiM2JhN2FmY2EyOWY3ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2NmMjBmZGU5MzQ5NTRlOGNhZTU2YmEyNDk5ZGU5ODFjID0gJCgnPGRpdiBpZD0iaHRtbF9jZjIwZmRlOTM0OTU0ZThjYWU1NmJhMjQ5OWRlOTgxYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TWFyeWxlYm9uZTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYTU4NWMwMjU4MmU3NDQyMzg1MGIzYmE3YWZjYTI5Zjcuc2V0Q29udGVudChodG1sX2NmMjBmZGU5MzQ5NTRlOGNhZTU2YmEyNDk5ZGU5ODFjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzM0NzQwZDBjODFhNDQ5ZGRiMTRhZWEwOWYyMTI1Zjk4LmJpbmRQb3B1cChwb3B1cF9hNTg1YzAyNTgyZTc0NDIzODUwYjNiYTdhZmNhMjlmNyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9jMWI1NTI3YTZjYjg0Mjk5YmMzMTIyNWRkZTM5NWM4ZSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxOTUyMzYsLTAuMTQwMjU0M10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZDgyYjlkZDZmOGYyNGIzZjlkMjBlMjhhMzhkNjNlNWYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfODgzNTcwMTllN2ViNGM4ODkwZGY0MjhkMTJjZTJjMzEgPSAkKCc8ZGl2IGlkPSJodG1sXzg4MzU3MDE5ZTdlYjRjODg5MGRmNDI4ZDEyY2UyYzMxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5HcmVhdCBQb3J0bGFuZCBTdHJlZXQsIEZpdHpyb3ZpYTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDgyYjlkZDZmOGYyNGIzZjlkMjBlMjhhMzhkNjNlNWYuc2V0Q29udGVudChodG1sXzg4MzU3MDE5ZTdlYjRjODg5MGRmNDI4ZDEyY2UyYzMxKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2MxYjU1MjdhNmNiODQyOTliYzMxMjI1ZGRlMzk1YzhlLmJpbmRQb3B1cChwb3B1cF9kODJiOWRkNmY4ZjI0YjNmOWQyMGUyOGEzOGQ2M2U1Zik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xNWJjNDU4ZWY3MDE0MTM2OTE4NTQzOTBmYTQ4MmNhNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUwOTYyODEsLTAuMTcwMzU0MV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfMTRiMWRiMjAyMmI5NDlmMjk1YjlmNDljNDY4ZWY1ZDggPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYjYwYTNiOTM3MDRiNGYyYjg4MWU4ODA2YjgzYmY1NWMgPSAkKCc8ZGl2IGlkPSJodG1sX2I2MGEzYjkzNzA0YjRmMmI4ODFlODgwNmI4M2JmNTVjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gUGFkZGluZ3RvbiwgQmF5c3dhdGVyLCBIeWRlIFBhcmssIFdlc3Rib3VybmUgR3JlZW4sIExpdHRsZSBWZW5pY2UgKHBhcnQpLCBOb3R0aW5nIEhpbGwgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xNGIxZGIyMDIyYjk0OWYyOTViOWY0OWM0NjhlZjVkOC5zZXRDb250ZW50KGh0bWxfYjYwYTNiOTM3MDRiNGYyYjg4MWU4ODA2YjgzYmY1NWMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMTViYzQ1OGVmNzAxNDEzNjkxODU0MzkwZmE0ODJjYTQuYmluZFBvcHVwKHBvcHVwXzE0YjFkYjIwMjJiOTQ5ZjI5NWI5ZjQ5YzQ2OGVmNWQ4KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzUxYjc4N2JkOWMwMjRiMDFiYTgwMjRjMDExZDE5YzgwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTEyMDc1MiwtMC4yNjc1NzI1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMyZWNjNzEiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83ZWNhYTEyMzJmOTE0ZmExYTgzN2U2M2ZkNWEzNDlmZSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9kZjMxNDVlNDQzMmE0NDExYWY2ZDc1MmU0NmVmOGQ5ZiA9ICQoJzxkaXYgaWQ9Imh0bWxfZGYzMTQ1ZTQ0MzJhNDQxMWFmNmQ3NTJlNDZlZjhkOWYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBBY3RvbiwgV2VzdCBBY3RvbiwgTm9ydGggQWN0b24gKHBhcnQpLCBTb3V0aCBBY3RvbiwgRWFzdCBBY3RvbiAod2VzdCksIFBhcmsgUm95YWwgKHNvdXRoKSwgSGFuZ2VyIEhpbGwgR2FyZGVuIEVzdGF0ZSwgR3VubmVyc2J1cnkgUGFyazwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfN2VjYWExMjMyZjkxNGZhMWE4MzdlNjNmZDVhMzQ5ZmUuc2V0Q29udGVudChodG1sX2RmMzE0NWU0NDMyYTQ0MTFhZjZkNzUyZTQ2ZWY4ZDlmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzUxYjc4N2JkOWMwMjRiMDFiYTgwMjRjMDExZDE5YzgwLmJpbmRQb3B1cChwb3B1cF83ZWNhYTEyMzJmOTE0ZmExYTgzN2U2M2ZkNWEzNDlmZSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mMDVmNTk2NjQyMzI0NzBkYTdiZDlkOGFkNjkwNzU3YSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjQ4ODQzMzUsLTAuMjY0NF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYmViMmJmMzU1ZGMwNDkzMmFmNGIzNzUxMTFlMDg4YWQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMWE1YjBjMTYwOGZiNDc3MGJkYmQxYjExZmU2MWI1OGMgPSAkKCc8ZGl2IGlkPSJodG1sXzFhNWIwYzE2MDhmYjQ3NzBiZGJkMWIxMWZlNjFiNThjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQ2hpc3dpY2ssIEd1bm5lcnNidXJ5LCBUdXJuaGFtIEdyZWVuLCBBY3RvbiBHcmVlbiwgU291dGggQWN0b24gKHBhcnQpLCBCZWRmb3JkIFBhcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2JlYjJiZjM1NWRjMDQ5MzJhZjRiMzc1MTExZTA4OGFkLnNldENvbnRlbnQoaHRtbF8xYTViMGMxNjA4ZmI0NzcwYmRiZDFiMTFmZTYxYjU4Yyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9mMDVmNTk2NjQyMzI0NzBkYTdiZDlkOGFkNjkwNzU3YS5iaW5kUG9wdXAocG9wdXBfYmViMmJmMzU1ZGMwNDkzMmFmNGIzNzUxMTFlMDg4YWQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWE4MWQ0NTI4MDYxNGYyMGI5OWU4N2JmNDYxOWRlYmMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MDkzNjg5LC0wLjI5OTk4OF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNzBiNDk3OTUyMGViNGEyYzkzMDM3ODQwNmIzZGJiNWYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOTA4NGQ0N2UxMjZlNGNkNzgwMmFiYzNlNjNlZTQxODggPSAkKCc8ZGl2IGlkPSJodG1sXzkwODRkNDdlMTI2ZTRjZDc4MDJhYmMzZTYzZWU0MTg4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gRWFsaW5nLCBTb3V0aCBFYWxpbmcsIEVhbGluZyBDb21tb24sIE5vcnRoIEVhbGluZywgTm9ydGhmaWVsZHMsIChzb3V0aCBhbmQgZWFzdCksIFBpdHNoYW5nZXIsIEhhbmdlciBMYW5lPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83MGI0OTc5NTIwZWI0YTJjOTMwMzc4NDA2YjNkYmI1Zi5zZXRDb250ZW50KGh0bWxfOTA4NGQ0N2UxMjZlNGNkNzgwMmFiYzNlNjNlZTQxODgpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfOWE4MWQ0NTI4MDYxNGYyMGI5OWU4N2JmNDYxOWRlYmMuYmluZFBvcHVwKHBvcHVwXzcwYjQ5Nzk1MjBlYjRhMmM5MzAzNzg0MDZiM2RiYjVmKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2ZjZThkZjI2ODIyNjQ0NWViZDU3N2U0NmYyOTE0Mjc5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNDkxMTQwNSwtMC4yMjYxNDExXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMyZWNjNzEiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81NDUwMmNkMDZmZGE0ZDAyOGU1OGQ4YjNlMzQ0ZWVmNCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jNGI5ZmMyYzVjYzk0MjgzYWMyYmJlYWU2OGY2NjI5YiA9ICQoJzxkaXYgaWQ9Imh0bWxfYzRiOWZjMmM1Y2M5NDI4M2FjMmJiZWFlNjhmNjYyOWIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBIYW1tZXJzbWl0aCwgUmF2ZW5zY291cnQgUGFyaywgU3RhbWZvcmQgQnJvb2sgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF81NDUwMmNkMDZmZGE0ZDAyOGU1OGQ4YjNlMzQ0ZWVmNC5zZXRDb250ZW50KGh0bWxfYzRiOWZjMmM1Y2M5NDI4M2FjMmJiZWFlNjhmNjYyOWIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZmNlOGRmMjY4MjI2NDQ1ZWJkNTc3ZTQ2ZjI5MTQyNzkuYmluZFBvcHVwKHBvcHVwXzU0NTAyY2QwNmZkYTRkMDI4ZTU4ZDhiM2UzNDRlZWY0KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQ4ZDk0OTY2NGZjNjQ2NDdiNmM1NDM5ZjRjY2QyZmZiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTExOTA5NiwtMC4zMzI0ODE1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMyZWNjNzEiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8yZjRmMmZkYzBhM2Q0ZWJlYjNhMjY3Mjk2Njk0NmQ4MCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83YzgxM2U1ZjgwYjQ0Zjg1YjViMTA1MDM1Y2Y2ZTljMiA9ICQoJzxkaXYgaWQ9Imh0bWxfN2M4MTNlNWY4MGI0NGY4NWI1YjEwNTAzNWNmNmU5YzIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBIYW53ZWxsLCBCb3N0b24gTWFub3IgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8yZjRmMmZkYzBhM2Q0ZWJlYjNhMjY3Mjk2Njk0NmQ4MC5zZXRDb250ZW50KGh0bWxfN2M4MTNlNWY4MGI0NGY4NWI1YjEwNTAzNWNmNmU5YzIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDhkOTQ5NjY0ZmM2NDY0N2I2YzU0MzlmNGNjZDJmZmIuYmluZFBvcHVwKHBvcHVwXzJmNGYyZmRjMGEzZDRlYmViM2EyNjcyOTY2OTQ2ZDgwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzc2NjU3OTcyZDY2MDQ2MDdiZDkwYTI1MWRkMTE4ZWU0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTAxNzEzLC0wLjE5MDkwMDhdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzJlY2M3MSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzI3ODIzMzJmNzZiODQ5YWY5MzJmNzIzYThlMDFlZGUwID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzcyNDNkZjFhMmMzMzRiM2Y4M2JhMTIyMzk1OWNmMWRjID0gJCgnPGRpdiBpZD0iaHRtbF83MjQzZGYxYTJjMzM0YjNmODNiYTEyMjM5NTljZjFkYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEtlbnNpbmd0b24sIEhvbGxhbmQgUGFyayAocGFydCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzI3ODIzMzJmNzZiODQ5YWY5MzJmNzIzYThlMDFlZGUwLnNldENvbnRlbnQoaHRtbF83MjQzZGYxYTJjMzM0YjNmODNiYTEyMjM5NTljZjFkYyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl83NjY1Nzk3MmQ2NjA0NjA3YmQ5MGEyNTFkZDExOGVlNC5iaW5kUG9wdXAocG9wdXBfMjc4MjMzMmY3NmI4NDlhZjkzMmY3MjNhOGUwMWVkZTApOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNGYyZjRkMzEzY2U0NDQ2OGI3NTVjMjc0ZmJkMjJlYTEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MjgwMDQzLC0wLjE4NTE5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMyZWNjNzEiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xN2YxYTFmYTRkZmI0YWJiOWVlY2U4YzM1NjExMjVlYiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83YTc4NGM1NWQ2MWQ0YWQ2YTNkNGRhMWZjZjcwMjcxZiA9ICQoJzxkaXYgaWQ9Imh0bWxfN2E3ODRjNTVkNjFkNGFkNmEzZDRkYTFmY2Y3MDI3MWYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBNYWlkYSBIaWxsLCBNYWlkYSBWYWxlLCBMaXR0bGUgVmVuaWNlIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMTdmMWExZmE0ZGZiNGFiYjllZWNlOGMzNTYxMTI1ZWIuc2V0Q29udGVudChodG1sXzdhNzg0YzU1ZDYxZDRhZDZhM2Q0ZGExZmNmNzAyNzFmKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzRmMmY0ZDMxM2NlNDQ0NjhiNzU1YzI3NGZiZDIyZWExLmJpbmRQb3B1cChwb3B1cF8xN2YxYTFmYTRkZmI0YWJiOWVlY2U4YzM1NjExMjVlYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl85MDc5MTg5YTc5NmU0YmVkOWQ0YzExNTYyMDhmYjcxNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUyMjY5NzA5OTk5OTk5LC0wLjIxNDYwOTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzJlY2M3MSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2JlOTVmNTAwNGQxZjQwMWM5YjAzNzE2YjYzYzU2MWZlID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzk3MzFhNTc0NmIzZDQ2YjI5Y2E0YTVjNDU0OWY3MGE1ID0gJCgnPGRpdiBpZD0iaHRtbF85NzMxYTU3NDZiM2Q0NmIyOWNhNGE1YzQ1NDlmNzBhNSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IE5vcnRoIEtlbnNpbmd0b24sIEtlbnNhbCBUb3duLCBMYWRicm9rZSBHcm92ZSAobm9ydGgpLCBRdWVlbiYjMzk7cyBQYXJrIChwYXJ0KTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfYmU5NWY1MDA0ZDFmNDAxYzliMDM3MTZiNjNjNTYxZmUuc2V0Q29udGVudChodG1sXzk3MzFhNTc0NmIzZDQ2YjI5Y2E0YTVjNDU0OWY3MGE1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzkwNzkxODlhNzk2ZTRiZWQ5ZDRjMTE1NjIwOGZiNzE2LmJpbmRQb3B1cChwb3B1cF9iZTk1ZjUwMDRkMWY0MDFjOWIwMzcxNmI2M2M1NjFmZSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8wODZlMmI5ZmVlOWQ0MjA5OTllYWY1OGFhYjY0MmIwNiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxMjE5NDUsLTAuMjA4NjM4OV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNjgyN2IxZTVjYjkwNDcyYmIwYTIwNDgyOWRkY2ZhMjcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZmViMjRjNmQ1ODVmNGRlNThjMDU3YWUxMjQ1ZDE4NjEgPSAkKCc8ZGl2IGlkPSJodG1sX2ZlYjI0YzZkNTg1ZjRkZTU4YzA1N2FlMTI0NWQxODYxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gTm90dGluZyBIaWxsLCBMYWRicm9rZSBHcm92ZSAoc291dGgpLCBIb2xsYW5kIFBhcmsgKHBhcnQpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82ODI3YjFlNWNiOTA0NzJiYjBhMjA0ODI5ZGRjZmEyNy5zZXRDb250ZW50KGh0bWxfZmViMjRjNmQ1ODVmNGRlNThjMDU3YWUxMjQ1ZDE4NjEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMDg2ZTJiOWZlZTlkNDIwOTk5ZWFmNThhYWI2NDJiMDYuYmluZFBvcHVwKHBvcHVwXzY4MjdiMWU1Y2I5MDQ3MmJiMGEyMDQ4MjlkZGNmYTI3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2YwY2ExNGQ3YTJiMTRmNDJiMThkMjkzNmFkZWMzYmM4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA5NTI4MSwtMC4yMjkyMzZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzJlY2M3MSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2MwMGJhZDY5NmQzMzQ4MzVhNTQ4MTc2ZmFhMTM0YzFiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzczMjE5OTVjMjJlODQ2ZDA4OGFkM2EwNjdlYzU1ZDk1ID0gJCgnPGRpdiBpZD0iaHRtbF83MzIxOTk1YzIyZTg0NmQwODhhZDNhMDY3ZWM1NWQ5NSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFNoZXBoZXJkcyBCdXNoLCBXaGl0ZSBDaXR5LCBXb3Jtd29vZCBTY3J1YnMsIEVhc3QgQWN0b24gKGVhc3QpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jMDBiYWQ2OTZkMzM0ODM1YTU0ODE3NmZhYTEzNGMxYi5zZXRDb250ZW50KGh0bWxfNzMyMTk5NWMyMmU4NDZkMDg4YWQzYTA2N2VjNTVkOTUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZjBjYTE0ZDdhMmIxNGY0MmIxOGQyOTM2YWRlYzNiYzguYmluZFBvcHVwKHBvcHVwX2MwMGJhZDY5NmQzMzQ4MzVhNTQ4MTc2ZmFhMTM0YzFiKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2JmZGFhMDY3ZWQ4ZTQzMDJhZjgyOWMyOGJkYjYxNzY1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTE3MTk4Njk5OTk5OTksLTAuMzIwNzMzOV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMmVjYzcxIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZDExYmE1Yzc5YjI5NDRhOTk1MGE1ODUzYjMzY2QxM2EgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYjVhYzBlMWIzNzk0NDhhNTk3YjFmYjRjNWEyYzU0NzQgPSAkKCc8ZGl2IGlkPSJodG1sX2I1YWMwZTFiMzc5NDQ4YTU5N2IxZmI0YzVhMmM1NDc0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gV2VzdCBFYWxpbmcsIE5vcnRoZmllbGRzIChub3J0aCBhbmQgd2VzdCk8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2QxMWJhNWM3OWIyOTQ0YTk5NTBhNTg1M2IzM2NkMTNhLnNldENvbnRlbnQoaHRtbF9iNWFjMGUxYjM3OTQ0OGE1OTdiMWZiNGM1YTJjNTQ3NCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iZmRhYTA2N2VkOGU0MzAyYWY4MjljMjhiZGI2MTc2NS5iaW5kUG9wdXAocG9wdXBfZDExYmE1Yzc5YjI5NDRhOTk1MGE1ODUzYjMzY2QxM2EpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzhlYmEyYzgxMGNmNDlhNGFlNjZmMzFjZWFmYTFiOGQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS40OTY0Mjc4LC0wLjIwODUyMTFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzJlY2M3MSIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzdmZTQ5NmYxOWNkMjQxOTI4NjZkNzdkY2EwZDNiNWY1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzlmNDI4MmJmOGM5YjQ3NzdhODY5NTc2NDRmMGNlYjA0ID0gJCgnPGRpdiBpZD0iaHRtbF85ZjQyODJiZjhjOWI0Nzc3YTg2OTU3NjQ0ZjBjZWIwNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFdlc3QgS2Vuc2luZ3RvbiwgS2Vuc2luZ3RvbiBPbHltcGlhLCBIb2xsYW5kIFBhcms8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzdmZTQ5NmYxOWNkMjQxOTI4NjZkNzdkY2EwZDNiNWY1LnNldENvbnRlbnQoaHRtbF85ZjQyODJiZjhjOWI0Nzc3YTg2OTU3NjQ0ZjBjZWIwNCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zOGViYTJjODEwY2Y0OWE0YWU2NmYzMWNlYWZhMWI4ZC5iaW5kUG9wdXAocG9wdXBfN2ZlNDk2ZjE5Y2QyNDE5Mjg2NmQ3N2RjYTBkM2I1ZjUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZGM1YjkxZjNlZmNiNGUwZDgwYjFjOTk0YTM2Zjg5MjYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MTc5MDAyLC0wLjEyMjk2ODddLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzM0OThkYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2NjZmE2ZWU5MGEzZDRiZjlhNWViYTYwN2YwNzIxYmVmID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2Q4ODUyYzNhNTZjZjQ5NDM5Y2NiNDk0YWUwNGE2YWFkID0gJCgnPGRpdiBpZD0iaHRtbF9kODg1MmMzYTU2Y2Y0OTQzOWNjYjQ5NGFlMDRhNmFhZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TmV3IE94Zm9yZCBTdHJlZXQ8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2NjZmE2ZWU5MGEzZDRiZjlhNWViYTYwN2YwNzIxYmVmLnNldENvbnRlbnQoaHRtbF9kODg1MmMzYTU2Y2Y0OTQzOWNjYjQ5NGFlMDRhNmFhZCk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9kYzViOTFmM2VmY2I0ZTBkODBiMWM5OTRhMzZmODkyNi5iaW5kUG9wdXAocG9wdXBfY2NmYTZlZTkwYTNkNGJmOWE1ZWJhNjA3ZjA3MjFiZWYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMzEyMTM0YjNjMDBhNDk5YThhODBkZjE5N2RkZWFhN2IgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MTk1NDE4LC0wLjEyNDA3ODZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzM0OThkYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzc0YTQxMGYyYzIxNjQ0MjBiZmExY2E4MTAxMzY2MjJjID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzEzNWNmOGJkNDViYTRjYTViODcxNjRmMzY1MjI5MDYwID0gJCgnPGRpdiBpZD0iaHRtbF8xMzVjZjhiZDQ1YmE0Y2E1Yjg3MTY0ZjM2NTIyOTA2MCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Qmxvb21zYnVyeSwgQnJpdGlzaCBNdXNldW0sIFNvdXRoYW1wdG9uIFJvdzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNzRhNDEwZjJjMjE2NDQyMGJmYTFjYTgxMDEzNjYyMmMuc2V0Q29udGVudChodG1sXzEzNWNmOGJkNDViYTRjYTViODcxNjRmMzY1MjI5MDYwKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzMxMjEzNGIzYzAwYTQ5OWE4YTgwZGYxOTdkZGVhYTdiLmJpbmRQb3B1cChwb3B1cF83NGE0MTBmMmMyMTY0NDIwYmZhMWNhODEwMTM2NjIyYyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xNTY3ZWQ0MGEwZGE0YzY4YTAxNjI2NzA0MGRiMmQyYyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUyMjE2MzksLTAuMTI5OTcyM10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMzQ5OGRiIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfODZkNzA3MmRmN2UzNDU4Njg5ZTFmZTFiZTYzYmViYjkgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNzE3ODIzN2VjYjQxNDdmYmJlODFkNjM4MjVlYjI1OGYgPSAkKCc8ZGl2IGlkPSJodG1sXzcxNzgyMzdlY2I0MTQ3ZmJiZTgxZDYzODI1ZWIyNThmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Vbml2ZXJzaXR5IENvbGxlZ2UgTG9uZG9uPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF84NmQ3MDcyZGY3ZTM0NTg2ODllMWZlMWJlNjNiZWJiOS5zZXRDb250ZW50KGh0bWxfNzE3ODIzN2VjYjQxNDdmYmJlODFkNjM4MjVlYjI1OGYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMTU2N2VkNDBhMGRhNGM2OGEwMTYyNjcwNDBkYjJkMmMuYmluZFBvcHVwKHBvcHVwXzg2ZDcwNzJkZjdlMzQ1ODY4OWUxZmUxYmU2M2JlYmI5KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2VlZmUwMmFlNTQ1YzRhYjc4ODIyZTY2YWIzMmY4NTg2ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA3MzUwOSwtMC4xMjc3NTgzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMzNDk4ZGIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hOTE4YTAxZjRmNjk0ZTBjYmMwMDkyOGU5MDYzODQxYSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mODc3ZDBhZmVhY2Q0NjAwYmNhODUwNWM4NDZiZjdkYSA9ICQoJzxkaXYgaWQ9Imh0bWxfZjg3N2QwYWZlYWNkNDYwMGJjYTg1MDVjODQ2YmY3ZGEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlN0IFBhbmNyYXM8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2E5MThhMDFmNGY2OTRlMGNiYzAwOTI4ZTkwNjM4NDFhLnNldENvbnRlbnQoaHRtbF9mODc3ZDBhZmVhY2Q0NjAwYmNhODUwNWM4NDZiZjdkYSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lZWZlMDJhZTU0NWM0YWI3ODgyMmU2NmFiMzJmODU4Ni5iaW5kUG9wdXAocG9wdXBfYTkxOGEwMWY0ZjY5NGUwY2JjMDA5MjhlOTA2Mzg0MWEpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZjVkNDIyMjc2ZDM3NDllYTk0ODhmMTgxZGU5NmFkNmEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MjQxNDcsLTAuMTE4OTUyNV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMzQ5OGRiIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZmViZDFiOWRlMWEwNDRlZGI3ODMwM2ZlNmNjMjI1YmUgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNmE5NTNmYzY1NzM0NDNlYjliNWE0YWNmNmI5YmNkNDEgPSAkKCc8ZGl2IGlkPSJodG1sXzZhOTUzZmM2NTczNDQzZWI5YjVhNGFjZjZiOWJjZDQxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5SdXNzZWxsIFNxdWFyZSwgR3JlYXQgT3Jtb25kIFN0cmVldDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZmViZDFiOWRlMWEwNDRlZGI3ODMwM2ZlNmNjMjI1YmUuc2V0Q29udGVudChodG1sXzZhOTUzZmM2NTczNDQzZWI5YjVhNGFjZjZiOWJjZDQxKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2Y1ZDQyMjI3NmQzNzQ5ZWE5NDg4ZjE4MWRlOTZhZDZhLmJpbmRQb3B1cChwb3B1cF9mZWJkMWI5ZGUxYTA0NGVkYjc4MzAzZmU2Y2MyMjViZSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80ZWRmYWU4NjAxNGY0ZjEzYjM1ZDY4MmQ2NDczNzY1OSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxOTg4MDQwMDAwMDAxLC0wLjExNDE1NjZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzM0OThkYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzM2YzdkODQ0MDg3ODQ1ZWM4ZDdiNGM0NDI4MWVkZTE0ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzQ5NmUxYWM0Nzg3NjRhN2JiMTUyMDA1OWIwMzAyYTBmID0gJCgnPGRpdiBpZD0iaHRtbF80OTZlMWFjNDc4NzY0YTdiYjE1MjAwNTliMDMwMmEwZiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+R3JheSYjMzk7cyBJbm48L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzM2YzdkODQ0MDg3ODQ1ZWM4ZDdiNGM0NDI4MWVkZTE0LnNldENvbnRlbnQoaHRtbF80OTZlMWFjNDc4NzY0YTdiYjE1MjAwNTliMDMwMmEwZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl80ZWRmYWU4NjAxNGY0ZjEzYjM1ZDY4MmQ2NDczNzY1OS5iaW5kUG9wdXAocG9wdXBfMzZjN2Q4NDQwODc4NDVlYzhkN2I0YzQ0MjgxZWRlMTQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTdjMzU3MDZiNzM1NDNmNTliYmQ0OWI5ZDI3MjNkYzUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MTc5MDcsLTAuMTE2MzUzNV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMzQ5OGRiIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjcwOTQ4ZGExY2U3NDQxYjhlZWEyYWJiMjkwMDAwOGYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNTM5NjI0Yjk1NjE3NGMxOTkxMGJlZDJkNDk3MzQ4NzcgPSAkKCc8ZGl2IGlkPSJodG1sXzUzOTYyNGI5NTYxNzRjMTk5MTBiZWQyZDQ5NzM0ODc3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5IaWdoIEhvbGJvcm48L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2I3MDk0OGRhMWNlNzQ0MWI4ZWVhMmFiYjI5MDAwMDhmLnNldENvbnRlbnQoaHRtbF81Mzk2MjRiOTU2MTc0YzE5OTEwYmVkMmQ0OTczNDg3Nyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9lN2MzNTcwNmI3MzU0M2Y1OWJiZDQ5YjlkMjcyM2RjNS5iaW5kUG9wdXAocG9wdXBfYjcwOTQ4ZGExY2U3NDQxYjhlZWEyYWJiMjkwMDAwOGYpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfMGMzYmIwZjdkMTlmNDU0ZmE5NWE1OWQwYWQ3NDE5MDUgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MjY3ODE0LC0wLjExMzA4MjFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzM0OThkYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzc0ZDUxNjdmZmJjZjQwZThiZGM0NGU3OTJmNDhmMDE2ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2Q3YmI5Zjk1NzMxYTQ0ZjI5YmM4YzY4MTk2NTFmNDc5ID0gJCgnPGRpdiBpZD0iaHRtbF9kN2JiOWY5NTczMWE0NGYyOWJjOGM2ODE5NjUxZjQ3OSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+S2luZ3MgQ3Jvc3MsIEZpbnNidXJ5ICh3ZXN0KSwgQ2xlcmtlbndlbGwgKG5vcnRoKTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNzRkNTE2N2ZmYmNmNDBlOGJkYzQ0ZTc5MmY0OGYwMTYuc2V0Q29udGVudChodG1sX2Q3YmI5Zjk1NzMxYTQ0ZjI5YmM4YzY4MTk2NTFmNDc5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzBjM2JiMGY3ZDE5ZjQ1NGZhOTVhNTlkMGFkNzQxOTA1LmJpbmRQb3B1cChwb3B1cF83NGQ1MTY3ZmZiY2Y0MGU4YmRjNDRlNzkyZjQ4ZjAxNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lODZkYjk0YzZiMmM0MWFhYjU3MjQ1OGI5MzIyMTMxNyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxNjI2NzUsLTAuMTEzMDM5NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMzQ5OGRiIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYTQ1YThkOWU3MmE4NDQ2MWFkZTMzNWMzNjRmZDI4NWIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMTEwNDE3NTMxMDdiNDIxNmJhZGNkNDU0N2QyNWRjNjMgPSAkKCc8ZGl2IGlkPSJodG1sXzExMDQxNzUzMTA3YjQyMTZiYWRjZDQ1NDdkMjVkYzYzIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5MaW5jb2xuJiMzOTtzIElubiBGaWVsZHMsIFJveWFsIENvdXJ0cyBvZiBKdXN0aWNlLCBDaGFuY2VyeSBMYW5lPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hNDVhOGQ5ZTcyYTg0NDYxYWRlMzM1YzM2NGZkMjg1Yi5zZXRDb250ZW50KGh0bWxfMTEwNDE3NTMxMDdiNDIxNmJhZGNkNDU0N2QyNWRjNjMpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTg2ZGI5NGM2YjJjNDFhYWI1NzI0NThiOTMyMjEzMTcuYmluZFBvcHVwKHBvcHVwX2E0NWE4ZDllNzJhODQ0NjFhZGUzMzVjMzY0ZmQyODViKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQ2MzkzMmVjODFmYzQ2ZDI4MWNmMWE3ZDcyNjU1N2E5ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTE0MjkxMywtMC4xMTgxNzU2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMzNDk4ZGIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wNTE0YmEyYTY3NjE0ZmViODBiNWZjZjcyMjlkNzk4NyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jNzZiOWY5ODAyMGQ0YmM0YmJhN2RjNzlhNGRkMzFkNCA9ICQoJzxkaXYgaWQ9Imh0bWxfYzc2YjlmOTgwMjBkNGJjNGJiYTdkYzc5YTRkZDMxZDQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRydXJ5IExhbmUsIEtpbmdzd2F5LCBBbGR3eWNoPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wNTE0YmEyYTY3NjE0ZmViODBiNWZjZjcyMjlkNzk4Ny5zZXRDb250ZW50KGh0bWxfYzc2YjlmOTgwMjBkNGJjNGJiYTdkYzc5YTRkZDMxZDQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDYzOTMyZWM4MWZjNDZkMjgxY2YxYTdkNzI2NTU3YTkuYmluZFBvcHVwKHBvcHVwXzA1MTRiYTJhNjc2MTRmZWI4MGI1ZmNmNzIyOWQ3OTg3KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzQ3N2I5YmU1OWViMTQ5ODY4N2JjYzgyMGUzMTFjZmE1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTExNjYwMTk5OTk5OTksLTAuMTIxMTAzOV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjMzQ5OGRiIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiBmYWxzZSwKICAiZmlsbENvbG9yIjogIiMzMTg2Y2MiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDYsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNDY0YmEyYThiOThkNDc3Y2I0YTBlZDM1Y2E2OGIzM2IpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNjYwMzdkNDhkNWFhNGM2MDlkN2RhNjQzNTJkMGI0NDAgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYzU1OWIxZWY4ZjMwNDUwM2I0MDBlNWQyMTJiM2FiNzIgPSAkKCc8ZGl2IGlkPSJodG1sX2M1NTliMWVmOGYzMDQ1MDNiNDAwZTVkMjEyYjNhYjcyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Db3ZlbnQgR2FyZGVuPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF82NjAzN2Q0OGQ1YWE0YzYwOWQ3ZGE2NDM1MmQwYjQ0MC5zZXRDb250ZW50KGh0bWxfYzU1OWIxZWY4ZjMwNDUwM2I0MDBlNWQyMTJiM2FiNzIpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNDc3YjliZTU5ZWIxNDk4Njg3YmNjODIwZTMxMWNmYTUuYmluZFBvcHVwKHBvcHVwXzY2MDM3ZDQ4ZDVhYTRjNjA5ZDdkYTY0MzUyZDBiNDQwKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2YwMTcxY2Y4OGU3MDRlMWZiOTFkMTBlNmM4OTM4MWY4ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTE0MjgzNywtMC4xMjU1MjUzXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMzNDk4ZGIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9jZDQzZDgyNzMzZTQ0MWM2YTgwZTE4NmI0MDliYTU0ZCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8wMzlmNzBlMWRkMjg0OTU5ODI5NGM1NzliNDE3ZGVmOCA9ICQoJzxkaXYgaWQ9Imh0bWxfMDM5ZjcwZTFkZDI4NDk1OTgyOTRjNTc5YjQxN2RlZjgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkxlaWNlc3RlciBTcXVhcmUsIFN0LiBHaWxlczwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfY2Q0M2Q4MjczM2U0NDFjNmE4MGUxODZiNDA5YmE1NGQuc2V0Q29udGVudChodG1sXzAzOWY3MGUxZGQyODQ5NTk4Mjk0YzU3OWI0MTdkZWY4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2YwMTcxY2Y4OGU3MDRlMWZiOTFkMTBlNmM4OTM4MWY4LmJpbmRQb3B1cChwb3B1cF9jZDQzZDgyNzMzZTQ0MWM2YTgwZTE4NmI0MDliYTU0ZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl81ZjgzYWQ0YTQwZWI0Yjc2OGZhY2I0Y2ZjMDNmZDlkMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUwNzcxNjU5OTk5OTk5LC0wLjEyMjU1NjRdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzM0OThkYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogZmFsc2UsCiAgImZpbGxDb2xvciI6ICIjMzE4NmNjIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA2LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzQ2NGJhMmE4Yjk4ZDQ3N2NiNGEwZWQzNWNhNjhiMzNiKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzJhOTM0MmMzN2I1ZjQwOWE5YmVhMWM5Nzc0Y2MyNmFiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzc3NmEyZTgyOWQ4YjRlN2ZiNTIzOTdkNzkzMTU3ZDI3ID0gJCgnPGRpdiBpZD0iaHRtbF83NzZhMmU4MjlkOGI0ZTdmYjUyMzk3ZDc5MzE1N2QyNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Q2hhcmluZyBDcm9zczwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfMmE5MzQyYzM3YjVmNDA5YTliZWExYzk3NzRjYzI2YWIuc2V0Q29udGVudChodG1sXzc3NmEyZTgyOWQ4YjRlN2ZiNTIzOTdkNzkzMTU3ZDI3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzVmODNhZDRhNDBlYjRiNzY4ZmFjYjRjZmMwM2ZkOWQzLmJpbmRQb3B1cChwb3B1cF8yYTkzNDJjMzdiNWY0MDlhOWJlYTFjOTc3NGNjMjZhYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lZjQ2YzdmNTQ5N2Y0MmViODEzYTI1NWQ5NDI4OTgxMCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUxMDM1MiwtMC4xMTUyMTk4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiMzNDk4ZGIiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IGZhbHNlLAogICJmaWxsQ29sb3IiOiAiIzMxODZjYyIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNiwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF80NjRiYTJhOGI5OGQ0NzdjYjRhMGVkMzVjYTY4YjMzYik7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83MmI2YjVlYTVlOTk0ZmIzYmU0ZTExOGEwZTVhMDNlMyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9jMmM4ZWJhOWM4NGQ0NGNhYWViYzI2Y2NmNDI0ZGE3NyA9ICQoJzxkaXYgaWQ9Imh0bWxfYzJjOGViYTljODRkNDRjYWFlYmMyNmNjZjQyNGRhNzciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlNvbWVyc2V0IEhvdXNlLCBUZW1wbGUgKHdlc3QpPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF83MmI2YjVlYTVlOTk0ZmIzYmU0ZTExOGEwZTVhMDNlMy5zZXRDb250ZW50KGh0bWxfYzJjOGViYTljODRkNDRjYWFlYmMyNmNjZjQyNGRhNzcpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZWY0NmM3ZjU0OTdmNDJlYjgxM2EyNTVkOTQyODk4MTAuYmluZFBvcHVwKHBvcHVwXzcyYjZiNWVhNWU5OTRmYjNiZTRlMTE4YTBlNWEwM2UzKTsKCiAgICAgICAgICAgIAogICAgICAgIAo8L3NjcmlwdD4=" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



## Select the Area Category with the least food restaurants



```python
CLIENT_ID = 'LED1F1141BJEBZ3T134KUY2WBXM4PRAJQ0OLYGZMQREJP0UZ' 
CLIENT_SECRET = 'VFAKLGEP2CND2HM4DN1XO0VGHXWS5I4IB1NPPTAK3GRJAAVK' 
VERSION = '20180605'

print('Your credentails:')
print('CLIENT_ID: ' + CLIENT_ID)
print('CLIENT_SECRET:' + CLIENT_SECRET)
```

    Your credentails:
    CLIENT_ID: LED1F1141BJEBZ3T134KUY2WBXM4PRAJQ0OLYGZMQREJP0UZ
    CLIENT_SECRET:VFAKLGEP2CND2HM4DN1XO0VGHXWS5I4IB1NPPTAK3GRJAAVK



```python
LIMIT = 100 #find the venues nearby our neighborhoods locations through Foursquare
radius = 500 
def getNearbyVenues(names, latitudes, longitudes, radius=500):
    
    venues_list=[]
    for name, lat, lng in zip(names, latitudes, longitudes):
        print(name)
            
        
        url = 'https://api.foursquare.com/v2/venues/explore?&client_id={}&client_secret={}&v={}&ll={},{}&radius={}&limit={}'.format(
            CLIENT_ID, 
            CLIENT_SECRET, 
            VERSION, 
            lat, 
            lng, 
            radius, 
            LIMIT)
            
       
        results = requests.get(url).json()["response"]['groups'][0]['items']
        
       
        venues_list.append([(
            name,
            lat, 
            lng,
            v['venue']['name'], 
            v['venue']['location']['lat'], 
            v['venue']['location']['lng'],  
            v['venue']['categories'][0]['name']) for v in results])

    nearby_venues = pd.DataFrame([item for venue_list in venues_list for item in venue_list])
    nearby_venues.columns = ['Neighborhood', 
                  'Neighborhood Latitude', 
                  'Neighborhood Longitude', 
                  'Venue', 
                  'Venue Latitude', 
                  'Venue Longitude', 
                  'Venue Category']
    return(nearby_venues)   

```


```python
London_venues = getNearbyVenues(names=frame['Neighborhood'],
                                   latitudes=frame['Latitude'],
                                   longitudes=frame['Longitude']
                                  )
```

     Barnsbury (part), Canonbury, Kings Cross, Islington, Pentonville, De Beauvoir Town, Hoxton. Shoreditch (part)
    Kings Cross Central
     East Finchley, Fortis Green, Hampstead Garden Suburb (part)
     Finchley, Church End, Finchley Central
     Finsbury Park, Manor House, Harringay (part), Stroud Green
     Highbury, Highbury Fields
     Highgate, Hampstead Heath (part)
     Holloway, Barnsbury (part), Islington (part), Tufnell Park (part)
     Hornsey, Crouch End, Harringay (part)
     Lower Edmonton, Edmonton (part)
     Muswell Hill, South Friern
     New Southgate, Friern Barnet, Bounds Green, Arnos Grove (part)
     North Finchley, Woodside Park, Friern Barnet (part)
     Palmers Green
     Southgate, Oakwood, Arnos Grove (part)
     South Tottenham, Harringay (part), West Green, Seven Sisters, Stamford Hill (part)
     Stoke Newington, Stamford Hill (part), Shacklewell, Dalston (part), Newington Green (part)
     Tottenham, Wood Green (part)
     Upper Edmonton, Edmonton (part)
     Upper Holloway, Archway, Tufnell Park (part)
     Whetstone, Totteridge, Oakleigh Park
     Winchmore Hill, Bush Hill, Grange Park
     Wood Green, Bounds Green (part), Bowes Park
     Bankside, South Bank, Lambeth (part), Southwark, Bermondsey (part), Vauxhall (part), Old Kent Road (part)
     Abbey Wood, West Heath, Crossness, Thamesmead (part), Plumstead (part)
     Blackheath, Kidbrooke, Westcombe Park
     Brockley, Crofton Park
     Camberwell, Denmark Hill, Peckham, Brixton (part)
     Catford, Bellingham, Hither Green (part)
     Charlton
     Deptford, Evelyn, Rotherhithe (part)
     Eltham, Mottingham, New Eltham, Well Hall, Avery Hill (part), Falconwood (part), Sidcup (part), Chinbrook (part), Longlands (part) Kidbrooke (part), Shooter's Hill (part)
     Greenwich, Maze Hill, Greenwich Peninsula
     Kennington, Lambeth (part), Vauxhall (part)
     Lee, Mottingham, Grove Park, Chinbrook, Hither Green (part), Eltham (part), Horn Park, Blackheath (part)
     Lewisham, Hither Green, Ladywell
     New Cross
     Peckham, Nunhead, South Bermondsey (part), Old Kent Road (part)
     Rotherhithe (part), Surrey Quays, South Bermondsey (part)
     Walworth, Kennington (part), Newington
     Woolwich, Royal Arsenal, Plumstead, Shooter's Hill
     Upper Norwood, Crystal Palace
     Anerley, Crystal Palace (part), Penge, Beckenham (part)
     Dulwich, Dulwich Village, West Dulwich, Tulse Hill (part)
     East Dulwich, Dulwich Village (part), Peckham Rye, Loughborough Junction, Herne Hill
     Forest Hill, Honor Oak, Crofton Park (part)
     Herne Hill, Tulse Hill (part), Dulwich (part)
     South Norwood, Selhurst (part), Thornton Heath (part), Woodside (part)
     Sydenham, Crystal Palace (part)
     West Norwood, Gipsy Hill (part)
     Thamesmead
     Aldgate (part), Bishopsgate (part), Whitechapel (mostly), Shoreditch (part), Spitalfields, Shadwell, Stepney (mostly), Mile End (part), Portsoken
    Wapping, St Katharine Docks, Stepney (part), Shadwell (part), Whitechapel (part)
     Bethnal Green (mostly), Haggerston, Hoxton (part), Shoreditch (part), Cambridge Heath (mostly), Globe Town
     Bow, Bow Common, Bromley-by-Bow, Old Ford, Mile End (mostly), Fish Island,Mill Meads (part), Stratford (part), West Ham (part)
     Chingford, Sewardstone, Highams Park, Upper Edmonton (part), Woodford Green (part)
     Leyton (Part), Upper Clapton, Lower Clapton, Stoke Newington (part)
     East Ham, Beckton, Upton Park (part), Barking (part)
     Forest Gate, Leytonstone (Part)
     Hackney Central, Dalston, London Fields, Stoke Newington (part), Cambridge Heath (part)
     Homerton, Hackney Wick, South Hackney, Hackney Marshes, Cambridge Heath (part)
     Leyton, Temple Mills, Hackney Marshes (part) Upper Clapton (part), Walthamstow Marshes
     Leytonstone, Wanstead, Aldersbrook (part), Snaresbrook, Cann Hall
     Manor Park, Little Ilford, Aldersbrook (part)
     Plaistow, West Ham (part), Upton Park (part)
     Poplar, Limehouse, Canary Wharf, Millwall, Blackwall, Cubitt Town, South Bromley, North Greenwich, Leamouth
     Stratford, West Ham (part), Maryland, Leyton (part), Leytonstone (part) Temple Mills (part), Hackney Wick (part), Bow (part)
     Canning Town, Silvertown, Royal Docks, North Woolwich, Beckton (part)
     Walthamstow, Upper Walthamstow, Leyton (part)
     Woodford, South Woodford
     Olympic Park, & parts of Stratford, Homerton, Leyton, Bow;
    St Bartholomew's Hospital
    Clerkenwell, Farringdon
    Hatton Garden
    Finsbury, Finsbury Estate (west)
    Finsbury (east), Moorfields Eye Hospital
    St Luke's, Bunhill Fields
    Shoreditch
    Broadgate, Liverpool Street
    Old Broad Street, Tower 42
    Bank of England
    Guildhall
    Barbican
    St Mary Axe, Aldgate
    Lloyd's of London, Fenchurch Street
    Tower Hill, Tower of London
    Monument, Billingsgate
    Cornhill, Gracechurch Street, Lombard Street
    Fetter Lane
    St Paul's
    Mansion House
    Cannon Street
    Blackfriars
    Temple
     Marylebone (part), Euston, Regent's Park, Baker Street, Camden Town,  Somers Town, Primrose Hill (part) and Lisson Grove (part)
     Cricklewood, Dollis Hill, Childs Hill, Golders Green (part), Brent Cross (part), Willesden (north), Neasden (north)
     Hampstead, Belsize Park, Frognal, Childs Hill (east), South Hampstead (north), Swiss Cottage (east), Primrose Hill (north), Chalk Farm (west), Gospel Oak
     Hendon, Brent Cross (part)
     Kentish Town, Camden Town (part), Gospel Oak (part), Dartmouth Park, Chalk Farm (east), Tufnell Park (west)
     Kilburn, Brondesbury, West Hampstead, Queen's Park, Kensal Green (part), South Hampstead (south), Swiss Cottage (west)
     Mill Hill, Arkley (part), Edgware (part)
     St John's Wood, Primrose Hill (south), Lisson Grove (north)
     The Hyde, Colindale, Kingsbury, West Hendon, Wembley Park (part), Queensbury (part)
     Willesden, Harlesden, Kensal Green, Brent Park, College Park, Stonebridge, North Acton (part), West Twyford, Neasden (south), Old Oak Common, Park Royal (north)
     Golders Green, Temple Fortune, Hampstead Garden Suburb (west), Hendon (part), Brent Cross (part)
    Whitehall and Buckingham Palace
    between Buckingham Gate and Victoria Station
    east of Buckingham Gate
    triangular area between Victoria Station, the Houses of Parliament, and Vauxhall Bridge
    triangular area between Vauxhall Bridge, Chelsea Bridge, and Victoria Station; Pimlico proper
    Belgravia, Chelsea (part), area between Sloane Square and Victoria Station, south of Kings Road
    Belgravia, north of Eaton Square, Knightsbridge (part), Chelsea (part)
    St James's
     Brixton Hill, Tulse Hill, Brixton, Streatham Hill, Clapham (part), Eastern parts of Balham, Lambeth
     Chelsea, Brompton, Knightsbridge (part)
     Clapham, Stockwell (part)
     Earls Court
     Fulham, Parsons Green
     South Kensington, Knightsbridge (part)
     South Lambeth, Wandsworth Road, Vauxhall, Battersea:Nine Elms (part), Clapham (part), NW area of Stockwell, Oval
     Brixton, Stockwell, Clapham (part), Oval
     West Brompton, Chelsea (west)
     Battersea(mostly), Clapham South
     Balham, Clapham South, Wandsworth Common (part)
     Barnes, Barnes Bridge
     Mortlake, East Sheen, Chiswick Bridge
     Putney, Roehampton, Kingston Vale, Putney Heath, Putney Vale, Richmond Park, Roehampton Vale
     Streatham, Streatham Common, Norbury, Streatham Park, Furzedown, Streatham Vale, Mitcham Common, Pollards Hill, Eastfields
     Tooting, Mitcham (part) Balham (part)
     Wandsworth Town, Southfields, Earlsfield
     Wimbledon, Colliers Wood, Merton Park, Merton Abbey, Southfields, Morden (part)
     Raynes Park, Lower Morden, Merton Park, Wimbledon Chase
    Portland Place, Regent Street
    Oxford Street (west)
    Soho (south east); Chinatown, Soho Square
    Soho (north west)
    Harley Street
    Marylebone
    Mayfair (south), Piccadilly, Royal Academy
    Mayfair (north), Grosvenor Square
    Mayfair (east), Hanover Square, Savile Row
    Fitzrovia, Tottenham Court Road
    Marylebone
    Great Portland Street, Fitzrovia
     Paddington, Bayswater, Hyde Park, Westbourne Green, Little Venice (part), Notting Hill (part)
     Acton, West Acton, North Acton (part), South Acton, East Acton (west), Park Royal (south), Hanger Hill Garden Estate, Gunnersbury Park
     Chiswick, Gunnersbury, Turnham Green, Acton Green, South Acton (part), Bedford Park
     Ealing, South Ealing, Ealing Common, North Ealing, Northfields, (south and east), Pitshanger, Hanger Lane
     Hammersmith, Ravenscourt Park, Stamford Brook (part)
     Hanwell, Boston Manor (part)
     Kensington, Holland Park (part)
     Maida Hill, Maida Vale, Little Venice (part)
     North Kensington, Kensal Town, Ladbroke Grove (north), Queen's Park (part)
     Notting Hill, Ladbroke Grove (south), Holland Park (part)
     Shepherds Bush, White City, Wormwood Scrubs, East Acton (east)
     West Ealing, Northfields (north and west)
     West Kensington, Kensington Olympia, Holland Park
    New Oxford Street
    Bloomsbury, British Museum, Southampton Row
    University College London
    St Pancras
    Russell Square, Great Ormond Street
    Gray's Inn
    High Holborn
    Kings Cross, Finsbury (west), Clerkenwell (north)
    Lincoln's Inn Fields, Royal Courts of Justice, Chancery Lane
    Drury Lane, Kingsway, Aldwych
    Covent Garden
    Leicester Square, St. Giles
    Charing Cross
    Somerset House, Temple (west)



```python
df_1=frame[['Neighborhood','Area Category']]
London_venues=pd.merge(London_venues, df_1, on='Neighborhood')
London_venues.head() #Add to our table all the nearby venues of each location, along with their lat,long & venue category
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
      <th>Neighborhood</th>
      <th>Neighborhood Latitude</th>
      <th>Neighborhood Longitude</th>
      <th>Venue</th>
      <th>Venue Latitude</th>
      <th>Venue Longitude</th>
      <th>Venue Category</th>
      <th>Area Category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barnsbury (part), Canonbury, Kings Cross, Isl...</td>
      <td>51.541262</td>
      <td>-0.088139</td>
      <td>De Beauvoir Deli</td>
      <td>51.541464</td>
      <td>-0.085000</td>
      <td>Deli / Bodega</td>
      <td>North</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Barnsbury (part), Canonbury, Kings Cross, Isl...</td>
      <td>51.541262</td>
      <td>-0.088139</td>
      <td>De Beauvoir Arms</td>
      <td>51.541710</td>
      <td>-0.085011</td>
      <td>Gastropub</td>
      <td>North</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Barnsbury (part), Canonbury, Kings Cross, Isl...</td>
      <td>51.541262</td>
      <td>-0.088139</td>
      <td>Rosemary Gardens</td>
      <td>51.538541</td>
      <td>-0.087633</td>
      <td>Park</td>
      <td>North</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Barnsbury (part), Canonbury, Kings Cross, Isl...</td>
      <td>51.541262</td>
      <td>-0.088139</td>
      <td>Trade Coffee</td>
      <td>51.543564</td>
      <td>-0.090469</td>
      <td>Café</td>
      <td>North</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Barnsbury (part), Canonbury, Kings Cross, Isl...</td>
      <td>51.541262</td>
      <td>-0.088139</td>
      <td>Sweet Thursday</td>
      <td>51.541131</td>
      <td>-0.085204</td>
      <td>Italian Restaurant</td>
      <td>North</td>
    </tr>
  </tbody>
</table>
</div>




```python
print('There are {} uniques categories.'.format(len(London_venues['Venue Category'].unique()))) #We have 355 unique categories
```

    There are 355 uniques categories.



```python
L1 = pd.get_dummies(London_venues[['Venue Category']], prefix="", prefix_sep="")
L1['Area Category'] = London_venues['Area Category'] 
fixed_columns = [L1.columns[-1]] + list(L1.columns[:-1])
L1 = L1[fixed_columns]
L1.head() #create dummy variables for each venue category of the available area categories.
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
      <th>Area Category</th>
      <th>Accessories Store</th>
      <th>African Restaurant</th>
      <th>American Restaurant</th>
      <th>Antique Shop</th>
      <th>Arcade</th>
      <th>Arepa Restaurant</th>
      <th>Argentinian Restaurant</th>
      <th>Art Gallery</th>
      <th>Art Museum</th>
      <th>...</th>
      <th>Waterfront</th>
      <th>Whisky Bar</th>
      <th>Wine Bar</th>
      <th>Wine Shop</th>
      <th>Wings Joint</th>
      <th>Women's Store</th>
      <th>Xinjiang Restaurant</th>
      <th>Yoga Studio</th>
      <th>Yoshoku Restaurant</th>
      <th>Zoo Exhibit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>North</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>North</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>North</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>North</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>North</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 356 columns</p>
</div>




```python
L1.shape
```




    (8414, 356)




```python
#find the frequencies of each venue category for each area category
London_grouped = L1.groupby('Area Category').mean().reset_index()
London_grouped
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
      <th>Area Category</th>
      <th>Accessories Store</th>
      <th>African Restaurant</th>
      <th>American Restaurant</th>
      <th>Antique Shop</th>
      <th>Arcade</th>
      <th>Arepa Restaurant</th>
      <th>Argentinian Restaurant</th>
      <th>Art Gallery</th>
      <th>Art Museum</th>
      <th>...</th>
      <th>Waterfront</th>
      <th>Whisky Bar</th>
      <th>Wine Bar</th>
      <th>Wine Shop</th>
      <th>Wings Joint</th>
      <th>Women's Store</th>
      <th>Xinjiang Restaurant</th>
      <th>Yoga Studio</th>
      <th>Yoshoku Restaurant</th>
      <th>Zoo Exhibit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>East</td>
      <td>0.002299</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.002299</td>
      <td>0.000000</td>
      <td>0.011494</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.002299</td>
      <td>0.000000</td>
      <td>0.006897</td>
      <td>0.004598</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.002299</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>East Central</td>
      <td>0.000000</td>
      <td>0.000460</td>
      <td>0.002298</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.004596</td>
      <td>0.011029</td>
      <td>0.000460</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.003676</td>
      <td>0.016085</td>
      <td>0.002757</td>
      <td>0.000000</td>
      <td>0.001838</td>
      <td>0.000000</td>
      <td>0.006893</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>North</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.002183</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.002183</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.004367</td>
      <td>0.000000</td>
      <td>0.002183</td>
      <td>0.000000</td>
      <td>0.002183</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>North West</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.004739</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.004739</td>
      <td>0.004739</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.004739</td>
      <td>0.000000</td>
      <td>0.004739</td>
    </tr>
    <tr>
      <th>4</th>
      <td>South East</td>
      <td>0.000000</td>
      <td>0.003317</td>
      <td>0.003317</td>
      <td>0.001658</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.009950</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.001658</td>
      <td>0.001658</td>
      <td>0.004975</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.001658</td>
      <td>0.001658</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>South West</td>
      <td>0.000000</td>
      <td>0.002230</td>
      <td>0.000743</td>
      <td>0.000743</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.003717</td>
      <td>0.011152</td>
      <td>0.002974</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.005948</td>
      <td>0.004461</td>
      <td>0.000000</td>
      <td>0.000743</td>
      <td>0.000000</td>
      <td>0.005204</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>West</td>
      <td>0.001088</td>
      <td>0.000000</td>
      <td>0.002720</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.003264</td>
      <td>0.011425</td>
      <td>0.001088</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000544</td>
      <td>0.010337</td>
      <td>0.002176</td>
      <td>0.000000</td>
      <td>0.007073</td>
      <td>0.000000</td>
      <td>0.001088</td>
      <td>0.000544</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>West Central</td>
      <td>0.002226</td>
      <td>0.001484</td>
      <td>0.004451</td>
      <td>0.000000</td>
      <td>0.002967</td>
      <td>0.000000</td>
      <td>0.004451</td>
      <td>0.010386</td>
      <td>0.005193</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.009644</td>
      <td>0.003709</td>
      <td>0.002967</td>
      <td>0.000742</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000742</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 356 columns</p>
</div>




```python
#The frequency distribution of the top 20 most common venue categories for each area category
num_top_venues = 20
for i in London_grouped['Area Category']:
    print("----"+i+"----")
    temp = London_grouped[London_grouped['Area Category'] == i].T.reset_index()
    temp.columns = ['venue','freq']
    temp = temp.iloc[1:]
    temp['freq'] = temp['freq'].astype(float)
    temp = temp.round({'freq': 2})
    print(temp.sort_values('freq', ascending=False).reset_index(drop=True).head(num_top_venues))
    print('\n')
```

    ----East----
                       venue  freq
    0                    Pub  0.09
    1                   Café  0.07
    2            Coffee Shop  0.06
    3          Grocery Store  0.05
    4                   Park  0.05
    5   Gym / Fitness Center  0.03
    6            Pizza Place  0.03
    7                  Hotel  0.02
    8                    Bar  0.02
    9     Italian Restaurant  0.02
    10  Fast Food Restaurant  0.02
    11            Restaurant  0.02
    12     Convenience Store  0.02
    13        Sandwich Place  0.01
    14        History Museum  0.01
    15       Thai Restaurant  0.01
    16         Movie Theater  0.01
    17           Supermarket  0.01
    18          Cocktail Bar  0.01
    19          Burger Joint  0.01
    
    
    ----East Central----
                        venue  freq
    0             Coffee Shop  0.09
    1                     Pub  0.06
    2                   Hotel  0.05
    3      Italian Restaurant  0.04
    4              Restaurant  0.03
    5          Sandwich Place  0.03
    6    Gym / Fitness Center  0.03
    7            Cocktail Bar  0.03
    8              Steakhouse  0.02
    9       French Restaurant  0.02
    10                   Café  0.02
    11  Vietnamese Restaurant  0.02
    12               Wine Bar  0.02
    13                 Museum  0.01
    14          Deli / Bodega  0.01
    15              Gastropub  0.01
    16         Breakfast Spot  0.01
    17            Salad Place  0.01
    18         History Museum  0.01
    19          Historic Site  0.01
    
    
    ----North----
                       venue  freq
    0                   Café  0.09
    1                    Pub  0.08
    2            Coffee Shop  0.07
    3          Grocery Store  0.04
    4   Gym / Fitness Center  0.03
    5                    Bar  0.03
    6     Italian Restaurant  0.03
    7         Sandwich Place  0.02
    8               Bus Stop  0.02
    9              Gastropub  0.02
    10    Turkish Restaurant  0.02
    11     Indian Restaurant  0.02
    12                  Park  0.02
    13           Pizza Place  0.02
    14  Fast Food Restaurant  0.02
    15            Restaurant  0.02
    16     Convenience Store  0.01
    17         Movie Theater  0.01
    18           Supermarket  0.01
    19                 Plaza  0.01
    
    
    ----North West----
                       venue  freq
    0                   Café  0.09
    1                    Pub  0.07
    2            Coffee Shop  0.07
    3                 Bakery  0.04
    4     Italian Restaurant  0.04
    5                   Park  0.03
    6          Grocery Store  0.03
    7      Indian Restaurant  0.03
    8                 Garden  0.02
    9        Thai Restaurant  0.02
    10         Deli / Bodega  0.02
    11           Pizza Place  0.02
    12                   Bar  0.02
    13              Pharmacy  0.01
    14  Brazilian Restaurant  0.01
    15             Bookstore  0.01
    16  Gym / Fitness Center  0.01
    17      Greek Restaurant  0.01
    18           Music Venue  0.01
    19           Supermarket  0.01
    
    
    ----South East----
                     venue  freq
    0                  Pub  0.09
    1                 Café  0.08
    2          Coffee Shop  0.04
    3        Grocery Store  0.03
    4                 Park  0.03
    5            Gastropub  0.03
    6             Platform  0.02
    7          Pizza Place  0.02
    8   Italian Restaurant  0.02
    9             Bus Stop  0.02
    10                 Bar  0.02
    11              Bakery  0.02
    12   Indian Restaurant  0.02
    13   Convenience Store  0.02
    14      Breakfast Spot  0.01
    15   Fish & Chips Shop  0.01
    16             Brewery  0.01
    17     Thai Restaurant  0.01
    18       Deli / Bodega  0.01
    19          Food Truck  0.01
    
    
    ----South West----
                       venue  freq
    0                  Hotel  0.08
    1                    Pub  0.06
    2                   Café  0.05
    3            Coffee Shop  0.05
    4     Italian Restaurant  0.04
    5         Sandwich Place  0.03
    6            Pizza Place  0.02
    7                   Park  0.02
    8                 Garden  0.02
    9                  Plaza  0.02
    10               Theater  0.02
    11        Ice Cream Shop  0.02
    12     Indian Restaurant  0.02
    13                Bakery  0.02
    14            Restaurant  0.02
    15   Japanese Restaurant  0.01
    16         Grocery Store  0.01
    17   Monument / Landmark  0.01
    18  Fast Food Restaurant  0.01
    19           Supermarket  0.01
    
    
    ----West----
                       venue  freq
    0            Coffee Shop  0.05
    1                  Hotel  0.05
    2     Italian Restaurant  0.04
    3                   Café  0.04
    4             Restaurant  0.03
    5                    Pub  0.03
    6            Pizza Place  0.02
    7      French Restaurant  0.02
    8           Burger Joint  0.02
    9         Clothing Store  0.02
    10                Bakery  0.02
    11             Juice Bar  0.02
    12     Indian Restaurant  0.02
    13        Sandwich Place  0.02
    14          Cocktail Bar  0.02
    15                   Spa  0.01
    16         Grocery Store  0.01
    17      Sushi Restaurant  0.01
    18           Supermarket  0.01
    19  Gym / Fitness Center  0.01
    
    
    ----West Central----
                      venue  freq
    0           Coffee Shop  0.07
    1                   Pub  0.06
    2                 Hotel  0.04
    3               Theater  0.04
    4                  Café  0.03
    5    Italian Restaurant  0.02
    6        History Museum  0.02
    7            Restaurant  0.02
    8                Bakery  0.02
    9   Japanese Restaurant  0.02
    10       Sandwich Place  0.02
    11         Burger Joint  0.02
    12                Plaza  0.02
    13         Cocktail Bar  0.02
    14            Bookstore  0.02
    15    French Restaurant  0.02
    16     Tapas Restaurant  0.01
    17         Cupcake Shop  0.01
    18             Tea Room  0.01
    19          Salad Place  0.01
    
    



```python
# The Area Category with the least frequencies of restaurants is the East London...
def return_most_common_venues(row, num_top_venues):
    row_categories = row.iloc[1:]
    row_categories_sorted = row_categories.sort_values(ascending=False)
    
    return row_categories_sorted.index.values[0:num_top_venues]
```


```python
#create a df for each area category and the top 20 most common venue categories
num_top_venues = 20

indicators = ['st', 'nd', 'rd']

# create columns according to number of top venues
columns = ['Area Category']
for ind in np.arange(num_top_venues):
    try:
        columns.append('{}{} Most Common Venue'.format(ind+1, indicators[ind]))
    except:
        columns.append('{}th Most Common Venue'.format(ind+1))

# create a new dataframe
category_venues_sorted = pd.DataFrame(columns=columns)
category_venues_sorted['Area Category'] = London_grouped['Area Category']

for ind in np.arange(London_grouped.shape[0]):
    category_venues_sorted.iloc[ind, 1:] = return_most_common_venues(London_grouped.iloc[ind, :], num_top_venues)

category_venues_sorted
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
      <th>Area Category</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>...</th>
      <th>11th Most Common Venue</th>
      <th>12th Most Common Venue</th>
      <th>13th Most Common Venue</th>
      <th>14th Most Common Venue</th>
      <th>15th Most Common Venue</th>
      <th>16th Most Common Venue</th>
      <th>17th Most Common Venue</th>
      <th>18th Most Common Venue</th>
      <th>19th Most Common Venue</th>
      <th>20th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>East</td>
      <td>Pub</td>
      <td>Café</td>
      <td>Coffee Shop</td>
      <td>Grocery Store</td>
      <td>Park</td>
      <td>Pizza Place</td>
      <td>Gym / Fitness Center</td>
      <td>Italian Restaurant</td>
      <td>Bar</td>
      <td>...</td>
      <td>Fast Food Restaurant</td>
      <td>Convenience Store</td>
      <td>Restaurant</td>
      <td>Supermarket</td>
      <td>Chinese Restaurant</td>
      <td>Burger Joint</td>
      <td>Indian Restaurant</td>
      <td>Bakery</td>
      <td>Cocktail Bar</td>
      <td>Train Station</td>
    </tr>
    <tr>
      <th>1</th>
      <td>East Central</td>
      <td>Coffee Shop</td>
      <td>Pub</td>
      <td>Hotel</td>
      <td>Italian Restaurant</td>
      <td>Restaurant</td>
      <td>Gym / Fitness Center</td>
      <td>Sandwich Place</td>
      <td>Cocktail Bar</td>
      <td>Café</td>
      <td>...</td>
      <td>Vietnamese Restaurant</td>
      <td>Steakhouse</td>
      <td>Wine Bar</td>
      <td>Food Truck</td>
      <td>Salad Place</td>
      <td>Sushi Restaurant</td>
      <td>Bar</td>
      <td>Indian Restaurant</td>
      <td>Burger Joint</td>
      <td>Art Gallery</td>
    </tr>
    <tr>
      <th>2</th>
      <td>North</td>
      <td>Café</td>
      <td>Pub</td>
      <td>Coffee Shop</td>
      <td>Grocery Store</td>
      <td>Italian Restaurant</td>
      <td>Bar</td>
      <td>Gym / Fitness Center</td>
      <td>Pizza Place</td>
      <td>Sandwich Place</td>
      <td>...</td>
      <td>Fast Food Restaurant</td>
      <td>Park</td>
      <td>Restaurant</td>
      <td>Bus Stop</td>
      <td>Indian Restaurant</td>
      <td>Gastropub</td>
      <td>Tapas Restaurant</td>
      <td>Fish &amp; Chips Shop</td>
      <td>Deli / Bodega</td>
      <td>Bookstore</td>
    </tr>
    <tr>
      <th>3</th>
      <td>North West</td>
      <td>Café</td>
      <td>Coffee Shop</td>
      <td>Pub</td>
      <td>Bakery</td>
      <td>Italian Restaurant</td>
      <td>Park</td>
      <td>Grocery Store</td>
      <td>Indian Restaurant</td>
      <td>Pizza Place</td>
      <td>...</td>
      <td>Deli / Bodega</td>
      <td>Garden</td>
      <td>Thai Restaurant</td>
      <td>Metro Station</td>
      <td>Cocktail Bar</td>
      <td>Gym / Fitness Center</td>
      <td>Fried Chicken Joint</td>
      <td>Restaurant</td>
      <td>Gastropub</td>
      <td>Ice Cream Shop</td>
    </tr>
    <tr>
      <th>4</th>
      <td>South East</td>
      <td>Pub</td>
      <td>Café</td>
      <td>Coffee Shop</td>
      <td>Park</td>
      <td>Gastropub</td>
      <td>Grocery Store</td>
      <td>Italian Restaurant</td>
      <td>Bus Stop</td>
      <td>Bar</td>
      <td>...</td>
      <td>Platform</td>
      <td>Convenience Store</td>
      <td>Bakery</td>
      <td>Indian Restaurant</td>
      <td>Fish &amp; Chips Shop</td>
      <td>Supermarket</td>
      <td>Thai Restaurant</td>
      <td>Gym / Fitness Center</td>
      <td>Chinese Restaurant</td>
      <td>Restaurant</td>
    </tr>
    <tr>
      <th>5</th>
      <td>South West</td>
      <td>Hotel</td>
      <td>Pub</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Italian Restaurant</td>
      <td>Sandwich Place</td>
      <td>Theater</td>
      <td>Plaza</td>
      <td>Restaurant</td>
      <td>...</td>
      <td>Pizza Place</td>
      <td>Garden</td>
      <td>Park</td>
      <td>Ice Cream Shop</td>
      <td>Bakery</td>
      <td>Grocery Store</td>
      <td>Thai Restaurant</td>
      <td>Bookstore</td>
      <td>French Restaurant</td>
      <td>Gym / Fitness Center</td>
    </tr>
    <tr>
      <th>6</th>
      <td>West</td>
      <td>Hotel</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Italian Restaurant</td>
      <td>Pub</td>
      <td>Restaurant</td>
      <td>Bakery</td>
      <td>French Restaurant</td>
      <td>Cocktail Bar</td>
      <td>...</td>
      <td>Clothing Store</td>
      <td>Burger Joint</td>
      <td>Pizza Place</td>
      <td>Sandwich Place</td>
      <td>Juice Bar</td>
      <td>Tapas Restaurant</td>
      <td>Thai Restaurant</td>
      <td>Boutique</td>
      <td>Middle Eastern Restaurant</td>
      <td>Chinese Restaurant</td>
    </tr>
    <tr>
      <th>7</th>
      <td>West Central</td>
      <td>Coffee Shop</td>
      <td>Pub</td>
      <td>Hotel</td>
      <td>Theater</td>
      <td>Café</td>
      <td>Bookstore</td>
      <td>Italian Restaurant</td>
      <td>French Restaurant</td>
      <td>Restaurant</td>
      <td>...</td>
      <td>Plaza</td>
      <td>Cocktail Bar</td>
      <td>History Museum</td>
      <td>Burger Joint</td>
      <td>Bakery</td>
      <td>Japanese Restaurant</td>
      <td>Garden</td>
      <td>Indian Restaurant</td>
      <td>Ice Cream Shop</td>
      <td>Bar</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 21 columns</p>
</div>



## Select East London for further exploration


```python
London_venues.set_index('Area Category',inplace=True)
east_venues=London_venues.loc['East']
London_venues.reset_index(inplace=True)
east_venues.reset_index(inplace=True)
east_venues.head() #The DF with East London data
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
      <th>Area Category</th>
      <th>Neighborhood</th>
      <th>Neighborhood Latitude</th>
      <th>Neighborhood Longitude</th>
      <th>Venue</th>
      <th>Venue Latitude</th>
      <th>Venue Longitude</th>
      <th>Venue Category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>East</td>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>51.508806</td>
      <td>-0.061552</td>
      <td>Skylight Rooftop Bar</td>
      <td>51.508288</td>
      <td>-0.060520</td>
      <td>Bar</td>
    </tr>
    <tr>
      <th>1</th>
      <td>East</td>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>51.508806</td>
      <td>-0.061552</td>
      <td>Tobacco Dock</td>
      <td>51.508553</td>
      <td>-0.059154</td>
      <td>Event Space</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East</td>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>51.508806</td>
      <td>-0.061552</td>
      <td>Wilton's Music Hall</td>
      <td>51.510409</td>
      <td>-0.066865</td>
      <td>Music Venue</td>
    </tr>
    <tr>
      <th>3</th>
      <td>East</td>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>51.508806</td>
      <td>-0.061552</td>
      <td>Waitrose</td>
      <td>51.507165</td>
      <td>-0.067348</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>4</th>
      <td>East</td>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>51.508806</td>
      <td>-0.061552</td>
      <td>Wombat's London</td>
      <td>51.510367</td>
      <td>-0.068269</td>
      <td>Hostel</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Create dummies for each venue categories of East London neighborhoods' 
L2 = pd.get_dummies(east_venues[['Venue Category']], prefix="", prefix_sep="")
L2['Neighborhood'] = east_venues['Neighborhood'] 
fixed_columns2 = [L2.columns[-1]] + list(L2.columns[:-1])
L2 = L2[fixed_columns2]
L2.head() 
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
      <th>Neighborhood</th>
      <th>Accessories Store</th>
      <th>Arepa Restaurant</th>
      <th>Art Gallery</th>
      <th>Arts &amp; Entertainment</th>
      <th>Asian Restaurant</th>
      <th>Auto Garage</th>
      <th>BBQ Joint</th>
      <th>Bakery</th>
      <th>Bar</th>
      <th>...</th>
      <th>Trail</th>
      <th>Train Station</th>
      <th>Turkish Restaurant</th>
      <th>Vegetarian / Vegan Restaurant</th>
      <th>Vietnamese Restaurant</th>
      <th>Warehouse Store</th>
      <th>Waterfront</th>
      <th>Wine Bar</th>
      <th>Wine Shop</th>
      <th>Yoga Studio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 140 columns</p>
</div>




```python
#the frequencies of each venue category for every East London's neghborhoods.
East_London_grouped = L2.groupby('Neighborhood').mean().reset_index()
East_London_grouped
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
      <th>Neighborhood</th>
      <th>Accessories Store</th>
      <th>Arepa Restaurant</th>
      <th>Art Gallery</th>
      <th>Arts &amp; Entertainment</th>
      <th>Asian Restaurant</th>
      <th>Auto Garage</th>
      <th>BBQ Joint</th>
      <th>Bakery</th>
      <th>Bar</th>
      <th>...</th>
      <th>Trail</th>
      <th>Train Station</th>
      <th>Turkish Restaurant</th>
      <th>Vegetarian / Vegan Restaurant</th>
      <th>Vietnamese Restaurant</th>
      <th>Warehouse Store</th>
      <th>Waterfront</th>
      <th>Wine Bar</th>
      <th>Wine Shop</th>
      <th>Yoga Studio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.068966</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bethnal Green (mostly), Haggerston, Hoxton (p...</td>
      <td>0.000000</td>
      <td>0.020408</td>
      <td>0.020408</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.020408</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.020408</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bow, Bow Common, Bromley-by-Bow, Old Ford, Mi...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.076923</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Canning Town, Silvertown, Royal Docks, North ...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.043478</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Chingford, Sewardstone, Highams Park, Upper E...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>East Ham, Beckton, Upton Park (part), Barking...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Forest Gate, Leytonstone (Part)</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.285714</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Hackney Central, Dalston, London Fields, Stok...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.025000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.025000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.050000</td>
      <td>0.025</td>
      <td>0.000000</td>
      <td>0.05</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.025000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Homerton, Hackney Wick, South Hackney, Hackne...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.058824</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.058824</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Leyton (Part), Upper Clapton, Lower Clapton, ...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.100000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Leyton, Temple Mills, Hackney Marshes (part) ...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Leytonstone, Wanstead, Aldersbrook (part), Sn...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Manor Park, Little Ilford, Aldersbrook (part)</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.125</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Olympic Park, &amp; parts of Stratford, Homerton,...</td>
      <td>0.017241</td>
      <td>0.000000</td>
      <td>0.017241</td>
      <td>0.000000</td>
      <td>0.017241</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.017241</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.017241</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Plaistow, West Ham (part), Upton Park (part)</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Poplar, Limehouse, Canary Wharf, Millwall, Bl...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.029412</td>
      <td>0.000</td>
      <td>0.029412</td>
      <td>0.029412</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.029412</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Stratford, West Ham (part), Maryland, Leyton ...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.058824</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.029412</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Walthamstow, Upper Walthamstow, Leyton (part)</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.047619</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.047619</td>
      <td>...</td>
      <td>0.047619</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.047619</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Woodford, South Woodford</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.040000</td>
      <td>0.040000</td>
      <td>0.120000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.028571</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.028571</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000</td>
      <td>0.000000</td>
      <td>0.00</td>
      <td>0.000000</td>
      <td>0.028571</td>
      <td>0.028571</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
<p>20 rows × 140 columns</p>
</div>




```python
#find the 10 most common venues of every East London's neighborhood
num_top_venues = 10

indicators = ['st', 'nd', 'rd']

# create columns according to number of top venues
columns = ['Neighborhood']
for ind in np.arange(num_top_venues):
    try:
        columns.append('{}{} Most Common Venue'.format(ind+1, indicators[ind]))
    except:
        columns.append('{}th Most Common Venue'.format(ind+1))

# create a new dataframe
neighborhood_venues_sorted = pd.DataFrame(columns=columns)
neighborhood_venues_sorted['Neighborhood'] = East_London_grouped['Neighborhood']

for ind in np.arange(East_London_grouped.shape[0]):
    neighborhood_venues_sorted.iloc[ind, 1:] = return_most_common_venues(East_London_grouped.iloc[ind, :], num_top_venues)

neighborhood_venues_sorted
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
      <th>Neighborhood</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>Grocery Store</td>
      <td>Gym / Fitness Center</td>
      <td>Coffee Shop</td>
      <td>Park</td>
      <td>Bar</td>
      <td>Deli / Bodega</td>
      <td>Pool</td>
      <td>Plaza</td>
      <td>Convenience Store</td>
      <td>Music Venue</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bethnal Green (mostly), Haggerston, Hoxton (p...</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Pub</td>
      <td>Cocktail Bar</td>
      <td>Restaurant</td>
      <td>Pizza Place</td>
      <td>Grocery Store</td>
      <td>Park</td>
      <td>Japanese Restaurant</td>
      <td>Fast Food Restaurant</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bow, Bow Common, Bromley-by-Bow, Old Ford, Mi...</td>
      <td>Bus Stop</td>
      <td>Pub</td>
      <td>Grocery Store</td>
      <td>Metro Station</td>
      <td>Fast Food Restaurant</td>
      <td>Café</td>
      <td>Coffee Shop</td>
      <td>Hotel</td>
      <td>Gym</td>
      <td>Art Gallery</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Canning Town, Silvertown, Royal Docks, North ...</td>
      <td>Hotel</td>
      <td>Coffee Shop</td>
      <td>Sandwich Place</td>
      <td>Hotel Bar</td>
      <td>Pub</td>
      <td>Park</td>
      <td>Café</td>
      <td>Bridge</td>
      <td>English Restaurant</td>
      <td>Mexican Restaurant</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Chingford, Sewardstone, Highams Park, Upper E...</td>
      <td>Rugby Pitch</td>
      <td>Park</td>
      <td>Pub</td>
      <td>Golf Driving Range</td>
      <td>Doner Restaurant</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
      <td>English Restaurant</td>
      <td>Electronics Store</td>
      <td>Eastern European Restaurant</td>
    </tr>
    <tr>
      <th>5</th>
      <td>East Ham, Beckton, Upton Park (part), Barking...</td>
      <td>Grocery Store</td>
      <td>Nature Preserve</td>
      <td>Café</td>
      <td>Yoga Studio</td>
      <td>Electronics Store</td>
      <td>Fish &amp; Chips Shop</td>
      <td>Fast Food Restaurant</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
      <td>English Restaurant</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Forest Gate, Leytonstone (Part)</td>
      <td>Train Station</td>
      <td>Flower Shop</td>
      <td>Grocery Store</td>
      <td>Hotel</td>
      <td>Pub</td>
      <td>Italian Restaurant</td>
      <td>Eastern European Restaurant</td>
      <td>Fast Food Restaurant</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Hackney Central, Dalston, London Fields, Stok...</td>
      <td>Pub</td>
      <td>Café</td>
      <td>Thai Restaurant</td>
      <td>Vietnamese Restaurant</td>
      <td>Train Station</td>
      <td>Movie Theater</td>
      <td>Pool</td>
      <td>Platform</td>
      <td>Pizza Place</td>
      <td>Performing Arts Venue</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Homerton, Hackney Wick, South Hackney, Hackne...</td>
      <td>Café</td>
      <td>Pub</td>
      <td>Convenience Store</td>
      <td>Park</td>
      <td>Bakery</td>
      <td>Gastropub</td>
      <td>Bus Stop</td>
      <td>Indian Restaurant</td>
      <td>Deli / Bodega</td>
      <td>Wine Shop</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Leyton (Part), Upper Clapton, Lower Clapton, ...</td>
      <td>Park</td>
      <td>Pub</td>
      <td>Convenience Store</td>
      <td>Gym / Fitness Center</td>
      <td>Coffee Shop</td>
      <td>Train Station</td>
      <td>Café</td>
      <td>Garden</td>
      <td>Eastern European Restaurant</td>
      <td>Falafel Restaurant</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Leyton, Temple Mills, Hackney Marshes (part) ...</td>
      <td>Convenience Store</td>
      <td>Grocery Store</td>
      <td>Cricket Ground</td>
      <td>Pub</td>
      <td>Fried Chicken Joint</td>
      <td>Chinese Restaurant</td>
      <td>Betting Shop</td>
      <td>Bus Stop</td>
      <td>Hotel</td>
      <td>Park</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Leytonstone, Wanstead, Aldersbrook (part), Sn...</td>
      <td>Pub</td>
      <td>Pizza Place</td>
      <td>Supermarket</td>
      <td>Irish Pub</td>
      <td>Indian Restaurant</td>
      <td>Fast Food Restaurant</td>
      <td>Coffee Shop</td>
      <td>Department Store</td>
      <td>Diner</td>
      <td>Deli / Bodega</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Manor Park, Little Ilford, Aldersbrook (part)</td>
      <td>Restaurant</td>
      <td>Health &amp; Beauty Service</td>
      <td>Hardware Store</td>
      <td>Cafeteria</td>
      <td>Auto Garage</td>
      <td>Print Shop</td>
      <td>Indian Restaurant</td>
      <td>Electronics Store</td>
      <td>Fast Food Restaurant</td>
      <td>Falafel Restaurant</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Olympic Park, &amp; parts of Stratford, Homerton,...</td>
      <td>Café</td>
      <td>Juice Bar</td>
      <td>Gym / Fitness Center</td>
      <td>Pub</td>
      <td>Toy / Game Store</td>
      <td>Park</td>
      <td>Clothing Store</td>
      <td>Grocery Store</td>
      <td>Pizza Place</td>
      <td>Burger Joint</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Plaistow, West Ham (part), Upton Park (part)</td>
      <td>Pub</td>
      <td>Fast Food Restaurant</td>
      <td>Café</td>
      <td>Yoga Studio</td>
      <td>Electronics Store</td>
      <td>Fish &amp; Chips Shop</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
      <td>English Restaurant</td>
      <td>Eastern European Restaurant</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Poplar, Limehouse, Canary Wharf, Millwall, Bl...</td>
      <td>Coffee Shop</td>
      <td>Pizza Place</td>
      <td>Burger Joint</td>
      <td>Gift Shop</td>
      <td>Ramen Restaurant</td>
      <td>Hotel Bar</td>
      <td>Smoothie Shop</td>
      <td>Cycle Studio</td>
      <td>Fish Market</td>
      <td>Scenic Lookout</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Stratford, West Ham (part), Maryland, Leyton ...</td>
      <td>Pub</td>
      <td>Café</td>
      <td>General Entertainment</td>
      <td>Coffee Shop</td>
      <td>Fast Food Restaurant</td>
      <td>Supermarket</td>
      <td>Bar</td>
      <td>Moroccan Restaurant</td>
      <td>Portuguese Restaurant</td>
      <td>Pizza Place</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Walthamstow, Upper Walthamstow, Leyton (part)</td>
      <td>Grocery Store</td>
      <td>Pizza Place</td>
      <td>Pub</td>
      <td>Café</td>
      <td>Coffee Shop</td>
      <td>Tea Room</td>
      <td>Park</td>
      <td>Concert Hall</td>
      <td>Chinese Restaurant</td>
      <td>Restaurant</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Woodford, South Woodford</td>
      <td>Bar</td>
      <td>Grocery Store</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Italian Restaurant</td>
      <td>Pizza Place</td>
      <td>Supermarket</td>
      <td>Pharmacy</td>
      <td>Cocktail Bar</td>
      <td>Metro Station</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>Park</td>
      <td>Coffee Shop</td>
      <td>Pub</td>
      <td>Italian Restaurant</td>
      <td>Grocery Store</td>
      <td>Gym / Fitness Center</td>
      <td>Chinese Restaurant</td>
      <td>Food &amp; Drink Shop</td>
      <td>Fast Food Restaurant</td>
      <td>Liquor Store</td>
    </tr>
  </tbody>
</table>
</div>



## K-means Clustering


```python
from sklearn.cluster import KMeans
```


```python
df2=frame[['PostCode','Neighborhood','Area','District','Area Category','Latitude','Longitude']]
df2.set_index('Area Category',inplace=True)
east_df=df2.loc['East']
df2.reset_index(inplace=True)
east_df.reset_index(inplace=True)
east_df
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
      <th>Area Category</th>
      <th>PostCode</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>District</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>East</td>
      <td>E1</td>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>Tower Hamlets, Hackney, City of London</td>
      <td>Eastern head</td>
      <td>51.508806</td>
      <td>-0.061552</td>
    </tr>
    <tr>
      <th>1</th>
      <td>East</td>
      <td>E1W</td>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>Tower Hamlets</td>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>51.505796</td>
      <td>-0.057199</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East</td>
      <td>E2</td>
      <td>Bethnal Green (mostly), Haggerston, Hoxton (p...</td>
      <td>Tower Hamlets, Hackney</td>
      <td>Bethnal Green</td>
      <td>51.528134</td>
      <td>-0.061651</td>
    </tr>
    <tr>
      <th>3</th>
      <td>East</td>
      <td>E3</td>
      <td>Bow, Bow Common, Bromley-by-Bow, Old Ford, Mi...</td>
      <td>Tower Hamlets, Newham</td>
      <td>Bow</td>
      <td>51.528148</td>
      <td>-0.020542</td>
    </tr>
    <tr>
      <th>4</th>
      <td>East</td>
      <td>E4</td>
      <td>Chingford, Sewardstone, Highams Park, Upper E...</td>
      <td>Waltham Forest, Enfield, Epping Forest (Essex)</td>
      <td>Chingford</td>
      <td>51.630750</td>
      <td>-0.011780</td>
    </tr>
    <tr>
      <th>5</th>
      <td>East</td>
      <td>E5</td>
      <td>Leyton (Part), Upper Clapton, Lower Clapton, ...</td>
      <td>Hackney, Waltham Forest</td>
      <td>Clapton</td>
      <td>51.559692</td>
      <td>-0.049959</td>
    </tr>
    <tr>
      <th>6</th>
      <td>East</td>
      <td>E6</td>
      <td>East Ham, Beckton, Upton Park (part), Barking...</td>
      <td>Newham, Barking and Dagenham</td>
      <td>East Ham</td>
      <td>51.525507</td>
      <td>0.058708</td>
    </tr>
    <tr>
      <th>7</th>
      <td>East</td>
      <td>E7</td>
      <td>Forest Gate, Leytonstone (Part)</td>
      <td>Newham, Waltham Forest</td>
      <td>Forest Gate</td>
      <td>51.551809</td>
      <td>0.029373</td>
    </tr>
    <tr>
      <th>8</th>
      <td>East</td>
      <td>E8</td>
      <td>Hackney Central, Dalston, London Fields, Stok...</td>
      <td>Hackney</td>
      <td>Hackney</td>
      <td>51.543908</td>
      <td>-0.061686</td>
    </tr>
    <tr>
      <th>9</th>
      <td>East</td>
      <td>E9</td>
      <td>Homerton, Hackney Wick, South Hackney, Hackne...</td>
      <td>Hackney, Tower Hamlets</td>
      <td>Homerton</td>
      <td>51.541288</td>
      <td>-0.041111</td>
    </tr>
    <tr>
      <th>10</th>
      <td>East</td>
      <td>E10</td>
      <td>Leyton, Temple Mills, Hackney Marshes (part) ...</td>
      <td>Waltham Forest, Hackney</td>
      <td>Leyton</td>
      <td>51.564962</td>
      <td>-0.014691</td>
    </tr>
    <tr>
      <th>11</th>
      <td>East</td>
      <td>E11</td>
      <td>Leytonstone, Wanstead, Aldersbrook (part), Sn...</td>
      <td>Waltham Forest, Redbridge</td>
      <td>Leytonstone</td>
      <td>51.572853</td>
      <td>0.017635</td>
    </tr>
    <tr>
      <th>12</th>
      <td>East</td>
      <td>E12</td>
      <td>Manor Park, Little Ilford, Aldersbrook (part)</td>
      <td>Newham, Redbridge</td>
      <td>Manor Park</td>
      <td>51.554429</td>
      <td>0.055829</td>
    </tr>
    <tr>
      <th>13</th>
      <td>East</td>
      <td>E13</td>
      <td>Plaistow, West Ham (part), Upton Park (part)</td>
      <td>Newham</td>
      <td>Plaistow</td>
      <td>51.530775</td>
      <td>0.029351</td>
    </tr>
    <tr>
      <th>14</th>
      <td>East</td>
      <td>E14</td>
      <td>Poplar, Limehouse, Canary Wharf, Millwall, Bl...</td>
      <td>Tower Hamlets</td>
      <td>Poplar</td>
      <td>51.509750</td>
      <td>-0.017595</td>
    </tr>
    <tr>
      <th>15</th>
      <td>East</td>
      <td>E15</td>
      <td>Stratford, West Ham (part), Maryland, Leyton ...</td>
      <td>Newham, Waltham Forest, Hackney, Tower Hamlets</td>
      <td>Stratford</td>
      <td>51.541295</td>
      <td>0.005871</td>
    </tr>
    <tr>
      <th>16</th>
      <td>East</td>
      <td>E16</td>
      <td>Canning Town, Silvertown, Royal Docks, North ...</td>
      <td>Newham</td>
      <td>Victoria Docks and North Woolwich</td>
      <td>51.509748</td>
      <td>0.029329</td>
    </tr>
    <tr>
      <th>17</th>
      <td>East</td>
      <td>E17</td>
      <td>Walthamstow, Upper Walthamstow, Leyton (part)</td>
      <td>Waltham Forest</td>
      <td>Walthamstow</td>
      <td>51.590177</td>
      <td>-0.017344</td>
    </tr>
    <tr>
      <th>18</th>
      <td>East</td>
      <td>E18</td>
      <td>Woodford, South Woodford</td>
      <td>Redbridge</td>
      <td>Woodford and South Woodford</td>
      <td>51.591267</td>
      <td>0.026472</td>
    </tr>
    <tr>
      <th>19</th>
      <td>East</td>
      <td>E20</td>
      <td>Olympic Park, &amp; parts of Stratford, Homerton,...</td>
      <td>Newham, Waltham Forest, Hackney, Tower Hamlets</td>
      <td>Olympic Park</td>
      <td>51.543924</td>
      <td>-0.008807</td>
    </tr>
  </tbody>
</table>
</div>




```python
#set number of clusters
kclusters = 5

East_London_grouped_clustering = East_London_grouped.drop('Neighborhood', 1)

# run k-means clustering
kmeans = KMeans(n_clusters=kclusters, random_state=0).fit(East_London_grouped_clustering)

# check cluster labels generated for each row in the dataframe
kmeans.labels_[0:10]
```




    array([1, 1, 1, 1, 3, 2, 1, 1, 1, 1], dtype=int32)




```python
# add clustering labels
east_df['Group Labels'] = kmeans.labels_
east_df = east_df.join(neighborhood_venues_sorted.set_index('Neighborhood'), on='Neighborhood')
east_df.head() # check the last columns!
```

    /home/jupyterlab/conda/lib/python3.6/site-packages/ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      





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
      <th>Area Category</th>
      <th>PostCode</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>District</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Group Labels</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>East</td>
      <td>E1</td>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>Tower Hamlets, Hackney, City of London</td>
      <td>Eastern head</td>
      <td>51.508806</td>
      <td>-0.061552</td>
      <td>1</td>
      <td>Grocery Store</td>
      <td>Gym / Fitness Center</td>
      <td>Coffee Shop</td>
      <td>Park</td>
      <td>Bar</td>
      <td>Deli / Bodega</td>
      <td>Pool</td>
      <td>Plaza</td>
      <td>Convenience Store</td>
      <td>Music Venue</td>
    </tr>
    <tr>
      <th>1</th>
      <td>East</td>
      <td>E1W</td>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>Tower Hamlets</td>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>51.505796</td>
      <td>-0.057199</td>
      <td>1</td>
      <td>Park</td>
      <td>Coffee Shop</td>
      <td>Pub</td>
      <td>Italian Restaurant</td>
      <td>Grocery Store</td>
      <td>Gym / Fitness Center</td>
      <td>Chinese Restaurant</td>
      <td>Food &amp; Drink Shop</td>
      <td>Fast Food Restaurant</td>
      <td>Liquor Store</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East</td>
      <td>E2</td>
      <td>Bethnal Green (mostly), Haggerston, Hoxton (p...</td>
      <td>Tower Hamlets, Hackney</td>
      <td>Bethnal Green</td>
      <td>51.528134</td>
      <td>-0.061651</td>
      <td>1</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Pub</td>
      <td>Cocktail Bar</td>
      <td>Restaurant</td>
      <td>Pizza Place</td>
      <td>Grocery Store</td>
      <td>Park</td>
      <td>Japanese Restaurant</td>
      <td>Fast Food Restaurant</td>
    </tr>
    <tr>
      <th>3</th>
      <td>East</td>
      <td>E3</td>
      <td>Bow, Bow Common, Bromley-by-Bow, Old Ford, Mi...</td>
      <td>Tower Hamlets, Newham</td>
      <td>Bow</td>
      <td>51.528148</td>
      <td>-0.020542</td>
      <td>1</td>
      <td>Bus Stop</td>
      <td>Pub</td>
      <td>Grocery Store</td>
      <td>Metro Station</td>
      <td>Fast Food Restaurant</td>
      <td>Café</td>
      <td>Coffee Shop</td>
      <td>Hotel</td>
      <td>Gym</td>
      <td>Art Gallery</td>
    </tr>
    <tr>
      <th>4</th>
      <td>East</td>
      <td>E4</td>
      <td>Chingford, Sewardstone, Highams Park, Upper E...</td>
      <td>Waltham Forest, Enfield, Epping Forest (Essex)</td>
      <td>Chingford</td>
      <td>51.630750</td>
      <td>-0.011780</td>
      <td>3</td>
      <td>Rugby Pitch</td>
      <td>Park</td>
      <td>Pub</td>
      <td>Golf Driving Range</td>
      <td>Doner Restaurant</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
      <td>English Restaurant</td>
      <td>Electronics Store</td>
      <td>Eastern European Restaurant</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Matplotlib and associated plotting modules
import matplotlib.cm as cm
import matplotlib.colors as colors
```


```python
# create map
map_clusters = folium.Map(location=[latitude, longitude], zoom_start=11)

# set color scheme for the clusters
x = np.arange(kclusters)
ys = [i+x+(i*x)**2 for i in range(kclusters)]
colors_array = cm.rainbow(np.linspace(0, 1, len(ys)))
rainbow = [colors.rgb2hex(i) for i in colors_array]

# add markers to the map
markers_colors = []
for lat, lon, poi, cluster in zip(east_df['Latitude'], east_df['Longitude'], east_df['Neighborhood'], east_df['Group Labels']):
    label = folium.Popup(str(poi) + ' Cluster ' + str(cluster), parse_html=True)
    folium.CircleMarker(
        [lat, lon],
        radius=5,
        popup=label,
        color=rainbow[cluster-1],
        fill=True,
        fill_color=rainbow[cluster-1],
        fill_opacity=0.7).add_to(map_clusters)
       
map_clusters
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfZWVjMTJkOTIxZGI2NGNkY2FiMWFiMTZmZDMyMGU1YzEgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwX2VlYzEyZDkyMWRiNjRjZGNhYjFhYjE2ZmQzMjBlNWMxIiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF9lZWMxMmQ5MjFkYjY0Y2RjYWIxYWIxNmZkMzIwZTVjMSA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF9lZWMxMmQ5MjFkYjY0Y2RjYWIxYWIxNmZkMzIwZTVjMScsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNTEuNTA5ODY1LC0wLjExODA5Ml0sCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB6b29tOiAxMSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbGF5ZXJzOiBbXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHdvcmxkQ29weUp1bXA6IGZhbHNlLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NwogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB9KTsKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfYzQ0NzU2YjQ1NjhhNDBmMGI2NGRlOTE5NWZkYWY0MGUgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICdodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZycsCiAgICAgICAgICAgICAgICB7CiAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgIm1heFpvb20iOiAxOCwKICAibWluWm9vbSI6IDEsCiAgIm5vV3JhcCI6IGZhbHNlLAogICJzdWJkb21haW5zIjogImFiYyIKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZWVjMTJkOTIxZGI2NGNkY2FiMWFiMTZmZDMyMGU1YzEpOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzBiYzc3NjMzODMwYjQ3MGI5M2RlMWQ4OGJmNDI3NmYwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA4ODA1OCwtMC4wNjE1NTIxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2VlYzEyZDkyMWRiNjRjZGNhYjFhYjE2ZmQzMjBlNWMxKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2ExYWE4YzRjYzNmZTQ2NTJhZDg1ZDQ5MmY1ZDY3NTc1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzA0NTljYzljZGIyYzQzYTE4NTMxODAzMWI3NDA4YjZhID0gJCgnPGRpdiBpZD0iaHRtbF8wNDU5Y2M5Y2RiMmM0M2ExODUzMTgwMzFiNzQwOGI2YSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEFsZGdhdGUgKHBhcnQpLCBCaXNob3BzZ2F0ZSAocGFydCksIFdoaXRlY2hhcGVsIChtb3N0bHkpLCBTaG9yZWRpdGNoIChwYXJ0KSwgU3BpdGFsZmllbGRzLCBTaGFkd2VsbCwgU3RlcG5leSAobW9zdGx5KSwgTWlsZSBFbmQgKHBhcnQpLCBQb3J0c29rZW4gQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hMWFhOGM0Y2MzZmU0NjUyYWQ4NWQ0OTJmNWQ2NzU3NS5zZXRDb250ZW50KGh0bWxfMDQ1OWNjOWNkYjJjNDNhMTg1MzE4MDMxYjc0MDhiNmEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMGJjNzc2MzM4MzBiNDcwYjkzZGUxZDg4YmY0Mjc2ZjAuYmluZFBvcHVwKHBvcHVwX2ExYWE4YzRjYzNmZTQ2NTJhZDg1ZDQ5MmY1ZDY3NTc1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzRkZTlhNTI5MmYwZjRiMWQ4NTAzZGJkYTk3NjY4MGE3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA1Nzk1NywtMC4wNTcxOTkxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2VlYzEyZDkyMWRiNjRjZGNhYjFhYjE2ZmQzMjBlNWMxKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2NjODcxZDI1NzIxNTRhYjE4OTc1NGM1NTY3OTU2ZGRkID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzVkYTVmMGIwMTk4YTRiNzk5NWJkOTU0OWNlYzRiNzJlID0gJCgnPGRpdiBpZD0iaHRtbF81ZGE1ZjBiMDE5OGE0Yjc5OTViZDk1NDljZWM0YjcyZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2FwcGluZywgU3QgS2F0aGFyaW5lIERvY2tzLCBTdGVwbmV5IChwYXJ0KSwgU2hhZHdlbGwgKHBhcnQpLCBXaGl0ZWNoYXBlbCAocGFydCkgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9jYzg3MWQyNTcyMTU0YWIxODk3NTRjNTU2Nzk1NmRkZC5zZXRDb250ZW50KGh0bWxfNWRhNWYwYjAxOThhNGI3OTk1YmQ5NTQ5Y2VjNGI3MmUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfNGRlOWE1MjkyZjBmNGIxZDg1MDNkYmRhOTc2NjgwYTcuYmluZFBvcHVwKHBvcHVwX2NjODcxZDI1NzIxNTRhYjE4OTc1NGM1NTY3OTU2ZGRkKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzg1MGRlOGVkNjI0ZjQ3ZjU4NGU4NDE1ZGU1OWM1NTNiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTI4MTMzOTk5OTk5OTksLTAuMDYxNjUxMTk5OTk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZWVjMTJkOTIxZGI2NGNkY2FiMWFiMTZmZDMyMGU1YzEpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNDVjZGNjMWI4ZDUyNDc2YTgxODljNjQ4MzE4MjBjZjcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMDcxMjMwMzQ3YjNiNDM0Mzk4MjRhYzkyN2Q2YTY3N2UgPSAkKCc8ZGl2IGlkPSJodG1sXzA3MTIzMDM0N2IzYjQzNDM5ODI0YWM5MjdkNmE2NzdlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQmV0aG5hbCBHcmVlbiAobW9zdGx5KSwgSGFnZ2Vyc3RvbiwgSG94dG9uIChwYXJ0KSwgU2hvcmVkaXRjaCAocGFydCksIENhbWJyaWRnZSBIZWF0aCAobW9zdGx5KSwgR2xvYmUgVG93biBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzQ1Y2RjYzFiOGQ1MjQ3NmE4MTg5YzY0ODMxODIwY2Y3LnNldENvbnRlbnQoaHRtbF8wNzEyMzAzNDdiM2I0MzQzOTgyNGFjOTI3ZDZhNjc3ZSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84NTBkZThlZDYyNGY0N2Y1ODRlODQxNWRlNTljNTUzYi5iaW5kUG9wdXAocG9wdXBfNDVjZGNjMWI4ZDUyNDc2YTgxODljNjQ4MzE4MjBjZjcpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYWM0M2NiMTI2MmEwNGYyMWFlMmZmNzJlNzlhMmI3MmEgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MjgxNDgzLC0wLjAyMDU0MTZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZWVjMTJkOTIxZGI2NGNkY2FiMWFiMTZmZDMyMGU1YzEpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZDFjODYwYzkwMGQzNGRiNTg0MWFjNGZlZGRkZjAzZWIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOTAzNGRmOWM1NTdkNDM4NWFiMTM0Njc3NjRjYTRlYTcgPSAkKCc8ZGl2IGlkPSJodG1sXzkwMzRkZjljNTU3ZDQzODVhYjEzNDY3NzY0Y2E0ZWE3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQm93LCBCb3cgQ29tbW9uLCBCcm9tbGV5LWJ5LUJvdywgT2xkIEZvcmQsIE1pbGUgRW5kIChtb3N0bHkpLCBGaXNoIElzbGFuZCxNaWxsIE1lYWRzIChwYXJ0KSwgU3RyYXRmb3JkIChwYXJ0KSwgV2VzdCBIYW0gKHBhcnQpIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDFjODYwYzkwMGQzNGRiNTg0MWFjNGZlZGRkZjAzZWIuc2V0Q29udGVudChodG1sXzkwMzRkZjljNTU3ZDQzODVhYjEzNDY3NzY0Y2E0ZWE3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2FjNDNjYjEyNjJhMDRmMjFhZTJmZjcyZTc5YTJiNzJhLmJpbmRQb3B1cChwb3B1cF9kMWM4NjBjOTAwZDM0ZGI1ODQxYWM0ZmVkZGRmMDNlYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8xOWY1ZGVjOWUzM2U0YTIxYTQyOGU5NzZjYTcwODAyYSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjYzMDc0OTksLTAuMDExNzgwM10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODBmZmI0IiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwZmZiNCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9lZWMxMmQ5MjFkYjY0Y2RjYWIxYWIxNmZkMzIwZTVjMSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF83ZDQ2YjEzYjBiZWY0NDNhYThjNDcxZjhjNDY1YWNjMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF83ODU2ZmFiNTNhOGQ0NDcyYjkzOWQ1ZGUwZTUwYmM4NSA9ICQoJzxkaXYgaWQ9Imh0bWxfNzg1NmZhYjUzYThkNDQ3MmI5MzlkNWRlMGU1MGJjODUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBDaGluZ2ZvcmQsIFNld2FyZHN0b25lLCBIaWdoYW1zIFBhcmssIFVwcGVyIEVkbW9udG9uIChwYXJ0KSwgV29vZGZvcmQgR3JlZW4gKHBhcnQpIENsdXN0ZXIgMzwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfN2Q0NmIxM2IwYmVmNDQzYWE4YzQ3MWY4YzQ2NWFjYzIuc2V0Q29udGVudChodG1sXzc4NTZmYWI1M2E4ZDQ0NzJiOTM5ZDVkZTBlNTBiYzg1KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzE5ZjVkZWM5ZTMzZTRhMjFhNDI4ZTk3NmNhNzA4MDJhLmJpbmRQb3B1cChwb3B1cF83ZDQ2YjEzYjBiZWY0NDNhYThjNDcxZjhjNDY1YWNjMik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl81OGQzN2FjZWE5Nzc0OWU3YTJjOGQ0ZTU4OTAyZDU0YyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU1OTY5MjAwMDAwMDAxLC0wLjA0OTk1ODVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzAwYjVlYiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiMwMGI1ZWIiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZWVjMTJkOTIxZGI2NGNkY2FiMWFiMTZmZDMyMGU1YzEpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNDBjYmRmMDNmZjliNGNmZWE2ZDM3YjliNTIwZTk1YjkgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMDgxNmQ1MGFkMjkyNGY5NDg3MDA0M2Q1YTFjMzdkYWYgPSAkKCc8ZGl2IGlkPSJodG1sXzA4MTZkNTBhZDI5MjRmOTQ4NzAwNDNkNWExYzM3ZGFmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gTGV5dG9uIChQYXJ0KSwgVXBwZXIgQ2xhcHRvbiwgTG93ZXIgQ2xhcHRvbiwgU3Rva2UgTmV3aW5ndG9uIChwYXJ0KSBDbHVzdGVyIDI8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzQwY2JkZjAzZmY5YjRjZmVhNmQzN2I5YjUyMGU5NWI5LnNldENvbnRlbnQoaHRtbF8wODE2ZDUwYWQyOTI0Zjk0ODcwMDQzZDVhMWMzN2RhZik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81OGQzN2FjZWE5Nzc0OWU3YTJjOGQ0ZTU4OTAyZDU0Yy5iaW5kUG9wdXAocG9wdXBfNDBjYmRmMDNmZjliNGNmZWE2ZDM3YjliNTIwZTk1YjkpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfOWJkYzg3ZWNkZDRiNDhmYzgwMjU2NzZiZjE0NTU1NjMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MjU1MDY4LDAuMDU4NzA4MTAwMDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZWVjMTJkOTIxZGI2NGNkY2FiMWFiMTZmZDMyMGU1YzEpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfYjk0MTc3MWM5ODBjNDA3ZjlkNjNlNDdhZjZiZDVlNGMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNWRkMDZhZTg5ODhmNGNlNWEwNWE5MDRhOTU3NTdmZWUgPSAkKCc8ZGl2IGlkPSJodG1sXzVkZDA2YWU4OTg4ZjRjZTVhMDVhOTA0YTk1NzU3ZmVlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gRWFzdCBIYW0sIEJlY2t0b24sIFVwdG9uIFBhcmsgKHBhcnQpLCBCYXJraW5nIChwYXJ0KSBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2I5NDE3NzFjOTgwYzQwN2Y5ZDYzZTQ3YWY2YmQ1ZTRjLnNldENvbnRlbnQoaHRtbF81ZGQwNmFlODk4OGY0Y2U1YTA1YTkwNGE5NTc1N2ZlZSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl85YmRjODdlY2RkNGI0OGZjODAyNTY3NmJmMTQ1NTU2My5iaW5kUG9wdXAocG9wdXBfYjk0MTc3MWM5ODBjNDA3ZjlkNjNlNDdhZjZiZDVlNGMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTE5YmI3ZGFiZDg3NGZiYzg4MmI4ZjA3NzMwZGZhMjQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41NTE4MDk0LDAuMDI5MzcyOF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9lZWMxMmQ5MjFkYjY0Y2RjYWIxYWIxNmZkMzIwZTVjMSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF81ZjdmYjVmZDEyNWM0YWNhYjZiZDNkMzEzNzA4OTllNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85NzA0MTBkMjhlZjA0YjVjYmIyY2JmMzdlODU5MDhiMiA9ICQoJzxkaXYgaWQ9Imh0bWxfOTcwNDEwZDI4ZWYwNGI1Y2JiMmNiZjM3ZTg1OTA4YjIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBGb3Jlc3QgR2F0ZSwgTGV5dG9uc3RvbmUgKFBhcnQpIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNWY3ZmI1ZmQxMjVjNGFjYWI2YmQzZDMxMzcwODk5ZTUuc2V0Q29udGVudChodG1sXzk3MDQxMGQyOGVmMDRiNWNiYjJjYmYzN2U4NTkwOGIyKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2UxOWJiN2RhYmQ4NzRmYmM4ODJiOGYwNzczMGRmYTI0LmJpbmRQb3B1cChwb3B1cF81ZjdmYjVmZDEyNWM0YWNhYjZiZDNkMzEzNzA4OTllNSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lYWRhZDRkOGVmYTU0N2E2YjVjMmU0Y2M1YzQ3NjU4MSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU0MzkwODMsLTAuMDYxNjg2MV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9lZWMxMmQ5MjFkYjY0Y2RjYWIxYWIxNmZkMzIwZTVjMSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wZDc2NmM0M2IyZDk0ZTQ0OWU0OGY1NzAzNTIzNGMxYyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84OGE0MWZkYjgyZjc0Mjc2YTU5MDFiYjE5YmViM2NlYSA9ICQoJzxkaXYgaWQ9Imh0bWxfODhhNDFmZGI4MmY3NDI3NmE1OTAxYmIxOWJlYjNjZWEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBIYWNrbmV5IENlbnRyYWwsIERhbHN0b24sIExvbmRvbiBGaWVsZHMsIFN0b2tlIE5ld2luZ3RvbiAocGFydCksIENhbWJyaWRnZSBIZWF0aCAocGFydCkgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wZDc2NmM0M2IyZDk0ZTQ0OWU0OGY1NzAzNTIzNGMxYy5zZXRDb250ZW50KGh0bWxfODhhNDFmZGI4MmY3NDI3NmE1OTAxYmIxOWJlYjNjZWEpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZWFkYWQ0ZDhlZmE1NDdhNmI1YzJlNGNjNWM0NzY1ODEuYmluZFBvcHVwKHBvcHVwXzBkNzY2YzQzYjJkOTRlNDQ5ZTQ4ZjU3MDM1MjM0YzFjKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2VlN2NjMmQ5N2YzOTRmN2ZiODQ2Y2NlZmFmNGMzYWMyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTQxMjg3OTk5OTk5OTksLTAuMDQxMTExNDAwMDAwMDAwMDFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZWVjMTJkOTIxZGI2NGNkY2FiMWFiMTZmZDMyMGU1YzEpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNWZmNThiOWUzOTJmNDgyNTk5YWE0MDdjOGVhNzBjZjMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfZDEzMTM2YWExOTFlNGJlMWI4NDU0NjBhZjE3MWFhZTkgPSAkKCc8ZGl2IGlkPSJodG1sX2QxMzEzNmFhMTkxZTRiZTFiODQ1NDYwYWYxNzFhYWU5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gSG9tZXJ0b24sIEhhY2tuZXkgV2ljaywgU291dGggSGFja25leSwgSGFja25leSBNYXJzaGVzLCBDYW1icmlkZ2UgSGVhdGggKHBhcnQpIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNWZmNThiOWUzOTJmNDgyNTk5YWE0MDdjOGVhNzBjZjMuc2V0Q29udGVudChodG1sX2QxMzEzNmFhMTkxZTRiZTFiODQ1NDYwYWYxNzFhYWU5KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2VlN2NjMmQ5N2YzOTRmN2ZiODQ2Y2NlZmFmNGMzYWMyLmJpbmRQb3B1cChwb3B1cF81ZmY1OGI5ZTM5MmY0ODI1OTlhYTQwN2M4ZWE3MGNmMyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iMTUzMDQ1NDBiNmI0MmMzYWQ2OGU3YzM2ZmVlYWMxNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU2NDk2MTksLTAuMDE0NjkxMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9lZWMxMmQ5MjFkYjY0Y2RjYWIxYWIxNmZkMzIwZTVjMSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9iZTVjNGEyM2I0NDM0M2I1OTI4NjFlNGFlN2ZjNDQ3MyA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zN2I2ODU4ZjQ2NDA0YWRiOGQzMzkyMjlkMjljM2QyNyA9ICQoJzxkaXYgaWQ9Imh0bWxfMzdiNjg1OGY0NjQwNGFkYjhkMzM5MjI5ZDI5YzNkMjciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBMZXl0b24sIFRlbXBsZSBNaWxscywgSGFja25leSBNYXJzaGVzIChwYXJ0KSBVcHBlciBDbGFwdG9uIChwYXJ0KSwgV2FsdGhhbXN0b3cgTWFyc2hlcyBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2JlNWM0YTIzYjQ0MzQzYjU5Mjg2MWU0YWU3ZmM0NDczLnNldENvbnRlbnQoaHRtbF8zN2I2ODU4ZjQ2NDA0YWRiOGQzMzkyMjlkMjljM2QyNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9iMTUzMDQ1NDBiNmI0MmMzYWQ2OGU3YzM2ZmVlYWMxNC5iaW5kUG9wdXAocG9wdXBfYmU1YzRhMjNiNDQzNDNiNTkyODYxZTRhZTdmYzQ0NzMpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNWY0ODQ3YzVkOWIwNGUxMGI5NzI4Yzg1NmQ5NDg1NjggPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41NzI4NTI1LDAuMDE3NjM0OF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9lZWMxMmQ5MjFkYjY0Y2RjYWIxYWIxNmZkMzIwZTVjMSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF80MDA5YmQ5MDMwZGQ0NGE0OTM5ZDNlMzBkODdmYzgxNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF81Y2NhZjEzM2ZmN2Y0OWVhYWE2NDIxYjVkNTEzZjgyNyA9ICQoJzxkaXYgaWQ9Imh0bWxfNWNjYWYxMzNmZjdmNDllYWFhNjQyMWI1ZDUxM2Y4MjciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBMZXl0b25zdG9uZSwgV2Fuc3RlYWQsIEFsZGVyc2Jyb29rIChwYXJ0KSwgU25hcmVzYnJvb2ssIENhbm4gSGFsbCBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzQwMDliZDkwMzBkZDQ0YTQ5MzlkM2UzMGQ4N2ZjODE1LnNldENvbnRlbnQoaHRtbF81Y2NhZjEzM2ZmN2Y0OWVhYWE2NDIxYjVkNTEzZjgyNyk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl81ZjQ4NDdjNWQ5YjA0ZTEwYjk3MjhjODU2ZDk0ODU2OC5iaW5kUG9wdXAocG9wdXBfNDAwOWJkOTAzMGRkNDRhNDkzOWQzZTMwZDg3ZmM4MTUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZmZmOWY1MjFiMjRlNGQxZjg0NjlkZWJlZDg5ODIwNzIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41NTQ0Mjk1LDAuMDU1ODI4OF0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjZmYwMDAwIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiI2ZmMDAwMCIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9lZWMxMmQ5MjFkYjY0Y2RjYWIxYWIxNmZkMzIwZTVjMSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9hNGE4YjI0YWNhYmU0ZDhkYmVhY2M4ZjNkYTUwZDJjYSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mZWZlOTk3OWM5ZWI0YTg4YmNiMzBjZTYyOGJmYjc4NiA9ICQoJzxkaXYgaWQ9Imh0bWxfZmVmZTk5NzljOWViNGE4OGJjYjMwY2U2MjhiZmI3ODYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBNYW5vciBQYXJrLCBMaXR0bGUgSWxmb3JkLCBBbGRlcnNicm9vayAocGFydCkgQ2x1c3RlciAwPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hNGE4YjI0YWNhYmU0ZDhkYmVhY2M4ZjNkYTUwZDJjYS5zZXRDb250ZW50KGh0bWxfZmVmZTk5NzljOWViNGE4OGJjYjMwY2U2MjhiZmI3ODYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZmZmOWY1MjFiMjRlNGQxZjg0NjlkZWJlZDg5ODIwNzIuYmluZFBvcHVwKHBvcHVwX2E0YThiMjRhY2FiZTRkOGRiZWFjYzhmM2RhNTBkMmNhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzFhM2QyZjY3YTQyYTQwYmFhOTA1OTI3NGYxODEwMGY0ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTMwNzc1Mjk5OTk5OTksMC4wMjkzNTA2XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2VlYzEyZDkyMWRiNjRjZGNhYjFhYjE2ZmQzMjBlNWMxKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2ZkZGJmZDg1YzQ0YzQxMzY4NjdjNWZmNGJlOGJjZDFkID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzI3Y2UzMWY1NDZhMjRmNDhhNGQ0NDBhMjZjMmY3ZTE0ID0gJCgnPGRpdiBpZD0iaHRtbF8yN2NlMzFmNTQ2YTI0ZjQ4YTRkNDQwYTI2YzJmN2UxNCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFBsYWlzdG93LCBXZXN0IEhhbSAocGFydCksIFVwdG9uIFBhcmsgKHBhcnQpIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZmRkYmZkODVjNDRjNDEzNjg2N2M1ZmY0YmU4YmNkMWQuc2V0Q29udGVudChodG1sXzI3Y2UzMWY1NDZhMjRmNDhhNGQ0NDBhMjZjMmY3ZTE0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzFhM2QyZjY3YTQyYTQwYmFhOTA1OTI3NGYxODEwMGY0LmJpbmRQb3B1cChwb3B1cF9mZGRiZmQ4NWM0NGM0MTM2ODY3YzVmZjRiZThiY2QxZCk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9iMGFjNjU4YjZmNmM0YzBlYmJhMDc4ZjE5MjYzMDliYiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUwOTc1MDIsLTAuMDE3NTk1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiNmZmIzNjAiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjZmZiMzYwIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2VlYzEyZDkyMWRiNjRjZGNhYjFhYjE2ZmQzMjBlNWMxKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2Q3NzFjODM3OGZjMzRhZjdhMGZkMWU2Y2FkM2ZmZTE5ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzAxYjBiYTYzNDk1MzRjNDBiYmJkYzc1NzczOTdiNmI4ID0gJCgnPGRpdiBpZD0iaHRtbF8wMWIwYmE2MzQ5NTM0YzQwYmJiZGM3NTc3Mzk3YjZiOCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFBvcGxhciwgTGltZWhvdXNlLCBDYW5hcnkgV2hhcmYsIE1pbGx3YWxsLCBCbGFja3dhbGwsIEN1Yml0dCBUb3duLCBTb3V0aCBCcm9tbGV5LCBOb3J0aCBHcmVlbndpY2gsIExlYW1vdXRoIENsdXN0ZXIgNDwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZDc3MWM4Mzc4ZmMzNGFmN2EwZmQxZTZjYWQzZmZlMTkuc2V0Q29udGVudChodG1sXzAxYjBiYTYzNDk1MzRjNDBiYmJkYzc1NzczOTdiNmI4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2IwYWM2NThiNmY2YzRjMGViYmEwNzhmMTkyNjMwOWJiLmJpbmRQb3B1cChwb3B1cF9kNzcxYzgzNzhmYzM0YWY3YTBmZDFlNmNhZDNmZmUxOSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9mNmFhMDAyYzQxNGU0ZmUzYTUyOGZkM2NhY2ZlNzc4MyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU0MTI5NSwwLjAwNTg3MDldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfZWVjMTJkOTIxZGI2NGNkY2FiMWFiMTZmZDMyMGU1YzEpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZDMzNWRjOGNlY2UyNGU1ZTlmOTQ4YzAzYmFjOGUwNTIgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfOTg4MDIzZWMzZjYxNGJmMDg0NTI5ZTkxMjNiYWQ2NzkgPSAkKCc8ZGl2IGlkPSJodG1sXzk4ODAyM2VjM2Y2MTRiZjA4NDUyOWU5MTIzYmFkNjc5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gU3RyYXRmb3JkLCBXZXN0IEhhbSAocGFydCksIE1hcnlsYW5kLCBMZXl0b24gKHBhcnQpLCBMZXl0b25zdG9uZSAocGFydCkgVGVtcGxlIE1pbGxzIChwYXJ0KSwgSGFja25leSBXaWNrIChwYXJ0KSwgQm93IChwYXJ0KSBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2QzMzVkYzhjZWNlMjRlNWU5Zjk0OGMwM2JhYzhlMDUyLnNldENvbnRlbnQoaHRtbF85ODgwMjNlYzNmNjE0YmYwODQ1MjllOTEyM2JhZDY3OSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9mNmFhMDAyYzQxNGU0ZmUzYTUyOGZkM2NhY2ZlNzc4My5iaW5kUG9wdXAocG9wdXBfZDMzNWRjOGNlY2UyNGU1ZTlmOTQ4YzAzYmFjOGUwNTIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTJkNmQ5N2FjYjRjNDQyZTljYWIxYzI5ZmY4N2UwNTcgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MDk3NDc4LDAuMDI5MzI4NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9lZWMxMmQ5MjFkYjY0Y2RjYWIxYWIxNmZkMzIwZTVjMSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9lZDM0NDZjZmI4NTE0NzZlOTI4YjdhNTY1ZmM4NmNiNCA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9mMDRjOTU1OThkNTc0ZDliODU5ZmZkZTAwODAyYTQ4OSA9ICQoJzxkaXYgaWQ9Imh0bWxfZjA0Yzk1NTk4ZDU3NGQ5Yjg1OWZmZGUwMDgwMmE0ODkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBDYW5uaW5nIFRvd24sIFNpbHZlcnRvd24sIFJveWFsIERvY2tzLCBOb3J0aCBXb29sd2ljaCwgQmVja3RvbiAocGFydCkgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lZDM0NDZjZmI4NTE0NzZlOTI4YjdhNTY1ZmM4NmNiNC5zZXRDb250ZW50KGh0bWxfZjA0Yzk1NTk4ZDU3NGQ5Yjg1OWZmZGUwMDgwMmE0ODkpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTJkNmQ5N2FjYjRjNDQyZTljYWIxYzI5ZmY4N2UwNTcuYmluZFBvcHVwKHBvcHVwX2VkMzQ0NmNmYjg1MTQ3NmU5MjhiN2E1NjVmYzg2Y2I0KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzJmNjM5ZjlmYmJkNzRiZWRhODFmZWI3N2Q3YzJjZjE3ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTkwMTc2OSwtMC4wMTczNDM3XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2VlYzEyZDkyMWRiNjRjZGNhYjFhYjE2ZmQzMjBlNWMxKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2YzMDY4ZTFlN2YwZDRlMmU5ZWE2MWE5ZWU3YWE1Y2ZiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2ZlNjA1NzI5N2NjYjQzNThhNmZiMjRmM2JiNTZlNTllID0gJCgnPGRpdiBpZD0iaHRtbF9mZTYwNTcyOTdjY2I0MzU4YTZmYjI0ZjNiYjU2ZTU5ZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFdhbHRoYW1zdG93LCBVcHBlciBXYWx0aGFtc3RvdywgTGV5dG9uIChwYXJ0KSBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwX2YzMDY4ZTFlN2YwZDRlMmU5ZWE2MWE5ZWU3YWE1Y2ZiLnNldENvbnRlbnQoaHRtbF9mZTYwNTcyOTdjY2I0MzU4YTZmYjI0ZjNiYjU2ZTU5ZSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8yZjYzOWY5ZmJiZDc0YmVkYTgxZmViNzdkN2MyY2YxNy5iaW5kUG9wdXAocG9wdXBfZjMwNjhlMWU3ZjBkNGUyZTllYTYxYTllZTdhYTVjZmIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfYzEwYWMwY2Q2ZWE0NGM5ZmIzYjdiNTRjYTcyMTJiZDYgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41OTEyNjcxLDAuMDI2NDcyMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9lZWMxMmQ5MjFkYjY0Y2RjYWIxYWIxNmZkMzIwZTVjMSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8wYTliYmM1MmM1YzA0NTNlOTBjYzViNjcxMmY2OTgzMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9kZDk2ZGM2ZGJiMWY0MDZmOGJjNzRkZDcxMGUxZjJjYyA9ICQoJzxkaXYgaWQ9Imh0bWxfZGQ5NmRjNmRiYjFmNDA2ZjhiYzc0ZGQ3MTBlMWYyY2MiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBXb29kZm9yZCwgU291dGggV29vZGZvcmQgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8wYTliYmM1MmM1YzA0NTNlOTBjYzViNjcxMmY2OTgzMi5zZXRDb250ZW50KGh0bWxfZGQ5NmRjNmRiYjFmNDA2ZjhiYzc0ZGQ3MTBlMWYyY2MpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYzEwYWMwY2Q2ZWE0NGM5ZmIzYjdiNTRjYTcyMTJiZDYuYmluZFBvcHVwKHBvcHVwXzBhOWJiYzUyYzVjMDQ1M2U5MGNjNWI2NzEyZjY5ODMyKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzJhNGRlZmQ3YWQxMzRkM2ZhYTgxNGY1NmM0ODA0ZGRhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTQzOTI0MSwtMC4wMDg4MDc1XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2VlYzEyZDkyMWRiNjRjZGNhYjFhYjE2ZmQzMjBlNWMxKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2NmZjM4ZTI4NmM2NzRjZjQ5ZDg4N2MwYzQ5ZDBiZTQxID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzgwYWRjMjMxNzk0MzQxMzQ4NjYwMjFiM2EzYjhjOWE3ID0gJCgnPGRpdiBpZD0iaHRtbF84MGFkYzIzMTc5NDM0MTM0ODY2MDIxYjNhM2I4YzlhNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IE9seW1waWMgUGFyaywgJmFtcDsgcGFydHMgb2YgU3RyYXRmb3JkLCBIb21lcnRvbiwgTGV5dG9uLCBCb3c7IENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfY2ZmMzhlMjg2YzY3NGNmNDlkODg3YzBjNDlkMGJlNDEuc2V0Q29udGVudChodG1sXzgwYWRjMjMxNzk0MzQxMzQ4NjYwMjFiM2EzYjhjOWE3KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzJhNGRlZmQ3YWQxMzRkM2ZhYTgxNGY1NmM0ODA0ZGRhLmJpbmRQb3B1cChwb3B1cF9jZmYzOGUyODZjNjc0Y2Y0OWQ4ODdjMGM0OWQwYmU0MSk7CgogICAgICAgICAgICAKICAgICAgICAKPC9zY3JpcHQ+" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



## Explore Clusters


```python
east_df.loc[east_df['Group Labels'] == 0, east_df.columns[[1] + list(range(2, east_df.shape[1]))]]
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
      <th>PostCode</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>District</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Group Labels</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>12</th>
      <td>E12</td>
      <td>Manor Park, Little Ilford, Aldersbrook (part)</td>
      <td>Newham, Redbridge</td>
      <td>Manor Park</td>
      <td>51.554429</td>
      <td>0.055829</td>
      <td>0</td>
      <td>Restaurant</td>
      <td>Health &amp; Beauty Service</td>
      <td>Hardware Store</td>
      <td>Cafeteria</td>
      <td>Auto Garage</td>
      <td>Print Shop</td>
      <td>Indian Restaurant</td>
      <td>Electronics Store</td>
      <td>Fast Food Restaurant</td>
      <td>Falafel Restaurant</td>
    </tr>
  </tbody>
</table>
</div>




```python
east_df.loc[east_df['Group Labels'] == 1, east_df.columns[[1] + list(range(2, east_df.shape[1]))]]
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
      <th>PostCode</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>District</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Group Labels</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>E1</td>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>Tower Hamlets, Hackney, City of London</td>
      <td>Eastern head</td>
      <td>51.508806</td>
      <td>-0.061552</td>
      <td>1</td>
      <td>Grocery Store</td>
      <td>Gym / Fitness Center</td>
      <td>Coffee Shop</td>
      <td>Park</td>
      <td>Bar</td>
      <td>Deli / Bodega</td>
      <td>Pool</td>
      <td>Plaza</td>
      <td>Convenience Store</td>
      <td>Music Venue</td>
    </tr>
    <tr>
      <th>1</th>
      <td>E1W</td>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>Tower Hamlets</td>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>51.505796</td>
      <td>-0.057199</td>
      <td>1</td>
      <td>Park</td>
      <td>Coffee Shop</td>
      <td>Pub</td>
      <td>Italian Restaurant</td>
      <td>Grocery Store</td>
      <td>Gym / Fitness Center</td>
      <td>Chinese Restaurant</td>
      <td>Food &amp; Drink Shop</td>
      <td>Fast Food Restaurant</td>
      <td>Liquor Store</td>
    </tr>
    <tr>
      <th>2</th>
      <td>E2</td>
      <td>Bethnal Green (mostly), Haggerston, Hoxton (p...</td>
      <td>Tower Hamlets, Hackney</td>
      <td>Bethnal Green</td>
      <td>51.528134</td>
      <td>-0.061651</td>
      <td>1</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Pub</td>
      <td>Cocktail Bar</td>
      <td>Restaurant</td>
      <td>Pizza Place</td>
      <td>Grocery Store</td>
      <td>Park</td>
      <td>Japanese Restaurant</td>
      <td>Fast Food Restaurant</td>
    </tr>
    <tr>
      <th>3</th>
      <td>E3</td>
      <td>Bow, Bow Common, Bromley-by-Bow, Old Ford, Mi...</td>
      <td>Tower Hamlets, Newham</td>
      <td>Bow</td>
      <td>51.528148</td>
      <td>-0.020542</td>
      <td>1</td>
      <td>Bus Stop</td>
      <td>Pub</td>
      <td>Grocery Store</td>
      <td>Metro Station</td>
      <td>Fast Food Restaurant</td>
      <td>Café</td>
      <td>Coffee Shop</td>
      <td>Hotel</td>
      <td>Gym</td>
      <td>Art Gallery</td>
    </tr>
    <tr>
      <th>6</th>
      <td>E6</td>
      <td>East Ham, Beckton, Upton Park (part), Barking...</td>
      <td>Newham, Barking and Dagenham</td>
      <td>East Ham</td>
      <td>51.525507</td>
      <td>0.058708</td>
      <td>1</td>
      <td>Grocery Store</td>
      <td>Nature Preserve</td>
      <td>Café</td>
      <td>Yoga Studio</td>
      <td>Electronics Store</td>
      <td>Fish &amp; Chips Shop</td>
      <td>Fast Food Restaurant</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
      <td>English Restaurant</td>
    </tr>
    <tr>
      <th>7</th>
      <td>E7</td>
      <td>Forest Gate, Leytonstone (Part)</td>
      <td>Newham, Waltham Forest</td>
      <td>Forest Gate</td>
      <td>51.551809</td>
      <td>0.029373</td>
      <td>1</td>
      <td>Train Station</td>
      <td>Flower Shop</td>
      <td>Grocery Store</td>
      <td>Hotel</td>
      <td>Pub</td>
      <td>Italian Restaurant</td>
      <td>Eastern European Restaurant</td>
      <td>Fast Food Restaurant</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
    </tr>
    <tr>
      <th>8</th>
      <td>E8</td>
      <td>Hackney Central, Dalston, London Fields, Stok...</td>
      <td>Hackney</td>
      <td>Hackney</td>
      <td>51.543908</td>
      <td>-0.061686</td>
      <td>1</td>
      <td>Pub</td>
      <td>Café</td>
      <td>Thai Restaurant</td>
      <td>Vietnamese Restaurant</td>
      <td>Train Station</td>
      <td>Movie Theater</td>
      <td>Pool</td>
      <td>Platform</td>
      <td>Pizza Place</td>
      <td>Performing Arts Venue</td>
    </tr>
    <tr>
      <th>9</th>
      <td>E9</td>
      <td>Homerton, Hackney Wick, South Hackney, Hackne...</td>
      <td>Hackney, Tower Hamlets</td>
      <td>Homerton</td>
      <td>51.541288</td>
      <td>-0.041111</td>
      <td>1</td>
      <td>Café</td>
      <td>Pub</td>
      <td>Convenience Store</td>
      <td>Park</td>
      <td>Bakery</td>
      <td>Gastropub</td>
      <td>Bus Stop</td>
      <td>Indian Restaurant</td>
      <td>Deli / Bodega</td>
      <td>Wine Shop</td>
    </tr>
    <tr>
      <th>10</th>
      <td>E10</td>
      <td>Leyton, Temple Mills, Hackney Marshes (part) ...</td>
      <td>Waltham Forest, Hackney</td>
      <td>Leyton</td>
      <td>51.564962</td>
      <td>-0.014691</td>
      <td>1</td>
      <td>Convenience Store</td>
      <td>Grocery Store</td>
      <td>Cricket Ground</td>
      <td>Pub</td>
      <td>Fried Chicken Joint</td>
      <td>Chinese Restaurant</td>
      <td>Betting Shop</td>
      <td>Bus Stop</td>
      <td>Hotel</td>
      <td>Park</td>
    </tr>
    <tr>
      <th>11</th>
      <td>E11</td>
      <td>Leytonstone, Wanstead, Aldersbrook (part), Sn...</td>
      <td>Waltham Forest, Redbridge</td>
      <td>Leytonstone</td>
      <td>51.572853</td>
      <td>0.017635</td>
      <td>1</td>
      <td>Pub</td>
      <td>Pizza Place</td>
      <td>Supermarket</td>
      <td>Irish Pub</td>
      <td>Indian Restaurant</td>
      <td>Fast Food Restaurant</td>
      <td>Coffee Shop</td>
      <td>Department Store</td>
      <td>Diner</td>
      <td>Deli / Bodega</td>
    </tr>
    <tr>
      <th>13</th>
      <td>E13</td>
      <td>Plaistow, West Ham (part), Upton Park (part)</td>
      <td>Newham</td>
      <td>Plaistow</td>
      <td>51.530775</td>
      <td>0.029351</td>
      <td>1</td>
      <td>Pub</td>
      <td>Fast Food Restaurant</td>
      <td>Café</td>
      <td>Yoga Studio</td>
      <td>Electronics Store</td>
      <td>Fish &amp; Chips Shop</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
      <td>English Restaurant</td>
      <td>Eastern European Restaurant</td>
    </tr>
    <tr>
      <th>15</th>
      <td>E15</td>
      <td>Stratford, West Ham (part), Maryland, Leyton ...</td>
      <td>Newham, Waltham Forest, Hackney, Tower Hamlets</td>
      <td>Stratford</td>
      <td>51.541295</td>
      <td>0.005871</td>
      <td>1</td>
      <td>Pub</td>
      <td>Café</td>
      <td>General Entertainment</td>
      <td>Coffee Shop</td>
      <td>Fast Food Restaurant</td>
      <td>Supermarket</td>
      <td>Bar</td>
      <td>Moroccan Restaurant</td>
      <td>Portuguese Restaurant</td>
      <td>Pizza Place</td>
    </tr>
    <tr>
      <th>16</th>
      <td>E16</td>
      <td>Canning Town, Silvertown, Royal Docks, North ...</td>
      <td>Newham</td>
      <td>Victoria Docks and North Woolwich</td>
      <td>51.509748</td>
      <td>0.029329</td>
      <td>1</td>
      <td>Hotel</td>
      <td>Coffee Shop</td>
      <td>Sandwich Place</td>
      <td>Hotel Bar</td>
      <td>Pub</td>
      <td>Park</td>
      <td>Café</td>
      <td>Bridge</td>
      <td>English Restaurant</td>
      <td>Mexican Restaurant</td>
    </tr>
    <tr>
      <th>17</th>
      <td>E17</td>
      <td>Walthamstow, Upper Walthamstow, Leyton (part)</td>
      <td>Waltham Forest</td>
      <td>Walthamstow</td>
      <td>51.590177</td>
      <td>-0.017344</td>
      <td>1</td>
      <td>Grocery Store</td>
      <td>Pizza Place</td>
      <td>Pub</td>
      <td>Café</td>
      <td>Coffee Shop</td>
      <td>Tea Room</td>
      <td>Park</td>
      <td>Concert Hall</td>
      <td>Chinese Restaurant</td>
      <td>Restaurant</td>
    </tr>
    <tr>
      <th>18</th>
      <td>E18</td>
      <td>Woodford, South Woodford</td>
      <td>Redbridge</td>
      <td>Woodford and South Woodford</td>
      <td>51.591267</td>
      <td>0.026472</td>
      <td>1</td>
      <td>Bar</td>
      <td>Grocery Store</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Italian Restaurant</td>
      <td>Pizza Place</td>
      <td>Supermarket</td>
      <td>Pharmacy</td>
      <td>Cocktail Bar</td>
      <td>Metro Station</td>
    </tr>
    <tr>
      <th>19</th>
      <td>E20</td>
      <td>Olympic Park, &amp; parts of Stratford, Homerton,...</td>
      <td>Newham, Waltham Forest, Hackney, Tower Hamlets</td>
      <td>Olympic Park</td>
      <td>51.543924</td>
      <td>-0.008807</td>
      <td>1</td>
      <td>Café</td>
      <td>Juice Bar</td>
      <td>Gym / Fitness Center</td>
      <td>Pub</td>
      <td>Toy / Game Store</td>
      <td>Park</td>
      <td>Clothing Store</td>
      <td>Grocery Store</td>
      <td>Pizza Place</td>
      <td>Burger Joint</td>
    </tr>
  </tbody>
</table>
</div>




```python
east_df.loc[east_df['Group Labels'] == 2, east_df.columns[[1] + list(range(2, east_df.shape[1]))]]
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
      <th>PostCode</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>District</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Group Labels</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>E5</td>
      <td>Leyton (Part), Upper Clapton, Lower Clapton, ...</td>
      <td>Hackney, Waltham Forest</td>
      <td>Clapton</td>
      <td>51.559692</td>
      <td>-0.049959</td>
      <td>2</td>
      <td>Park</td>
      <td>Pub</td>
      <td>Convenience Store</td>
      <td>Gym / Fitness Center</td>
      <td>Coffee Shop</td>
      <td>Train Station</td>
      <td>Café</td>
      <td>Garden</td>
      <td>Eastern European Restaurant</td>
      <td>Falafel Restaurant</td>
    </tr>
  </tbody>
</table>
</div>




```python
east_df.loc[east_df['Group Labels'] == 3, east_df.columns[[1] + list(range(2, east_df.shape[1]))]]
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
      <th>PostCode</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>District</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Group Labels</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>E4</td>
      <td>Chingford, Sewardstone, Highams Park, Upper E...</td>
      <td>Waltham Forest, Enfield, Epping Forest (Essex)</td>
      <td>Chingford</td>
      <td>51.63075</td>
      <td>-0.01178</td>
      <td>3</td>
      <td>Rugby Pitch</td>
      <td>Park</td>
      <td>Pub</td>
      <td>Golf Driving Range</td>
      <td>Doner Restaurant</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
      <td>English Restaurant</td>
      <td>Electronics Store</td>
      <td>Eastern European Restaurant</td>
    </tr>
  </tbody>
</table>
</div>




```python
east_df.loc[east_df['Group Labels'] == 4, east_df.columns[[1] + list(range(2, east_df.shape[1]))]]
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
      <th>PostCode</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>District</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Group Labels</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>14</th>
      <td>E14</td>
      <td>Poplar, Limehouse, Canary Wharf, Millwall, Bl...</td>
      <td>Tower Hamlets</td>
      <td>Poplar</td>
      <td>51.50975</td>
      <td>-0.017595</td>
      <td>4</td>
      <td>Coffee Shop</td>
      <td>Pizza Place</td>
      <td>Burger Joint</td>
      <td>Gift Shop</td>
      <td>Ramen Restaurant</td>
      <td>Hotel Bar</td>
      <td>Smoothie Shop</td>
      <td>Cycle Studio</td>
      <td>Fish Market</td>
      <td>Scenic Lookout</td>
    </tr>
  </tbody>
</table>
</div>



## Select 2nd group because there is not a strong presence of Food industries...


```python
cluster=1
east_df.set_index('Group Labels',inplace=True)
final_cluster= east_df.loc[cluster]                       
east_df.reset_index(inplace=True)
final_cluster.reset_index(inplace=True)
final_cluster
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
      <th>Group Labels</th>
      <th>Area Category</th>
      <th>PostCode</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>District</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>East</td>
      <td>E1</td>
      <td>Aldgate (part), Bishopsgate (part), Whitechap...</td>
      <td>Tower Hamlets, Hackney, City of London</td>
      <td>Eastern head</td>
      <td>51.508806</td>
      <td>-0.061552</td>
      <td>Grocery Store</td>
      <td>Gym / Fitness Center</td>
      <td>Coffee Shop</td>
      <td>Park</td>
      <td>Bar</td>
      <td>Deli / Bodega</td>
      <td>Pool</td>
      <td>Plaza</td>
      <td>Convenience Store</td>
      <td>Music Venue</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>East</td>
      <td>E1W</td>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>Tower Hamlets</td>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>51.505796</td>
      <td>-0.057199</td>
      <td>Park</td>
      <td>Coffee Shop</td>
      <td>Pub</td>
      <td>Italian Restaurant</td>
      <td>Grocery Store</td>
      <td>Gym / Fitness Center</td>
      <td>Chinese Restaurant</td>
      <td>Food &amp; Drink Shop</td>
      <td>Fast Food Restaurant</td>
      <td>Liquor Store</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>East</td>
      <td>E2</td>
      <td>Bethnal Green (mostly), Haggerston, Hoxton (p...</td>
      <td>Tower Hamlets, Hackney</td>
      <td>Bethnal Green</td>
      <td>51.528134</td>
      <td>-0.061651</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Pub</td>
      <td>Cocktail Bar</td>
      <td>Restaurant</td>
      <td>Pizza Place</td>
      <td>Grocery Store</td>
      <td>Park</td>
      <td>Japanese Restaurant</td>
      <td>Fast Food Restaurant</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>East</td>
      <td>E3</td>
      <td>Bow, Bow Common, Bromley-by-Bow, Old Ford, Mi...</td>
      <td>Tower Hamlets, Newham</td>
      <td>Bow</td>
      <td>51.528148</td>
      <td>-0.020542</td>
      <td>Bus Stop</td>
      <td>Pub</td>
      <td>Grocery Store</td>
      <td>Metro Station</td>
      <td>Fast Food Restaurant</td>
      <td>Café</td>
      <td>Coffee Shop</td>
      <td>Hotel</td>
      <td>Gym</td>
      <td>Art Gallery</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>East</td>
      <td>E6</td>
      <td>East Ham, Beckton, Upton Park (part), Barking...</td>
      <td>Newham, Barking and Dagenham</td>
      <td>East Ham</td>
      <td>51.525507</td>
      <td>0.058708</td>
      <td>Grocery Store</td>
      <td>Nature Preserve</td>
      <td>Café</td>
      <td>Yoga Studio</td>
      <td>Electronics Store</td>
      <td>Fish &amp; Chips Shop</td>
      <td>Fast Food Restaurant</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
      <td>English Restaurant</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1</td>
      <td>East</td>
      <td>E7</td>
      <td>Forest Gate, Leytonstone (Part)</td>
      <td>Newham, Waltham Forest</td>
      <td>Forest Gate</td>
      <td>51.551809</td>
      <td>0.029373</td>
      <td>Train Station</td>
      <td>Flower Shop</td>
      <td>Grocery Store</td>
      <td>Hotel</td>
      <td>Pub</td>
      <td>Italian Restaurant</td>
      <td>Eastern European Restaurant</td>
      <td>Fast Food Restaurant</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1</td>
      <td>East</td>
      <td>E8</td>
      <td>Hackney Central, Dalston, London Fields, Stok...</td>
      <td>Hackney</td>
      <td>Hackney</td>
      <td>51.543908</td>
      <td>-0.061686</td>
      <td>Pub</td>
      <td>Café</td>
      <td>Thai Restaurant</td>
      <td>Vietnamese Restaurant</td>
      <td>Train Station</td>
      <td>Movie Theater</td>
      <td>Pool</td>
      <td>Platform</td>
      <td>Pizza Place</td>
      <td>Performing Arts Venue</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1</td>
      <td>East</td>
      <td>E9</td>
      <td>Homerton, Hackney Wick, South Hackney, Hackne...</td>
      <td>Hackney, Tower Hamlets</td>
      <td>Homerton</td>
      <td>51.541288</td>
      <td>-0.041111</td>
      <td>Café</td>
      <td>Pub</td>
      <td>Convenience Store</td>
      <td>Park</td>
      <td>Bakery</td>
      <td>Gastropub</td>
      <td>Bus Stop</td>
      <td>Indian Restaurant</td>
      <td>Deli / Bodega</td>
      <td>Wine Shop</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1</td>
      <td>East</td>
      <td>E10</td>
      <td>Leyton, Temple Mills, Hackney Marshes (part) ...</td>
      <td>Waltham Forest, Hackney</td>
      <td>Leyton</td>
      <td>51.564962</td>
      <td>-0.014691</td>
      <td>Convenience Store</td>
      <td>Grocery Store</td>
      <td>Cricket Ground</td>
      <td>Pub</td>
      <td>Fried Chicken Joint</td>
      <td>Chinese Restaurant</td>
      <td>Betting Shop</td>
      <td>Bus Stop</td>
      <td>Hotel</td>
      <td>Park</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1</td>
      <td>East</td>
      <td>E11</td>
      <td>Leytonstone, Wanstead, Aldersbrook (part), Sn...</td>
      <td>Waltham Forest, Redbridge</td>
      <td>Leytonstone</td>
      <td>51.572853</td>
      <td>0.017635</td>
      <td>Pub</td>
      <td>Pizza Place</td>
      <td>Supermarket</td>
      <td>Irish Pub</td>
      <td>Indian Restaurant</td>
      <td>Fast Food Restaurant</td>
      <td>Coffee Shop</td>
      <td>Department Store</td>
      <td>Diner</td>
      <td>Deli / Bodega</td>
    </tr>
    <tr>
      <th>10</th>
      <td>1</td>
      <td>East</td>
      <td>E13</td>
      <td>Plaistow, West Ham (part), Upton Park (part)</td>
      <td>Newham</td>
      <td>Plaistow</td>
      <td>51.530775</td>
      <td>0.029351</td>
      <td>Pub</td>
      <td>Fast Food Restaurant</td>
      <td>Café</td>
      <td>Yoga Studio</td>
      <td>Electronics Store</td>
      <td>Fish &amp; Chips Shop</td>
      <td>Falafel Restaurant</td>
      <td>Event Space</td>
      <td>English Restaurant</td>
      <td>Eastern European Restaurant</td>
    </tr>
    <tr>
      <th>11</th>
      <td>1</td>
      <td>East</td>
      <td>E15</td>
      <td>Stratford, West Ham (part), Maryland, Leyton ...</td>
      <td>Newham, Waltham Forest, Hackney, Tower Hamlets</td>
      <td>Stratford</td>
      <td>51.541295</td>
      <td>0.005871</td>
      <td>Pub</td>
      <td>Café</td>
      <td>General Entertainment</td>
      <td>Coffee Shop</td>
      <td>Fast Food Restaurant</td>
      <td>Supermarket</td>
      <td>Bar</td>
      <td>Moroccan Restaurant</td>
      <td>Portuguese Restaurant</td>
      <td>Pizza Place</td>
    </tr>
    <tr>
      <th>12</th>
      <td>1</td>
      <td>East</td>
      <td>E16</td>
      <td>Canning Town, Silvertown, Royal Docks, North ...</td>
      <td>Newham</td>
      <td>Victoria Docks and North Woolwich</td>
      <td>51.509748</td>
      <td>0.029329</td>
      <td>Hotel</td>
      <td>Coffee Shop</td>
      <td>Sandwich Place</td>
      <td>Hotel Bar</td>
      <td>Pub</td>
      <td>Park</td>
      <td>Café</td>
      <td>Bridge</td>
      <td>English Restaurant</td>
      <td>Mexican Restaurant</td>
    </tr>
    <tr>
      <th>13</th>
      <td>1</td>
      <td>East</td>
      <td>E17</td>
      <td>Walthamstow, Upper Walthamstow, Leyton (part)</td>
      <td>Waltham Forest</td>
      <td>Walthamstow</td>
      <td>51.590177</td>
      <td>-0.017344</td>
      <td>Grocery Store</td>
      <td>Pizza Place</td>
      <td>Pub</td>
      <td>Café</td>
      <td>Coffee Shop</td>
      <td>Tea Room</td>
      <td>Park</td>
      <td>Concert Hall</td>
      <td>Chinese Restaurant</td>
      <td>Restaurant</td>
    </tr>
    <tr>
      <th>14</th>
      <td>1</td>
      <td>East</td>
      <td>E18</td>
      <td>Woodford, South Woodford</td>
      <td>Redbridge</td>
      <td>Woodford and South Woodford</td>
      <td>51.591267</td>
      <td>0.026472</td>
      <td>Bar</td>
      <td>Grocery Store</td>
      <td>Coffee Shop</td>
      <td>Café</td>
      <td>Italian Restaurant</td>
      <td>Pizza Place</td>
      <td>Supermarket</td>
      <td>Pharmacy</td>
      <td>Cocktail Bar</td>
      <td>Metro Station</td>
    </tr>
    <tr>
      <th>15</th>
      <td>1</td>
      <td>East</td>
      <td>E20</td>
      <td>Olympic Park, &amp; parts of Stratford, Homerton,...</td>
      <td>Newham, Waltham Forest, Hackney, Tower Hamlets</td>
      <td>Olympic Park</td>
      <td>51.543924</td>
      <td>-0.008807</td>
      <td>Café</td>
      <td>Juice Bar</td>
      <td>Gym / Fitness Center</td>
      <td>Pub</td>
      <td>Toy / Game Store</td>
      <td>Park</td>
      <td>Clothing Store</td>
      <td>Grocery Store</td>
      <td>Pizza Place</td>
      <td>Burger Joint</td>
    </tr>
  </tbody>
</table>
</div>




```python
# create map
map_finalcluster = folium.Map(location=[latitude, longitude], zoom_start=11)

# set color scheme for the clusters
x = np.arange(kclusters)
ys = [i+x+(i*x)**2 for i in range(kclusters)]
colors_array = cm.rainbow(np.linspace(0, 1, len(ys)))
rainbow = [colors.rgb2hex(i) for i in colors_array]

# add markers to the map
markers_colors = []
for lat, lon, poi, cluster in zip(final_cluster['Latitude'], final_cluster['Longitude'], final_cluster['Neighborhood'], final_cluster['Group Labels']):
    label = folium.Popup(str(poi) + ' Cluster ' + str(cluster), parse_html=True)
    folium.CircleMarker(
        [lat, lon],
        radius=5,
        popup=label,
        color=rainbow[cluster-1],
        fill=True,
        fill_color=rainbow[cluster-1],
        fill_opacity=0.7).add_to(map_finalcluster)
       
map_finalcluster
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfYzc0NzI4NjlhYzAzNDQwNWExNWFmYjgxMTI0NTAzNjUgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwX2M3NDcyODY5YWMwMzQ0MDVhMTVhZmI4MTEyNDUwMzY1IiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF9jNzQ3Mjg2OWFjMDM0NDA1YTE1YWZiODExMjQ1MDM2NSA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF9jNzQ3Mjg2OWFjMDM0NDA1YTE1YWZiODExMjQ1MDM2NScsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNTEuNTA5ODY1LC0wLjExODA5Ml0sCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB6b29tOiAxMSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbGF5ZXJzOiBbXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHdvcmxkQ29weUp1bXA6IGZhbHNlLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NwogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB9KTsKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfNzU0MmUwN2U3ZTA2NGEyMzhmZDM4NWZiN2RiNDYzZTUgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICdodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZycsCiAgICAgICAgICAgICAgICB7CiAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgIm1heFpvb20iOiAxOCwKICAibWluWm9vbSI6IDEsCiAgIm5vV3JhcCI6IGZhbHNlLAogICJzdWJkb21haW5zIjogImFiYyIKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYzc0NzI4NjlhYzAzNDQwNWExNWFmYjgxMTI0NTAzNjUpOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzE1OTU3YjFhZjhlYjQ4NjU4NzNmMjU1YzFiYjM3ODlhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA4ODA1OCwtMC4wNjE1NTIxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2M3NDcyODY5YWMwMzQ0MDVhMTVhZmI4MTEyNDUwMzY1KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2U3NzRjZDkyNjU0NDRhYWJhOWVlODVmNWY4N2Y2YjA1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2FmYjUyMTA0NzcyYjQ5ZjZhZTY5OGQyZDkyNWM3ZjAwID0gJCgnPGRpdiBpZD0iaHRtbF9hZmI1MjEwNDc3MmI0OWY2YWU2OThkMmQ5MjVjN2YwMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEFsZGdhdGUgKHBhcnQpLCBCaXNob3BzZ2F0ZSAocGFydCksIFdoaXRlY2hhcGVsIChtb3N0bHkpLCBTaG9yZWRpdGNoIChwYXJ0KSwgU3BpdGFsZmllbGRzLCBTaGFkd2VsbCwgU3RlcG5leSAobW9zdGx5KSwgTWlsZSBFbmQgKHBhcnQpLCBQb3J0c29rZW4gQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9lNzc0Y2Q5MjY1NDQ0YWFiYTllZTg1ZjVmODdmNmIwNS5zZXRDb250ZW50KGh0bWxfYWZiNTIxMDQ3NzJiNDlmNmFlNjk4ZDJkOTI1YzdmMDApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMTU5NTdiMWFmOGViNDg2NTg3M2YyNTVjMWJiMzc4OWEuYmluZFBvcHVwKHBvcHVwX2U3NzRjZDkyNjU0NDRhYWJhOWVlODVmNWY4N2Y2YjA1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2E0NGE2MjM1OTgxMDRhMmFhMGFmMGVjZGNkODEwZTdhID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA1Nzk1NywtMC4wNTcxOTkxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2M3NDcyODY5YWMwMzQ0MDVhMTVhZmI4MTEyNDUwMzY1KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzljOTk2NjAwYWI5NTQzZjlhZGZmYjhmYjY1N2YwODhhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzY2MDA2YzViNGYyZjQ1ZDNiYjBmYmE2MTI3ZTRkZWFlID0gJCgnPGRpdiBpZD0iaHRtbF82NjAwNmM1YjRmMmY0NWQzYmIwZmJhNjEyN2U0ZGVhZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2FwcGluZywgU3QgS2F0aGFyaW5lIERvY2tzLCBTdGVwbmV5IChwYXJ0KSwgU2hhZHdlbGwgKHBhcnQpLCBXaGl0ZWNoYXBlbCAocGFydCkgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF85Yzk5NjYwMGFiOTU0M2Y5YWRmZmI4ZmI2NTdmMDg4YS5zZXRDb250ZW50KGh0bWxfNjYwMDZjNWI0ZjJmNDVkM2JiMGZiYTYxMjdlNGRlYWUpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYTQ0YTYyMzU5ODEwNGEyYWEwYWYwZWNkY2Q4MTBlN2EuYmluZFBvcHVwKHBvcHVwXzljOTk2NjAwYWI5NTQzZjlhZGZmYjhmYjY1N2YwODhhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2NmZDZjNDYyMDc1NzQ5ZWQ5MTBjODNhMTU2NzhhMTIyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTI4MTMzOTk5OTk5OTksLTAuMDYxNjUxMTk5OTk5OTk5OTldLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYzc0NzI4NjlhYzAzNDQwNWExNWFmYjgxMTI0NTAzNjUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOTZhMDkxYmQ5Yzk2NDEzZTljMjQ3ODY4ODQ4MjFjYzQgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfMTQyOGQyNTU1NDAyNGQ5NGEwMjI1MDUwOTU3NmJjYmIgPSAkKCc8ZGl2IGlkPSJodG1sXzE0MjhkMjU1NTQwMjRkOTRhMDIyNTA1MDk1NzZiY2JiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQmV0aG5hbCBHcmVlbiAobW9zdGx5KSwgSGFnZ2Vyc3RvbiwgSG94dG9uIChwYXJ0KSwgU2hvcmVkaXRjaCAocGFydCksIENhbWJyaWRnZSBIZWF0aCAobW9zdGx5KSwgR2xvYmUgVG93biBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzk2YTA5MWJkOWM5NjQxM2U5YzI0Nzg2ODg0ODIxY2M0LnNldENvbnRlbnQoaHRtbF8xNDI4ZDI1NTU0MDI0ZDk0YTAyMjUwNTA5NTc2YmNiYik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl9jZmQ2YzQ2MjA3NTc0OWVkOTEwYzgzYTE1Njc4YTEyMi5iaW5kUG9wdXAocG9wdXBfOTZhMDkxYmQ5Yzk2NDEzZTljMjQ3ODY4ODQ4MjFjYzQpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfNGYxN2U3YjEyZTdlNDZjZTk3MmM5NGQ0Y2E0OTk5YTMgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41MjgxNDgzLC0wLjAyMDU0MTZdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYzc0NzI4NjlhYzAzNDQwNWExNWFmYjgxMTI0NTAzNjUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfZTM5YTk2MDZiZWNmNGUyN2I0YTg5OTVlNjU2MWFhYmMgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNmI0YTY2MGU2MGQxNGQ5ZGJiODIyMmZhNGUyZmY3ODQgPSAkKCc8ZGl2IGlkPSJodG1sXzZiNGE2NjBlNjBkMTRkOWRiYjgyMjJmYTRlMmZmNzg0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQm93LCBCb3cgQ29tbW9uLCBCcm9tbGV5LWJ5LUJvdywgT2xkIEZvcmQsIE1pbGUgRW5kIChtb3N0bHkpLCBGaXNoIElzbGFuZCxNaWxsIE1lYWRzIChwYXJ0KSwgU3RyYXRmb3JkIChwYXJ0KSwgV2VzdCBIYW0gKHBhcnQpIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZTM5YTk2MDZiZWNmNGUyN2I0YTg5OTVlNjU2MWFhYmMuc2V0Q29udGVudChodG1sXzZiNGE2NjBlNjBkMTRkOWRiYjgyMjJmYTRlMmZmNzg0KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzRmMTdlN2IxMmU3ZTQ2Y2U5NzJjOTRkNGNhNDk5OWEzLmJpbmRQb3B1cChwb3B1cF9lMzlhOTYwNmJlY2Y0ZTI3YjRhODk5NWU2NTYxYWFiYyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8yYzBlYjdjNGQzMjc0MGRjYjA4YjA0NDc3MTNiMzdiZiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUyNTUwNjgsMC4wNTg3MDgxMDAwMDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9jNzQ3Mjg2OWFjMDM0NDA1YTE1YWZiODExMjQ1MDM2NSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF84YmNlYmMyYzIxMzU0NTllODZjN2VmMDY4OTA0MDEzOSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF80NzhiMDQxYjRhMTk0MDMwYTRlYmI3M2UwNGI5MmRiMyA9ICQoJzxkaXYgaWQ9Imh0bWxfNDc4YjA0MWI0YTE5NDAzMGE0ZWJiNzNlMDRiOTJkYjMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBFYXN0IEhhbSwgQmVja3RvbiwgVXB0b24gUGFyayAocGFydCksIEJhcmtpbmcgKHBhcnQpIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOGJjZWJjMmMyMTM1NDU5ZTg2YzdlZjA2ODkwNDAxMzkuc2V0Q29udGVudChodG1sXzQ3OGIwNDFiNGExOTQwMzBhNGViYjczZTA0YjkyZGIzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzJjMGViN2M0ZDMyNzQwZGNiMDhiMDQ0NzcxM2IzN2JmLmJpbmRQb3B1cChwb3B1cF84YmNlYmMyYzIxMzU0NTllODZjN2VmMDY4OTA0MDEzOSk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9jMjMwMTY5ZTRjNTA0YTllYjg0MjhmZTYwZDVhYTcwNCA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU1MTgwOTQsMC4wMjkzNzI4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2M3NDcyODY5YWMwMzQ0MDVhMTVhZmI4MTEyNDUwMzY1KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzQxMWEzYWYyNjI2MzQ3OGY5Nzc0NDQ1ODViNWRhMTRhID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzQ3YWUxYTkwZjY4OTQ0MjBhZDc5MDU3YzEzZDEyODQ2ID0gJCgnPGRpdiBpZD0iaHRtbF80N2FlMWE5MGY2ODk0NDIwYWQ3OTA1N2MxM2QxMjg0NiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEZvcmVzdCBHYXRlLCBMZXl0b25zdG9uZSAoUGFydCkgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF80MTFhM2FmMjYyNjM0NzhmOTc3NDQ0NTg1YjVkYTE0YS5zZXRDb250ZW50KGh0bWxfNDdhZTFhOTBmNjg5NDQyMGFkNzkwNTdjMTNkMTI4NDYpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfYzIzMDE2OWU0YzUwNGE5ZWI4NDI4ZmU2MGQ1YWE3MDQuYmluZFBvcHVwKHBvcHVwXzQxMWEzYWYyNjI2MzQ3OGY5Nzc0NDQ1ODViNWRhMTRhKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzY4ZWI3OWIwNGU4YjQ3YmI5MDZjYjdkNzU2MWM2OWMyID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTQzOTA4MywtMC4wNjE2ODYxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2M3NDcyODY5YWMwMzQ0MDVhMTVhZmI4MTEyNDUwMzY1KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzY2OWZmMTU5MWQwNTQ1MWZiOTRlZjcxNzg0MzU2NTQ1ID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2E4MTFlMGQwZWYyNzQwYmJhZGFkYzJjNzI1N2FhNGViID0gJCgnPGRpdiBpZD0iaHRtbF9hODExZTBkMGVmMjc0MGJiYWRhZGMyYzcyNTdhYTRlYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IEhhY2tuZXkgQ2VudHJhbCwgRGFsc3RvbiwgTG9uZG9uIEZpZWxkcywgU3Rva2UgTmV3aW5ndG9uIChwYXJ0KSwgQ2FtYnJpZGdlIEhlYXRoIChwYXJ0KSBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzY2OWZmMTU5MWQwNTQ1MWZiOTRlZjcxNzg0MzU2NTQ1LnNldENvbnRlbnQoaHRtbF9hODExZTBkMGVmMjc0MGJiYWRhZGMyYzcyNTdhYTRlYik7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl82OGViNzliMDRlOGI0N2JiOTA2Y2I3ZDc1NjFjNjljMi5iaW5kUG9wdXAocG9wdXBfNjY5ZmYxNTkxZDA1NDUxZmI5NGVmNzE3ODQzNTY1NDUpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfZTMxMWE4ODg2YWQ5NGJlMWEwMDE2MWJhNTljN2NkMTQgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41NDEyODc5OTk5OTk5OSwtMC4wNDExMTE0MDAwMDAwMDAwMV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9jNzQ3Mjg2OWFjMDM0NDA1YTE1YWZiODExMjQ1MDM2NSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8xYmIyYjdmMDhjYTE0N2M4YWIzNDhhMzEyNTNlOTM0MSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF9hYWNkMjdjNjJhZGI0NmQ1YTgyOWMyOTc4YmEzODIyZCA9ICQoJzxkaXYgaWQ9Imh0bWxfYWFjZDI3YzYyYWRiNDZkNWE4MjljMjk3OGJhMzgyMmQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBIb21lcnRvbiwgSGFja25leSBXaWNrLCBTb3V0aCBIYWNrbmV5LCBIYWNrbmV5IE1hcnNoZXMsIENhbWJyaWRnZSBIZWF0aCAocGFydCkgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF8xYmIyYjdmMDhjYTE0N2M4YWIzNDhhMzEyNTNlOTM0MS5zZXRDb250ZW50KGh0bWxfYWFjZDI3YzYyYWRiNDZkNWE4MjljMjk3OGJhMzgyMmQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTMxMWE4ODg2YWQ5NGJlMWEwMDE2MWJhNTljN2NkMTQuYmluZFBvcHVwKHBvcHVwXzFiYjJiN2YwOGNhMTQ3YzhhYjM0OGEzMTI1M2U5MzQxKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2ZlYjM1NWE1NWE4YTQ0MGE5YjZlMzcyMTNjYzRhYzkwID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTY0OTYxOSwtMC4wMTQ2OTExXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2M3NDcyODY5YWMwMzQ0MDVhMTVhZmI4MTEyNDUwMzY1KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzVmZTRjYWQxNmYxMDRkZTNhOTBlZTI5MDA0Y2ZhMWQzID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzQ1MDU5YmY3ZWFiZDQ2Y2JiZjlmODBkNzgyZjA3NWUzID0gJCgnPGRpdiBpZD0iaHRtbF80NTA1OWJmN2VhYmQ0NmNiYmY5ZjgwZDc4MmYwNzVlMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IExleXRvbiwgVGVtcGxlIE1pbGxzLCBIYWNrbmV5IE1hcnNoZXMgKHBhcnQpIFVwcGVyIENsYXB0b24gKHBhcnQpLCBXYWx0aGFtc3RvdyBNYXJzaGVzIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNWZlNGNhZDE2ZjEwNGRlM2E5MGVlMjkwMDRjZmExZDMuc2V0Q29udGVudChodG1sXzQ1MDU5YmY3ZWFiZDQ2Y2JiZjlmODBkNzgyZjA3NWUzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2ZlYjM1NWE1NWE4YTQ0MGE5YjZlMzcyMTNjYzRhYzkwLmJpbmRQb3B1cChwb3B1cF81ZmU0Y2FkMTZmMTA0ZGUzYTkwZWUyOTAwNGNmYTFkMyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl80ZmJjNjAxZjZhZmM0YzFhODMyM2E5YzhiMjFhODkyOSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU3Mjg1MjUsMC4wMTc2MzQ4XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2M3NDcyODY5YWMwMzQ0MDVhMTVhZmI4MTEyNDUwMzY1KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2ZkZWU1NmI3ZDkxZTRkOWQ4OTE3MDYwNWVkNDg2MmFiID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzZjMTRjZThjZTUzMjRhNjNiZWY3YzFmZjM3NDdmOWU4ID0gJCgnPGRpdiBpZD0iaHRtbF82YzE0Y2U4Y2U1MzI0YTYzYmVmN2MxZmYzNzQ3ZjllOCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IExleXRvbnN0b25lLCBXYW5zdGVhZCwgQWxkZXJzYnJvb2sgKHBhcnQpLCBTbmFyZXNicm9vaywgQ2FubiBIYWxsIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfZmRlZTU2YjdkOTFlNGQ5ZDg5MTcwNjA1ZWQ0ODYyYWIuc2V0Q29udGVudChodG1sXzZjMTRjZThjZTUzMjRhNjNiZWY3YzFmZjM3NDdmOWU4KTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzRmYmM2MDFmNmFmYzRjMWE4MzIzYTljOGIyMWE4OTI5LmJpbmRQb3B1cChwb3B1cF9mZGVlNTZiN2Q5MWU0ZDlkODkxNzA2MDVlZDQ4NjJhYik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl84ZjU5NWVmYjFjZDU0MjBkOTk0NGYwMDM3OTE1MWRiYSA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjUzMDc3NTI5OTk5OTk5LDAuMDI5MzUwNl0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9jNzQ3Mjg2OWFjMDM0NDA1YTE1YWZiODExMjQ1MDM2NSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF8zMTdmMjllNDFiZGY0NTBhYjJiZDBlYTBkOTY1MmRmMiA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF8zMjJkZDgyNDBiOGM0OTgzYWJkMmY3YTcwY2RjYjNhYSA9ICQoJzxkaXYgaWQ9Imh0bWxfMzIyZGQ4MjQwYjhjNDk4M2FiZDJmN2E3MGNkY2IzYWEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBQbGFpc3RvdywgV2VzdCBIYW0gKHBhcnQpLCBVcHRvbiBQYXJrIChwYXJ0KSBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzMxN2YyOWU0MWJkZjQ1MGFiMmJkMGVhMGQ5NjUyZGYyLnNldENvbnRlbnQoaHRtbF8zMjJkZDgyNDBiOGM0OTgzYWJkMmY3YTcwY2RjYjNhYSk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl84ZjU5NWVmYjFjZDU0MjBkOTk0NGYwMDM3OTE1MWRiYS5iaW5kUG9wdXAocG9wdXBfMzE3ZjI5ZTQxYmRmNDUwYWIyYmQwZWEwZDk2NTJkZjIpOwoKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIGNpcmNsZV9tYXJrZXJfY2ZmNzc2NmE5Y2MzNDU4NGE5MzZlMTJlMDcwMzkzNjIgPSBMLmNpcmNsZU1hcmtlcigKICAgICAgICAgICAgICAgIFs1MS41NDEyOTUsMC4wMDU4NzA5XSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwX2M3NDcyODY5YWMwMzQ0MDVhMTVhZmI4MTEyNDUwMzY1KTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwXzk0NmEwNzkwZDY5MDRmNGQ5YjkyOTg4Zjc2NTFiMGEyID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sX2MyOWE3MTJhMDYyNjRjYzM4NTBhY2NjOWJiMDY1ODMwID0gJCgnPGRpdiBpZD0iaHRtbF9jMjlhNzEyYTA2MjY0Y2MzODUwYWNjYzliYjA2NTgzMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+IFN0cmF0Zm9yZCwgV2VzdCBIYW0gKHBhcnQpLCBNYXJ5bGFuZCwgTGV5dG9uIChwYXJ0KSwgTGV5dG9uc3RvbmUgKHBhcnQpIFRlbXBsZSBNaWxscyAocGFydCksIEhhY2tuZXkgV2ljayAocGFydCksIEJvdyAocGFydCkgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF85NDZhMDc5MGQ2OTA0ZjRkOWI5Mjk4OGY3NjUxYjBhMi5zZXRDb250ZW50KGh0bWxfYzI5YTcxMmEwNjI2NGNjMzg1MGFjY2M5YmIwNjU4MzApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfY2ZmNzc2NmE5Y2MzNDU4NGE5MzZlMTJlMDcwMzkzNjIuYmluZFBvcHVwKHBvcHVwXzk0NmEwNzkwZDY5MDRmNGQ5YjkyOTg4Zjc2NTFiMGEyKTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzM2ZmViMTk3M2UyMjRkMWJhMjMwOTkxNGRkMmE0ZWNiID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA5NzQ3OCwwLjAyOTMyODVdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYzc0NzI4NjlhYzAzNDQwNWExNWFmYjgxMTI0NTAzNjUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfOThkN2NjMzM4YjgxNDEzODhmMTNiYWYyYzA3NzhhMjYgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfYzI5ZWFkZGU1MDAzNGZhNDlmMWY4YjBmN2NiZDI0NjMgPSAkKCc8ZGl2IGlkPSJodG1sX2MyOWVhZGRlNTAwMzRmYTQ5ZjFmOGIwZjdjYmQyNDYzIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gQ2FubmluZyBUb3duLCBTaWx2ZXJ0b3duLCBSb3lhbCBEb2NrcywgTm9ydGggV29vbHdpY2gsIEJlY2t0b24gKHBhcnQpIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfOThkN2NjMzM4YjgxNDEzODhmMTNiYWYyYzA3NzhhMjYuc2V0Q29udGVudChodG1sX2MyOWVhZGRlNTAwMzRmYTQ5ZjFmOGIwZjdjYmQyNDYzKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyXzM2ZmViMTk3M2UyMjRkMWJhMjMwOTkxNGRkMmE0ZWNiLmJpbmRQb3B1cChwb3B1cF85OGQ3Y2MzMzhiODE0MTM4OGYxM2JhZjJjMDc3OGEyNik7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl9lMGZjM2QzNDJhYWI0Y2JmOWUxMzJmYjhjYTRhNWVjMyA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU5MDE3NjksLTAuMDE3MzQzN10sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9jNzQ3Mjg2OWFjMDM0NDA1YTE1YWZiODExMjQ1MDM2NSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF9mNzFmMDdhMWFiMDg0YTMwODcxZjFiMDM4ZjJiYTljNSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF85ZjU3ZmUwNTY5MzY0MTA5OGI3MWNkYTFlMDRkODEzMCA9ICQoJzxkaXYgaWQ9Imh0bWxfOWY1N2ZlMDU2OTM2NDEwOThiNzFjZGExZTA0ZDgxMzAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBXYWx0aGFtc3RvdywgVXBwZXIgV2FsdGhhbXN0b3csIExleXRvbiAocGFydCkgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9mNzFmMDdhMWFiMDg0YTMwODcxZjFiMDM4ZjJiYTljNS5zZXRDb250ZW50KGh0bWxfOWY1N2ZlMDU2OTM2NDEwOThiNzFjZGExZTA0ZDgxMzApOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfZTBmYzNkMzQyYWFiNGNiZjllMTMyZmI4Y2E0YTVlYzMuYmluZFBvcHVwKHBvcHVwX2Y3MWYwN2ExYWIwODRhMzA4NzFmMWIwMzhmMmJhOWM1KTsKCiAgICAgICAgICAgIAogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyX2RjZmE2YTI4ZjEwNTQzN2NhNmEzZTg2NWE1YTE2YjRjID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTkxMjY3MSwwLjAyNjQ3MjFdLAogICAgICAgICAgICAgICAgewogICJidWJibGluZ01vdXNlRXZlbnRzIjogdHJ1ZSwKICAiY29sb3IiOiAiIzgwMDBmZiIsCiAgImRhc2hBcnJheSI6IG51bGwsCiAgImRhc2hPZmZzZXQiOiBudWxsLAogICJmaWxsIjogdHJ1ZSwKICAiZmlsbENvbG9yIjogIiM4MDAwZmYiLAogICJmaWxsT3BhY2l0eSI6IDAuNywKICAiZmlsbFJ1bGUiOiAiZXZlbm9kZCIsCiAgImxpbmVDYXAiOiAicm91bmQiLAogICJsaW5lSm9pbiI6ICJyb3VuZCIsCiAgIm9wYWNpdHkiOiAxLjAsCiAgInJhZGl1cyI6IDUsCiAgInN0cm9rZSI6IHRydWUsCiAgIndlaWdodCI6IDMKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfYzc0NzI4NjlhYzAzNDQwNWExNWFmYjgxMTI0NTAzNjUpOwogICAgICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgcG9wdXBfNDAwMWE2OTRmN2IzNGU2NDk2ZTM4OWE2YzY4ZmFlYzcgPSBMLnBvcHVwKHttYXhXaWR0aDogJzMwMCd9KTsKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGh0bWxfNjhiNGM3ZTZkNmUxNDZjNWI0MmUxMGNjMDUwYWI3MmMgPSAkKCc8ZGl2IGlkPSJodG1sXzY4YjRjN2U2ZDZlMTQ2YzViNDJlMTBjYzA1MGFiNzJjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij4gV29vZGZvcmQsIFNvdXRoIFdvb2Rmb3JkIENsdXN0ZXIgMTwvZGl2PicpWzBdOwogICAgICAgICAgICAgICAgcG9wdXBfNDAwMWE2OTRmN2IzNGU2NDk2ZTM4OWE2YzY4ZmFlYzcuc2V0Q29udGVudChodG1sXzY4YjRjN2U2ZDZlMTQ2YzViNDJlMTBjYzA1MGFiNzJjKTsKICAgICAgICAgICAgCgogICAgICAgICAgICBjaXJjbGVfbWFya2VyX2RjZmE2YTI4ZjEwNTQzN2NhNmEzZTg2NWE1YTE2YjRjLmJpbmRQb3B1cChwb3B1cF80MDAxYTY5NGY3YjM0ZTY0OTZlMzg5YTZjNjhmYWVjNyk7CgogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgY2lyY2xlX21hcmtlcl8zMDU4ZWE3NDliNGU0MmYxYTRkYWQxNzE0Y2E2ZDE1YiA9IEwuY2lyY2xlTWFya2VyKAogICAgICAgICAgICAgICAgWzUxLjU0MzkyNDEsLTAuMDA4ODA3NV0sCiAgICAgICAgICAgICAgICB7CiAgImJ1YmJsaW5nTW91c2VFdmVudHMiOiB0cnVlLAogICJjb2xvciI6ICIjODAwMGZmIiwKICAiZGFzaEFycmF5IjogbnVsbCwKICAiZGFzaE9mZnNldCI6IG51bGwsCiAgImZpbGwiOiB0cnVlLAogICJmaWxsQ29sb3IiOiAiIzgwMDBmZiIsCiAgImZpbGxPcGFjaXR5IjogMC43LAogICJmaWxsUnVsZSI6ICJldmVub2RkIiwKICAibGluZUNhcCI6ICJyb3VuZCIsCiAgImxpbmVKb2luIjogInJvdW5kIiwKICAib3BhY2l0eSI6IDEuMCwKICAicmFkaXVzIjogNSwKICAic3Ryb2tlIjogdHJ1ZSwKICAid2VpZ2h0IjogMwp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF9jNzQ3Mjg2OWFjMDM0NDA1YTE1YWZiODExMjQ1MDM2NSk7CiAgICAgICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBwb3B1cF85ZDNlMjU5ZjBlODk0MzVkOTI1OGM0YzAxZTI3ODE3OSA9IEwucG9wdXAoe21heFdpZHRoOiAnMzAwJ30pOwoKICAgICAgICAgICAgCiAgICAgICAgICAgICAgICB2YXIgaHRtbF84NzI4NDMxYmMwNjE0YjgwYjJmZWQwOGZiZDdmN2Y0MyA9ICQoJzxkaXYgaWQ9Imh0bWxfODcyODQzMWJjMDYxNGI4MGIyZmVkMDhmYmQ3ZjdmNDMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPiBPbHltcGljIFBhcmssICZhbXA7IHBhcnRzIG9mIFN0cmF0Zm9yZCwgSG9tZXJ0b24sIExleXRvbiwgQm93OyBDbHVzdGVyIDE8L2Rpdj4nKVswXTsKICAgICAgICAgICAgICAgIHBvcHVwXzlkM2UyNTlmMGU4OTQzNWQ5MjU4YzRjMDFlMjc4MTc5LnNldENvbnRlbnQoaHRtbF84NzI4NDMxYmMwNjE0YjgwYjJmZWQwOGZiZDdmN2Y0Myk7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgY2lyY2xlX21hcmtlcl8zMDU4ZWE3NDliNGU0MmYxYTRkYWQxNzE0Y2E2ZDE1Yi5iaW5kUG9wdXAocG9wdXBfOWQzZTI1OWYwZTg5NDM1ZDkyNThjNGMwMWUyNzgxNzkpOwoKICAgICAgICAgICAgCiAgICAgICAgCjwvc2NyaXB0Pg==" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



## Select the optimal Neighborhood


```python
# Looking at data from cluster 2, there might be an opportunity in E1 & E1W PostCodes.Other neighborhoods seem to have some bigger presence of food industry
# E1 & E1W are closest to central London


final_neighborhood=final_cluster.loc[final_cluster['PostCode'] == 'E1W',final_cluster.columns[[1] + list(range(0, final_cluster.shape[1]))]]
final_neighborhood
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
      <th>Area Category</th>
      <th>Group Labels</th>
      <th>Area Category</th>
      <th>PostCode</th>
      <th>Neighborhood</th>
      <th>Area</th>
      <th>District</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>1st Most Common Venue</th>
      <th>2nd Most Common Venue</th>
      <th>3rd Most Common Venue</th>
      <th>4th Most Common Venue</th>
      <th>5th Most Common Venue</th>
      <th>6th Most Common Venue</th>
      <th>7th Most Common Venue</th>
      <th>8th Most Common Venue</th>
      <th>9th Most Common Venue</th>
      <th>10th Most Common Venue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>East</td>
      <td>1</td>
      <td>East</td>
      <td>E1W</td>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>Tower Hamlets</td>
      <td>Wapping, St Katharine Docks, Stepney (part), S...</td>
      <td>51.505796</td>
      <td>-0.057199</td>
      <td>Park</td>
      <td>Coffee Shop</td>
      <td>Pub</td>
      <td>Italian Restaurant</td>
      <td>Grocery Store</td>
      <td>Gym / Fitness Center</td>
      <td>Chinese Restaurant</td>
      <td>Food &amp; Drink Shop</td>
      <td>Fast Food Restaurant</td>
      <td>Liquor Store</td>
    </tr>
  </tbody>
</table>
</div>




```python
# create map
map_final_neighborhood = folium.Map(location=[latitude, longitude], zoom_start=11)

# set color scheme for the clusters
x = np.arange(kclusters)
ys = [i+x+(i*x)**2 for i in range(kclusters)]
colors_array = cm.rainbow(np.linspace(0, 1, len(ys)))
rainbow = [colors.rgb2hex(i) for i in colors_array]

# add markers to the map
markers_colors = []
for lat, lon, poi, cluster in zip(final_neighborhood['Latitude'], final_neighborhood['Longitude'], final_neighborhood['Neighborhood'], final_neighborhood['Group Labels']):
    label = folium.Popup(str(poi) + ' Cluster ' + str(cluster), parse_html=True)
    folium.CircleMarker(
        [lat, lon],
        radius=5,
        popup=label,
        color=rainbow[cluster-1],
        fill=True,
        fill_color=rainbow[cluster-1],
        fill_opacity=0.7).add_to(map_final_neighborhood)
       
map_final_neighborhood
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfNGQ2YjFmMjVlZmU1NDdmOWIwOTk2ZTIwMDdlYjdjODMgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwXzRkNmIxZjI1ZWZlNTQ3ZjliMDk5NmUyMDA3ZWI3YzgzIiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF80ZDZiMWYyNWVmZTU0N2Y5YjA5OTZlMjAwN2ViN2M4MyA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF80ZDZiMWYyNWVmZTU0N2Y5YjA5OTZlMjAwN2ViN2M4MycsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNTEuNTA5ODY1LC0wLjExODA5Ml0sCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB6b29tOiAxMSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIG1heEJvdW5kczogYm91bmRzLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbGF5ZXJzOiBbXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHdvcmxkQ29weUp1bXA6IGZhbHNlLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgY3JzOiBMLkNSUy5FUFNHMzg1NwogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB9KTsKICAgICAgICAgICAgCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfOWMwOWQ0ZjUzY2Q1NDhiOGIxYTI4N2NjOTdhYTg4ZTAgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICdodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZycsCiAgICAgICAgICAgICAgICB7CiAgImF0dHJpYnV0aW9uIjogbnVsbCwKICAiZGV0ZWN0UmV0aW5hIjogZmFsc2UsCiAgIm1heFpvb20iOiAxOCwKICAibWluWm9vbSI6IDEsCiAgIm5vV3JhcCI6IGZhbHNlLAogICJzdWJkb21haW5zIjogImFiYyIKfQogICAgICAgICAgICAgICAgKS5hZGRUbyhtYXBfNGQ2YjFmMjVlZmU1NDdmOWIwOTk2ZTIwMDdlYjdjODMpOwogICAgICAgIAogICAgCiAgICAgICAgICAgIHZhciBjaXJjbGVfbWFya2VyXzBhYmVjZDA3ZWNiZjQ3YTg5ZWIwN2I5OTI2MGZjODI1ID0gTC5jaXJjbGVNYXJrZXIoCiAgICAgICAgICAgICAgICBbNTEuNTA1Nzk1NywtMC4wNTcxOTkxXSwKICAgICAgICAgICAgICAgIHsKICAiYnViYmxpbmdNb3VzZUV2ZW50cyI6IHRydWUsCiAgImNvbG9yIjogIiM4MDAwZmYiLAogICJkYXNoQXJyYXkiOiBudWxsLAogICJkYXNoT2Zmc2V0IjogbnVsbCwKICAiZmlsbCI6IHRydWUsCiAgImZpbGxDb2xvciI6ICIjODAwMGZmIiwKICAiZmlsbE9wYWNpdHkiOiAwLjcsCiAgImZpbGxSdWxlIjogImV2ZW5vZGQiLAogICJsaW5lQ2FwIjogInJvdW5kIiwKICAibGluZUpvaW4iOiAicm91bmQiLAogICJvcGFjaXR5IjogMS4wLAogICJyYWRpdXMiOiA1LAogICJzdHJva2UiOiB0cnVlLAogICJ3ZWlnaHQiOiAzCn0KICAgICAgICAgICAgICAgICkuYWRkVG8obWFwXzRkNmIxZjI1ZWZlNTQ3ZjliMDk5NmUyMDA3ZWI3YzgzKTsKICAgICAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHBvcHVwX2FmMGQyYTA4MTNlNjQ2ZGNhOTc2NDgyNTBlNjkxMTVlID0gTC5wb3B1cCh7bWF4V2lkdGg6ICczMDAnfSk7CgogICAgICAgICAgICAKICAgICAgICAgICAgICAgIHZhciBodG1sXzgyMTFlNzZlYzkwOTQyN2U5MmRlNzJmZmY5MjI5NjlkID0gJCgnPGRpdiBpZD0iaHRtbF84MjExZTc2ZWM5MDk0MjdlOTJkZTcyZmZmOTIyOTY5ZCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+V2FwcGluZywgU3QgS2F0aGFyaW5lIERvY2tzLCBTdGVwbmV5IChwYXJ0KSwgU2hhZHdlbGwgKHBhcnQpLCBXaGl0ZWNoYXBlbCAocGFydCkgQ2x1c3RlciAxPC9kaXY+JylbMF07CiAgICAgICAgICAgICAgICBwb3B1cF9hZjBkMmEwODEzZTY0NmRjYTk3NjQ4MjUwZTY5MTE1ZS5zZXRDb250ZW50KGh0bWxfODIxMWU3NmVjOTA5NDI3ZTkyZGU3MmZmZjkyMjk2OWQpOwogICAgICAgICAgICAKCiAgICAgICAgICAgIGNpcmNsZV9tYXJrZXJfMGFiZWNkMDdlY2JmNDdhODllYjA3Yjk5MjYwZmM4MjUuYmluZFBvcHVwKHBvcHVwX2FmMGQyYTA4MTNlNjQ2ZGNhOTc2NDgyNTBlNjkxMTVlKTsKCiAgICAgICAgICAgIAogICAgICAgIAo8L3NjcmlwdD4=" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



##We choose the E1W neighborhood closest to the centre of London in order to approach more customers,with the least food restaurants and with other entertainment venues nearby


```python
##PostCode: E1W
##Borough/District: Tower Hamlets
##Neighborhoods: Wapping, St Katharine Docks, Stepney, Shadwell, Whitechapel
```
