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

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
old_items_tim <- c(50,50,50,150,500,200)
new_items_tim <- c(250,250,400,50,25,25)
confidence <- c("extreme weak","very weak", "fairly weak", "fairly strong", "very strong", "extreme strong")
df_tim <- data.frame(old_items_tim, new_items_tim, confidence)
nice_table(df_tim,
           title= "Dataset for Tim")
```

<div class="tabwid"><style>.cl-6c36730e{table-layout:auto;}.cl-6c2f356c{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6c2f3576{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6c3208aa{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6c3208ab{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6c3219a8{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c3219b2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c3219b3{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c3219b4{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c3219bc{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c3219bd{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-6c36730e'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-6c3219a8"><p class="cl-6c3208aa"><span class="cl-6c2f356c">Dataset for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-6c3219b2"><p class="cl-6c3208ab"><span class="cl-6c2f3576">old_items_tim</span></p></th><th class="cl-6c3219b2"><p class="cl-6c3208ab"><span class="cl-6c2f3576">new_items_tim</span></p></th><th class="cl-6c3219b2"><p class="cl-6c3208ab"><span class="cl-6c2f3576">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-6c3219b3"><p class="cl-6c3208aa"><span class="cl-6c2f3576">50.00</span></p></td><td class="cl-6c3219b4"><p class="cl-6c3208ab"><span class="cl-6c2f3576">250.00</span></p></td><td class="cl-6c3219b4"><p class="cl-6c3208ab"><span class="cl-6c2f3576">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c3219b3"><p class="cl-6c3208aa"><span class="cl-6c2f3576">50.00</span></p></td><td class="cl-6c3219b4"><p class="cl-6c3208ab"><span class="cl-6c2f3576">250.00</span></p></td><td class="cl-6c3219b4"><p class="cl-6c3208ab"><span class="cl-6c2f3576">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c3219b3"><p class="cl-6c3208aa"><span class="cl-6c2f3576">50.00</span></p></td><td class="cl-6c3219b4"><p class="cl-6c3208ab"><span class="cl-6c2f3576">400.00</span></p></td><td class="cl-6c3219b4"><p class="cl-6c3208ab"><span class="cl-6c2f3576">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c3219b3"><p class="cl-6c3208aa"><span class="cl-6c2f3576">150.00</span></p></td><td class="cl-6c3219b4"><p class="cl-6c3208ab"><span class="cl-6c2f3576">50.00</span></p></td><td class="cl-6c3219b4"><p class="cl-6c3208ab"><span class="cl-6c2f3576">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c3219b3"><p class="cl-6c3208aa"><span class="cl-6c2f3576">500.00</span></p></td><td class="cl-6c3219b4"><p class="cl-6c3208ab"><span class="cl-6c2f3576">25.00</span></p></td><td class="cl-6c3219b4"><p class="cl-6c3208ab"><span class="cl-6c2f3576">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c3219bc"><p class="cl-6c3208aa"><span class="cl-6c2f3576">200.00</span></p></td><td class="cl-6c3219bd"><p class="cl-6c3208ab"><span class="cl-6c2f3576">25.00</span></p></td><td class="cl-6c3219bd"><p class="cl-6c3208ab"><span class="cl-6c2f3576">extreme strong</span></p></td></tr></tbody></table></div>

</div>

- Dataset John

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

``` r
old_items_john <- c(50,75,75,500,200,100)
new_items_john <- c(100,400,300,50,75,75) # changed very strong to 75 instead of 25 to have equal number items between tim and john
df_john <- data.frame(old_items_john, new_items_john, confidence)
nice_table(df_john,
           title = "Dataset for John")
```

<div class="tabwid"><style>.cl-6c4dcf72{table-layout:auto;}.cl-6c47d91e{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6c47d928{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6c4a47d0{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6c4a47e4{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6c4a54d2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c4a54dc{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c4a54dd{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c4a54de{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c4a54df{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c4a54e0{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-6c4dcf72'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-6c4a54d2"><p class="cl-6c4a47d0"><span class="cl-6c47d91e">Dataset for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-6c4a54dc"><p class="cl-6c4a47e4"><span class="cl-6c47d928">old_items_john</span></p></th><th class="cl-6c4a54dc"><p class="cl-6c4a47e4"><span class="cl-6c47d928">new_items_john</span></p></th><th class="cl-6c4a54dc"><p class="cl-6c4a47e4"><span class="cl-6c47d928">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-6c4a54dd"><p class="cl-6c4a47d0"><span class="cl-6c47d928">50.00</span></p></td><td class="cl-6c4a54de"><p class="cl-6c4a47e4"><span class="cl-6c47d928">100.00</span></p></td><td class="cl-6c4a54de"><p class="cl-6c4a47e4"><span class="cl-6c47d928">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c4a54dd"><p class="cl-6c4a47d0"><span class="cl-6c47d928">75.00</span></p></td><td class="cl-6c4a54de"><p class="cl-6c4a47e4"><span class="cl-6c47d928">400.00</span></p></td><td class="cl-6c4a54de"><p class="cl-6c4a47e4"><span class="cl-6c47d928">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c4a54dd"><p class="cl-6c4a47d0"><span class="cl-6c47d928">75.00</span></p></td><td class="cl-6c4a54de"><p class="cl-6c4a47e4"><span class="cl-6c47d928">300.00</span></p></td><td class="cl-6c4a54de"><p class="cl-6c4a47e4"><span class="cl-6c47d928">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c4a54dd"><p class="cl-6c4a47d0"><span class="cl-6c47d928">500.00</span></p></td><td class="cl-6c4a54de"><p class="cl-6c4a47e4"><span class="cl-6c47d928">50.00</span></p></td><td class="cl-6c4a54de"><p class="cl-6c4a47e4"><span class="cl-6c47d928">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c4a54dd"><p class="cl-6c4a47d0"><span class="cl-6c47d928">200.00</span></p></td><td class="cl-6c4a54de"><p class="cl-6c4a47e4"><span class="cl-6c47d928">75.00</span></p></td><td class="cl-6c4a54de"><p class="cl-6c4a47e4"><span class="cl-6c47d928">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c4a54df"><p class="cl-6c4a47d0"><span class="cl-6c47d928">100.00</span></p></td><td class="cl-6c4a54e0"><p class="cl-6c4a47e4"><span class="cl-6c47d928">75.00</span></p></td><td class="cl-6c4a54e0"><p class="cl-6c4a47e4"><span class="cl-6c47d928">extreme strong</span></p></td></tr></tbody></table></div>

</div>

------------------------------------------------------------------------

## Add hit rate and false alarm rate

------------------------------------------------------------------------

- **Tim**

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

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

<div class="tabwid"><style>.cl-6c6087d4{table-layout:auto;}.cl-6c5aabe8{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6c5aabf2{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6c5cefac{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6c5cefad{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6c5cfdc6{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c5cfdc7{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c5cfdd0{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c5cfdd1{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c5cfdd2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c5cfdda{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-6c6087d4'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-6c5cfdc6"><p class="cl-6c5cefac"><span class="cl-6c5aabe8">Dataset + Hit and False Alarm rate for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-6c5cfdc7"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">old_items_tim</span></p></th><th class="cl-6c5cfdc7"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">new_items_tim</span></p></th><th class="cl-6c5cfdc7"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">confidence</span></p></th><th class="cl-6c5cfdc7"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">hit</span></p></th><th class="cl-6c5cfdc7"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-6c5cfdd0"><p class="cl-6c5cefac"><span class="cl-6c5aabf2">50.00</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">250.00</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">extreme weak</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">0.05</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c5cfdd0"><p class="cl-6c5cefac"><span class="cl-6c5aabf2">50.00</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">250.00</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">very weak</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">0.10</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c5cfdd0"><p class="cl-6c5cefac"><span class="cl-6c5aabf2">50.00</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">400.00</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">fairly weak</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">0.15</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">0.90</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c5cfdd0"><p class="cl-6c5cefac"><span class="cl-6c5aabf2">150.00</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">50.00</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">fairly strong</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">0.30</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">0.95</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c5cfdd0"><p class="cl-6c5cefac"><span class="cl-6c5aabf2">500.00</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">25.00</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">very strong</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">0.80</span></p></td><td class="cl-6c5cfdd1"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">0.97</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c5cfdd2"><p class="cl-6c5cefac"><span class="cl-6c5aabf2">200.00</span></p></td><td class="cl-6c5cfdda"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">25.00</span></p></td><td class="cl-6c5cfdda"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">extreme strong</span></p></td><td class="cl-6c5cfdda"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">1.00</span></p></td><td class="cl-6c5cfdda"><p class="cl-6c5cefad"><span class="cl-6c5aabf2">1.00</span></p></td></tr></tbody></table></div>

</div>

- **John**

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

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

<div class="tabwid"><style>.cl-6c73cd08{table-layout:auto;}.cl-6c6d8362{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6c6d836c{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6c6fd5cc{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6c6fd5d6{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6c6fe256{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c6fe257{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c6fe258{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c6fe259{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c6fe260{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6c6fe261{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-6c73cd08'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-6c6fe256"><p class="cl-6c6fd5cc"><span class="cl-6c6d8362">Dataset + Hit and False Alarm rate for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-6c6fe257"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">old_items_john</span></p></th><th class="cl-6c6fe257"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">new_items_john</span></p></th><th class="cl-6c6fe257"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">confidence</span></p></th><th class="cl-6c6fe257"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">hit</span></p></th><th class="cl-6c6fe257"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-6c6fe258"><p class="cl-6c6fd5cc"><span class="cl-6c6d836c">50.00</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">100.00</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">extreme weak</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">0.05</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">0.10</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c6fe258"><p class="cl-6c6fd5cc"><span class="cl-6c6d836c">75.00</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">400.00</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">very weak</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">0.12</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c6fe258"><p class="cl-6c6fd5cc"><span class="cl-6c6d836c">75.00</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">300.00</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">fairly weak</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">0.20</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">0.80</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c6fe258"><p class="cl-6c6fd5cc"><span class="cl-6c6d836c">500.00</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">50.00</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">fairly strong</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">0.70</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">0.85</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c6fe258"><p class="cl-6c6fd5cc"><span class="cl-6c6d836c">200.00</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">75.00</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">very strong</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">0.90</span></p></td><td class="cl-6c6fe259"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">0.92</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6c6fe260"><p class="cl-6c6fd5cc"><span class="cl-6c6d836c">100.00</span></p></td><td class="cl-6c6fe261"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">75.00</span></p></td><td class="cl-6c6fe261"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">extreme strong</span></p></td><td class="cl-6c6fe261"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">1.00</span></p></td><td class="cl-6c6fe261"><p class="cl-6c6fd5d6"><span class="cl-6c6d836c">1.00</span></p></td></tr></tbody></table></div>

</div>

------------------------------------------------------------------------

## Plot ROC Curves for Tim and John

------------------------------------------------------------------------

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

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

</div>

## ROC Curve via Logistic regression

- Create datasets for Tim and John that are compatible for logistic regression.

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

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

</div>

------------------------------------------------------------------------

- **Create ROC Curves for both**

------------------------------------------------------------------------

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

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

</div>

------------------------------------------------------------------------

## True and False Positive per Threshold

------------------------------------------------------------------------

- This can be used to examine the blackstone ratio (10:1; false negative to false positive ratio)
- True and False Positive percentages for each confidence threshold for Tim

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

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

<div class="tabwid"><style>.cl-6cca27f2{table-layout:auto;}.cl-6cc3c9a2{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6cc3c9ac{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6cc69bfa{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6cc69c04{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6cc6aa3c{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6cc6aa3d{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6cc6aa3e{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6cc6aa3f{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6cc6aa46{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6cc6aa47{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-6cca27f2'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-6cc6aa3c"><p class="cl-6cc69bfa"><span class="cl-6cc3c9a2">True Positive and False Positive Percentages for each Threshold</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-6cc6aa3d"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">Confidence</span></p></th><th class="cl-6cc6aa3d"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">tpp</span></p></th><th class="cl-6cc6aa3d"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">fpp</span></p></th><th class="cl-6cc6aa3d"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">thresholds</span></p></th><th class="cl-6cc6aa3d"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-6cc6aa3e"><p class="cl-6cc69bfa"><span class="cl-6cc3c9ac">Extreme-very weak</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">95.00</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">75.00</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">0.09</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">0.07</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6cc6aa3e"><p class="cl-6cc69bfa"><span class="cl-6cc3c9ac">Very-fairly weak</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">90.00</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">50.00</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">0.24</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">0.20</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6cc6aa3e"><p class="cl-6cc69bfa"><span class="cl-6cc3c9ac">Weak-strong</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">85.00</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">10.00</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">0.49</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">1.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6cc6aa3e"><p class="cl-6cc69bfa"><span class="cl-6cc3c9ac">Fairly-very strong</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">70.00</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">5.00</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">0.75</span></p></td><td class="cl-6cc6aa3f"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">6.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6cc6aa46"><p class="cl-6cc69bfa"><span class="cl-6cc3c9ac">Very-extreme strong</span></p></td><td class="cl-6cc6aa47"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">20.00</span></p></td><td class="cl-6cc6aa47"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">2.50</span></p></td><td class="cl-6cc6aa47"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">0.90</span></p></td><td class="cl-6cc6aa47"><p class="cl-6cc69c04"><span class="cl-6cc3c9ac">32.00</span></p></td></tr></tbody></table></div>

</div>

- True and False Positive percentages for each confidence threshold for John

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

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

<div class="tabwid"><style>.cl-6cdfe39e{table-layout:auto;}.cl-6cd92aea{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6cd92af4{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6cdbc714{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6cdbc71e{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6cdbd7ae{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6cdbd7b8{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6cdbd7b9{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6cdbd7c2{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6cdbd7c3{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6cdbd7cc{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-6cdfe39e'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-6cdbd7ae"><p class="cl-6cdbc714"><span class="cl-6cd92aea">True Positive and False Positive Percentages for each Threshold for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-6cdbd7b8"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">Confidence</span></p></th><th class="cl-6cdbd7b8"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">tpp</span></p></th><th class="cl-6cdbd7b8"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">fpp</span></p></th><th class="cl-6cdbd7b8"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">thresholds</span></p></th><th class="cl-6cdbd7b8"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-6cdbd7b9"><p class="cl-6cdbc714"><span class="cl-6cd92af4">Extreme-very weak</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">95.00</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">90.00</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">0.21</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">0.06</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6cdbd7b9"><p class="cl-6cdbc714"><span class="cl-6cd92af4">Very-fairly weak</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">87.50</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">50.00</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">0.35</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6cdbd7b9"><p class="cl-6cdbc714"><span class="cl-6cd92af4">Weak-strong</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">80.00</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">20.00</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">0.51</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6cdbd7b9"><p class="cl-6cdbc714"><span class="cl-6cd92af4">Fairly-very strong</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">30.00</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">15.00</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">0.68</span></p></td><td class="cl-6cdbd7c2"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">4.67</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6cdbd7c3"><p class="cl-6cdbc714"><span class="cl-6cd92af4">Very-extreme strong</span></p></td><td class="cl-6cdbd7cc"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">10.00</span></p></td><td class="cl-6cdbd7cc"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">7.50</span></p></td><td class="cl-6cdbd7cc"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">0.81</span></p></td><td class="cl-6cdbd7cc"><p class="cl-6cdbc71e"><span class="cl-6cd92af4">12.00</span></p></td></tr></tbody></table></div>

</div>

------------------------------------------------------------------------

## Confidence Intervals

------------------------------------------------------------------------

- The graphs provide important information for whos memory is better.
  - For example, the ROC curve of Tim is greater at eacg threshold than the ROC curve of John.
- However, to fully examine the AUC we would like 95% CI.
  - This can be easily extracted in the pROC package.
  - You can either choose to bootstrap or use the [delong method](https://doi.org/10.1109/LSP.2014.2337313).
  - Using the 95%CI the AUC can be compared to the smallest effect size of interest (e.g., AUC_null = .8).

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

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

<div class="tabwid"><style>.cl-6d986ac2{table-layout:auto;}.cl-6d91ea3a{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6d91ea44{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-6d946f4e{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6d946f58{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-6d947da4{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6d947dae{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6d947daf{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6d947db0{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6d947db8{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-6d947dc2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-6d986ac2'><thead><tr style="overflow-wrap:break-word;"><th  colspan="7"class="cl-6d947da4"><p class="cl-6d946f4e"><span class="cl-6d91ea3a">AUC values with 95% CI via Delong or bootstrapping</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-6d947dae"><p class="cl-6d946f58"><span class="cl-6d91ea44">Name</span></p></th><th class="cl-6d947dae"><p class="cl-6d946f58"><span class="cl-6d91ea44">AUC</span></p></th><th class="cl-6d947dae"><p class="cl-6d946f58"><span class="cl-6d91ea44">CI_low_Delong</span></p></th><th class="cl-6d947dae"><p class="cl-6d946f58"><span class="cl-6d91ea44">CI_upper_Delong</span></p></th><th class="cl-6d947dae"><p class="cl-6d946f58"><span class="cl-6d91ea44">CI_low_boot</span></p></th><th class="cl-6d947dae"><p class="cl-6d946f58"><span class="cl-6d91ea44">CI_upper_boot</span></p></th><th class="cl-6d947dae"><p class="cl-6d946f58"><span class="cl-6d91ea44">Power</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-6d947daf"><p class="cl-6d946f4e"><span class="cl-6d91ea44">Tim</span></p></td><td class="cl-6d947db0"><p class="cl-6d946f58"><span class="cl-6d91ea44">0.88</span></p></td><td class="cl-6d947db0"><p class="cl-6d946f58"><span class="cl-6d91ea44">0.86</span></p></td><td class="cl-6d947db0"><p class="cl-6d946f58"><span class="cl-6d91ea44">0.89</span></p></td><td class="cl-6d947db0"><p class="cl-6d946f58"><span class="cl-6d91ea44">0.86</span></p></td><td class="cl-6d947db0"><p class="cl-6d946f58"><span class="cl-6d91ea44">0.89</span></p></td><td class="cl-6d947db0"><p class="cl-6d946f58"><span class="cl-6d91ea44">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-6d947db8"><p class="cl-6d946f4e"><span class="cl-6d91ea44">John</span></p></td><td class="cl-6d947dc2"><p class="cl-6d946f58"><span class="cl-6d91ea44">0.76</span></p></td><td class="cl-6d947dc2"><p class="cl-6d946f58"><span class="cl-6d91ea44">0.74</span></p></td><td class="cl-6d947dc2"><p class="cl-6d946f58"><span class="cl-6d91ea44">0.78</span></p></td><td class="cl-6d947dc2"><p class="cl-6d946f58"><span class="cl-6d91ea44">0.74</span></p></td><td class="cl-6d947dc2"><p class="cl-6d946f58"><span class="cl-6d91ea44">0.78</span></p></td><td class="cl-6d947dc2"><p class="cl-6d946f58"><span class="cl-6d91ea44">1.00</span></p></td></tr></tbody></table></div>

</div>

------------------------------------------------------------------------

## ROC curves with 95%

------------------------------------------------------------------------

- Preferably, we plot the confidence intervals along the ROC curve to better interpret the results

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

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

</div>

------------------------------------------------------------------------

## Partial AUC

------------------------------------------------------------------------

- We can calculate the area under the curve for a part of the curve.
  - One example is to calculate the curve for values under the 10% false positive
  - That is because we want to minimize the amount of false positive when it comes to eyewitness identification.

<div style="border: 1px solid #ccc; padding: 10px; background-color: #f8f9fa; border-radius: 5px;">

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

</div>
