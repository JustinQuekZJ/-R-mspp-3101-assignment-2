library(tidyverse)
library(janitor)

raw_sqf_2023 <- read_csv("sqf-2023.csv") %>% 
  clean_names()

sqf_2023 <- raw_sqf_2023 %>% 
  select(stop_id, stop_frisk_date, stop_frisk_time, stop_duration_minutes, officer_explained_stop_flag,
         suspect_arrested_flag, suspect_reported_age, suspect_sex, suspect_race_description,
         stop_location_precinct, stop_location_boro_name) %>% 
  filter(!is.na(suspect_race_description), suspect_race_description != "(null)") %>% 
  mutate(updated_race = if_else(
    condition = suspect_race_description == "BLACK HISPANIC" | suspect_race_description == "WHITE HISPANIC",
    true = "HISPANIC",
    false = suspect_race_description
  )) ## combines "Black Hispanic" and "White Hispanic" into a single "Hispanic" category

sqf_2023_summarized <- sqf_2023 %>% 
  group_by(updated_race) %>% 
  summarize(total_stops = n()) ## number of stops per racial group

write_csv(sqf_2023_summarized, "stops-by-race-2023.csv")
