## Step 3: Make List of Variables for Queries

Following the advice above, Table 3.1. lists the variables for this study. For each one, I would also get the variable "M" which gives accompanying margin of error. Just keeping them out to keep this short(er).

Table 3.1. Estimates for ACS 2024 and ACS  2025 1-year query
|Group | Name| Label |  Component <br> of Total? |
|--|--|--|--|
| Sex by Age<sup>[2]</sup> |B01001_001E| Estimate!!Total |  |
| Means of Transportation to Work<sup>[3]</sup>  | <br>B08301_001E| <br>Estimate!!Total |  |
|  | B08301_003E| > Car, truck, or van <br> > > Drove alone | 001E, 002E |
| | B08301_011E| > Public transportation <br> > > Bus  | Yes (001E, 010E  |
| | B08301_013E|  >  > Long-distance train or commuter rail | 001E, 010E|
| Vehicles Available<br>  by Electric Vehicles<sup>[4]</sup>   | B25147_001E	| Estimate!!Total:| | |
|  | B25147_003E	| >  1 or more vehicles available |  001E |
|  | B25147_004E	| >  > Electric vehicle available |  001E, 003E |

<sup>[2]</sup> -  [B01001](https://api.census.gov/data/2024/acs/acs1/groups/B01001.html) (Sex by Age). Only extracting this to get a total population-- not extracting by age or sex.<br>
<sup>[3]</sup>  -  [B08301](https://api.census.gov/data/2024/acs/acs1/groups/B08301.html) (Means of Transportation to Work). Did not include all options because I don't expect to sum them up to compare.<br>
<sup>[4]</sup>  -  2025 only. [B25147](https://api.census.gov/data/2025/acs/acs1/groups/B25147.html) (Vehicles Available by Electric Vehicles). Universe: Within occupied housing units. Did not select no vehicles available for this. 

## Step 4: Perform full queries for 2024 and 2025

I am separating out these queries so that only ~4 variables are requested at a time. Otherwise hard to keep track.
These are formatted for 2024, but just change the `2024` to `2025` !

### Population 

I will show full queries and results (both years for this, The rest only have 2024 query.

#### 2024 

``https://api.census.gov/data/2024/acs/acs1?get=NAME,B01001_001E,B01001_001M&for=place:36770,69000&in=state:06&key=[yourkeyhere]&descriptive=true``


Results:

> [["NAME","B01001_001E","B01001_001M","state","place"],
> ["Geographic Area Name","Estimate!!Total:","Margin of Error!!Total:","State","Place"],
> ["Irvine city, California","318693","56","06","36770"],
> ["Santa Ana city, California","316188","66","06","69000"]]

#### 2025

``https://api.census.gov/data/2025/acs/acs1?get=NAME,B01001_001E,B01001_001M&for=place:36770,69000&in=state:06&key=[yourkeyhere]&descriptive=true``
 
 Results:

> [["NAME","B01001_001E","B01001_001M","state","place"],
> ["Geographic Area Name","Estimate!!Total:","Margin of Error!!Total:","State","Place"],
> ["Irvine city, California","318758","55","06","36770"],
> ["Santa Ana city, California","315562","76","06","69000"]]

### Means of Transportation

Total and car/drove alone

``https://api.census.gov/data/2024/acs/acs1?get=NAME,B01001_001E,B01001_001M,B08301_003E,B08301_003M&for=place:36770,69000&in=state:06&key=[yourkeyhere]&descriptive=true``

> [["NAME","B01001_001E","B01001_001M","B08301_003E","B08301_003M","state","place"],
> ["Geographic Area Name","Estimate!!Total:","Margin of Error!!Total:","Estimate!!Total:!!Car, truck, or van:!!Drove alone","Margin of Error!!Total:!!Car, truck, or van:!!Drove alone","State","Place"],
> ["Irvine city, California","318693","56",null,null,"06","36770"],
> ["Santa Ana city, California","316188","66","111384","5478","06","69000"]]

Public transportation

``https://api.census.gov/data/2024/acs/acs1?get=NAME,B08301_010E,B08301_010M,B08301_013E,B08301_013M&for=place:36770,69000&in=state:06&key=[yourkeyhere]&descriptive=true``

Results

> [["NAME","B08301_010E","B08301_010M","B08301_013E","B08301_013M","state","place"],
> ["Geographic Area Name","Estimate!!Total:!!Public transportation:","Margin of Error!!Total:!!Public transportation:","Estimate!!Total:!!Public transportation:!!Long-distance train or commuter rail","Margin of Error!!Total:!!Public transportation:!!Long-distance train or commuter rail","State","Place"],
> ["Irvine city, California",null,null,null,null,"06","36770"],
> ["Santa Ana city, California","3369","931","261","256","06","69000"]]

### Only  2025

#### Electric Vehicles

``https://api.census.gov/data/2025/acs/acs1?get=NAME,B25147_003E,B25147_003M,B25147_004E,B25147_004M&for=place:36770,69000&in=state:06&key=[yourkeyhere]&descriptive=true``

> [["NAME","B25147_003E","B25147_003M","B25147_004E","B25147_004M","state","place"],
> 
> ["Geographic Area Name","Estimate!!Total:!!1 or more vehicles available:","Margin of Error!!Total:!!1 or more vehicles available:","Estimate!!Total:!!1 or more vehicles available:!!Electric vehicle available","Margin of Error!!Total:!!1 or more vehicles available:!!Electric vehicle available","State","Place"],
> 
> ["Irvine city, California","114413","4310","35708","3919","06","36770"],
> 
> ["Santa Ana city, California","82582","3270","7958","1854","06","69000"]]
