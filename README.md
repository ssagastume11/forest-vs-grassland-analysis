# forest-vs-grassland-analysis
"Forest vs. Grassland Bird Distribution Analysis" This project exams bird species distributions in forest and grassland habitats using NCRN LAND Bird Monitoring data (2007-2017). It explores species richness, abundance, and habitat differences to support conservation efforts.

# Introduction
Bird populations serve as essential indicators of ecosystem health. This study examines bird species distributions within grassland habitats in the National Capital Region (NCR) using data from the NCRN LAND Bird Monitoring Data (2007 - 2017) dataset. By analyzing species richness, abundance, and habitat preferences, this study provides insights that can inform conservation strategies and land management practices.

# Scenario
Over a span of ten years, bird observations were systematically recorded across national parks in the National Capital Region. Initially, surveys focused on forest environments, but monitoring efforts expanded to include grassland sites in 2014. The goal is to compare the diversity and abundance of bird species in grassland environments and identify key conservation needs.

# Ask
## Business Problem
How do bird species distributions vary within grassland habitats, and what conservation measures can be implemented to support bird populations in these areas?

## Key Questions
* What are the most common bird species in grassland habitats?
* How does species richness compare to other habitats?
* Are certain bird species particularly abundant or rare in grasslands?
* What trends emerge when visualizing species distributions in grasslands?

## Prepare
**Dataset:** NCRN LAND Bird Monitoring Data (2007 - 2017)  
**Source:** [Catalog.Data.Gov](https://catalog.data.gov)  

**Data Summary:**
- Observations collected at various grassland sites
- Variables include species, count, location type, weather conditions, and observer details
- Data is structured with point-count survey records

## Process
### Data Cleaning Steps:
1. Remove missing or inconsistent values
2. Standardize column names for easy analysis
3. Convert categorical variables (e.g., species names) into factors
4. Aggregate data by location type and species

```r
# Load necessary libraries
library(dplyr)
library(ggplot2)
library(readxl)
library(knitr)

# Load dataset
bird_data <- read_excel("NCRN LAND Bird Monitoring Data 2007 - 2017_Public.xlsx")

# Clean the dataset
bird_data_clean <- bird_data %>%
  filter(!is.na(Common_Name), !is.na(Location_Type)) %>%
  mutate(location_type = as.factor(Location_Type))

# Verify clean dataset
summary(bird_data_clean)
```
## Analyze
### 1. Bird Species Count by Habitat
```r
species_distribution <- bird_data_clean %>%
  group_by(Common_Name) %>%
  summarise(count = n(), .groups = "drop") %>%
  arrange(desc(count))

# Display top species
kable(head(species_distribution, 10), caption = "Top 10 Bird Species in Grassland Habitats")
```

### 2. Visualization: Species Distribution in Grasslands
```r
ggplot(species_distribution, aes(x = reorder(Common_Name, -count), y = count)) +
  geom_bar(stat = "identity", fill = "steelblue") +
  coord_flip() +
  labs(
    title = "Bird Species Distribution in Grassland Habitats",
    x = "Species",
    y = "Observation Count",
    caption = "Source: NCRN LAND Bird Monitoring Data (2007 - 2017), catalog.data.gov"
  ) +
  theme_minimal()
```
