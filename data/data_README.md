# Data

Place the following files in this folder:

- `52_accidents.csv` — accident circumstances (5,001 rows × 10 columns)
- `52_vehicles.csv` — vehicle and driver details (7,822 rows × 11 columns)

These files are not included in the repository as they were provided as part of a university assessment. The datasets are linked via the `Accident_Index` column.

## Columns

### 52_accidents.csv
| Column | Type | Description |
|--------|------|-------------|
| Accident_Index | float | Unique accident identifier |
| Accident_Severity | int | Target: 1=Fatal, 2=Serious, 3=Slight |
| Day_of_Week | str | Day the accident occurred |
| Speed_limit | float | Speed limit at accident location (mph) |
| Light_Conditions | str | Lighting at time of accident |
| Road_Surface_Conditions | str | Road surface condition |
| Longitude | float | GPS longitude |
| Latitude | float | GPS latitude |
| Datetime | str | Date and time of accident |
| 1st_Road_Number | int | Road identifier |

### 52_vehicles.csv
| Column | Type | Description |
|--------|------|-------------|
| Accident_Index | float | Links to accidents file |
| Vehicle_Type | str | Type of vehicle |
| Age_of_Driver | float | Driver age (years) |
| Sex_of_Driver | str | Driver sex |
| Engine_Capacity_(CC) | int | Engine size |
| Age_of_Vehicle | float | Vehicle age (years) |
| Propulsion_Code | str | Fuel type |
| Journey_Purpose_of_Driver | str | Trip purpose |
| Was_Vehicle_Left_Hand_Drive? | str | LHD indicator |
| Driver_IMD_Decile | int | Deprivation index |
| Driver_Home_Area_Type | str | Urban/rural classification |
