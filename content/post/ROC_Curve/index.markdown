---
title: "ROC Curve Brady et al. (2022)"
author: 'Paul Riesthuis'
authors: [Paul Riesthuis]
slug: roc-curve-brady
categories: []
tags: []
subtitle: ''
summary: 'In this post, I will conduct the ROC analyses performed in the article of Brady et al. (2022) '
lastmod: '2023-09-23T16:22:26+02:00'
date: "2023-09-23"
projects: []
output: 
  html_document:
    theme: readable
    code_folding: hide
    toc: TRUE
    toc_float: TRUE
---

<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>
<link href="{{< blogdown/postref >}}index_files/tabwid/tabwid.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/tabwid/tabwid.js"></script>

# ROC cuve analysis of [Brady et al. (2022)](https://doi.org/10.3758/s13423-022-02179-w). 

- In this blogpost, I will show how to construct the datasets of Tim and John and the Receiver Operator Characteristic (ROC) curves provided in the article of Brady et al. (2022)
  - Then, I will show the following:
    - How to calculate common statistics that can be calculated based on this type of dataset. Most statistics are similar to the ones shown at the page about [“old” and “new” memory task](https://paul-riesthuis.netlify.app/post/new-old-memory-task-data-analysis/)
    - How to construct (ROC) curves by hand and via logistic regression.
    - How to calculate the area under the ROC curve.
    - How to calculate confidence intervals for the AUC
    - How to calculate partial area under the curve.

## Datasets constructed by hand

- Dataset of Tim

``` r
old_items_tim <- c(50,50,50,150,500,200)
new_items_tim <- c(250,250,400,50,25,25)
confidence <- c("extreme weak","very weak", "fairly weak", "fairly strong", "very strong", "extreme strong")
df_tim <- data.frame(old_items_tim, new_items_tim, confidence)
nice_table(df_tim,
           title= "Dataset for Tim")
```

<div class="tabwid"><style>.cl-a78b4c64{table-layout:auto;}.cl-a784326c{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a7843276{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a786f614{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a786f61e{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a7870bd6{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7870bd7{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7870be0{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7870be1{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7870be2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7870be3{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-a78b4c64'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-a7870bd6"><p class="cl-a786f614"><span class="cl-a784326c">Dataset for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-a7870bd7"><p class="cl-a786f61e"><span class="cl-a7843276">old_items_tim</span></p></th><th class="cl-a7870bd7"><p class="cl-a786f61e"><span class="cl-a7843276">new_items_tim</span></p></th><th class="cl-a7870bd7"><p class="cl-a786f61e"><span class="cl-a7843276">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-a7870be0"><p class="cl-a786f614"><span class="cl-a7843276">50.00</span></p></td><td class="cl-a7870be1"><p class="cl-a786f61e"><span class="cl-a7843276">250.00</span></p></td><td class="cl-a7870be1"><p class="cl-a786f61e"><span class="cl-a7843276">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7870be0"><p class="cl-a786f614"><span class="cl-a7843276">50.00</span></p></td><td class="cl-a7870be1"><p class="cl-a786f61e"><span class="cl-a7843276">250.00</span></p></td><td class="cl-a7870be1"><p class="cl-a786f61e"><span class="cl-a7843276">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7870be0"><p class="cl-a786f614"><span class="cl-a7843276">50.00</span></p></td><td class="cl-a7870be1"><p class="cl-a786f61e"><span class="cl-a7843276">400.00</span></p></td><td class="cl-a7870be1"><p class="cl-a786f61e"><span class="cl-a7843276">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7870be0"><p class="cl-a786f614"><span class="cl-a7843276">150.00</span></p></td><td class="cl-a7870be1"><p class="cl-a786f61e"><span class="cl-a7843276">50.00</span></p></td><td class="cl-a7870be1"><p class="cl-a786f61e"><span class="cl-a7843276">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7870be0"><p class="cl-a786f614"><span class="cl-a7843276">500.00</span></p></td><td class="cl-a7870be1"><p class="cl-a786f61e"><span class="cl-a7843276">25.00</span></p></td><td class="cl-a7870be1"><p class="cl-a786f61e"><span class="cl-a7843276">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7870be2"><p class="cl-a786f614"><span class="cl-a7843276">200.00</span></p></td><td class="cl-a7870be3"><p class="cl-a786f61e"><span class="cl-a7843276">25.00</span></p></td><td class="cl-a7870be3"><p class="cl-a786f61e"><span class="cl-a7843276">extreme strong</span></p></td></tr></tbody></table></div>

- Dataset John

``` r
old_items_john <- c(50,75,75,500,200,100)
new_items_john <- c(100,400,300,50,75,75) # changed very strong to 75 instead of 25 to have equal number items between tim and john
df_john <- data.frame(old_items_john, new_items_john, confidence)
nice_table(df_john,
           title = "Dataset for John")
```

<div class="tabwid"><style>.cl-a7a13a92{table-layout:auto;}.cl-a79b0258{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a79b0262{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a79d8870{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a79d887a{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a79d96a8{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a79d96a9{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a79d96b2{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a79d96b3{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a79d96b4{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a79d96bc{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-a7a13a92'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-a79d96a8"><p class="cl-a79d8870"><span class="cl-a79b0258">Dataset for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-a79d96a9"><p class="cl-a79d887a"><span class="cl-a79b0262">old_items_john</span></p></th><th class="cl-a79d96a9"><p class="cl-a79d887a"><span class="cl-a79b0262">new_items_john</span></p></th><th class="cl-a79d96a9"><p class="cl-a79d887a"><span class="cl-a79b0262">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-a79d96b2"><p class="cl-a79d8870"><span class="cl-a79b0262">50.00</span></p></td><td class="cl-a79d96b3"><p class="cl-a79d887a"><span class="cl-a79b0262">100.00</span></p></td><td class="cl-a79d96b3"><p class="cl-a79d887a"><span class="cl-a79b0262">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a79d96b2"><p class="cl-a79d8870"><span class="cl-a79b0262">75.00</span></p></td><td class="cl-a79d96b3"><p class="cl-a79d887a"><span class="cl-a79b0262">400.00</span></p></td><td class="cl-a79d96b3"><p class="cl-a79d887a"><span class="cl-a79b0262">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a79d96b2"><p class="cl-a79d8870"><span class="cl-a79b0262">75.00</span></p></td><td class="cl-a79d96b3"><p class="cl-a79d887a"><span class="cl-a79b0262">300.00</span></p></td><td class="cl-a79d96b3"><p class="cl-a79d887a"><span class="cl-a79b0262">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a79d96b2"><p class="cl-a79d8870"><span class="cl-a79b0262">500.00</span></p></td><td class="cl-a79d96b3"><p class="cl-a79d887a"><span class="cl-a79b0262">50.00</span></p></td><td class="cl-a79d96b3"><p class="cl-a79d887a"><span class="cl-a79b0262">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a79d96b2"><p class="cl-a79d8870"><span class="cl-a79b0262">200.00</span></p></td><td class="cl-a79d96b3"><p class="cl-a79d887a"><span class="cl-a79b0262">75.00</span></p></td><td class="cl-a79d96b3"><p class="cl-a79d887a"><span class="cl-a79b0262">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a79d96b4"><p class="cl-a79d8870"><span class="cl-a79b0262">100.00</span></p></td><td class="cl-a79d96bc"><p class="cl-a79d887a"><span class="cl-a79b0262">75.00</span></p></td><td class="cl-a79d96bc"><p class="cl-a79d887a"><span class="cl-a79b0262">extreme strong</span></p></td></tr></tbody></table></div>

------------------------------------------------------------------------

## Add hit rate and false alarm rate

------------------------------------------------------------------------

- **Tim**

``` r
#hit rates Tim
tot_items_tim <- sum(old_items_tim) # to get the total amount of items
df_tim$hit <- c(old_items_tim[1]/tot_items_tim, 
         (old_items_tim[1]+old_items_tim[2])/tot_items_tim,
         (old_items_tim[1]+old_items_tim[2]+old_items_tim[3])/tot_items_tim,
         (old_items_tim[1]+old_items_tim[2]+old_items_tim[3]+old_items_tim[4])/tot_items_tim,
         (old_items_tim[1]+old_items_tim[2]+old_items_tim[3]+old_items_tim[4]+old_items_tim[5])/tot_items_tim,
         (old_items_tim[1]+old_items_tim[2]+old_items_tim[3]+old_items_tim[4]+old_items_tim[5]+old_items_tim[6])/tot_items_tim)
        
  
#false alarm rate Tim
tot_new_tim <- sum(new_items_tim)
df_tim$far <- c(new_items_tim[1]/tot_new_tim, 
         (new_items_tim[1]+new_items_tim[2])/tot_new_tim,
         (new_items_tim[1]+new_items_tim[2]+new_items_tim[3])/tot_new_tim,
         (new_items_tim[1]+new_items_tim[2]+new_items_tim[3]+new_items_tim[4])/tot_new_tim,
         (new_items_tim[1]+new_items_tim[2]+new_items_tim[3]+new_items_tim[4]+new_items_tim[5])/tot_new_tim,
         (new_items_tim[1]+new_items_tim[2]+new_items_tim[3]+new_items_tim[4]+new_items_tim[5]+new_items_tim[6])/tot_new_tim)
nice_table(df_tim,
           title= "Dataset + Hit and False Alarm rate for Tim")
```

<div class="tabwid"><style>.cl-a7b4b2b6{table-layout:auto;}.cl-a7aeb708{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a7aeb712{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a7b10f80{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a7b10f8a{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a7b11cc8{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7b11cd2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7b11cd3{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7b11cdc{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7b11cdd{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7b11cde{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-a7b4b2b6'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-a7b11cc8"><p class="cl-a7b10f80"><span class="cl-a7aeb708">Dataset + Hit and False Alarm rate for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-a7b11cd2"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">old_items_tim</span></p></th><th class="cl-a7b11cd2"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">new_items_tim</span></p></th><th class="cl-a7b11cd2"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">confidence</span></p></th><th class="cl-a7b11cd2"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">hit</span></p></th><th class="cl-a7b11cd2"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-a7b11cd3"><p class="cl-a7b10f80"><span class="cl-a7aeb712">50.00</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">250.00</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">extreme weak</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">0.05</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7b11cd3"><p class="cl-a7b10f80"><span class="cl-a7aeb712">50.00</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">250.00</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">very weak</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">0.10</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7b11cd3"><p class="cl-a7b10f80"><span class="cl-a7aeb712">50.00</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">400.00</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">fairly weak</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">0.15</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">0.90</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7b11cd3"><p class="cl-a7b10f80"><span class="cl-a7aeb712">150.00</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">50.00</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">fairly strong</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">0.30</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">0.95</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7b11cd3"><p class="cl-a7b10f80"><span class="cl-a7aeb712">500.00</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">25.00</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">very strong</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">0.80</span></p></td><td class="cl-a7b11cdc"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">0.97</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7b11cdd"><p class="cl-a7b10f80"><span class="cl-a7aeb712">200.00</span></p></td><td class="cl-a7b11cde"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">25.00</span></p></td><td class="cl-a7b11cde"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">extreme strong</span></p></td><td class="cl-a7b11cde"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">1.00</span></p></td><td class="cl-a7b11cde"><p class="cl-a7b10f8a"><span class="cl-a7aeb712">1.00</span></p></td></tr></tbody></table></div>

- **John**

``` r
#hit rates for john
tot_old_john <- sum(old_items_john)
df_john$hit <- c(old_items_john[1]/tot_old_john, 
         (old_items_john[1]+old_items_john[2])/tot_old_john,
         (old_items_john[1]+old_items_john[2]+old_items_john[3])/tot_old_john,
         (old_items_john[1]+old_items_john[2]+old_items_john[3]+old_items_john[4])/tot_old_john,
         (old_items_john[1]+old_items_john[2]+old_items_john[3]+old_items_john[4]+old_items_john[5])/tot_old_john,
         (old_items_john[1]+old_items_john[2]+old_items_john[3]+old_items_john[4]+old_items_john[5]+old_items_john[6])/tot_old_john)

# false alarm rate john
tot_new_john <- sum(new_items_john)
df_john$far <- c(new_items_john[1]/tot_new_john, 
         (new_items_john[1]+new_items_john[2])/tot_new_john,
         (new_items_john[1]+new_items_john[2]+new_items_john[3])/tot_new_john,
         (new_items_john[1]+new_items_john[2]+new_items_john[3]+new_items_john[4])/tot_new_john,
         (new_items_john[1]+new_items_john[2]+new_items_john[3]+new_items_john[4]+new_items_john[5])/tot_new_john,
         (new_items_john[1]+new_items_john[2]+new_items_john[3]+new_items_john[4]+new_items_john[5]+new_items_john[6])/tot_new_john)
nice_table(df_john,
           title= "Dataset + Hit and False Alarm rate for John")
```

<div class="tabwid"><style>.cl-a7c9ed48{table-layout:auto;}.cl-a7c2641a{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a7c26424{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a7c54f86{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a7c54f90{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a7c55f3a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7c55f44{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7c55f45{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7c55f46{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7c55f47{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a7c55f4e{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-a7c9ed48'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-a7c55f3a"><p class="cl-a7c54f86"><span class="cl-a7c2641a">Dataset + Hit and False Alarm rate for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-a7c55f44"><p class="cl-a7c54f90"><span class="cl-a7c26424">old_items_john</span></p></th><th class="cl-a7c55f44"><p class="cl-a7c54f90"><span class="cl-a7c26424">new_items_john</span></p></th><th class="cl-a7c55f44"><p class="cl-a7c54f90"><span class="cl-a7c26424">confidence</span></p></th><th class="cl-a7c55f44"><p class="cl-a7c54f90"><span class="cl-a7c26424">hit</span></p></th><th class="cl-a7c55f44"><p class="cl-a7c54f90"><span class="cl-a7c26424">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-a7c55f45"><p class="cl-a7c54f86"><span class="cl-a7c26424">50.00</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">100.00</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">extreme weak</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">0.05</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">0.10</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7c55f45"><p class="cl-a7c54f86"><span class="cl-a7c26424">75.00</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">400.00</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">very weak</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">0.12</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7c55f45"><p class="cl-a7c54f86"><span class="cl-a7c26424">75.00</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">300.00</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">fairly weak</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">0.20</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">0.80</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7c55f45"><p class="cl-a7c54f86"><span class="cl-a7c26424">500.00</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">50.00</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">fairly strong</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">0.70</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">0.85</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7c55f45"><p class="cl-a7c54f86"><span class="cl-a7c26424">200.00</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">75.00</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">very strong</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">0.90</span></p></td><td class="cl-a7c55f46"><p class="cl-a7c54f90"><span class="cl-a7c26424">0.92</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a7c55f47"><p class="cl-a7c54f86"><span class="cl-a7c26424">100.00</span></p></td><td class="cl-a7c55f4e"><p class="cl-a7c54f90"><span class="cl-a7c26424">75.00</span></p></td><td class="cl-a7c55f4e"><p class="cl-a7c54f90"><span class="cl-a7c26424">extreme strong</span></p></td><td class="cl-a7c55f4e"><p class="cl-a7c54f90"><span class="cl-a7c26424">1.00</span></p></td><td class="cl-a7c55f4e"><p class="cl-a7c54f90"><span class="cl-a7c26424">1.00</span></p></td></tr></tbody></table></div>

------------------------------------------------------------------------

## Plot ROC Curves for Tim and John

------------------------------------------------------------------------

``` r
roc_data <- data.frame(df_tim,df_john)
ggplot(roc_data, aes(x = 1- far, y = 1- hit, color = "Tim")) +
  geom_line()+ geom_point()+
  geom_line(aes(x = 1 - far.1, y = 1 - hit.1, color="John"))+ geom_point(aes(x = 1 - far.1, y = 1 - hit.1, color="John"))+
  geom_abline(intercept = 0, slope = 1, linetype = "dashed") +
  labs(x = "False Positive Rate (1 - Specificity)", y = "True Positive Rate (Sensitivity)") +
  ggtitle("ROC Curve")+
  theme_classic()+
  ylim(0,1)+
  geom_vline(xintercept = .10,
             lwd = 1,
             linetype="dashed")+
  annotate("text", x=.23, y=1, label="10% false positives")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

## ROC Curve via Logistic regression

- Create datasets for Tim and John that are compatible for logistic regression.

``` r
correct_mem <- rep(c(1, 0), each = 1000)
confidence_tim  <- c(rep(1,50),
                 rep(2,50),
                 rep(3,50),
                 rep(4,150),
                 rep(5,500),
                 rep(6,200),
                 rep(1,250),
                 rep(2,250),
                 rep(3,400),
                 rep(4,50),
                 rep(5,25),
                 rep(6,25))
confidence_john  <- c(rep(1,50),
                 rep(2,75),
                 rep(3,75),
                 rep(4,500),
                 rep(5,200),
                 rep(6,100),
                 rep(1,100),
                 rep(2,400),
                 rep(3,300),
                 rep(4,50),
                 rep(5,75), # i made this 75 as in the article there are otherwise only 1950 items for john instead 2000 as with Tim's. It was orignally 25
                 rep(6,75))
df <- data.frame(correct_mem,confidence_tim,confidence_john)
```

------------------------------------------------------------------------

- **Create ROC Curves for both**

------------------------------------------------------------------------

``` r
glm.fit_tim=glm(correct_mem ~ confidence_tim, family=binomial)
glm.fit_john=glm(correct_mem ~ confidence_john, family=binomial)

par(pty="s")
p1 <- roc(correct_mem, glm.fit_tim$fitted.values, 
    plot= TRUE,
    legacy.axes=TRUE, 
    percent=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="#377eb8", 
    lwd=4,
    print.auc = TRUE)

p2 <- roc(correct_mem, glm.fit_john$fitted.values, 
    plot= TRUE,
    legacy.axes=TRUE, 
    percent=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="red", 
    lwd=4,
    print.auc = TRUE,
    print.auc.y = 40,
    add=TRUE)
abline(v = 90, col = "black", lty = 2)
text(x = 60, y = 98, labels = "10% false positive", col = "black")
legend("bottomright", legend=c("Tim", "John"),
       col=c("Blue","red"), lwd=2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="672" />

------------------------------------------------------------------------

## True and False Positive per Threshold

------------------------------------------------------------------------

- This can be used to examine the blackstone ratio (10:1; false negative to false positive ratio)
- True and False Positive percentages for each confidence threshold for Tim

``` r
info_tim <- roc(correct_mem, glm.fit_tim$fitted.values) 
tim.df <- data.frame(
  Confidence = c("","Extreme-very weak", "Very-fairly weak", "Weak-strong", "Fairly-very strong","Very-extreme strong",""),
  tpp=info_tim$sensitivities*100,
  fpp=(1-info_tim$specificities)*100, #to make sure the order goes from weak to high confidence
  thresholds = info_tim$thresholds,
  FN_FP = (length(info_tim$cases)*(1-info_tim$sensitivities))/(length(info_tim$controls)*(1-info_tim$specificities)))

tim.df <- tim.df[-c(1,7), ]  
nice_table(tim.df,
           title="True Positive and False Positive Percentages for each Threshold")
```

<div class="tabwid"><style>.cl-a83f2446{table-layout:auto;}.cl-a83989a0{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a83989aa{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a83bba54{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a83bba5e{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a83bc79c{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a83bc79d{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a83bc7a6{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a83bc7a7{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a83bc7a8{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a83bc7b0{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-a83f2446'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-a83bc79c"><p class="cl-a83bba54"><span class="cl-a83989a0">True Positive and False Positive Percentages for each Threshold</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-a83bc79d"><p class="cl-a83bba5e"><span class="cl-a83989aa">Confidence</span></p></th><th class="cl-a83bc79d"><p class="cl-a83bba5e"><span class="cl-a83989aa">tpp</span></p></th><th class="cl-a83bc79d"><p class="cl-a83bba5e"><span class="cl-a83989aa">fpp</span></p></th><th class="cl-a83bc79d"><p class="cl-a83bba5e"><span class="cl-a83989aa">thresholds</span></p></th><th class="cl-a83bc79d"><p class="cl-a83bba5e"><span class="cl-a83989aa">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-a83bc7a6"><p class="cl-a83bba54"><span class="cl-a83989aa">Extreme-very weak</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">95.00</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">75.00</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">0.09</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">0.07</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a83bc7a6"><p class="cl-a83bba54"><span class="cl-a83989aa">Very-fairly weak</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">90.00</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">50.00</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">0.24</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">0.20</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a83bc7a6"><p class="cl-a83bba54"><span class="cl-a83989aa">Weak-strong</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">85.00</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">10.00</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">0.49</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">1.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a83bc7a6"><p class="cl-a83bba54"><span class="cl-a83989aa">Fairly-very strong</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">70.00</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">5.00</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">0.75</span></p></td><td class="cl-a83bc7a7"><p class="cl-a83bba5e"><span class="cl-a83989aa">6.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a83bc7a8"><p class="cl-a83bba54"><span class="cl-a83989aa">Very-extreme strong</span></p></td><td class="cl-a83bc7b0"><p class="cl-a83bba5e"><span class="cl-a83989aa">20.00</span></p></td><td class="cl-a83bc7b0"><p class="cl-a83bba5e"><span class="cl-a83989aa">2.50</span></p></td><td class="cl-a83bc7b0"><p class="cl-a83bba5e"><span class="cl-a83989aa">0.90</span></p></td><td class="cl-a83bc7b0"><p class="cl-a83bba5e"><span class="cl-a83989aa">32.00</span></p></td></tr></tbody></table></div>

- True and False Positive percentages for each confidence threshold for John

``` r
info_john <- roc(correct_mem, glm.fit_john$fitted.values) 
john.df <- data.frame(
  Confidence = c("","Extreme-very weak", "Very-fairly weak", "Weak-strong", "Fairly-very strong","Very-extreme strong",""),
  tpp=info_john$sensitivities*100,
  fpp=(1-info_john$specificities)*100, #to make sure the order goes from weak to high confidence
  thresholds = info_john$thresholds,
  FN_FP = (length(info_john$cases)*(1-info_john$sensitivities))/(length(info_john$controls)*(1-info_john$specificities)))

john.df <- john.df[-c(1,7), ]  
nice_table(john.df,
           title="True Positive and False Positive Percentages for each Threshold for John")
```

<div class="tabwid"><style>.cl-a8531b9a{table-layout:auto;}.cl-a84cd5c8{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a84cd5d2{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a84f238c{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a84f238d{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a84f3020{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a84f3021{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a84f302a{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a84f302b{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a84f302c{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a84f3034{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-a8531b9a'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-a84f3020"><p class="cl-a84f238c"><span class="cl-a84cd5c8">True Positive and False Positive Percentages for each Threshold for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-a84f3021"><p class="cl-a84f238d"><span class="cl-a84cd5d2">Confidence</span></p></th><th class="cl-a84f3021"><p class="cl-a84f238d"><span class="cl-a84cd5d2">tpp</span></p></th><th class="cl-a84f3021"><p class="cl-a84f238d"><span class="cl-a84cd5d2">fpp</span></p></th><th class="cl-a84f3021"><p class="cl-a84f238d"><span class="cl-a84cd5d2">thresholds</span></p></th><th class="cl-a84f3021"><p class="cl-a84f238d"><span class="cl-a84cd5d2">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-a84f302a"><p class="cl-a84f238c"><span class="cl-a84cd5d2">Extreme-very weak</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">95.00</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">90.00</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">0.21</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">0.06</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a84f302a"><p class="cl-a84f238c"><span class="cl-a84cd5d2">Very-fairly weak</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">87.50</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">50.00</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">0.35</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a84f302a"><p class="cl-a84f238c"><span class="cl-a84cd5d2">Weak-strong</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">80.00</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">20.00</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">0.51</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a84f302a"><p class="cl-a84f238c"><span class="cl-a84cd5d2">Fairly-very strong</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">30.00</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">15.00</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">0.68</span></p></td><td class="cl-a84f302b"><p class="cl-a84f238d"><span class="cl-a84cd5d2">4.67</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a84f302c"><p class="cl-a84f238c"><span class="cl-a84cd5d2">Very-extreme strong</span></p></td><td class="cl-a84f3034"><p class="cl-a84f238d"><span class="cl-a84cd5d2">10.00</span></p></td><td class="cl-a84f3034"><p class="cl-a84f238d"><span class="cl-a84cd5d2">7.50</span></p></td><td class="cl-a84f3034"><p class="cl-a84f238d"><span class="cl-a84cd5d2">0.81</span></p></td><td class="cl-a84f3034"><p class="cl-a84f238d"><span class="cl-a84cd5d2">12.00</span></p></td></tr></tbody></table></div>

------------------------------------------------------------------------

## Confidence Intervals

------------------------------------------------------------------------

- The graphs provide important information for whos memory is better.
  - For example, the ROC curve of Tim is greater at eacg threshold than the ROC curve of John.
- However, to fully examine the AUC we would like 95% CI.
  - This can be easily extracted in the pROC package.
  - You can either choose to bootstrap or use the [delong method](https://doi.org/10.1109/LSP.2014.2337313).
  - Using the 95%CI the AUC can be compared to the smallest effect size of interest (e.g., AUC_null = .8).

``` r
# Delong method
auc_tim <- roc(correct_mem, glm.fit_tim$fitted.values)
auc_john <- roc(correct_mem, glm.fit_john$fitted.values)
CI_delong_tim <- ci.auc(auc_tim) # default is 95%
CI_delong_john <- ci.auc(auc_john) 
CI_boot_tim <- ci.auc(auc_tim, method = "bootstrap", boot.n = 1000) # bootstrapping 1000 times
CI_boot_john <- ci.auc(auc_john, method = "bootstrap", boot.n = 1000) # bootstrapping 1000 times
p_tim <- power.roc.test(auc_tim) # null hypothesis is set to .5
p_john <- power.roc.test(auc_john)# null hypothesis is set to .5
tab_ci <- data.frame(
  Name = c("Tim","John"),
  AUC = c(CI_delong_tim[2],CI_delong_john[2]), #AUC values
  CI_low_Delong = c(CI_delong_tim[1],CI_delong_john[1]), # lower bound CI delong
  CI_upper_Delong = c(CI_delong_tim[3],CI_delong_john[3]),# upper bound CI delong
  CI_low_boot = c(CI_delong_tim[1],CI_delong_john[1]),# lower bound CI bootstrap
  CI_upper_boot = c(CI_delong_tim[3],CI_delong_john[3]),# upper bound CI bootstrap
  Power = c(p_tim$power,p_john$power)
)
nice_table(tab_ci,
           title = "AUC values with 95% CI via Delong or bootstrapping",
)
```

<div class="tabwid"><style>.cl-a90e32fe{table-layout:auto;}.cl-a9081a18{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a9081a22{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-a90aa4d6{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a90aa4e0{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-a90ab2aa{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a90ab2ab{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a90ab2b4{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a90ab2b5{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a90ab2b6{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-a90ab2b7{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-a90e32fe'><thead><tr style="overflow-wrap:break-word;"><th  colspan="7"class="cl-a90ab2aa"><p class="cl-a90aa4d6"><span class="cl-a9081a18">AUC values with 95% CI via Delong or bootstrapping</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-a90ab2ab"><p class="cl-a90aa4e0"><span class="cl-a9081a22">Name</span></p></th><th class="cl-a90ab2ab"><p class="cl-a90aa4e0"><span class="cl-a9081a22">AUC</span></p></th><th class="cl-a90ab2ab"><p class="cl-a90aa4e0"><span class="cl-a9081a22">CI_low_Delong</span></p></th><th class="cl-a90ab2ab"><p class="cl-a90aa4e0"><span class="cl-a9081a22">CI_upper_Delong</span></p></th><th class="cl-a90ab2ab"><p class="cl-a90aa4e0"><span class="cl-a9081a22">CI_low_boot</span></p></th><th class="cl-a90ab2ab"><p class="cl-a90aa4e0"><span class="cl-a9081a22">CI_upper_boot</span></p></th><th class="cl-a90ab2ab"><p class="cl-a90aa4e0"><span class="cl-a9081a22">Power</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-a90ab2b4"><p class="cl-a90aa4d6"><span class="cl-a9081a22">Tim</span></p></td><td class="cl-a90ab2b5"><p class="cl-a90aa4e0"><span class="cl-a9081a22">0.88</span></p></td><td class="cl-a90ab2b5"><p class="cl-a90aa4e0"><span class="cl-a9081a22">0.86</span></p></td><td class="cl-a90ab2b5"><p class="cl-a90aa4e0"><span class="cl-a9081a22">0.89</span></p></td><td class="cl-a90ab2b5"><p class="cl-a90aa4e0"><span class="cl-a9081a22">0.86</span></p></td><td class="cl-a90ab2b5"><p class="cl-a90aa4e0"><span class="cl-a9081a22">0.89</span></p></td><td class="cl-a90ab2b5"><p class="cl-a90aa4e0"><span class="cl-a9081a22">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-a90ab2b6"><p class="cl-a90aa4d6"><span class="cl-a9081a22">John</span></p></td><td class="cl-a90ab2b7"><p class="cl-a90aa4e0"><span class="cl-a9081a22">0.76</span></p></td><td class="cl-a90ab2b7"><p class="cl-a90aa4e0"><span class="cl-a9081a22">0.74</span></p></td><td class="cl-a90ab2b7"><p class="cl-a90aa4e0"><span class="cl-a9081a22">0.78</span></p></td><td class="cl-a90ab2b7"><p class="cl-a90aa4e0"><span class="cl-a9081a22">0.74</span></p></td><td class="cl-a90ab2b7"><p class="cl-a90aa4e0"><span class="cl-a9081a22">0.78</span></p></td><td class="cl-a90ab2b7"><p class="cl-a90aa4e0"><span class="cl-a9081a22">1.00</span></p></td></tr></tbody></table></div>

- We can also plot the 95%CI intervals of the ROC curves

``` r
par(pty="s")
p1 <- roc(correct_mem, glm.fit_tim$fitted.values, 
    plot= TRUE,
    legacy.axes=TRUE, 
    percent=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="#377eb8", 
    lwd=4,
    print.auc = TRUE)
p3 <- ci.se(p1,                         # CI of Tim
            specificities = seq(0, 100, 1),  # CI of sensitivity at at what specificities? 0,5,10,15,....,100
            conf.level=0.95,                 # level of Confidence interval
) 

plot(p3, type = "shape", col = "blue")

p2 <- roc(correct_mem, glm.fit_john$fitted.values, 
    plot= TRUE,
    legacy.axes=TRUE, 
    percent=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="red", 
    lwd=4,
    print.auc = TRUE,
    print.auc.y = 40,
    add=TRUE)
p4 <- ci.se(p2,                         # CI of john 
            specificities = seq(0, 100, 1),  # CI at which thresholds?
            conf.level=0.95,                 # indicate confidence interval
) 
plot(p4, type = "shape", col = "red")     # plot as a blue shape
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-11-1.png" width="672" />

------------------------------------------------------------------------

## Partial AUC

------------------------------------------------------------------------

- We can calculate the area under the curve for a part of the curve.
  - One example is to calculate the curve for values under the 10% false positive
  - That is because we want to minimize the amount of false positive when it comes to eyewitness identification.

``` r
# Calculate the partial AUC
partial_auc_tim <- roc(correct_mem, glm.fit_tim$fitted.values, partial.auc=c(100,90), percent = T)
partial_auc_john <- roc(correct_mem, glm.fit_john$fitted.values, partial.auc=c(100,90), percent = T)

# Print the partial AUC
par(pty="s")
p1 <- plot(partial_auc_tim,
    plot= TRUE,
    legacy.axes=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="red", 
    lwd=4,
    print.auc = TRUE,
    print.auc.x = 50,
    print.auc.y = 40,
    auc.polygon=TRUE,
    percent=TRUE)
p1 <- plot(partial_auc_john,
    plot= TRUE,
    legacy.axes=TRUE, 
    xlab="False Positive Percentage", 
    ylab="True Postive Percentage", 
    col="blue", 
    lwd=4,
    print.auc = TRUE,
    print.auc.x = 50,
    print.auc.y = 30,
    auc.polygon=TRUE,
    percent=TRUE,
    add=T)
abline(v = 90, col = "black", lty = 2)
text(x = 65, y = 98, labels = "10% False positive", col = "black")
legend("bottomright", legend=c("Tim", "John"),
       col=c("Blue","red"), lwd=2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-12-1.png" width="672" />
