# Module 7 - Codethrough  
**Thomas O’Dell**  
*2025-02-27*

<span style="color:orange"> ***NOTE: An interactive version of this map is accessible via the "Codethru" link in the "Resources tab on the top navigation of this site*** </span>


## Interactive Map Tutorial  
This Code-through demonstrates how to create an interactive map using the **giscoR** API from the European Geographic Information System of the Commission (GISCO).  

The map can be customized to showcase up to 4 administrative levels known as **Nomenclature of Territorial Units for Statistics (NUTS)**. These levels vary by country but generally follow this structure:

- **NUTS 0** → Country  
- **NUTS 1** → Geographical Regions  
- **NUTS 2** → Provinces  
- **NUTS 3** → Large Cities or Counties  

## Steps Overview  
1. Install and load `tidyverse`, `giscoR`, and `ggiraph` packages.
    
2. Determine desired NUTS level, and define objects using the `gisco_get_nuts` function.
   
3. Input NUTS level, desired year(s), and map projection (EPSG 3035 is used in this case, as it is optimal for Europe).
   
4. Associate `gg_plt` with your defined NUTS levels (e.g., “Provinces”). Define aesthetics (`aes`) and any thematic elements (e.g., border line width and color).
   
5. Associate `ggobj` with `gg_plt` to ensure `girafe()` (graphing function) works appropriately.
 
6. ### **Knit!**  

---

## **Step-by-Step Process**  

### **STEP 1: Load Packages**
```r
library(tidyverse)
library(giscoR)
```

### **STEP 2: Define NUTS Levels and Projection

```r
italy_municipalities <- gisco_get_nuts(
  country = 'Italy',
  nuts_level = '3',
  year = "2024",
  epsg = 3035
) |>
  as.tibble() |>
  janitor::clean_names()
```

### **STEP 3: Define Additional NUTS Levels

```r
italy_regions <- gisco_get_nuts(
  country = 'Italy',
  nuts_level = 1,
  year = "2024",
  epsg = 3035
) |>
  as.tibble() |>
  janitor::clean_names()
```

### **STEP 4: Load additional packages
```r
library(ggplot2)
library(ggiraph)
```

### **STEP 5: Generate interactive map

```r
gg_plt <- italy_municipalities |>
  ggplot(aes(geometry = geometry)) +
  geom_sf(
    data = italy_regions,
    aes(fill = nuts_name),
    color = 'black',
    linewidth = 0.5
  ) +
  geom_sf_interactive(
    fill = NA,
    aes(
      data_id = nuts_id,
      tooltip = nuts_name
    ),
    color = 'black',
    linewidth = 0.1
  ) + 
  theme_void() +
  theme(legend.position = 'none')

girafe(ggobj = gg_plt)
```

<img src="assets/img/Screenshot%202025-02-27%20at%2022.15.11.png" alt="Map" width="200">


### **Final Thoughts**

These simple steps, using the giscoR API, allow users to rapidly generate detailed and highly customizable maps. The predefined NUTS levels help users adjust the level of detail to suit their needs, and the interactive map with tooltips enhances the user experience.

This package is specific to Europe, but feel free to try it yourself with your country of choice! 

