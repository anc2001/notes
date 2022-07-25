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