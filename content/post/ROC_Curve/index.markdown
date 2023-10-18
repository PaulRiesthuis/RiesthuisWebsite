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

# ROC cuve analysis of [Brady et al. (2022)](https://doi.org/10.3758/s13423-022-02179-w).

- Datasets of Tim and John

``` r
old_items_tim <- c(50,50,50,150,500,200)
new_items_tim <- c(250,250,400,50,25,25)
confidence <- c("extreme weak","very weak", "fairly weak", "fairly strong", "very strong", "extreme strong")
df_tim <- data.frame(old_items_tim, new_items_tim, confidence)
nice_table(df_tim,
           title= "Dataset for Tim")
```

<div class="tabwid"><style>.cl-392c2744{table-layout:auto;}.cl-390f6a28{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-390f6a3c{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3919ee30{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3919ee44{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-391a31f6{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-391a320a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-391a3214{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-391a321e{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-391a3228{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-391a3232{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-392c2744'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-391a31f6"><p class="cl-3919ee30"><span class="cl-390f6a28">Dataset for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-391a320a"><p class="cl-3919ee44"><span class="cl-390f6a3c">old_items_tim</span></p></th><th class="cl-391a320a"><p class="cl-3919ee44"><span class="cl-390f6a3c">new_items_tim</span></p></th><th class="cl-391a320a"><p class="cl-3919ee44"><span class="cl-390f6a3c">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-391a3214"><p class="cl-3919ee30"><span class="cl-390f6a3c">50.00</span></p></td><td class="cl-391a321e"><p class="cl-3919ee44"><span class="cl-390f6a3c">250.00</span></p></td><td class="cl-391a321e"><p class="cl-3919ee44"><span class="cl-390f6a3c">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-391a3214"><p class="cl-3919ee30"><span class="cl-390f6a3c">50.00</span></p></td><td class="cl-391a321e"><p class="cl-3919ee44"><span class="cl-390f6a3c">250.00</span></p></td><td class="cl-391a321e"><p class="cl-3919ee44"><span class="cl-390f6a3c">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-391a3214"><p class="cl-3919ee30"><span class="cl-390f6a3c">50.00</span></p></td><td class="cl-391a321e"><p class="cl-3919ee44"><span class="cl-390f6a3c">400.00</span></p></td><td class="cl-391a321e"><p class="cl-3919ee44"><span class="cl-390f6a3c">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-391a3214"><p class="cl-3919ee30"><span class="cl-390f6a3c">150.00</span></p></td><td class="cl-391a321e"><p class="cl-3919ee44"><span class="cl-390f6a3c">50.00</span></p></td><td class="cl-391a321e"><p class="cl-3919ee44"><span class="cl-390f6a3c">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-391a3214"><p class="cl-3919ee30"><span class="cl-390f6a3c">500.00</span></p></td><td class="cl-391a321e"><p class="cl-3919ee44"><span class="cl-390f6a3c">25.00</span></p></td><td class="cl-391a321e"><p class="cl-3919ee44"><span class="cl-390f6a3c">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-391a3228"><p class="cl-3919ee30"><span class="cl-390f6a3c">200.00</span></p></td><td class="cl-391a3232"><p class="cl-3919ee44"><span class="cl-390f6a3c">25.00</span></p></td><td class="cl-391a3232"><p class="cl-3919ee44"><span class="cl-390f6a3c">extreme strong</span></p></td></tr></tbody></table></div>

- Dataset John

``` r
old_items_john <- c(50,75,75,500,200,100)
new_items_john <- c(100,400,300,50,75,75) # changed very strong to 75 instead of 25 to have equal number items between tim and john
df_john <- data.frame(old_items_john, new_items_john, confidence)
nice_table(df_john,
           title = "Dataset for John")
```

<div class="tabwid"><style>.cl-396b7912{table-layout:auto;}.cl-3959c4c4{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3959c4d8{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-39608ebc{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-39608ec6{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3960acda{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3960ace4{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3960ace5{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3960acee{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3960acef{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3960acf8{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-396b7912'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-3960acda"><p class="cl-39608ebc"><span class="cl-3959c4c4">Dataset for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-3960ace4"><p class="cl-39608ec6"><span class="cl-3959c4d8">old_items_john</span></p></th><th class="cl-3960ace4"><p class="cl-39608ec6"><span class="cl-3959c4d8">new_items_john</span></p></th><th class="cl-3960ace4"><p class="cl-39608ec6"><span class="cl-3959c4d8">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-3960ace5"><p class="cl-39608ebc"><span class="cl-3959c4d8">50.00</span></p></td><td class="cl-3960acee"><p class="cl-39608ec6"><span class="cl-3959c4d8">100.00</span></p></td><td class="cl-3960acee"><p class="cl-39608ec6"><span class="cl-3959c4d8">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3960ace5"><p class="cl-39608ebc"><span class="cl-3959c4d8">75.00</span></p></td><td class="cl-3960acee"><p class="cl-39608ec6"><span class="cl-3959c4d8">400.00</span></p></td><td class="cl-3960acee"><p class="cl-39608ec6"><span class="cl-3959c4d8">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3960ace5"><p class="cl-39608ebc"><span class="cl-3959c4d8">75.00</span></p></td><td class="cl-3960acee"><p class="cl-39608ec6"><span class="cl-3959c4d8">300.00</span></p></td><td class="cl-3960acee"><p class="cl-39608ec6"><span class="cl-3959c4d8">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3960ace5"><p class="cl-39608ebc"><span class="cl-3959c4d8">500.00</span></p></td><td class="cl-3960acee"><p class="cl-39608ec6"><span class="cl-3959c4d8">50.00</span></p></td><td class="cl-3960acee"><p class="cl-39608ec6"><span class="cl-3959c4d8">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3960ace5"><p class="cl-39608ebc"><span class="cl-3959c4d8">200.00</span></p></td><td class="cl-3960acee"><p class="cl-39608ec6"><span class="cl-3959c4d8">75.00</span></p></td><td class="cl-3960acee"><p class="cl-39608ec6"><span class="cl-3959c4d8">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3960acef"><p class="cl-39608ebc"><span class="cl-3959c4d8">100.00</span></p></td><td class="cl-3960acf8"><p class="cl-39608ec6"><span class="cl-3959c4d8">75.00</span></p></td><td class="cl-3960acf8"><p class="cl-39608ec6"><span class="cl-3959c4d8">extreme strong</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-39a9ceba{table-layout:auto;}.cl-39960484{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-39960498{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-399dcd68{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-399dcd7c{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-399def0a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-399def14{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-399def1e{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-399def28{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-399def29{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-399def32{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-39a9ceba'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-399def0a"><p class="cl-399dcd68"><span class="cl-39960484">Dataset + Hit and False Alarm rate for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-399def14"><p class="cl-399dcd7c"><span class="cl-39960498">old_items_tim</span></p></th><th class="cl-399def14"><p class="cl-399dcd7c"><span class="cl-39960498">new_items_tim</span></p></th><th class="cl-399def14"><p class="cl-399dcd7c"><span class="cl-39960498">confidence</span></p></th><th class="cl-399def14"><p class="cl-399dcd7c"><span class="cl-39960498">hit</span></p></th><th class="cl-399def14"><p class="cl-399dcd7c"><span class="cl-39960498">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-399def1e"><p class="cl-399dcd68"><span class="cl-39960498">50.00</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">250.00</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">extreme weak</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">0.05</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-399def1e"><p class="cl-399dcd68"><span class="cl-39960498">50.00</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">250.00</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">very weak</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">0.10</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-399def1e"><p class="cl-399dcd68"><span class="cl-39960498">50.00</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">400.00</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">fairly weak</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">0.15</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">0.90</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-399def1e"><p class="cl-399dcd68"><span class="cl-39960498">150.00</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">50.00</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">fairly strong</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">0.30</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">0.95</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-399def1e"><p class="cl-399dcd68"><span class="cl-39960498">500.00</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">25.00</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">very strong</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">0.80</span></p></td><td class="cl-399def28"><p class="cl-399dcd7c"><span class="cl-39960498">0.97</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-399def29"><p class="cl-399dcd68"><span class="cl-39960498">200.00</span></p></td><td class="cl-399def32"><p class="cl-399dcd7c"><span class="cl-39960498">25.00</span></p></td><td class="cl-399def32"><p class="cl-399dcd7c"><span class="cl-39960498">extreme strong</span></p></td><td class="cl-399def32"><p class="cl-399dcd7c"><span class="cl-39960498">1.00</span></p></td><td class="cl-399def32"><p class="cl-399dcd7c"><span class="cl-39960498">1.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-39ed4e06{table-layout:auto;}.cl-39d77f72{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-39d77f86{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-39e031da{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-39e031f8{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-39e078ca{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-39e078de{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-39e078e8{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-39e078e9{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-39e078f2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-39e078fc{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-39ed4e06'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-39e078ca"><p class="cl-39e031da"><span class="cl-39d77f72">Dataset + Hit and False Alarm rate for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-39e078de"><p class="cl-39e031f8"><span class="cl-39d77f86">old_items_john</span></p></th><th class="cl-39e078de"><p class="cl-39e031f8"><span class="cl-39d77f86">new_items_john</span></p></th><th class="cl-39e078de"><p class="cl-39e031f8"><span class="cl-39d77f86">confidence</span></p></th><th class="cl-39e078de"><p class="cl-39e031f8"><span class="cl-39d77f86">hit</span></p></th><th class="cl-39e078de"><p class="cl-39e031f8"><span class="cl-39d77f86">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-39e078e8"><p class="cl-39e031da"><span class="cl-39d77f86">50.00</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">100.00</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">extreme weak</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">0.05</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">0.10</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-39e078e8"><p class="cl-39e031da"><span class="cl-39d77f86">75.00</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">400.00</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">very weak</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">0.12</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-39e078e8"><p class="cl-39e031da"><span class="cl-39d77f86">75.00</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">300.00</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">fairly weak</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">0.20</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">0.80</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-39e078e8"><p class="cl-39e031da"><span class="cl-39d77f86">500.00</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">50.00</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">fairly strong</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">0.70</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">0.85</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-39e078e8"><p class="cl-39e031da"><span class="cl-39d77f86">200.00</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">75.00</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">very strong</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">0.90</span></p></td><td class="cl-39e078e9"><p class="cl-39e031f8"><span class="cl-39d77f86">0.92</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-39e078f2"><p class="cl-39e031da"><span class="cl-39d77f86">100.00</span></p></td><td class="cl-39e078fc"><p class="cl-39e031f8"><span class="cl-39d77f86">75.00</span></p></td><td class="cl-39e078fc"><p class="cl-39e031f8"><span class="cl-39d77f86">extreme strong</span></p></td><td class="cl-39e078fc"><p class="cl-39e031f8"><span class="cl-39d77f86">1.00</span></p></td><td class="cl-39e078fc"><p class="cl-39e031f8"><span class="cl-39d77f86">1.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-3b1062dc{table-layout:auto;}.cl-3afcfb84{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3afcfb98{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3b062c40{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3b062c54{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3b0650ee{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3b065102{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3b065103{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3b06510c{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3b06510d{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3b065116{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-3b1062dc'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-3b0650ee"><p class="cl-3b062c40"><span class="cl-3afcfb84">True Positive and False Positive Percentages for each Threshold</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-3b065102"><p class="cl-3b062c54"><span class="cl-3afcfb98">Confidence</span></p></th><th class="cl-3b065102"><p class="cl-3b062c54"><span class="cl-3afcfb98">tpp</span></p></th><th class="cl-3b065102"><p class="cl-3b062c54"><span class="cl-3afcfb98">fpp</span></p></th><th class="cl-3b065102"><p class="cl-3b062c54"><span class="cl-3afcfb98">thresholds</span></p></th><th class="cl-3b065102"><p class="cl-3b062c54"><span class="cl-3afcfb98">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-3b065103"><p class="cl-3b062c40"><span class="cl-3afcfb98">Extreme-very weak</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">95.00</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">75.00</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">0.09</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">0.07</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3b065103"><p class="cl-3b062c40"><span class="cl-3afcfb98">Very-fairly weak</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">90.00</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">50.00</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">0.24</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">0.20</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3b065103"><p class="cl-3b062c40"><span class="cl-3afcfb98">Weak-strong</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">85.00</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">10.00</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">0.49</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">1.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3b065103"><p class="cl-3b062c40"><span class="cl-3afcfb98">Fairly-very strong</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">70.00</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">5.00</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">0.75</span></p></td><td class="cl-3b06510c"><p class="cl-3b062c54"><span class="cl-3afcfb98">6.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3b06510d"><p class="cl-3b062c40"><span class="cl-3afcfb98">Very-extreme strong</span></p></td><td class="cl-3b065116"><p class="cl-3b062c54"><span class="cl-3afcfb98">20.00</span></p></td><td class="cl-3b065116"><p class="cl-3b062c54"><span class="cl-3afcfb98">2.50</span></p></td><td class="cl-3b065116"><p class="cl-3b062c54"><span class="cl-3afcfb98">0.90</span></p></td><td class="cl-3b065116"><p class="cl-3b062c54"><span class="cl-3afcfb98">32.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-3b57ace6{table-layout:auto;}.cl-3b43479c{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3b4347b0{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3b4b3fb0{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3b4b3fc4{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3b4b69a4{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3b4b69b8{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3b4b69c2{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3b4b69c3{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3b4b69cc{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3b4b69cd{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-3b57ace6'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-3b4b69a4"><p class="cl-3b4b3fb0"><span class="cl-3b43479c">True Positive and False Positive Percentages for each Threshold for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-3b4b69b8"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">Confidence</span></p></th><th class="cl-3b4b69b8"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">tpp</span></p></th><th class="cl-3b4b69b8"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">fpp</span></p></th><th class="cl-3b4b69b8"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">thresholds</span></p></th><th class="cl-3b4b69b8"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-3b4b69c2"><p class="cl-3b4b3fb0"><span class="cl-3b4347b0">Extreme-very weak</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">95.00</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">90.00</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">0.21</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">0.06</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3b4b69c2"><p class="cl-3b4b3fb0"><span class="cl-3b4347b0">Very-fairly weak</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">87.50</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">50.00</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">0.35</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3b4b69c2"><p class="cl-3b4b3fb0"><span class="cl-3b4347b0">Weak-strong</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">80.00</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">20.00</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">0.51</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3b4b69c2"><p class="cl-3b4b3fb0"><span class="cl-3b4347b0">Fairly-very strong</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">30.00</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">15.00</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">0.68</span></p></td><td class="cl-3b4b69c3"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">4.67</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3b4b69cc"><p class="cl-3b4b3fb0"><span class="cl-3b4347b0">Very-extreme strong</span></p></td><td class="cl-3b4b69cd"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">10.00</span></p></td><td class="cl-3b4b69cd"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">7.50</span></p></td><td class="cl-3b4b69cd"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">0.81</span></p></td><td class="cl-3b4b69cd"><p class="cl-3b4b3fc4"><span class="cl-3b4347b0">12.00</span></p></td></tr></tbody></table></div>

## Confidence Intervals

- However, to fully examine the AUC we would like 95% CI
  - This can be easily extracted in the pROC package
  - You can either choose to bootstrap or use the [delong](https://doi.org/10.1109/LSP.2014.2337313)
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

<div class="tabwid"><style>.cl-3ea34f4a{table-layout:auto;}.cl-3e85342e{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3e853438{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3e8ee050{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3e8ee06e{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3e8f4130{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3e8f414e{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3e8f4158{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3e8f4162{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3e8f416c{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3e8f4176{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-3ea34f4a'><thead><tr style="overflow-wrap:break-word;"><th  colspan="7"class="cl-3e8f4130"><p class="cl-3e8ee050"><span class="cl-3e85342e">AUC values with 95% CI via Delong or bootstrapping</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-3e8f414e"><p class="cl-3e8ee06e"><span class="cl-3e853438">Name</span></p></th><th class="cl-3e8f414e"><p class="cl-3e8ee06e"><span class="cl-3e853438">AUC</span></p></th><th class="cl-3e8f414e"><p class="cl-3e8ee06e"><span class="cl-3e853438">CI_low_Delong</span></p></th><th class="cl-3e8f414e"><p class="cl-3e8ee06e"><span class="cl-3e853438">CI_upper_Delong</span></p></th><th class="cl-3e8f414e"><p class="cl-3e8ee06e"><span class="cl-3e853438">CI_low_boot</span></p></th><th class="cl-3e8f414e"><p class="cl-3e8ee06e"><span class="cl-3e853438">CI_upper_boot</span></p></th><th class="cl-3e8f414e"><p class="cl-3e8ee06e"><span class="cl-3e853438">Power</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-3e8f4158"><p class="cl-3e8ee050"><span class="cl-3e853438">Tim</span></p></td><td class="cl-3e8f4162"><p class="cl-3e8ee06e"><span class="cl-3e853438">0.88</span></p></td><td class="cl-3e8f4162"><p class="cl-3e8ee06e"><span class="cl-3e853438">0.86</span></p></td><td class="cl-3e8f4162"><p class="cl-3e8ee06e"><span class="cl-3e853438">0.89</span></p></td><td class="cl-3e8f4162"><p class="cl-3e8ee06e"><span class="cl-3e853438">0.86</span></p></td><td class="cl-3e8f4162"><p class="cl-3e8ee06e"><span class="cl-3e853438">0.89</span></p></td><td class="cl-3e8f4162"><p class="cl-3e8ee06e"><span class="cl-3e853438">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3e8f416c"><p class="cl-3e8ee050"><span class="cl-3e853438">John</span></p></td><td class="cl-3e8f4176"><p class="cl-3e8ee06e"><span class="cl-3e853438">0.76</span></p></td><td class="cl-3e8f4176"><p class="cl-3e8ee06e"><span class="cl-3e853438">0.74</span></p></td><td class="cl-3e8f4176"><p class="cl-3e8ee06e"><span class="cl-3e853438">0.78</span></p></td><td class="cl-3e8f4176"><p class="cl-3e8ee06e"><span class="cl-3e853438">0.74</span></p></td><td class="cl-3e8f4176"><p class="cl-3e8ee06e"><span class="cl-3e853438">0.78</span></p></td><td class="cl-3e8f4176"><p class="cl-3e8ee06e"><span class="cl-3e853438">1.00</span></p></td></tr></tbody></table></div>

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
