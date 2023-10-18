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
    theme: journal
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

**Page Under Construction**

# ROC cuve analysis of [Brady et al. (2022)](https://doi.org/10.3758/BF03207704).

- Datasets of Tim and John

``` r
old_items_tim <- c(50,50,50,150,500,200)
new_items_tim <- c(250,250,400,50,25,25)
confidence <- c("extreme weak","very weak", "fairly weak", "fairly strong", "very strong", "extreme strong")
df_tim <- data.frame(old_items_tim, new_items_tim, confidence)
nice_table(df_tim,
           title= "Dataset for Tim")
```

<div class="tabwid"><style>.cl-17579118{table-layout:auto;}.cl-173770d6{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-173770f4{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-17451164{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-17451178{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-17454a94{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-17454aa8{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-17454ab2{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-17454ab3{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-17454abc{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-17454ac6{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-17579118'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-17454a94"><p class="cl-17451164"><span class="cl-173770d6">Dataset for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-17454aa8"><p class="cl-17451178"><span class="cl-173770f4">old_items_tim</span></p></th><th class="cl-17454aa8"><p class="cl-17451178"><span class="cl-173770f4">new_items_tim</span></p></th><th class="cl-17454aa8"><p class="cl-17451178"><span class="cl-173770f4">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-17454ab2"><p class="cl-17451164"><span class="cl-173770f4">50.00</span></p></td><td class="cl-17454ab3"><p class="cl-17451178"><span class="cl-173770f4">250.00</span></p></td><td class="cl-17454ab3"><p class="cl-17451178"><span class="cl-173770f4">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-17454ab2"><p class="cl-17451164"><span class="cl-173770f4">50.00</span></p></td><td class="cl-17454ab3"><p class="cl-17451178"><span class="cl-173770f4">250.00</span></p></td><td class="cl-17454ab3"><p class="cl-17451178"><span class="cl-173770f4">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-17454ab2"><p class="cl-17451164"><span class="cl-173770f4">50.00</span></p></td><td class="cl-17454ab3"><p class="cl-17451178"><span class="cl-173770f4">400.00</span></p></td><td class="cl-17454ab3"><p class="cl-17451178"><span class="cl-173770f4">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-17454ab2"><p class="cl-17451164"><span class="cl-173770f4">150.00</span></p></td><td class="cl-17454ab3"><p class="cl-17451178"><span class="cl-173770f4">50.00</span></p></td><td class="cl-17454ab3"><p class="cl-17451178"><span class="cl-173770f4">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-17454ab2"><p class="cl-17451164"><span class="cl-173770f4">500.00</span></p></td><td class="cl-17454ab3"><p class="cl-17451178"><span class="cl-173770f4">25.00</span></p></td><td class="cl-17454ab3"><p class="cl-17451178"><span class="cl-173770f4">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-17454abc"><p class="cl-17451164"><span class="cl-173770f4">200.00</span></p></td><td class="cl-17454ac6"><p class="cl-17451178"><span class="cl-173770f4">25.00</span></p></td><td class="cl-17454ac6"><p class="cl-17451178"><span class="cl-173770f4">extreme strong</span></p></td></tr></tbody></table></div>

- Dataset John

``` r
old_items_john <- c(50,75,75,500,200,100)
new_items_john <- c(100,400,300,50,75,75) # changed very strong to 75 instead of 25 to have equal number items between tim and john
df_john <- data.frame(old_items_john, new_items_john, confidence)
nice_table(df_john,
           title = "Dataset for John")
```

<div class="tabwid"><style>.cl-179f3ce8{table-layout:auto;}.cl-1788cbc0{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1788cbd4{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-17903590{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-179035a4{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-179064d4{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-179064e8{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-179064f2{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-179064f3{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-179064fc{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-17906506{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-179f3ce8'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-179064d4"><p class="cl-17903590"><span class="cl-1788cbc0">Dataset for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-179064e8"><p class="cl-179035a4"><span class="cl-1788cbd4">old_items_john</span></p></th><th class="cl-179064e8"><p class="cl-179035a4"><span class="cl-1788cbd4">new_items_john</span></p></th><th class="cl-179064e8"><p class="cl-179035a4"><span class="cl-1788cbd4">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-179064f2"><p class="cl-17903590"><span class="cl-1788cbd4">50.00</span></p></td><td class="cl-179064f3"><p class="cl-179035a4"><span class="cl-1788cbd4">100.00</span></p></td><td class="cl-179064f3"><p class="cl-179035a4"><span class="cl-1788cbd4">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-179064f2"><p class="cl-17903590"><span class="cl-1788cbd4">75.00</span></p></td><td class="cl-179064f3"><p class="cl-179035a4"><span class="cl-1788cbd4">400.00</span></p></td><td class="cl-179064f3"><p class="cl-179035a4"><span class="cl-1788cbd4">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-179064f2"><p class="cl-17903590"><span class="cl-1788cbd4">75.00</span></p></td><td class="cl-179064f3"><p class="cl-179035a4"><span class="cl-1788cbd4">300.00</span></p></td><td class="cl-179064f3"><p class="cl-179035a4"><span class="cl-1788cbd4">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-179064f2"><p class="cl-17903590"><span class="cl-1788cbd4">500.00</span></p></td><td class="cl-179064f3"><p class="cl-179035a4"><span class="cl-1788cbd4">50.00</span></p></td><td class="cl-179064f3"><p class="cl-179035a4"><span class="cl-1788cbd4">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-179064f2"><p class="cl-17903590"><span class="cl-1788cbd4">200.00</span></p></td><td class="cl-179064f3"><p class="cl-179035a4"><span class="cl-1788cbd4">75.00</span></p></td><td class="cl-179064f3"><p class="cl-179035a4"><span class="cl-1788cbd4">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-179064fc"><p class="cl-17903590"><span class="cl-1788cbd4">100.00</span></p></td><td class="cl-17906506"><p class="cl-179035a4"><span class="cl-1788cbd4">75.00</span></p></td><td class="cl-17906506"><p class="cl-179035a4"><span class="cl-1788cbd4">extreme strong</span></p></td></tr></tbody></table></div>

## Add hit rate and false alarm rate

- Tim

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

<div class="tabwid"><style>.cl-17ea350e{table-layout:auto;}.cl-17d4de98{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-17d4deb6{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-17dd8250{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-17dd825a{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-17ddb0c2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-17ddb0cc{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-17ddb0d6{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-17ddb0e0{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-17ddb0ea{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-17ddb0eb{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-17ea350e'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-17ddb0c2"><p class="cl-17dd8250"><span class="cl-17d4de98">Dataset + Hit and False Alarm rate for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-17ddb0cc"><p class="cl-17dd825a"><span class="cl-17d4deb6">old_items_tim</span></p></th><th class="cl-17ddb0cc"><p class="cl-17dd825a"><span class="cl-17d4deb6">new_items_tim</span></p></th><th class="cl-17ddb0cc"><p class="cl-17dd825a"><span class="cl-17d4deb6">confidence</span></p></th><th class="cl-17ddb0cc"><p class="cl-17dd825a"><span class="cl-17d4deb6">hit</span></p></th><th class="cl-17ddb0cc"><p class="cl-17dd825a"><span class="cl-17d4deb6">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-17ddb0d6"><p class="cl-17dd8250"><span class="cl-17d4deb6">50.00</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">250.00</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">extreme weak</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">0.05</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-17ddb0d6"><p class="cl-17dd8250"><span class="cl-17d4deb6">50.00</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">250.00</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">very weak</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">0.10</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-17ddb0d6"><p class="cl-17dd8250"><span class="cl-17d4deb6">50.00</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">400.00</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">fairly weak</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">0.15</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">0.90</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-17ddb0d6"><p class="cl-17dd8250"><span class="cl-17d4deb6">150.00</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">50.00</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">fairly strong</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">0.30</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">0.95</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-17ddb0d6"><p class="cl-17dd8250"><span class="cl-17d4deb6">500.00</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">25.00</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">very strong</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">0.80</span></p></td><td class="cl-17ddb0e0"><p class="cl-17dd825a"><span class="cl-17d4deb6">0.97</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-17ddb0ea"><p class="cl-17dd8250"><span class="cl-17d4deb6">200.00</span></p></td><td class="cl-17ddb0eb"><p class="cl-17dd825a"><span class="cl-17d4deb6">25.00</span></p></td><td class="cl-17ddb0eb"><p class="cl-17dd825a"><span class="cl-17d4deb6">extreme strong</span></p></td><td class="cl-17ddb0eb"><p class="cl-17dd825a"><span class="cl-17d4deb6">1.00</span></p></td><td class="cl-17ddb0eb"><p class="cl-17dd825a"><span class="cl-17d4deb6">1.00</span></p></td></tr></tbody></table></div>

- John

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

<div class="tabwid"><style>.cl-184e8284{table-layout:auto;}.cl-1819730a{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-18197328{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1820e252{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1820e266{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-182103ea{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-182103fe{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-182103ff{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-18210408{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-18210409{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-18210412{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-184e8284'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-182103ea"><p class="cl-1820e252"><span class="cl-1819730a">Dataset + Hit and False Alarm rate for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-182103fe"><p class="cl-1820e266"><span class="cl-18197328">old_items_john</span></p></th><th class="cl-182103fe"><p class="cl-1820e266"><span class="cl-18197328">new_items_john</span></p></th><th class="cl-182103fe"><p class="cl-1820e266"><span class="cl-18197328">confidence</span></p></th><th class="cl-182103fe"><p class="cl-1820e266"><span class="cl-18197328">hit</span></p></th><th class="cl-182103fe"><p class="cl-1820e266"><span class="cl-18197328">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-182103ff"><p class="cl-1820e252"><span class="cl-18197328">50.00</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">100.00</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">extreme weak</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">0.05</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">0.10</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-182103ff"><p class="cl-1820e252"><span class="cl-18197328">75.00</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">400.00</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">very weak</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">0.12</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-182103ff"><p class="cl-1820e252"><span class="cl-18197328">75.00</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">300.00</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">fairly weak</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">0.20</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">0.80</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-182103ff"><p class="cl-1820e252"><span class="cl-18197328">500.00</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">50.00</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">fairly strong</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">0.70</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">0.85</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-182103ff"><p class="cl-1820e252"><span class="cl-18197328">200.00</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">75.00</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">very strong</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">0.90</span></p></td><td class="cl-18210408"><p class="cl-1820e266"><span class="cl-18197328">0.92</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-18210409"><p class="cl-1820e252"><span class="cl-18197328">100.00</span></p></td><td class="cl-18210412"><p class="cl-1820e266"><span class="cl-18197328">75.00</span></p></td><td class="cl-18210412"><p class="cl-1820e266"><span class="cl-18197328">extreme strong</span></p></td><td class="cl-18210412"><p class="cl-1820e266"><span class="cl-18197328">1.00</span></p></td><td class="cl-18210412"><p class="cl-1820e266"><span class="cl-18197328">1.00</span></p></td></tr></tbody></table></div>

## Plot ROC Curves for Tim and John

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

## Same ROC curve via pROC

- First we have to create an item data set

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

p1 <- roc(correct_mem, glm.fit_john$fitted.values, 
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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" />

## True and False Positive per Threshold

- This can be used to examine the blackstone ratio (10:1; false negative to false positive ratio)
- For Tim

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

<div class="tabwid"><style>.cl-19ecd730{table-layout:auto;}.cl-19d29366{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-19d2937a{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-19ddb6ce{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-19ddb6ec{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-19ddf526{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19ddf53a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19ddf544{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19ddf545{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19ddf54e{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19ddf558{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-19ecd730'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-19ddf526"><p class="cl-19ddb6ce"><span class="cl-19d29366">True Positive and False Positive Percentages for each Threshold</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-19ddf53a"><p class="cl-19ddb6ec"><span class="cl-19d2937a">Confidence</span></p></th><th class="cl-19ddf53a"><p class="cl-19ddb6ec"><span class="cl-19d2937a">tpp</span></p></th><th class="cl-19ddf53a"><p class="cl-19ddb6ec"><span class="cl-19d2937a">fpp</span></p></th><th class="cl-19ddf53a"><p class="cl-19ddb6ec"><span class="cl-19d2937a">thresholds</span></p></th><th class="cl-19ddf53a"><p class="cl-19ddb6ec"><span class="cl-19d2937a">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-19ddf544"><p class="cl-19ddb6ce"><span class="cl-19d2937a">Extreme-very weak</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">95.00</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">75.00</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">0.09</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">0.07</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19ddf544"><p class="cl-19ddb6ce"><span class="cl-19d2937a">Very-fairly weak</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">90.00</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">50.00</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">0.24</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">0.20</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19ddf544"><p class="cl-19ddb6ce"><span class="cl-19d2937a">Weak-strong</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">85.00</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">10.00</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">0.49</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">1.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19ddf544"><p class="cl-19ddb6ce"><span class="cl-19d2937a">Fairly-very strong</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">70.00</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">5.00</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">0.75</span></p></td><td class="cl-19ddf545"><p class="cl-19ddb6ec"><span class="cl-19d2937a">6.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19ddf54e"><p class="cl-19ddb6ce"><span class="cl-19d2937a">Very-extreme strong</span></p></td><td class="cl-19ddf558"><p class="cl-19ddb6ec"><span class="cl-19d2937a">20.00</span></p></td><td class="cl-19ddf558"><p class="cl-19ddb6ec"><span class="cl-19d2937a">2.50</span></p></td><td class="cl-19ddf558"><p class="cl-19ddb6ec"><span class="cl-19d2937a">0.90</span></p></td><td class="cl-19ddf558"><p class="cl-19ddb6ec"><span class="cl-19d2937a">32.00</span></p></td></tr></tbody></table></div>

- John

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

<div class="tabwid"><style>.cl-1a443e9e{table-layout:auto;}.cl-1a2a19f6{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1a2a1a14{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1a345326{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1a345344{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1a349638{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1a349656{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1a349660{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1a34966a{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1a349674{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1a349675{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-1a443e9e'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-1a349638"><p class="cl-1a345326"><span class="cl-1a2a19f6">True Positive and False Positive Percentages for each Threshold for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-1a349656"><p class="cl-1a345344"><span class="cl-1a2a1a14">Confidence</span></p></th><th class="cl-1a349656"><p class="cl-1a345344"><span class="cl-1a2a1a14">tpp</span></p></th><th class="cl-1a349656"><p class="cl-1a345344"><span class="cl-1a2a1a14">fpp</span></p></th><th class="cl-1a349656"><p class="cl-1a345344"><span class="cl-1a2a1a14">thresholds</span></p></th><th class="cl-1a349656"><p class="cl-1a345344"><span class="cl-1a2a1a14">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-1a349660"><p class="cl-1a345326"><span class="cl-1a2a1a14">Extreme-very weak</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">95.00</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">90.00</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">0.21</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">0.06</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1a349660"><p class="cl-1a345326"><span class="cl-1a2a1a14">Very-fairly weak</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">87.50</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">50.00</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">0.35</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1a349660"><p class="cl-1a345326"><span class="cl-1a2a1a14">Weak-strong</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">80.00</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">20.00</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">0.51</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1a349660"><p class="cl-1a345326"><span class="cl-1a2a1a14">Fairly-very strong</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">30.00</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">15.00</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">0.68</span></p></td><td class="cl-1a34966a"><p class="cl-1a345344"><span class="cl-1a2a1a14">4.67</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1a349674"><p class="cl-1a345326"><span class="cl-1a2a1a14">Very-extreme strong</span></p></td><td class="cl-1a349675"><p class="cl-1a345344"><span class="cl-1a2a1a14">10.00</span></p></td><td class="cl-1a349675"><p class="cl-1a345344"><span class="cl-1a2a1a14">7.50</span></p></td><td class="cl-1a349675"><p class="cl-1a345344"><span class="cl-1a2a1a14">0.81</span></p></td><td class="cl-1a349675"><p class="cl-1a345344"><span class="cl-1a2a1a14">12.00</span></p></td></tr></tbody></table></div>

## Confidence Intervals

- However, to fully examine the AUC we would like 95% CI
  - This can be easily extracted in the pROC package
  - You can either choose to bootstrap or use the [delong](10.1109/LSP.2014.2337313)
  - Using the 95%CI the AUC can be compared to the smallest effect size of interest (e.g., AUC_null = .8)

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

<div class="tabwid"><style>.cl-1cd9e118{table-layout:auto;}.cl-1cc39a52{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1cc39a66{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1ccb9158{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1ccb916c{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1ccbbe6c{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1ccbbe76{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1ccbbe80{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1ccbbe8a{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1ccbbe94{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1ccbbe9e{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-1cd9e118'><thead><tr style="overflow-wrap:break-word;"><th  colspan="7"class="cl-1ccbbe6c"><p class="cl-1ccb9158"><span class="cl-1cc39a52">AUC values with 95% CI via Delong or bootstrapping</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-1ccbbe76"><p class="cl-1ccb916c"><span class="cl-1cc39a66">Name</span></p></th><th class="cl-1ccbbe76"><p class="cl-1ccb916c"><span class="cl-1cc39a66">AUC</span></p></th><th class="cl-1ccbbe76"><p class="cl-1ccb916c"><span class="cl-1cc39a66">CI_low_Delong</span></p></th><th class="cl-1ccbbe76"><p class="cl-1ccb916c"><span class="cl-1cc39a66">CI_upper_Delong</span></p></th><th class="cl-1ccbbe76"><p class="cl-1ccb916c"><span class="cl-1cc39a66">CI_low_boot</span></p></th><th class="cl-1ccbbe76"><p class="cl-1ccb916c"><span class="cl-1cc39a66">CI_upper_boot</span></p></th><th class="cl-1ccbbe76"><p class="cl-1ccb916c"><span class="cl-1cc39a66">Power</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-1ccbbe80"><p class="cl-1ccb9158"><span class="cl-1cc39a66">Tim</span></p></td><td class="cl-1ccbbe8a"><p class="cl-1ccb916c"><span class="cl-1cc39a66">0.88</span></p></td><td class="cl-1ccbbe8a"><p class="cl-1ccb916c"><span class="cl-1cc39a66">0.86</span></p></td><td class="cl-1ccbbe8a"><p class="cl-1ccb916c"><span class="cl-1cc39a66">0.89</span></p></td><td class="cl-1ccbbe8a"><p class="cl-1ccb916c"><span class="cl-1cc39a66">0.86</span></p></td><td class="cl-1ccbbe8a"><p class="cl-1ccb916c"><span class="cl-1cc39a66">0.89</span></p></td><td class="cl-1ccbbe8a"><p class="cl-1ccb916c"><span class="cl-1cc39a66">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1ccbbe94"><p class="cl-1ccb9158"><span class="cl-1cc39a66">John</span></p></td><td class="cl-1ccbbe9e"><p class="cl-1ccb916c"><span class="cl-1cc39a66">0.76</span></p></td><td class="cl-1ccbbe9e"><p class="cl-1ccb916c"><span class="cl-1cc39a66">0.74</span></p></td><td class="cl-1ccbbe9e"><p class="cl-1ccb916c"><span class="cl-1cc39a66">0.78</span></p></td><td class="cl-1ccbbe9e"><p class="cl-1ccb916c"><span class="cl-1cc39a66">0.74</span></p></td><td class="cl-1ccbbe9e"><p class="cl-1ccb916c"><span class="cl-1cc39a66">0.78</span></p></td><td class="cl-1ccbbe9e"><p class="cl-1ccb916c"><span class="cl-1cc39a66">1.00</span></p></td></tr></tbody></table></div>

## Partial AUC

- We can calculate the area under the curve for a part of the curve.
  - One example is to calculate the curve for values under the 10% false positive
  - That is because we want to minimize the amount of false positive when it comes to eyewitness identification.

``` r
# Calculate the partial AUC and confidence intervals
partial_auc_tim <- roc(correct_mem, glm.fit_tim$fitted.values, partial.auc=c(100,90), percent = T)
partial_auc_john <- roc(correct_mem, glm.fit_john$fitted.values, partial.auc=c(100,90), percent = T)

# Print the partial AUC and confidence intervals
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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-10-1.png" width="672" />
