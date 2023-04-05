title: " What Impacts do Artificial Oyster Reef have on Land Erosion Caused by Storm Surges in the Gulf Coast? " 
Author: "Lydia Clark and Rachel Hendricks"
date: "2023-03-30"
---

#IMPORTING DATA#


*Hurricane Dataset*
```{r}
hurricane_data <- read_csv("Hurricane Dataset/hf011-01-hurricanes.csv")
```
```{r}
hurricane_damage <- read_csv("Hurricane Dataset/hf011-02-damage.csv")
```
```{r}
hurricane_tracks <- read_csv("Hurricane Dataset/hf011-03-tracks.csv")
```
```{r}
hurricane_site <- read_csv("Hurricane Dataset/hf011-04-site.csv")
```

*Storm Surge Dataset* 
```{r}
stormsurge <- read_csv("globalpeaksurgedb.csv")
```

*Oyster Modeling data*
```{r}
oyster_model_tb1 <- read_csv("Oyster Model Data/ofr20211063_table_1.1 (1).csv")
```
```{r}
oyster_model_tb2 <- read_csv("Oyster Model Data/ofr20211063_table_2.1 (2).csv")
```
```{r}
oyster_model_tb3 <- read_csv("Oyster Model Data/ofr20211063_table_3.1 (1).csv")
```

*Oyster Data*
```{r}
Oyster_data_NC <-  read_csv("Oyster Data/bcodmo_dataset_704333_712b_5843_9069.csv")
```
#SKIM DATA#

""






```{r}
skim(stormsurge)
```
```{r}
skim (Oyster_data_NC)
```
```{r}
skim(oyster_model_tb1)
skim(oyster_model_tb2)
skim(oyster_model_tb3)
```