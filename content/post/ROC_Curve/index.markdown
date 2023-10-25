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

<div class="tabwid"><style>.cl-190e81a0{table-layout:auto;}.cl-18f34d72{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-18f34d86{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-18fc106a{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-18fc1074{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-18fc410c{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-18fc4116{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-18fc4120{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-18fc412a{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-18fc412b{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-18fc4134{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-190e81a0'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-18fc410c"><p class="cl-18fc106a"><span class="cl-18f34d72">Dataset for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-18fc4116"><p class="cl-18fc1074"><span class="cl-18f34d86">old_items_tim</span></p></th><th class="cl-18fc4116"><p class="cl-18fc1074"><span class="cl-18f34d86">new_items_tim</span></p></th><th class="cl-18fc4116"><p class="cl-18fc1074"><span class="cl-18f34d86">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-18fc4120"><p class="cl-18fc106a"><span class="cl-18f34d86">50.00</span></p></td><td class="cl-18fc412a"><p class="cl-18fc1074"><span class="cl-18f34d86">250.00</span></p></td><td class="cl-18fc412a"><p class="cl-18fc1074"><span class="cl-18f34d86">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-18fc4120"><p class="cl-18fc106a"><span class="cl-18f34d86">50.00</span></p></td><td class="cl-18fc412a"><p class="cl-18fc1074"><span class="cl-18f34d86">250.00</span></p></td><td class="cl-18fc412a"><p class="cl-18fc1074"><span class="cl-18f34d86">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-18fc4120"><p class="cl-18fc106a"><span class="cl-18f34d86">50.00</span></p></td><td class="cl-18fc412a"><p class="cl-18fc1074"><span class="cl-18f34d86">400.00</span></p></td><td class="cl-18fc412a"><p class="cl-18fc1074"><span class="cl-18f34d86">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-18fc4120"><p class="cl-18fc106a"><span class="cl-18f34d86">150.00</span></p></td><td class="cl-18fc412a"><p class="cl-18fc1074"><span class="cl-18f34d86">50.00</span></p></td><td class="cl-18fc412a"><p class="cl-18fc1074"><span class="cl-18f34d86">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-18fc4120"><p class="cl-18fc106a"><span class="cl-18f34d86">500.00</span></p></td><td class="cl-18fc412a"><p class="cl-18fc1074"><span class="cl-18f34d86">25.00</span></p></td><td class="cl-18fc412a"><p class="cl-18fc1074"><span class="cl-18f34d86">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-18fc412b"><p class="cl-18fc106a"><span class="cl-18f34d86">200.00</span></p></td><td class="cl-18fc4134"><p class="cl-18fc1074"><span class="cl-18f34d86">25.00</span></p></td><td class="cl-18fc4134"><p class="cl-18fc1074"><span class="cl-18f34d86">extreme strong</span></p></td></tr></tbody></table></div>

- Dataset John

``` r
old_items_john <- c(50,75,75,500,200,100)
new_items_john <- c(100,400,300,50,75,75) # changed very strong to 75 instead of 25 to have equal number items between tim and john
df_john <- data.frame(old_items_john, new_items_john, confidence)
nice_table(df_john,
           title = "Dataset for John")
```

<div class="tabwid"><style>.cl-195864be{table-layout:auto;}.cl-1941eb8a{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1941eb9e{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-194a96f4{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-194a9712{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-194ac430{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-194ac444{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-194ac445{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-194ac44e{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-194ac458{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-194ac459{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-195864be'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-194ac430"><p class="cl-194a96f4"><span class="cl-1941eb8a">Dataset for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-194ac444"><p class="cl-194a9712"><span class="cl-1941eb9e">old_items_john</span></p></th><th class="cl-194ac444"><p class="cl-194a9712"><span class="cl-1941eb9e">new_items_john</span></p></th><th class="cl-194ac444"><p class="cl-194a9712"><span class="cl-1941eb9e">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-194ac445"><p class="cl-194a96f4"><span class="cl-1941eb9e">50.00</span></p></td><td class="cl-194ac44e"><p class="cl-194a9712"><span class="cl-1941eb9e">100.00</span></p></td><td class="cl-194ac44e"><p class="cl-194a9712"><span class="cl-1941eb9e">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-194ac445"><p class="cl-194a96f4"><span class="cl-1941eb9e">75.00</span></p></td><td class="cl-194ac44e"><p class="cl-194a9712"><span class="cl-1941eb9e">400.00</span></p></td><td class="cl-194ac44e"><p class="cl-194a9712"><span class="cl-1941eb9e">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-194ac445"><p class="cl-194a96f4"><span class="cl-1941eb9e">75.00</span></p></td><td class="cl-194ac44e"><p class="cl-194a9712"><span class="cl-1941eb9e">300.00</span></p></td><td class="cl-194ac44e"><p class="cl-194a9712"><span class="cl-1941eb9e">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-194ac445"><p class="cl-194a96f4"><span class="cl-1941eb9e">500.00</span></p></td><td class="cl-194ac44e"><p class="cl-194a9712"><span class="cl-1941eb9e">50.00</span></p></td><td class="cl-194ac44e"><p class="cl-194a9712"><span class="cl-1941eb9e">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-194ac445"><p class="cl-194a96f4"><span class="cl-1941eb9e">200.00</span></p></td><td class="cl-194ac44e"><p class="cl-194a9712"><span class="cl-1941eb9e">75.00</span></p></td><td class="cl-194ac44e"><p class="cl-194a9712"><span class="cl-1941eb9e">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-194ac458"><p class="cl-194a96f4"><span class="cl-1941eb9e">100.00</span></p></td><td class="cl-194ac459"><p class="cl-194a9712"><span class="cl-1941eb9e">75.00</span></p></td><td class="cl-194ac459"><p class="cl-194a9712"><span class="cl-1941eb9e">extreme strong</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-199ced78{table-layout:auto;}.cl-198a5294{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-198a52a8{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1991e3a6{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1991e3ba{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-19921222{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1992122c{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19921236{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19921237{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19921240{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1992124a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-199ced78'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-19921222"><p class="cl-1991e3a6"><span class="cl-198a5294">Dataset + Hit and False Alarm rate for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-1992122c"><p class="cl-1991e3ba"><span class="cl-198a52a8">old_items_tim</span></p></th><th class="cl-1992122c"><p class="cl-1991e3ba"><span class="cl-198a52a8">new_items_tim</span></p></th><th class="cl-1992122c"><p class="cl-1991e3ba"><span class="cl-198a52a8">confidence</span></p></th><th class="cl-1992122c"><p class="cl-1991e3ba"><span class="cl-198a52a8">hit</span></p></th><th class="cl-1992122c"><p class="cl-1991e3ba"><span class="cl-198a52a8">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-19921236"><p class="cl-1991e3a6"><span class="cl-198a52a8">50.00</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">250.00</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">extreme weak</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">0.05</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19921236"><p class="cl-1991e3a6"><span class="cl-198a52a8">50.00</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">250.00</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">very weak</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">0.10</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19921236"><p class="cl-1991e3a6"><span class="cl-198a52a8">50.00</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">400.00</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">fairly weak</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">0.15</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">0.90</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19921236"><p class="cl-1991e3a6"><span class="cl-198a52a8">150.00</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">50.00</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">fairly strong</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">0.30</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">0.95</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19921236"><p class="cl-1991e3a6"><span class="cl-198a52a8">500.00</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">25.00</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">very strong</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">0.80</span></p></td><td class="cl-19921237"><p class="cl-1991e3ba"><span class="cl-198a52a8">0.97</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19921240"><p class="cl-1991e3a6"><span class="cl-198a52a8">200.00</span></p></td><td class="cl-1992124a"><p class="cl-1991e3ba"><span class="cl-198a52a8">25.00</span></p></td><td class="cl-1992124a"><p class="cl-1991e3ba"><span class="cl-198a52a8">extreme strong</span></p></td><td class="cl-1992124a"><p class="cl-1991e3ba"><span class="cl-198a52a8">1.00</span></p></td><td class="cl-1992124a"><p class="cl-1991e3ba"><span class="cl-198a52a8">1.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-19df1ffe{table-layout:auto;}.cl-19ca7298{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-19ca72a2{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-19d24e64{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-19d24e78{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-19d27b32{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19d27b46{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19d27b47{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19d27b50{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19d27b5a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-19d27b64{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-19df1ffe'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-19d27b32"><p class="cl-19d24e64"><span class="cl-19ca7298">Dataset + Hit and False Alarm rate for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-19d27b46"><p class="cl-19d24e78"><span class="cl-19ca72a2">old_items_john</span></p></th><th class="cl-19d27b46"><p class="cl-19d24e78"><span class="cl-19ca72a2">new_items_john</span></p></th><th class="cl-19d27b46"><p class="cl-19d24e78"><span class="cl-19ca72a2">confidence</span></p></th><th class="cl-19d27b46"><p class="cl-19d24e78"><span class="cl-19ca72a2">hit</span></p></th><th class="cl-19d27b46"><p class="cl-19d24e78"><span class="cl-19ca72a2">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-19d27b47"><p class="cl-19d24e64"><span class="cl-19ca72a2">50.00</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">100.00</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">extreme weak</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">0.05</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">0.10</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19d27b47"><p class="cl-19d24e64"><span class="cl-19ca72a2">75.00</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">400.00</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">very weak</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">0.12</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19d27b47"><p class="cl-19d24e64"><span class="cl-19ca72a2">75.00</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">300.00</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">fairly weak</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">0.20</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">0.80</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19d27b47"><p class="cl-19d24e64"><span class="cl-19ca72a2">500.00</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">50.00</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">fairly strong</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">0.70</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">0.85</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19d27b47"><p class="cl-19d24e64"><span class="cl-19ca72a2">200.00</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">75.00</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">very strong</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">0.90</span></p></td><td class="cl-19d27b50"><p class="cl-19d24e78"><span class="cl-19ca72a2">0.92</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-19d27b5a"><p class="cl-19d24e64"><span class="cl-19ca72a2">100.00</span></p></td><td class="cl-19d27b64"><p class="cl-19d24e78"><span class="cl-19ca72a2">75.00</span></p></td><td class="cl-19d27b64"><p class="cl-19d24e78"><span class="cl-19ca72a2">extreme strong</span></p></td><td class="cl-19d27b64"><p class="cl-19d24e78"><span class="cl-19ca72a2">1.00</span></p></td><td class="cl-19d27b64"><p class="cl-19d24e78"><span class="cl-19ca72a2">1.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-1ade80f2{table-layout:auto;}.cl-1ac73f32{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1ac73f46{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1ad09cbc{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1ad09cd0{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1ad0c61a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1ad0c62e{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1ad0c638{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1ad0c639{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1ad0c642{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1ad0c643{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-1ade80f2'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-1ad0c61a"><p class="cl-1ad09cbc"><span class="cl-1ac73f32">True Positive and False Positive Percentages for each Threshold</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-1ad0c62e"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">Confidence</span></p></th><th class="cl-1ad0c62e"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">tpp</span></p></th><th class="cl-1ad0c62e"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">fpp</span></p></th><th class="cl-1ad0c62e"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">thresholds</span></p></th><th class="cl-1ad0c62e"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-1ad0c638"><p class="cl-1ad09cbc"><span class="cl-1ac73f46">Extreme-very weak</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">95.00</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">75.00</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">0.09</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">0.07</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1ad0c638"><p class="cl-1ad09cbc"><span class="cl-1ac73f46">Very-fairly weak</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">90.00</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">50.00</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">0.24</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">0.20</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1ad0c638"><p class="cl-1ad09cbc"><span class="cl-1ac73f46">Weak-strong</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">85.00</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">10.00</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">0.49</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">1.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1ad0c638"><p class="cl-1ad09cbc"><span class="cl-1ac73f46">Fairly-very strong</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">70.00</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">5.00</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">0.75</span></p></td><td class="cl-1ad0c639"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">6.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1ad0c642"><p class="cl-1ad09cbc"><span class="cl-1ac73f46">Very-extreme strong</span></p></td><td class="cl-1ad0c643"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">20.00</span></p></td><td class="cl-1ad0c643"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">2.50</span></p></td><td class="cl-1ad0c643"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">0.90</span></p></td><td class="cl-1ad0c643"><p class="cl-1ad09cd0"><span class="cl-1ac73f46">32.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-1b1cf08a{table-layout:auto;}.cl-1b09212c{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1b092140{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1b10da70{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1b10da7a{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1b10fd98{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1b10fda2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1b10fdac{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1b10fdb6{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1b10fdb7{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1b10fdc0{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-1b1cf08a'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-1b10fd98"><p class="cl-1b10da70"><span class="cl-1b09212c">True Positive and False Positive Percentages for each Threshold for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-1b10fda2"><p class="cl-1b10da7a"><span class="cl-1b092140">Confidence</span></p></th><th class="cl-1b10fda2"><p class="cl-1b10da7a"><span class="cl-1b092140">tpp</span></p></th><th class="cl-1b10fda2"><p class="cl-1b10da7a"><span class="cl-1b092140">fpp</span></p></th><th class="cl-1b10fda2"><p class="cl-1b10da7a"><span class="cl-1b092140">thresholds</span></p></th><th class="cl-1b10fda2"><p class="cl-1b10da7a"><span class="cl-1b092140">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-1b10fdac"><p class="cl-1b10da70"><span class="cl-1b092140">Extreme-very weak</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">95.00</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">90.00</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">0.21</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">0.06</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1b10fdac"><p class="cl-1b10da70"><span class="cl-1b092140">Very-fairly weak</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">87.50</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">50.00</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">0.35</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1b10fdac"><p class="cl-1b10da70"><span class="cl-1b092140">Weak-strong</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">80.00</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">20.00</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">0.51</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1b10fdac"><p class="cl-1b10da70"><span class="cl-1b092140">Fairly-very strong</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">30.00</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">15.00</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">0.68</span></p></td><td class="cl-1b10fdb6"><p class="cl-1b10da7a"><span class="cl-1b092140">4.67</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1b10fdb7"><p class="cl-1b10da70"><span class="cl-1b092140">Very-extreme strong</span></p></td><td class="cl-1b10fdc0"><p class="cl-1b10da7a"><span class="cl-1b092140">10.00</span></p></td><td class="cl-1b10fdc0"><p class="cl-1b10da7a"><span class="cl-1b092140">7.50</span></p></td><td class="cl-1b10fdc0"><p class="cl-1b10da7a"><span class="cl-1b092140">0.81</span></p></td><td class="cl-1b10fdc0"><p class="cl-1b10da7a"><span class="cl-1b092140">12.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-1d41d0ba{table-layout:auto;}.cl-1d2f34aa{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1d2f34be{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-1d36a852{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1d36a870{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-1d36db1a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1d36db2e{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1d36db2f{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1d36db38{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1d36db39{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-1d36db42{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-1d41d0ba'><thead><tr style="overflow-wrap:break-word;"><th  colspan="7"class="cl-1d36db1a"><p class="cl-1d36a852"><span class="cl-1d2f34aa">AUC values with 95% CI via Delong or bootstrapping</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-1d36db2e"><p class="cl-1d36a870"><span class="cl-1d2f34be">Name</span></p></th><th class="cl-1d36db2e"><p class="cl-1d36a870"><span class="cl-1d2f34be">AUC</span></p></th><th class="cl-1d36db2e"><p class="cl-1d36a870"><span class="cl-1d2f34be">CI_low_Delong</span></p></th><th class="cl-1d36db2e"><p class="cl-1d36a870"><span class="cl-1d2f34be">CI_upper_Delong</span></p></th><th class="cl-1d36db2e"><p class="cl-1d36a870"><span class="cl-1d2f34be">CI_low_boot</span></p></th><th class="cl-1d36db2e"><p class="cl-1d36a870"><span class="cl-1d2f34be">CI_upper_boot</span></p></th><th class="cl-1d36db2e"><p class="cl-1d36a870"><span class="cl-1d2f34be">Power</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-1d36db2f"><p class="cl-1d36a852"><span class="cl-1d2f34be">Tim</span></p></td><td class="cl-1d36db38"><p class="cl-1d36a870"><span class="cl-1d2f34be">0.88</span></p></td><td class="cl-1d36db38"><p class="cl-1d36a870"><span class="cl-1d2f34be">0.86</span></p></td><td class="cl-1d36db38"><p class="cl-1d36a870"><span class="cl-1d2f34be">0.89</span></p></td><td class="cl-1d36db38"><p class="cl-1d36a870"><span class="cl-1d2f34be">0.86</span></p></td><td class="cl-1d36db38"><p class="cl-1d36a870"><span class="cl-1d2f34be">0.89</span></p></td><td class="cl-1d36db38"><p class="cl-1d36a870"><span class="cl-1d2f34be">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-1d36db39"><p class="cl-1d36a852"><span class="cl-1d2f34be">John</span></p></td><td class="cl-1d36db42"><p class="cl-1d36a870"><span class="cl-1d2f34be">0.76</span></p></td><td class="cl-1d36db42"><p class="cl-1d36a870"><span class="cl-1d2f34be">0.74</span></p></td><td class="cl-1d36db42"><p class="cl-1d36a870"><span class="cl-1d2f34be">0.78</span></p></td><td class="cl-1d36db42"><p class="cl-1d36a870"><span class="cl-1d2f34be">0.74</span></p></td><td class="cl-1d36db42"><p class="cl-1d36a870"><span class="cl-1d2f34be">0.78</span></p></td><td class="cl-1d36db42"><p class="cl-1d36a870"><span class="cl-1d2f34be">1.00</span></p></td></tr></tbody></table></div>

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
