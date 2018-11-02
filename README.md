
README
================
Maria Guideng

# flavorsofcacao

### Data based on tasting ratings for plain dark chocolate bars.

**About this dataset**    
The source of the data is The Manhattan Chocolate Society and is made available by Brady and Andrea Brelinski via the [flavorsofcacao.com](http://flavorsofcacao.com/chocolate_database.html) website. The CSV file can be found in the `data` folder.

**File dimensions**    

| Filename | Total Rows | Total Columns | Columns |
|------------|------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| raw_df.csv | 1,937 | 9 | 'Company (Maker-if known)',      'Specific Bean Origin or Bar Name',     'REF',    'Review Date',    'Cocoa Percent',    'Company Location',     'Rating',    'Bean Type',     'Broad Bean Origin' |

**Exploration ideas**    
  * EDA using visualizations
  * Chocolate bar rating analysis

**To get the latest data**    
Updated as of 10/6/2018.


```python
# Prelims
import os as os
import requests
from bs4 import BeautifulSoup
import csv
import pandas as pd

# Set working directory to where CSV file will be saved, e.g.:
# os.chdir('C:\\Users\\...\\folder-name')

# Scraper
# Credit: https://stackoverflow.com/questions/52066389/scrape-table-built-with-spry-framework-using-beautifulsoup

r = requests.get('http://flavorsofcacao.com/database_w_REF.html')
soup = BeautifulSoup(r.content, 'html.parser')

with open('raw_df.csv', 'w', newline = '', encoding = 'utf-8') as f_output:
    csv_output = csv.writer(f_output)
    csv_output.writerow([th.get_text(strip = True) for th in soup.table.tr.find_all('th')])
    for tr in soup.table.find_all('tr')[1:]:
        csv_output.writerow([td.get_text(strip = True) for td in tr.find_all('td')])

# Import data & review
df = pd.read_csv('raw_df.csv', delimiter = ',')
print(df.info)
```

    <bound method DataFrame.info of            Company (Maker-if known)      Specific Bean Origin or Bar Name  \
    0                          A. Morin                               Bolivia   
    1                          A. Morin                                  Peru   
    2                          A. Morin                                Brazil   
    3                          A. Morin                              Birmanie   
    4                          A. Morin                              Colombie   
    5                          A. Morin                              Equateur   
    6                          A. Morin                            Madagascar   
    7                          A. Morin                                Panama   
    8                          A. Morin                      Papua New Guinea   
    9                          A. Morin                  Chanchamayo Province   
    10                         A. Morin                  Chanchamayo Province   
    11                         A. Morin                                 Piura   
    12                         A. Morin                                 Chuao   
    13                         A. Morin                                  Cuba   
    14                         A. Morin                               Pablino   
    15                         A. Morin                        Puerto Cabello   
    16                         A. Morin                          Sur del Lago   
    17                         A. Morin                              Carenero   
    18                         A. Morin                                Quilla   
    19                         A. Morin                                 Akata   
    20                         A. Morin                                Atsane   
    21                         A. Morin                                 Kpime   
    22                         A. Morin                           Agua Grande   
    23                           Acalli               Chulucanas, El Platanal   
    24                           Acalli                     Tumbes, Norandino   
    25    Adi aka Fijiana (Easy In Ltd)                            Vanua Levu   
    26    Adi aka Fijiana (Easy In Ltd)                Vanua Levu, Ami-Ami-CA   
    27    Adi aka Fijiana (Easy In Ltd)                    Vanua Levu, Toto-A   
    28    Adi aka Fijiana (Easy In Ltd)                            Vanua Levu   
    29               Aequare (Gianduja)             Los Rios, Quevedo, Arriba   
    ...                             ...                                   ...   
    1907                  Zart Pralinen        Kakao Kamili, Kilombero Valley   
    1908                  Zart Pralinen           San Juan Estate, Gran Couva   
    1909                  Zart Pralinen                       San Juan Estate   
    1910                         Zokoko                  Tranquilidad, Baures   
    1911                         Zokoko                             Alto Beni   
    1912                         Zokoko                               Tokiala   
    1913                         Zokoko                           Guadalcanal   
    1914                         Zokoko                         Goddess Blend   
    1915              Zoto (Chocolatoa)  El Castillero, batch ca1705, 3 turns   
    1916              Zoto (Chocolatoa)                                Rugoso   
    1917              Zoto (Chocolatoa)   Jeru Antiguo, batch jan705, 2 turns   
    1918                         Zotter                    Brazil, Mitzi Blue   
    1919                         Zotter                                 Congo   
    1920                         Zotter                          Kerala State   
    1921                         Zotter                          Kerala State   
    1922                         Zotter                                  Peru   
    1923                         Zotter                         El Ceibo Coop   
    1924                         Zotter                      Kongo, Highlands   
    1925                         Zotter    Loma Los Pinos, Yacao region, D.R.   
    1926                         Zotter                         Santo Domingo   
    1927                         Zotter                                El Oro   
    1928                         Zotter          Bocas del Toro, Cocabo Co-op   
    1929                         Zotter                          Huiwani Coop   
    1930                         Zotter     Satipo Pangoa region, 20hr conche   
    1931                         Zotter     Satipo Pangoa region, 16hr conche   
    1932                         Zotter                       Amazonas Frucht   
    1933                         Zotter                         Indianer, Raw   
    1934                         Zotter                                   Raw   
    1935                         Zotter                      APROCAFA, Acandi   
    1936                         Zotter       Dry Aged, 30 yr Anniversary bar   
    
           REF  Review Date Cocoa Percent Company Location  Rating  \
    0      797         2012           70%           France    3.50   
    1      797         2012           63%           France    3.75   
    2     1011         2013           70%           France    3.25   
    3     1015         2013           70%           France    3.00   
    4     1015         2013           70%           France    2.75   
    5     1011         2013           70%           France    3.75   
    6     1011         2013           70%           France    3.00   
    7     1011         2013           70%           France    2.75   
    8     1015         2013           70%           France    3.25   
    9     1019         2013           63%           France    4.00   
    10    1019         2013           70%           France    3.50   
    11    1019         2013           70%           France    3.25   
    12    1015         2013           70%           France    4.00   
    13    1315         2014           70%           France    3.50   
    14    1319         2014           70%           France    4.00   
    15    1319         2014           70%           France    3.75   
    16    1315         2014           70%           France    3.50   
    17    1315         2014           70%           France    2.75   
    18    1704         2015           70%           France    3.50   
    19    1680         2015           70%           France    3.50   
    20    1676         2015           70%           France    3.00   
    21    1676         2015           70%           France    2.75   
    22    1876         2016           63%           France    3.75   
    23    1462         2015           70%           U.S.A.    3.75   
    24    1470         2015           70%           U.S.A.    3.75   
    25     705         2011           88%             Fiji    3.50   
    26     705         2011           72%             Fiji    3.50   
    27     705         2011           80%             Fiji    3.25   
    28     705         2011           60%             Fiji    2.75   
    29     370         2009           70%          Ecuador    3.00   
    ...    ...          ...           ...              ...     ...   
    1907  1824         2016           85%          Austria    3.00   
    1908  1880         2016           78%          Austria    3.50   
    1909  1824         2016           85%          Austria    2.75   
    1910   701         2011           72%        Australia    3.75   
    1911   697         2011           68%        Australia    3.50   
    1912   701         2011           66%        Australia    3.50   
    1913  1716         2016           78%        Australia    3.75   
    1914  1780         2016           65%        Australia    3.25   
    1915  2048         2018           70%          Belgium    3.75   
    1916  2064         2018           75%          Belgium    3.25   
    1917  2044         2018           72%          Belgium    3.00   
    1918   486         2010           65%          Austria    3.00   
    1919   749         2011           65%          Austria    3.00   
    1920   749         2011           65%          Austria    3.50   
    1921   781         2011           62%          Austria    3.25   
    1922   647         2011           70%          Austria    3.75   
    1923   879         2012           90%          Austria    3.25   
    1924   883         2012           68%          Austria    3.25   
    1925   875         2012           62%          Austria    3.75   
    1926   879         2012           70%          Austria    3.75   
    1927   879         2012           75%          Austria    3.00   
    1928   801         2012           72%          Austria    3.50   
    1929   879         2012           75%          Austria    3.00   
    1930   875         2012           70%          Austria    3.50   
    1931   875         2012           70%          Austria    3.00   
    1932   801         2012           65%          Austria    3.50   
    1933   883         2012           58%          Austria    3.50   
    1934  1205         2014           80%          Austria    2.75   
    1935  1996         2017           75%          Austria    3.75   
    1936  2036         2018           75%          Austria    3.00   
    
                     Bean Type   Broad Bean Origin  
    0                      NaN             Bolivia  
    1                      NaN                Peru  
    2                      NaN              Brazil  
    3                      NaN               Burma  
    4                      NaN            Colombia  
    5                      NaN             Ecuador  
    6                  Criollo          Madagascar  
    7                      NaN              Panama  
    8                      NaN    Papua New Guinea  
    9                      NaN                Peru  
    10                     NaN                Peru  
    11                     NaN                Peru  
    12              Trinitario           Venezuela  
    13                     NaN                Cuba  
    14                     NaN                Peru  
    15                 Criollo           Venezuela  
    16                 Criollo           Venezuela  
    17                 Criollo           Venezuela  
    18                     NaN                Peru  
    19                     NaN                Togo  
    20                     NaN                Togo  
    21                     NaN                Togo  
    22                     NaN            Sao Tome  
    23                     NaN                Peru  
    24                 Criollo                Peru  
    25              Trinitario                Fiji  
    26              Trinitario                Fiji  
    27              Trinitario                Fiji  
    28              Trinitario                Fiji  
    29      Forastero (Arriba)             Ecuador  
    ...                    ...                 ...  
    1907   Criollo, Trinitario            Tanzania  
    1908            Trinitario            Trinidad  
    1909            Trinitario            Trinidad  
    1910                   NaN             Bolivia  
    1911                   NaN             Bolivia  
    1912            Trinitario    Papua New Guinea  
    1913                   NaN     Solomon Islands  
    1914                   NaN                 NaN  
    1915            Trinitario           Nicaragua  
    1916                   NaN           Nicaragua  
    1917            Trinitario           Nicaragua  
    1918                   NaN              Brazil  
    1919             Forastero               Congo  
    1920             Forastero               India  
    1921                   NaN               India  
    1922                   NaN                Peru  
    1923                   NaN             Bolivia  
    1924             Forastero               Congo  
    1925                   NaN  Dominican Republic  
    1926                   NaN  Dominican Republic  
    1927  Forastero (Nacional)             Ecuador  
    1928                   NaN              Panama  
    1929   Criollo, Trinitario    Papua New Guinea  
    1930      Criollo (Amarru)                Peru  
    1931      Criollo (Amarru)                Peru  
    1932                   NaN                 NaN  
    1933                   NaN                 NaN  
    1934                   NaN                 NaN  
    1935                   NaN            Colombia  
    1936                 blend               blend  
    
    [1937 rows x 9 columns]>
    
