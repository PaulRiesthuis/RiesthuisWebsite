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

<div class="tabwid"><style>.cl-d4f3db96{table-layout:auto;}.cl-d4bb5e2e{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d4bb5e42{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d4d894d0{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d4d894f8{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d4d98494{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d4d984d0{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d4d984e4{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d4d984ee{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d4d984f8{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d4d98502{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-d4f3db96'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-d4d98494"><p class="cl-d4d894d0"><span class="cl-d4bb5e2e">Dataset for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-d4d984d0"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">old_items_tim</span></p></th><th class="cl-d4d984d0"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">new_items_tim</span></p></th><th class="cl-d4d984d0"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-d4d984e4"><p class="cl-d4d894d0"><span class="cl-d4bb5e42">50.00</span></p></td><td class="cl-d4d984ee"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">250.00</span></p></td><td class="cl-d4d984ee"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d4d984e4"><p class="cl-d4d894d0"><span class="cl-d4bb5e42">50.00</span></p></td><td class="cl-d4d984ee"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">250.00</span></p></td><td class="cl-d4d984ee"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d4d984e4"><p class="cl-d4d894d0"><span class="cl-d4bb5e42">50.00</span></p></td><td class="cl-d4d984ee"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">400.00</span></p></td><td class="cl-d4d984ee"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d4d984e4"><p class="cl-d4d894d0"><span class="cl-d4bb5e42">150.00</span></p></td><td class="cl-d4d984ee"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">50.00</span></p></td><td class="cl-d4d984ee"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d4d984e4"><p class="cl-d4d894d0"><span class="cl-d4bb5e42">500.00</span></p></td><td class="cl-d4d984ee"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">25.00</span></p></td><td class="cl-d4d984ee"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d4d984f8"><p class="cl-d4d894d0"><span class="cl-d4bb5e42">200.00</span></p></td><td class="cl-d4d98502"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">25.00</span></p></td><td class="cl-d4d98502"><p class="cl-d4d894f8"><span class="cl-d4bb5e42">extreme strong</span></p></td></tr></tbody></table></div>

- Dataset John

``` r
old_items_john <- c(50,75,75,500,200,100)
new_items_john <- c(100,400,300,50,75,75) # changed very strong to 75 instead of 25 to have equal number items between tim and john
df_john <- data.frame(old_items_john, new_items_john, confidence)
nice_table(df_john,
           title = "Dataset for John")
```

<div class="tabwid"><style>.cl-d56d819e{table-layout:auto;}.cl-d52dd65c{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d52dd67a{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d54232e6{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d5423304{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d542f41a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d542f438{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d542f442{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d542f456{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d542f460{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d542f461{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-d56d819e'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-d542f41a"><p class="cl-d54232e6"><span class="cl-d52dd65c">Dataset for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-d542f438"><p class="cl-d5423304"><span class="cl-d52dd67a">old_items_john</span></p></th><th class="cl-d542f438"><p class="cl-d5423304"><span class="cl-d52dd67a">new_items_john</span></p></th><th class="cl-d542f438"><p class="cl-d5423304"><span class="cl-d52dd67a">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-d542f442"><p class="cl-d54232e6"><span class="cl-d52dd67a">50.00</span></p></td><td class="cl-d542f456"><p class="cl-d5423304"><span class="cl-d52dd67a">100.00</span></p></td><td class="cl-d542f456"><p class="cl-d5423304"><span class="cl-d52dd67a">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d542f442"><p class="cl-d54232e6"><span class="cl-d52dd67a">75.00</span></p></td><td class="cl-d542f456"><p class="cl-d5423304"><span class="cl-d52dd67a">400.00</span></p></td><td class="cl-d542f456"><p class="cl-d5423304"><span class="cl-d52dd67a">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d542f442"><p class="cl-d54232e6"><span class="cl-d52dd67a">75.00</span></p></td><td class="cl-d542f456"><p class="cl-d5423304"><span class="cl-d52dd67a">300.00</span></p></td><td class="cl-d542f456"><p class="cl-d5423304"><span class="cl-d52dd67a">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d542f442"><p class="cl-d54232e6"><span class="cl-d52dd67a">500.00</span></p></td><td class="cl-d542f456"><p class="cl-d5423304"><span class="cl-d52dd67a">50.00</span></p></td><td class="cl-d542f456"><p class="cl-d5423304"><span class="cl-d52dd67a">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d542f442"><p class="cl-d54232e6"><span class="cl-d52dd67a">200.00</span></p></td><td class="cl-d542f456"><p class="cl-d5423304"><span class="cl-d52dd67a">75.00</span></p></td><td class="cl-d542f456"><p class="cl-d5423304"><span class="cl-d52dd67a">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d542f460"><p class="cl-d54232e6"><span class="cl-d52dd67a">100.00</span></p></td><td class="cl-d542f461"><p class="cl-d5423304"><span class="cl-d52dd67a">75.00</span></p></td><td class="cl-d542f461"><p class="cl-d5423304"><span class="cl-d52dd67a">extreme strong</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-d5be638e{table-layout:auto;}.cl-d5a49756{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d5a49774{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d5ace01e{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d5ace032{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d5ad0ed6{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d5ad0ee0{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d5ad0eea{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d5ad0eeb{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d5ad0ef4{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d5ad0ef5{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-d5be638e'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-d5ad0ed6"><p class="cl-d5ace01e"><span class="cl-d5a49756">Dataset + Hit and False Alarm rate for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-d5ad0ee0"><p class="cl-d5ace032"><span class="cl-d5a49774">old_items_tim</span></p></th><th class="cl-d5ad0ee0"><p class="cl-d5ace032"><span class="cl-d5a49774">new_items_tim</span></p></th><th class="cl-d5ad0ee0"><p class="cl-d5ace032"><span class="cl-d5a49774">confidence</span></p></th><th class="cl-d5ad0ee0"><p class="cl-d5ace032"><span class="cl-d5a49774">hit</span></p></th><th class="cl-d5ad0ee0"><p class="cl-d5ace032"><span class="cl-d5a49774">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-d5ad0eea"><p class="cl-d5ace01e"><span class="cl-d5a49774">50.00</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">250.00</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">extreme weak</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">0.05</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d5ad0eea"><p class="cl-d5ace01e"><span class="cl-d5a49774">50.00</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">250.00</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">very weak</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">0.10</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d5ad0eea"><p class="cl-d5ace01e"><span class="cl-d5a49774">50.00</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">400.00</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">fairly weak</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">0.15</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">0.90</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d5ad0eea"><p class="cl-d5ace01e"><span class="cl-d5a49774">150.00</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">50.00</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">fairly strong</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">0.30</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">0.95</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d5ad0eea"><p class="cl-d5ace01e"><span class="cl-d5a49774">500.00</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">25.00</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">very strong</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">0.80</span></p></td><td class="cl-d5ad0eeb"><p class="cl-d5ace032"><span class="cl-d5a49774">0.97</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d5ad0ef4"><p class="cl-d5ace01e"><span class="cl-d5a49774">200.00</span></p></td><td class="cl-d5ad0ef5"><p class="cl-d5ace032"><span class="cl-d5a49774">25.00</span></p></td><td class="cl-d5ad0ef5"><p class="cl-d5ace032"><span class="cl-d5a49774">extreme strong</span></p></td><td class="cl-d5ad0ef5"><p class="cl-d5ace032"><span class="cl-d5a49774">1.00</span></p></td><td class="cl-d5ad0ef5"><p class="cl-d5ace032"><span class="cl-d5a49774">1.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-d6137d7e{table-layout:auto;}.cl-d5f91376{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d5f9138a{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d602a080{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d602a09e{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d602f09e{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d602f0b2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d602f0b3{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d602f0bc{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d602f0c6{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d602f0c7{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-d6137d7e'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-d602f09e"><p class="cl-d602a080"><span class="cl-d5f91376">Dataset + Hit and False Alarm rate for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-d602f0b2"><p class="cl-d602a09e"><span class="cl-d5f9138a">old_items_john</span></p></th><th class="cl-d602f0b2"><p class="cl-d602a09e"><span class="cl-d5f9138a">new_items_john</span></p></th><th class="cl-d602f0b2"><p class="cl-d602a09e"><span class="cl-d5f9138a">confidence</span></p></th><th class="cl-d602f0b2"><p class="cl-d602a09e"><span class="cl-d5f9138a">hit</span></p></th><th class="cl-d602f0b2"><p class="cl-d602a09e"><span class="cl-d5f9138a">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-d602f0b3"><p class="cl-d602a080"><span class="cl-d5f9138a">50.00</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">100.00</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">extreme weak</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">0.05</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">0.10</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d602f0b3"><p class="cl-d602a080"><span class="cl-d5f9138a">75.00</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">400.00</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">very weak</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">0.12</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d602f0b3"><p class="cl-d602a080"><span class="cl-d5f9138a">75.00</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">300.00</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">fairly weak</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">0.20</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">0.80</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d602f0b3"><p class="cl-d602a080"><span class="cl-d5f9138a">500.00</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">50.00</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">fairly strong</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">0.70</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">0.85</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d602f0b3"><p class="cl-d602a080"><span class="cl-d5f9138a">200.00</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">75.00</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">very strong</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">0.90</span></p></td><td class="cl-d602f0bc"><p class="cl-d602a09e"><span class="cl-d5f9138a">0.92</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d602f0c6"><p class="cl-d602a080"><span class="cl-d5f9138a">100.00</span></p></td><td class="cl-d602f0c7"><p class="cl-d602a09e"><span class="cl-d5f9138a">75.00</span></p></td><td class="cl-d602f0c7"><p class="cl-d602a09e"><span class="cl-d5f9138a">extreme strong</span></p></td><td class="cl-d602f0c7"><p class="cl-d602a09e"><span class="cl-d5f9138a">1.00</span></p></td><td class="cl-d602f0c7"><p class="cl-d602a09e"><span class="cl-d5f9138a">1.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-d7383d0c{table-layout:auto;}.cl-d7212644{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d721264e{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d72b211c{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d72b2130{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d72b4bb0{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d72b4bba{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d72b4bc4{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d72b4bc5{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d72b4bce{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d72b4bcf{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-d7383d0c'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-d72b4bb0"><p class="cl-d72b211c"><span class="cl-d7212644">True Positive and False Positive Percentages for each Threshold</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-d72b4bba"><p class="cl-d72b2130"><span class="cl-d721264e">Confidence</span></p></th><th class="cl-d72b4bba"><p class="cl-d72b2130"><span class="cl-d721264e">tpp</span></p></th><th class="cl-d72b4bba"><p class="cl-d72b2130"><span class="cl-d721264e">fpp</span></p></th><th class="cl-d72b4bba"><p class="cl-d72b2130"><span class="cl-d721264e">thresholds</span></p></th><th class="cl-d72b4bba"><p class="cl-d72b2130"><span class="cl-d721264e">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-d72b4bc4"><p class="cl-d72b211c"><span class="cl-d721264e">Extreme-very weak</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">95.00</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">75.00</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">0.09</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">0.07</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d72b4bc4"><p class="cl-d72b211c"><span class="cl-d721264e">Very-fairly weak</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">90.00</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">50.00</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">0.24</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">0.20</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d72b4bc4"><p class="cl-d72b211c"><span class="cl-d721264e">Weak-strong</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">85.00</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">10.00</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">0.49</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">1.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d72b4bc4"><p class="cl-d72b211c"><span class="cl-d721264e">Fairly-very strong</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">70.00</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">5.00</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">0.75</span></p></td><td class="cl-d72b4bc5"><p class="cl-d72b2130"><span class="cl-d721264e">6.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d72b4bce"><p class="cl-d72b211c"><span class="cl-d721264e">Very-extreme strong</span></p></td><td class="cl-d72b4bcf"><p class="cl-d72b2130"><span class="cl-d721264e">20.00</span></p></td><td class="cl-d72b4bcf"><p class="cl-d72b2130"><span class="cl-d721264e">2.50</span></p></td><td class="cl-d72b4bcf"><p class="cl-d72b2130"><span class="cl-d721264e">0.90</span></p></td><td class="cl-d72b4bcf"><p class="cl-d72b2130"><span class="cl-d721264e">32.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-d77edd84{table-layout:auto;}.cl-d7678076{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d7678080{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d7705e1c{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d7705e30{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d7707e74{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d7707e75{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d7707e7e{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d7707e7f{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d7707e88{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d7707e89{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-d77edd84'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-d7707e74"><p class="cl-d7705e1c"><span class="cl-d7678076">True Positive and False Positive Percentages for each Threshold for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-d7707e75"><p class="cl-d7705e30"><span class="cl-d7678080">Confidence</span></p></th><th class="cl-d7707e75"><p class="cl-d7705e30"><span class="cl-d7678080">tpp</span></p></th><th class="cl-d7707e75"><p class="cl-d7705e30"><span class="cl-d7678080">fpp</span></p></th><th class="cl-d7707e75"><p class="cl-d7705e30"><span class="cl-d7678080">thresholds</span></p></th><th class="cl-d7707e75"><p class="cl-d7705e30"><span class="cl-d7678080">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-d7707e7e"><p class="cl-d7705e1c"><span class="cl-d7678080">Extreme-very weak</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">95.00</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">90.00</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">0.21</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">0.06</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d7707e7e"><p class="cl-d7705e1c"><span class="cl-d7678080">Very-fairly weak</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">87.50</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">50.00</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">0.35</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d7707e7e"><p class="cl-d7705e1c"><span class="cl-d7678080">Weak-strong</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">80.00</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">20.00</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">0.51</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d7707e7e"><p class="cl-d7705e1c"><span class="cl-d7678080">Fairly-very strong</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">30.00</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">15.00</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">0.68</span></p></td><td class="cl-d7707e7f"><p class="cl-d7705e30"><span class="cl-d7678080">4.67</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d7707e88"><p class="cl-d7705e1c"><span class="cl-d7678080">Very-extreme strong</span></p></td><td class="cl-d7707e89"><p class="cl-d7705e30"><span class="cl-d7678080">10.00</span></p></td><td class="cl-d7707e89"><p class="cl-d7705e30"><span class="cl-d7678080">7.50</span></p></td><td class="cl-d7707e89"><p class="cl-d7705e30"><span class="cl-d7678080">0.81</span></p></td><td class="cl-d7707e89"><p class="cl-d7705e30"><span class="cl-d7678080">12.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-d9ba6276{table-layout:auto;}.cl-d9a9b7aa{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d9a9b7b4{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-d9b09976{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d9b0998a{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-d9b0c892{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d9b0c8a6{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d9b0c8b0{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d9b0c8ba{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d9b0c8bb{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-d9b0c8c4{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-d9ba6276'><thead><tr style="overflow-wrap:break-word;"><th  colspan="7"class="cl-d9b0c892"><p class="cl-d9b09976"><span class="cl-d9a9b7aa">AUC values with 95% CI via Delong or bootstrapping</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-d9b0c8a6"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">Name</span></p></th><th class="cl-d9b0c8a6"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">AUC</span></p></th><th class="cl-d9b0c8a6"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">CI_low_Delong</span></p></th><th class="cl-d9b0c8a6"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">CI_upper_Delong</span></p></th><th class="cl-d9b0c8a6"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">CI_low_boot</span></p></th><th class="cl-d9b0c8a6"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">CI_upper_boot</span></p></th><th class="cl-d9b0c8a6"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">Power</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-d9b0c8b0"><p class="cl-d9b09976"><span class="cl-d9a9b7b4">Tim</span></p></td><td class="cl-d9b0c8ba"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">0.88</span></p></td><td class="cl-d9b0c8ba"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">0.86</span></p></td><td class="cl-d9b0c8ba"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">0.89</span></p></td><td class="cl-d9b0c8ba"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">0.86</span></p></td><td class="cl-d9b0c8ba"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">0.89</span></p></td><td class="cl-d9b0c8ba"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-d9b0c8bb"><p class="cl-d9b09976"><span class="cl-d9a9b7b4">John</span></p></td><td class="cl-d9b0c8c4"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">0.76</span></p></td><td class="cl-d9b0c8c4"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">0.74</span></p></td><td class="cl-d9b0c8c4"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">0.78</span></p></td><td class="cl-d9b0c8c4"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">0.74</span></p></td><td class="cl-d9b0c8c4"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">0.78</span></p></td><td class="cl-d9b0c8c4"><p class="cl-d9b0998a"><span class="cl-d9a9b7b4">1.00</span></p></td></tr></tbody></table></div>

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
