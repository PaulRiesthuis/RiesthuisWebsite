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

<div class="tabwid"><style>.cl-3008ae32{table-layout:auto;}.cl-3001cd6a{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3001cd74{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-30049b62{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-30049b6c{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3004ad28{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3004ad32{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3004ad33{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3004ad34{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3004ad3c{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3004ad3d{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-3008ae32'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-3004ad28"><p class="cl-30049b62"><span class="cl-3001cd6a">Dataset for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-3004ad32"><p class="cl-30049b6c"><span class="cl-3001cd74">old_items_tim</span></p></th><th class="cl-3004ad32"><p class="cl-30049b6c"><span class="cl-3001cd74">new_items_tim</span></p></th><th class="cl-3004ad32"><p class="cl-30049b6c"><span class="cl-3001cd74">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-3004ad33"><p class="cl-30049b62"><span class="cl-3001cd74">50.00</span></p></td><td class="cl-3004ad34"><p class="cl-30049b6c"><span class="cl-3001cd74">250.00</span></p></td><td class="cl-3004ad34"><p class="cl-30049b6c"><span class="cl-3001cd74">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3004ad33"><p class="cl-30049b62"><span class="cl-3001cd74">50.00</span></p></td><td class="cl-3004ad34"><p class="cl-30049b6c"><span class="cl-3001cd74">250.00</span></p></td><td class="cl-3004ad34"><p class="cl-30049b6c"><span class="cl-3001cd74">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3004ad33"><p class="cl-30049b62"><span class="cl-3001cd74">50.00</span></p></td><td class="cl-3004ad34"><p class="cl-30049b6c"><span class="cl-3001cd74">400.00</span></p></td><td class="cl-3004ad34"><p class="cl-30049b6c"><span class="cl-3001cd74">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3004ad33"><p class="cl-30049b62"><span class="cl-3001cd74">150.00</span></p></td><td class="cl-3004ad34"><p class="cl-30049b6c"><span class="cl-3001cd74">50.00</span></p></td><td class="cl-3004ad34"><p class="cl-30049b6c"><span class="cl-3001cd74">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3004ad33"><p class="cl-30049b62"><span class="cl-3001cd74">500.00</span></p></td><td class="cl-3004ad34"><p class="cl-30049b6c"><span class="cl-3001cd74">25.00</span></p></td><td class="cl-3004ad34"><p class="cl-30049b6c"><span class="cl-3001cd74">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3004ad3c"><p class="cl-30049b62"><span class="cl-3001cd74">200.00</span></p></td><td class="cl-3004ad3d"><p class="cl-30049b6c"><span class="cl-3001cd74">25.00</span></p></td><td class="cl-3004ad3d"><p class="cl-30049b6c"><span class="cl-3001cd74">extreme strong</span></p></td></tr></tbody></table></div>

- Dataset John

``` r
old_items_john <- c(50,75,75,500,200,100)
new_items_john <- c(100,400,300,50,75,75) # changed very strong to 75 instead of 25 to have equal number items between tim and john
df_john <- data.frame(old_items_john, new_items_john, confidence)
nice_table(df_john,
           title = "Dataset for John")
```

<div class="tabwid"><style>.cl-301c46a4{table-layout:auto;}.cl-301685f2{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-301685fc{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3018ea4a{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3018ea54{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3018f652{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3018f653{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3018f65c{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3018f65d{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3018f65e{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3018f65f{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-301c46a4'><thead><tr style="overflow-wrap:break-word;"><th  colspan="3"class="cl-3018f652"><p class="cl-3018ea4a"><span class="cl-301685f2">Dataset for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-3018f653"><p class="cl-3018ea54"><span class="cl-301685fc">old_items_john</span></p></th><th class="cl-3018f653"><p class="cl-3018ea54"><span class="cl-301685fc">new_items_john</span></p></th><th class="cl-3018f653"><p class="cl-3018ea54"><span class="cl-301685fc">confidence</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-3018f65c"><p class="cl-3018ea4a"><span class="cl-301685fc">50.00</span></p></td><td class="cl-3018f65d"><p class="cl-3018ea54"><span class="cl-301685fc">100.00</span></p></td><td class="cl-3018f65d"><p class="cl-3018ea54"><span class="cl-301685fc">extreme weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3018f65c"><p class="cl-3018ea4a"><span class="cl-301685fc">75.00</span></p></td><td class="cl-3018f65d"><p class="cl-3018ea54"><span class="cl-301685fc">400.00</span></p></td><td class="cl-3018f65d"><p class="cl-3018ea54"><span class="cl-301685fc">very weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3018f65c"><p class="cl-3018ea4a"><span class="cl-301685fc">75.00</span></p></td><td class="cl-3018f65d"><p class="cl-3018ea54"><span class="cl-301685fc">300.00</span></p></td><td class="cl-3018f65d"><p class="cl-3018ea54"><span class="cl-301685fc">fairly weak</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3018f65c"><p class="cl-3018ea4a"><span class="cl-301685fc">500.00</span></p></td><td class="cl-3018f65d"><p class="cl-3018ea54"><span class="cl-301685fc">50.00</span></p></td><td class="cl-3018f65d"><p class="cl-3018ea54"><span class="cl-301685fc">fairly strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3018f65c"><p class="cl-3018ea4a"><span class="cl-301685fc">200.00</span></p></td><td class="cl-3018f65d"><p class="cl-3018ea54"><span class="cl-301685fc">75.00</span></p></td><td class="cl-3018f65d"><p class="cl-3018ea54"><span class="cl-301685fc">very strong</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3018f65e"><p class="cl-3018ea4a"><span class="cl-301685fc">100.00</span></p></td><td class="cl-3018f65f"><p class="cl-3018ea54"><span class="cl-301685fc">75.00</span></p></td><td class="cl-3018f65f"><p class="cl-3018ea54"><span class="cl-301685fc">extreme strong</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-302f2eb8{table-layout:auto;}.cl-302930e4{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-302930ee{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-302b95aa{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-302b95b4{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-302ba298{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-302ba2a2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-302ba2a3{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-302ba2a4{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-302ba2ac{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-302ba2ad{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-302f2eb8'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-302ba298"><p class="cl-302b95aa"><span class="cl-302930e4">Dataset + Hit and False Alarm rate for Tim</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-302ba2a2"><p class="cl-302b95b4"><span class="cl-302930ee">old_items_tim</span></p></th><th class="cl-302ba2a2"><p class="cl-302b95b4"><span class="cl-302930ee">new_items_tim</span></p></th><th class="cl-302ba2a2"><p class="cl-302b95b4"><span class="cl-302930ee">confidence</span></p></th><th class="cl-302ba2a2"><p class="cl-302b95b4"><span class="cl-302930ee">hit</span></p></th><th class="cl-302ba2a2"><p class="cl-302b95b4"><span class="cl-302930ee">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-302ba2a3"><p class="cl-302b95aa"><span class="cl-302930ee">50.00</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">250.00</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">extreme weak</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">0.05</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-302ba2a3"><p class="cl-302b95aa"><span class="cl-302930ee">50.00</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">250.00</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">very weak</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">0.10</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-302ba2a3"><p class="cl-302b95aa"><span class="cl-302930ee">50.00</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">400.00</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">fairly weak</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">0.15</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">0.90</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-302ba2a3"><p class="cl-302b95aa"><span class="cl-302930ee">150.00</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">50.00</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">fairly strong</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">0.30</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">0.95</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-302ba2a3"><p class="cl-302b95aa"><span class="cl-302930ee">500.00</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">25.00</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">very strong</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">0.80</span></p></td><td class="cl-302ba2a4"><p class="cl-302b95b4"><span class="cl-302930ee">0.97</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-302ba2ac"><p class="cl-302b95aa"><span class="cl-302930ee">200.00</span></p></td><td class="cl-302ba2ad"><p class="cl-302b95b4"><span class="cl-302930ee">25.00</span></p></td><td class="cl-302ba2ad"><p class="cl-302b95b4"><span class="cl-302930ee">extreme strong</span></p></td><td class="cl-302ba2ad"><p class="cl-302b95b4"><span class="cl-302930ee">1.00</span></p></td><td class="cl-302ba2ad"><p class="cl-302b95b4"><span class="cl-302930ee">1.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-30425f10{table-layout:auto;}.cl-303c604c{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-303c604d{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-303e92b8{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-303e92c2{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-303ef500{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-303ef501{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-303ef502{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-303ef50a{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-303ef50b{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-303ef50c{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-30425f10'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-303ef500"><p class="cl-303e92b8"><span class="cl-303c604c">Dataset + Hit and False Alarm rate for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-303ef501"><p class="cl-303e92c2"><span class="cl-303c604d">old_items_john</span></p></th><th class="cl-303ef501"><p class="cl-303e92c2"><span class="cl-303c604d">new_items_john</span></p></th><th class="cl-303ef501"><p class="cl-303e92c2"><span class="cl-303c604d">confidence</span></p></th><th class="cl-303ef501"><p class="cl-303e92c2"><span class="cl-303c604d">hit</span></p></th><th class="cl-303ef501"><p class="cl-303e92c2"><span class="cl-303c604d">far</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-303ef502"><p class="cl-303e92b8"><span class="cl-303c604d">50.00</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">100.00</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">extreme weak</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">0.05</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">0.10</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-303ef502"><p class="cl-303e92b8"><span class="cl-303c604d">75.00</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">400.00</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">very weak</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">0.12</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">0.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-303ef502"><p class="cl-303e92b8"><span class="cl-303c604d">75.00</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">300.00</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">fairly weak</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">0.20</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">0.80</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-303ef502"><p class="cl-303e92b8"><span class="cl-303c604d">500.00</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">50.00</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">fairly strong</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">0.70</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">0.85</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-303ef502"><p class="cl-303e92b8"><span class="cl-303c604d">200.00</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">75.00</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">very strong</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">0.90</span></p></td><td class="cl-303ef50a"><p class="cl-303e92c2"><span class="cl-303c604d">0.92</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-303ef50b"><p class="cl-303e92b8"><span class="cl-303c604d">100.00</span></p></td><td class="cl-303ef50c"><p class="cl-303e92c2"><span class="cl-303c604d">75.00</span></p></td><td class="cl-303ef50c"><p class="cl-303e92c2"><span class="cl-303c604d">extreme strong</span></p></td><td class="cl-303ef50c"><p class="cl-303e92c2"><span class="cl-303c604d">1.00</span></p></td><td class="cl-303ef50c"><p class="cl-303e92c2"><span class="cl-303c604d">1.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-3098682e{table-layout:auto;}.cl-3092a434{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3092a43e{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-3094eb54{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3094eb5e{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-3094f7b6{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3094f7b7{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3094f7c0{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3094f7c1{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3094f7c2{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-3094f7ca{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-3098682e'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-3094f7b6"><p class="cl-3094eb54"><span class="cl-3092a434">True Positive and False Positive Percentages for each Threshold</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-3094f7b7"><p class="cl-3094eb5e"><span class="cl-3092a43e">Confidence</span></p></th><th class="cl-3094f7b7"><p class="cl-3094eb5e"><span class="cl-3092a43e">tpp</span></p></th><th class="cl-3094f7b7"><p class="cl-3094eb5e"><span class="cl-3092a43e">fpp</span></p></th><th class="cl-3094f7b7"><p class="cl-3094eb5e"><span class="cl-3092a43e">thresholds</span></p></th><th class="cl-3094f7b7"><p class="cl-3094eb5e"><span class="cl-3092a43e">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-3094f7c0"><p class="cl-3094eb54"><span class="cl-3092a43e">Extreme-very weak</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">95.00</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">75.00</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">0.09</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">0.07</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3094f7c0"><p class="cl-3094eb54"><span class="cl-3092a43e">Very-fairly weak</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">90.00</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">50.00</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">0.24</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">0.20</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3094f7c0"><p class="cl-3094eb54"><span class="cl-3092a43e">Weak-strong</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">85.00</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">10.00</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">0.49</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">1.50</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3094f7c0"><p class="cl-3094eb54"><span class="cl-3092a43e">Fairly-very strong</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">70.00</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">5.00</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">0.75</span></p></td><td class="cl-3094f7c1"><p class="cl-3094eb5e"><span class="cl-3092a43e">6.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-3094f7c2"><p class="cl-3094eb54"><span class="cl-3092a43e">Very-extreme strong</span></p></td><td class="cl-3094f7ca"><p class="cl-3094eb5e"><span class="cl-3092a43e">20.00</span></p></td><td class="cl-3094f7ca"><p class="cl-3094eb5e"><span class="cl-3092a43e">2.50</span></p></td><td class="cl-3094f7ca"><p class="cl-3094eb5e"><span class="cl-3092a43e">0.90</span></p></td><td class="cl-3094f7ca"><p class="cl-3094eb5e"><span class="cl-3092a43e">32.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-30ab687a{table-layout:auto;}.cl-30a5a354{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-30a5a35e{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-30a7e0b0{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-30a7e0ba{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-30a7ed8a{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-30a7ed8b{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-30a7ed94{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-30a7ed95{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-30a7ed96{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-30a7ed97{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-30ab687a'><thead><tr style="overflow-wrap:break-word;"><th  colspan="5"class="cl-30a7ed8a"><p class="cl-30a7e0b0"><span class="cl-30a5a354">True Positive and False Positive Percentages for each Threshold for John</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-30a7ed8b"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">Confidence</span></p></th><th class="cl-30a7ed8b"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">tpp</span></p></th><th class="cl-30a7ed8b"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">fpp</span></p></th><th class="cl-30a7ed8b"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">thresholds</span></p></th><th class="cl-30a7ed8b"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">FN_FP</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-30a7ed94"><p class="cl-30a7e0b0"><span class="cl-30a5a35e">Extreme-very weak</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">95.00</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">90.00</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">0.21</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">0.06</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-30a7ed94"><p class="cl-30a7e0b0"><span class="cl-30a5a35e">Very-fairly weak</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">87.50</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">50.00</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">0.35</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">0.25</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-30a7ed94"><p class="cl-30a7e0b0"><span class="cl-30a5a35e">Weak-strong</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">80.00</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">20.00</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">0.51</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-30a7ed94"><p class="cl-30a7e0b0"><span class="cl-30a5a35e">Fairly-very strong</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">30.00</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">15.00</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">0.68</span></p></td><td class="cl-30a7ed95"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">4.67</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-30a7ed96"><p class="cl-30a7e0b0"><span class="cl-30a5a35e">Very-extreme strong</span></p></td><td class="cl-30a7ed97"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">10.00</span></p></td><td class="cl-30a7ed97"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">7.50</span></p></td><td class="cl-30a7ed97"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">0.81</span></p></td><td class="cl-30a7ed97"><p class="cl-30a7e0ba"><span class="cl-30a5a35e">12.00</span></p></td></tr></tbody></table></div>

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

<div class="tabwid"><style>.cl-315e07a0{table-layout:auto;}.cl-3158611a{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:italic;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-31586124{font-family:'Times New Roman';font-size:12pt;font-weight:normal;font-style:normal;text-decoration:none;color:rgba(0, 0, 0, 1.00);background-color:transparent;}.cl-315a9228{margin:0;text-align:left;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-315a9232{margin:0;text-align:center;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);padding-bottom:5pt;padding-top:5pt;padding-left:5pt;padding-right:5pt;line-height: 2;background-color:transparent;}.cl-315a9e58{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(102, 102, 102, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-315a9e59{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0.5pt solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-315a9e5a{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-315a9e62{background-color:transparent;vertical-align: middle;border-bottom: 0 solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-315a9e63{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}.cl-315a9e64{background-color:transparent;vertical-align: middle;border-bottom: 0.5pt solid rgba(0, 0, 0, 1.00);border-top: 0 solid rgba(0, 0, 0, 1.00);border-left: 0 solid rgba(0, 0, 0, 1.00);border-right: 0 solid rgba(0, 0, 0, 1.00);margin-bottom:0;margin-top:0;margin-left:0;margin-right:0;}</style><table data-quarto-disable-processing='true' class='cl-315e07a0'><thead><tr style="overflow-wrap:break-word;"><th  colspan="7"class="cl-315a9e58"><p class="cl-315a9228"><span class="cl-3158611a">AUC values with 95% CI via Delong or bootstrapping</span></p></th></tr><tr style="overflow-wrap:break-word;"><th class="cl-315a9e59"><p class="cl-315a9232"><span class="cl-31586124">Name</span></p></th><th class="cl-315a9e59"><p class="cl-315a9232"><span class="cl-31586124">AUC</span></p></th><th class="cl-315a9e59"><p class="cl-315a9232"><span class="cl-31586124">CI_low_Delong</span></p></th><th class="cl-315a9e59"><p class="cl-315a9232"><span class="cl-31586124">CI_upper_Delong</span></p></th><th class="cl-315a9e59"><p class="cl-315a9232"><span class="cl-31586124">CI_low_boot</span></p></th><th class="cl-315a9e59"><p class="cl-315a9232"><span class="cl-31586124">CI_upper_boot</span></p></th><th class="cl-315a9e59"><p class="cl-315a9232"><span class="cl-31586124">Power</span></p></th></tr></thead><tbody><tr style="overflow-wrap:break-word;"><td class="cl-315a9e5a"><p class="cl-315a9228"><span class="cl-31586124">Tim</span></p></td><td class="cl-315a9e62"><p class="cl-315a9232"><span class="cl-31586124">0.88</span></p></td><td class="cl-315a9e62"><p class="cl-315a9232"><span class="cl-31586124">0.86</span></p></td><td class="cl-315a9e62"><p class="cl-315a9232"><span class="cl-31586124">0.89</span></p></td><td class="cl-315a9e62"><p class="cl-315a9232"><span class="cl-31586124">0.86</span></p></td><td class="cl-315a9e62"><p class="cl-315a9232"><span class="cl-31586124">0.89</span></p></td><td class="cl-315a9e62"><p class="cl-315a9232"><span class="cl-31586124">1.00</span></p></td></tr><tr style="overflow-wrap:break-word;"><td class="cl-315a9e63"><p class="cl-315a9228"><span class="cl-31586124">John</span></p></td><td class="cl-315a9e64"><p class="cl-315a9232"><span class="cl-31586124">0.76</span></p></td><td class="cl-315a9e64"><p class="cl-315a9232"><span class="cl-31586124">0.74</span></p></td><td class="cl-315a9e64"><p class="cl-315a9232"><span class="cl-31586124">0.78</span></p></td><td class="cl-315a9e64"><p class="cl-315a9232"><span class="cl-31586124">0.74</span></p></td><td class="cl-315a9e64"><p class="cl-315a9232"><span class="cl-31586124">0.78</span></p></td><td class="cl-315a9e64"><p class="cl-315a9232"><span class="cl-31586124">1.00</span></p></td></tr></tbody></table></div>

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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-11-1.png" width="672" />
