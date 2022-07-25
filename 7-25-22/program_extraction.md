# Program Extraction + Filtering

Procedure the same as [previously](../7-19-22/program_extraction.md) except for few changes. 

 * Scaling attach minimum distance by the object size 
    * `MAX_ALLOWED_ATTACH_DISTANCE = 0.1 * magnitude(object dimensions)`
 * Wall constraints are found iterating through all walls and finding the minimum distance from the object to the wall segment. If the distance is less than `MAX_ALLOWED_ATTACH_DISTANCE` add an `attach` constraint in the direction of the wall segment. Only one count per direction is allowed. 

For walls, do not consider the overall appearance and only remove the constraint if the occurence metric is `< 0.1`

# Bed
<table>
<tr>
<th>Constraints</th>
<th>Constraints Filtered</th>
</tr>
<tr>
<td>

```
attach wall left | occurrence: 0.30387558627499384
attach wall bottom | occurrence: 0.17427795606023205
attach wall right | occurrence: 0.3228832387064922
attach wall top | occurrence: 0.3070846704517403

attach nightstand right | occurrence: 0.40258557902403497
attach nightstand left | occurrence: 0.41314639475600873
attach nightstand top | occurrence: 0.03932993445010925
attach nightstand bottom | occurrence: 0.02530954115076475
attach nightstand all | occurrence: 0.8803714493809177

attach chair bottom | occurrence: 0.08121827411167512
attach chair right | occurrence: 0.08798646362098139
attach chair top | occurrence: 0.050761421319796954
attach chair left | occurrence: 0.0727580372250423
attach chair all | occurrence: 0.2927241962774958

attach wardrobe top | occurrence: 0.09306625577812018
attach wardrobe left | occurrence: 0.012634822804314329
attach wardrobe bottom | occurrence: 0.005855161787365178
attach wardrobe right | occurrence: 0.01201848998459168
attach wardrobe all | occurrence: 0.12357473035439137

attach desk right | occurrence: 0.18562874251497005
attach desk top | occurrence: 0.12874251497005987
attach desk left | occurrence: 0.20958083832335328
attach desk bottom | occurrence: 0.011976047904191617
attach desk all | occurrence: 0.5359281437125748

attach bed bottom | occurrence: 0.05
attach bed left | occurrence: 0.25
attach bed top | occurrence: 0.15
attach bed all | occurrence: 0.45

reachable chair bottom | occurrence: 0.20473773265651438
reachable chair left | occurrence: 0.2233502538071066
reachable chair right | occurrence: 0.19120135363790186
reachable chair top | occurrence: 0.116751269035533
reachable chair all | occurrence: 0.7360406091370558

reachable bed top | occurrence: 0.25
reachable bed left | occurrence: 0.45
reachable bed bottom | occurrence: 0.05
reachable bed right | occurrence: 0.05
reachable bed all | occurrence: 0.8

align nightstand | occurrence: 0.8270211216314639
align wardrobe | occurrence: 0.01694915254237288
align desk | occurrence: 0.3263473053892216
align chair | occurrence: 0.25042301184433163
align bed | occurrence: 0.1

face chair | occurrence: 0.25042301184433163
face nightstand | occurrence: 0.012199563000728332
face wardrobe | occurrence: 0.01448382126348228
face desk | occurrence: 0.10479041916167664
face bed | occurrence: 0.2
```
  
</td>
<td>

```
attach wall left | occurrence: 0.30387558627499384
attach wall bottom | occurrence: 0.17427795606023205
attach wall right | occurrence: 0.3228832387064922
attach wall top | occurrence: 0.3070846704517403

attach nightstand right | occurrence: 0.40258557902403497
attach nightstand left | occurrence: 0.41314639475600873
attach nightstand all | occurrence: 0.8803714493809177

attach desk right | occurrence: 0.18562874251497005
attach desk top | occurrence: 0.12874251497005987
attach desk left | occurrence: 0.20958083832335328
attach desk all | occurrence: 0.5359281437125748

attach bed left | occurrence: 0.25
attach bed top | occurrence: 0.15
attach bed all | occurrence: 0.45

reachable chair bottom | occurrence: 0.20473773265651438
reachable chair left | occurrence: 0.2233502538071066
reachable chair right | occurrence: 0.19120135363790186
reachable chair all | occurrence: 0.7360406091370558

reachable bed top | occurrence: 0.25
reachable bed left | occurrence: 0.45
reachable bed all | occurrence: 0.8

align nightstand | occurrence: 0.8270211216314639
```

</td>
</tr>
</table>

## Removed Constraints 
`attach nightstand top` and `attach nightstand bottom`

![temp](diagrams/8-MasterBedroom-19148.png)

`attach chair all`

![temp]()

`attach wardrobe all`

![temp]()

`attach desk bottom`
![temp]()

# Chair
<table>
<tr>
<th>Constraints</th>
<th>Constraints Filtered</th>
</tr>
<tr>
<td>

```
attach wall right | occurrence: 0.04786324786324787
attach wall top | occurrence: 0.04786324786324787
attach wall bottom | occurrence: 0.05982905982905983
attach wall left | occurrence: 0.05641025641025641

attach desk top | occurrence: 0.5253456221198156
attach desk left | occurrence: 0.03686635944700461
attach desk bottom | occurrence: 0.07373271889400922
attach desk right | occurrence: 0.013824884792626729
attach desk all | occurrence: 0.6497695852534562

attach bed left | occurrence: 0.05752961082910321
attach bed right | occurrence: 0.06429780033840947
attach bed top | occurrence: 0.05922165820642978
attach bed bottom | occurrence: 0.00676818950930626
attach bed all | occurrence: 0.18781725888324874

attach wardrobe right | occurrence: 0.006787330316742082
attach wardrobe top | occurrence: 0.038461538461538464
attach wardrobe bottom | occurrence: 0.00904977375565611
attach wardrobe left | occurrence: 0.0022624434389140274
attach wardrobe all | occurrence: 0.05656108597285068

attach nightstand right | occurrence: 0.010769230769230769
attach nightstand left | occurrence: 0.003076923076923077
attach nightstand bottom | occurrence: 0.004615384615384616
attach nightstand top | occurrence: 0.006153846153846154
attach nightstand all | occurrence: 0.024615384615384615

attach chair left | occurrence: 0.016129032258064516
attach chair top | occurrence: 0.016129032258064516
attach chair all | occurrence: 0.03225806451612903

reachable bed right | occurrence: 0.25888324873096447
reachable bed bottom | occurrence: 0.011844331641285956
reachable bed top | occurrence: 0.23181049069373943
reachable bed left | occurrence: 0.233502538071066
reachable bed all | occurrence: 0.7360406091370558

reachable chair left | occurrence: 0.12903225806451613
reachable chair top | occurrence: 0.12903225806451613
reachable chair right | occurrence: 0.11290322580645161
reachable chair bottom | occurrence: 0.08064516129032258
reachable chair all | occurrence: 0.45161290322580644

align bed | occurrence: 0.25042301184433163
align chair | occurrence: 0.16129032258064516
align nightstand | occurrence: 0.009230769230769232
align desk | occurrence: 0.0967741935483871

face bed | occurrence: 0.25042301184433163
face desk | occurrence: 0.4976958525345622
face wardrobe | occurrence: 0.03619909502262444
face nightstand | occurrence: 0.007692307692307693
face chair | occurrence: 0.03225806451612903
```
  
</td>
<td>

```
attach desk top | occurrence: 0.5253456221198156
attach desk all | occurrence: 0.6497695852534562

reachable bed right | occurrence: 0.25888324873096447
reachable bed top | occurrence: 0.23181049069373943
reachable bed left | occurrence: 0.233502538071066
reachable bed all | occurrence: 0.7360406091370558

reachable chair left | occurrence: 0.12903225806451613
reachable chair top | occurrence: 0.12903225806451613
reachable chair right | occurrence: 0.11290322580645161
reachable chair all | occurrence: 0.45161290322580644

face desk | occurrence: 0.4976958525345622
```

</td>
</tr>
</table>

# Desk
<table>
<tr>
<th>Constraints</th>
<th>Constraints Filtered</th>
</tr>
<tr>
<td>

```
attach wall bottom | occurrence: 0.4367469879518072
attach wall right | occurrence: 0.39156626506024095
attach wall left | occurrence: 0.3644578313253012
attach wall top | occurrence: 0.35843373493975905

attach wardrobe left | occurrence: 0.042735042735042736
attach wardrobe right | occurrence: 0.05982905982905983
attach wardrobe top | occurrence: 0.05555555555555555
attach wardrobe bottom | occurrence: 0.008547008547008548
attach wardrobe all | occurrence: 0.16666666666666666

attach bed bottom | occurrence: 0.10778443113772455
attach bed top | occurrence: 0.11676646706586827
attach bed left | occurrence: 0.15269461077844312
attach bed right | occurrence: 0.1377245508982036
attach bed all | occurrence: 0.5149700598802395

attach nightstand left | occurrence: 0.021505376344086023
attach nightstand bottom | occurrence: 0.014336917562724014
attach nightstand right | occurrence: 0.03225806451612903
attach nightstand top | occurrence: 0.007168458781362007
attach nightstand all | occurrence: 0.07526881720430108

attach chair top | occurrence: 0.4976958525345622
attach chair left | occurrence: 0.03225806451612903
attach chair bottom | occurrence: 0.16129032258064516
attach chair right | occurrence: 0.041474654377880185
attach chair all | occurrence: 0.7327188940092166

attach desk right | occurrence: 0.2
attach desk top | occurrence: 0.3
attach desk left | occurrence: 0.1
attach desk all | occurrence: 0.6

reachable bed bottom | occurrence: 0.10778443113772455
reachable bed top | occurrence: 0.2245508982035928
reachable bed left | occurrence: 0.20359281437125748
reachable bed right | occurrence: 0.20359281437125748
reachable bed all | occurrence: 0.7395209580838323

reachable chair top | occurrence: 0.6497695852534562
reachable chair left | occurrence: 0.03686635944700461
reachable chair right | occurrence: 0.07373271889400922
reachable chair bottom | occurrence: 0.17972350230414746
reachable chair all | occurrence: 0.9400921658986175

align wardrobe | occurrence: 0.042735042735042736
align bed | occurrence: 0.38622754491017963
align nightstand | occurrence: 0.06093189964157706
align chair | occurrence: 0.14285714285714285

face bed | occurrence: 0.18862275449101795
face chair | occurrence: 0.7142857142857143
face nightstand | occurrence: 0.010752688172043012
face desk | occurrence: 0.4
face wardrobe | occurrence: 0.03418803418803419
```
  
</td>
<td>

```
attach wall bottom | occurrence: 0.4367469879518072
attach wall right | occurrence: 0.39156626506024095
attach wall left | occurrence: 0.3644578313253012
attach wall top | occurrence: 0.35843373493975905

attach bed bottom | occurrence: 0.10778443113772455
attach bed top | occurrence: 0.11676646706586827
attach bed left | occurrence: 0.15269461077844312
attach bed right | occurrence: 0.1377245508982036
attach bed all | occurrence: 0.5149700598802395

attach chair top | occurrence: 0.4976958525345622
attach chair bottom | occurrence: 0.16129032258064516
attach chair all | occurrence: 0.7327188940092166

attach desk right | occurrence: 0.2
attach desk top | occurrence: 0.3
attach desk all | occurrence: 0.6

reachable bed top | occurrence: 0.2245508982035928
reachable bed left | occurrence: 0.20359281437125748
reachable bed right | occurrence: 0.20359281437125748
reachable bed all | occurrence: 0.7395209580838323

reachable chair top | occurrence: 0.6497695852534562
reachable chair all | occurrence: 0.9400921658986175

face chair | occurrence: 0.7142857142857143
face desk | occurrence: 0.4
```

</td>
</tr>
</table>

# Nightstand 
<table>
<tr>
<th>Constraints</th>
<th>Constraints Filtered</th>
</tr>
<tr>
<td>

```
attach wall left | occurrence: 0.218619056294407
attach wall top | occurrence: 0.23100746948442338
attach wall bottom | occurrence: 0.13353980688650027
attach wall right | occurrence: 0.23920568409546367

attach bed bottom | occurrence: 0.06755280407865986
attach bed right | occurrence: 0.29916241806263655
attach bed left | occurrence: 0.29806991988346687
attach bed top | occurrence: 0.0049162418062636565
attach bed all | occurrence: 0.669701383831027

attach wardrobe top | occurrence: 0.07155756207674943
attach wardrobe right | occurrence: 0.023250564334085778
attach wardrobe left | occurrence: 0.020993227990970656
attach wardrobe bottom | occurrence: 0.002708803611738149
attach wardrobe all | occurrence: 0.11851015801354402

attach desk bottom | occurrence: 0.014336917562724014
attach desk right | occurrence: 0.017921146953405017
attach desk top | occurrence: 0.010752688172043012
attach desk left | occurrence: 0.025089605734767026
attach desk all | occurrence: 0.06810035842293907

attach chair left | occurrence: 0.009230769230769232
attach chair right | occurrence: 0.004615384615384616
attach chair top | occurrence: 0.004615384615384616
attach chair bottom | occurrence: 0.003076923076923077
attach chair all | occurrence: 0.021538461538461538

attach nightstand right | occurrence: 0.0006613756613756613
attach nightstand left | occurrence: 0.0008818342151675485
attach nightstand top | occurrence: 0.0002204585537918871
attach nightstand all | occurrence: 0.001763668430335097

reachable bed bottom | occurrence: 0.06973780043699927
reachable bed right | occurrence: 0.45721048798252
reachable bed left | occurrence: 0.46285506190823017
reachable bed top | occurrence: 0.0065549890750182084
reachable bed all | occurrence: 0.9963583394027676

reachable chair bottom | occurrence: 0.024615384615384615
reachable chair right | occurrence: 0.026153846153846153
reachable chair left | occurrence: 0.04153846153846154
reachable chair top | occurrence: 0.02
reachable chair all | occurrence: 0.1123076923076923

align bed | occurrence: 0.9388201019664967
align wardrobe | occurrence: 0.005191873589164785
align chair | occurrence: 0.036923076923076927
align desk | occurrence: 0.053763440860215055
align nightstand | occurrence: 0.0013227513227513227

face bed | occurrence: 0.014020393299344501
face chair | occurrence: 0.03538461538461538
face desk | occurrence: 0.010752688172043012
face wardrobe | occurrence: 0.0013544018058690745
```
  
</td>
<td>

```
attach wall left | occurrence: 0.218619056294407
attach wall top | occurrence: 0.23100746948442338
attach wall bottom | occurrence: 0.13353980688650027
attach wall right | occurrence: 0.23920568409546367

attach bed right | occurrence: 0.29916241806263655
attach bed left | occurrence: 0.29806991988346687
attach bed all | occurrence: 0.669701383831027

reachable bed right | occurrence: 0.45721048798252
reachable bed left | occurrence: 0.46285506190823017
reachable bed all | occurrence: 0.9963583394027676

align bed | occurrence: 0.9388201019664967
```

</td>
</tr>
</table>

# Wardrobe 
<table>
<tr>
<th>Constraints</th>
<th>Constraints Filtered</th>
</tr>
<tr>
<td>

```
attach wall left | occurrence: 0.376078914919852
attach wall top | occurrence: 0.5225030826140568
attach wall right | occurrence: 0.4118372379778052
attach wall bottom | occurrence: 0.3649815043156597

attach nightstand left | occurrence: 0.07945823927765237
attach nightstand right | occurrence: 0.07765237020316026
attach nightstand top | occurrence: 0.013544018058690745
attach nightstand bottom | occurrence: 0.005417607223476298
attach nightstand all | occurrence: 0.17607223476297967

attach wardrobe left | occurrence: 0.33287671232876714
attach wardrobe right | occurrence: 0.33287671232876714
attach wardrobe top | occurrence: 0.038356164383561646
attach wardrobe bottom | occurrence: 0.021917808219178082
attach wardrobe all | occurrence: 0.726027397260274

attach desk right | occurrence: 0.05982905982905983
attach desk left | occurrence: 0.04700854700854701
attach desk top | occurrence: 0.05128205128205128
attach desk bottom | occurrence: 0.017094017094017096
attach desk all | occurrence: 0.1752136752136752

attach bed bottom | occurrence: 0.005855161787365178
attach bed left | occurrence: 0.04129429892141757
attach bed right | occurrence: 0.04375963020030817
attach bed top | occurrence: 0.01325115562403698
attach bed all | occurrence: 0.10416024653312789

attach chair left | occurrence: 0.011312217194570135
attach chair right | occurrence: 0.02262443438914027
attach chair top | occurrence: 0.033936651583710405
attach chair bottom | occurrence: 0.013574660633484163
attach chair all | occurrence: 0.08144796380090498

reachable bed left | occurrence: 0.23020030816640985
reachable bed right | occurrence: 0.22742681047765795
reachable bed bottom | occurrence: 0.012326656394453005
reachable bed top | occurrence: 0.04930662557781202
reachable bed all | occurrence: 0.5192604006163328

reachable chair bottom | occurrence: 0.033936651583710405
reachable chair left | occurrence: 0.04524886877828054
reachable chair right | occurrence: 0.05429864253393665
reachable chair top | occurrence: 0.038461538461538464
reachable chair all | occurrence: 0.17194570135746606

align wardrobe | occurrence: 0.6684931506849315
align desk | occurrence: 0.042735042735042736
align bed | occurrence: 0.028043143297380585
align nightstand | occurrence: 0.006997742663656885
align chair | occurrence: 0.024886877828054297

face bed | occurrence: 0.048998459167950696
face chair | occurrence: 0.05429864253393665
face nightstand | occurrence: 0.0020316027088036117
face desk | occurrence: 0.03418803418803419
```
  
</td>
<td>

```
attach wall left | occurrence: 0.376078914919852
attach wall top | occurrence: 0.5225030826140568
attach wall right | occurrence: 0.4118372379778052
attach wall bottom | occurrence: 0.3649815043156597

attach wardrobe left | occurrence: 0.33287671232876714
attach wardrobe right | occurrence: 0.33287671232876714
attach wardrobe all | occurrence: 0.726027397260274

reachable bed left | occurrence: 0.23020030816640985
reachable bed right | occurrence: 0.22742681047765795
reachable bed all | occurrence: 0.5192604006163328

align wardrobe | occurrence: 0.6684931506849315

```

</td>
</tr>
</table>