# Program Extraction Preliminary Results
Methodology (no subsampling) 
 * Mark all objects with tag `holds_humans`
 * Find the minimum distance between the object in question and and all other objects in the room (walls included)
 * Add `attach` constraint if the minimum distance is less than the maximum allowed attach distance (5 cm in this case) 
 * Add `reachable_by_arm` constraint if the reference object holds humans and the minimum distance is less than the maximum allowed reach distance (0.6 m)
 * If a location constraint is applied, see if either `align` or `face` is applicable to both objects by looking at their rotations. If so add those constraints. 

The occurence metric for each constraint is the total number of times the constraint appears / the total number of times that particular object co appears. For location constraints `all` is the the total number of times the location constraint appears for any direction /  the total number of times that particular object co appears. 

## Bed 
Constraints 
```
attach wall left | occurence: 0.13901234567901236
attach wall top | occurence: 0.174320987654321
attach wall right | occurence: 0.194320987654321
attach wall bottom | occurence: 0.06320987654320988
attach wall all | occurence: 0.5708641975308641

attach nightstand top | occurence: 0.30428441203281675
attach nightstand right | occurence: 0.16226071103008205
attach nightstand left | occurence: 0.1626253418413856
attach nightstand bottom | occurence: 0.011850501367365542
attach nightstand all | occurence: 0.64102096627165

attach wardrobe top | occurence: 0.0584958217270195
attach wardrobe bottom | occurence: 0.003714020427112349
attach wardrobe right | occurence: 0.0030950170225936243
attach wardrobe left | occurence: 0.002785515320334262
attach wardrobe all | occurence: 0.06809037449705974

attach desk top | occurence: 0.38023952095808383
attach desk right | occurence: 0.03592814371257485
attach desk left | occurence: 0.02694610778443114
attach desk bottom | occurence: 0.014970059880239521
attach desk all | occurence: 0.45808383233532934

attach chair right | occurence: 0.04607508532423208
attach chair bottom | occurence: 0.06143344709897611
attach chair left | occurence: 0.04607508532423208
attach chair top | occurence: 0.011945392491467578
attach chair all | occurence: 0.16552901023890784

attach bed left | occurence: 0.25
attach bed top | occurence: 0.15
attach bed all | occurence: 0.4

reachable chair bottom | occurence: 0.1962457337883959
reachable chair right | occurence: 0.19283276450511946
reachable chair left | occurence: 0.21843003412969283
reachable chair top | occurence: 0.09556313993174062
reachable chair all | occurence: 0.7030716723549488

reachable bed top | occurence: 0.25
reachable bed left | occurence: 0.5
reachable bed right | occurence: 0.05
reachable bed all | occurence: 0.8

align wall | occurence: 0.5708641975308641
align nightstand | occurence: 0.5965360072926162
align wardrobe | occurence: 0.015165583410708759
align desk | occurence: 0.2964071856287425
align chair | occurence: 0.24232081911262798
align bed | occurence: 0.1

face chair | occurence: 0.2354948805460751
face nightstand | occurence: 0.009480401093892433
face wardrobe | occurence: 0.010523057876818322
face desk | occurence: 0.0748502994011976
face bed | occurence: 0.2
```

## Chair 
```
attach wall right | occurence: 0.02586206896551724
attach wall bottom | occurence: 0.022413793103448276
attach wall left | occurence: 0.03103448275862069
attach wall top | occurence: 0.027586206896551724
attach wall all | occurence: 0.10689655172413794

attach bed right | occurence: 0.04948805460750853
attach bed top | occurence: 0.059726962457337884
attach bed left | occurence: 0.03924914675767918
attach bed bottom | occurence: 0.008532423208191127
attach bed all | occurence: 0.15699658703071673

attach desk top | occurence: 0.49528301886792453
attach desk left | occurence: 0.06132075471698113
attach desk bottom | occurence: 0.07547169811320754
attach desk right | occurence: 0.0330188679245283
attach desk all | occurence: 0.6650943396226415

attach wardrobe top | occurence: 0.03218390804597701
attach wardrobe bottom | occurence: 0.006896551724137931
attach wardrobe left | occurence: 0.0022988505747126436
attach wardrobe all | occurence: 0.041379310344827586

attach nightstand top | occurence: 0.006220839813374806
attach nightstand bottom | occurence: 0.0015552099533437014
attach nightstand right | occurence: 0.003110419906687403
attach nightstand left | occurence: 0.0015552099533437014
attach nightstand all | occurence: 0.012441679626749611

attach chair left | occurence: 0.016129032258064516
attach chair top | occurence: 0.016129032258064516
attach chair all | occurence: 0.03225806451612903

reachable bed right | occurence: 0.2440273037542662
reachable bed top | occurence: 0.2235494880546075
reachable bed left | occurence: 0.2235494880546075
reachable bed bottom | occurence: 0.011945392491467578
reachable bed all | occurence: 0.7030716723549488

reachable chair left | occurence: 0.12903225806451613
reachable chair top | occurence: 0.14516129032258066
reachable chair right | occurence: 0.11290322580645161
reachable chair bottom | occurence: 0.06451612903225806
reachable chair all | occurence: 0.45161290322580644

align wall | occurence: 0.10689655172413794
align bed | occurence: 0.24232081911262798
align chair | occurence: 0.16129032258064516
align nightstand | occurence: 0.004665629860031105
align desk | occurence: 0.09433962264150944

face bed | occurence: 0.23720136518771331
face desk | occurence: 0.5047169811320755
face nightstand | occurence: 0.004665629860031105
face wardrobe | occurence: 0.020689655172413793
face chair | occurence: 0.03225806451612903
```

## Wardrobe 
```
attach wall right | occurence: 0.1541318477251625
attach wall left | occurence: 0.15134633240482823
attach wall top | occurence: 0.24233983286908078
attach wall bottom | occurence: 0.17517796347879913
attach wall all | occurence: 0.7229959764778706

attach nightstand top | occurence: 0.10358927760109041
attach nightstand bottom | occurence: 0.0015901862789641072
attach nightstand right | occurence: 0.0029532030895047705
attach nightstand left | occurence: 0.0029532030895047705
attach nightstand all | occurence: 0.11108587005906406

attach wardrobe right | occurence: 0.2932551319648094
attach wardrobe left | occurence: 0.30058651026392963
attach wardrobe top | occurence: 0.05278592375366569
attach wardrobe bottom | occurence: 0.02785923753665689
attach wardrobe all | occurence: 0.6744868035190615

attach desk right | occurence: 0.03879310344827586
attach desk top | occurence: 0.08620689655172414
attach desk bottom | occurence: 0.008620689655172414
attach desk left | occurence: 0.03017241379310345
attach desk all | occurence: 0.16379310344827586

attach bed right | occurence: 0.028783658310120707
attach bed left | occurence: 0.02692664809656453
attach bed top | occurence: 0.009904054472299598
attach bed bottom | occurence: 0.0015475085112968121
attach bed all | occurence: 0.06716186939028165

attach chair bottom | occurence: 0.011494252873563218
attach chair top | occurence: 0.009195402298850575
attach chair left | occurence: 0.013793103448275862
attach chair right | occurence: 0.004597701149425287
attach chair all | occurence: 0.03908045977011494

reachable bed left | occurence: 0.21231816774992263
reachable bed right | occurence: 0.2135561745589601
reachable bed top | occurence: 0.0485917672547199
reachable bed bottom | occurence: 0.007428040854224698
reachable bed all | occurence: 0.4818941504178273

reachable chair right | occurence: 0.04597701149425287
reachable chair left | occurence: 0.04827586206896552
reachable chair top | occurence: 0.027586206896551724
reachable chair bottom | occurence: 0.034482758620689655
reachable chair all | occurence: 0.15632183908045977

align wall | occurence: 0.7229959764778706
align wardrobe | occurence: 0.6187683284457478
align desk | occurence: 0.04310344827586207
align bed | occurence: 0.027236149798823894
align chair | occurence: 0.022988505747126436
align nightstand | occurence: 0.004770558836892321

face bed | occurence: 0.04549675023212628
face chair | occurence: 0.05057471264367816
face nightstand | occurence: 0.0013630168105406633
face desk | occurence: 0.034482758620689655
```

## Nightstand 
```
attach wall left | occurence: 0.1601605253557096
attach wall top | occurence: 0.19153593578985773
attach wall bottom | occurence: 0.08664720904779277
attach wall right | occurence: 0.21141919007661436
attach wall all | occurence: 0.6497628602699744

attach bed left | occurence: 0.295897903372835
attach bed right | occurence: 0.2999088422971741
attach bed bottom | occurence: 0.04047402005469462
attach bed top | occurence: 0.004557885141294439
attach bed all | occurence: 0.6408386508659982

attach wardrobe top | occurence: 0.1040436165379373
attach wardrobe left | occurence: 0.002044525215810995
attach wardrobe right | occurence: 0.0034075420263516582
attach wardrobe bottom | occurence: 0.001817355747387551
attach wardrobe all | occurence: 0.1113130395274875

attach desk bottom | occurence: 0.014388489208633094
attach desk right | occurence: 0.014388489208633094
attach desk top | occurence: 0.01079136690647482
attach desk left | occurence: 0.025179856115107913
attach desk all | occurence: 0.06474820143884892

attach chair left | occurence: 0.006220839813374806
attach chair top | occurence: 0.004665629860031105
attach chair bottom | occurence: 0.003110419906687403
attach chair right | occurence: 0.0015552099533437014
attach chair all | occurence: 0.015552099533437015

attach nightstand right | occurence: 0.0004420866489832007
attach nightstand left | occurence: 0.000663129973474801
attach nightstand top | occurence: 0.00022104332449160034
attach nightstand all | occurence: 0.001326259946949602

reachable bed left | occurence: 0.47675478577939834
reachable bed right | occurence: 0.46873290793072014
reachable bed bottom | occurence: 0.04266180492251595
reachable bed top | occurence: 0.00601640838650866
reachable bed all | occurence: 0.9941659070191431

reachable chair bottom | occurence: 0.02177293934681182
reachable chair right | occurence: 0.024883359253499222
reachable chair left | occurence: 0.04354587869362364
reachable chair top | occurence: 0.02021772939346812
reachable chair all | occurence: 0.1104199066874028

align wall | occurence: 0.6497628602699744
align bed | occurence: 0.9371011850501367
align chair | occurence: 0.03732503888024884
align desk | occurence: 0.050359712230215826
align wardrobe | occurence: 0.004770558836892321
align nightstand | occurence: 0.0008841732979664014

face bed | occurence: 0.014038286235186874
face chair | occurence: 0.03421461897356143
face desk | occurence: 0.01079136690647482
face wardrobe | occurence: 0.0013630168105406633
```

## Desk
```
attach wardrobe left | occurence: 0.03017241379310345
attach wardrobe top | occurence: 0.11637931034482758
attach wardrobe right | occurence: 0.017241379310344827
attach wardrobe all | occurence: 0.16379310344827586

attach wall right | occurence: 0.18674698795180722
attach wall bottom | occurence: 0.1897590361445783
attach wall top | occurence: 0.1355421686746988
attach wall left | occurence: 0.1897590361445783
attach wall all | occurence: 0.7018072289156626

attach nightstand right | occurence: 0.03237410071942446
attach nightstand top | occurence: 0.017985611510791366
attach nightstand left | occurence: 0.014388489208633094
attach nightstand all | occurence: 0.06474820143884892

attach bed left | occurence: 0.16766467065868262
attach bed top | occurence: 0.08682634730538923
attach bed right | occurence: 0.16167664670658682
attach bed bottom | occurence: 0.02694610778443114
attach bed all | occurence: 0.4431137724550898

attach chair top | occurence: 0.41037735849056606
attach chair left | occurence: 0.11320754716981132
attach chair right | occurence: 0.09905660377358491
attach chair bottom | occurence: 0.03773584905660377
attach chair all | occurence: 0.660377358490566

attach desk right | occurence: 0.2
attach desk top | occurence: 0.3
attach desk left | occurence: 0.1
attach desk all | occurence: 0.6

reachable bed left | occurence: 0.23952095808383234
reachable bed top | occurence: 0.20958083832335328
reachable bed right | occurence: 0.25449101796407186
reachable bed bottom | occurence: 0.02694610778443114
reachable bed all | occurence: 0.7305389221556886

reachable chair top | occurence: 0.660377358490566
reachable chair left | occurence: 0.11320754716981132
reachable chair right | occurence: 0.10377358490566038
reachable chair bottom | occurence: 0.06132075471698113
reachable chair all | occurence: 0.9386792452830188

align wardrobe | occurence: 0.04310344827586207
align wall | occurence: 0.7018072289156626
align bed | occurence: 0.38622754491017963
align nightstand | occurence: 0.050359712230215826
align chair | occurence: 0.14622641509433962

face bed | occurence: 0.18562874251497005
face chair | occurence: 0.7075471698113207
face nightstand | occurence: 0.01079136690647482
face desk | occurence: 0.4
face wardrobe | occurence: 0.034482758620689655
```

# Filtering constraints 
Constraints are removed with the following procedure 

 * If a constraint for that object type overall appears less than 40 percent of the time, remove it
 * Remaining Location Constraints: filtering spurious directions 
    * divide the occurence in each direction by the overall occurrence value to get a normalized occurence value. Remove directions with a normalized occurence metric less than 0.2. 

## Bed
```
attach wall left | occurrence: 0.13901234567901236
attach wall top | occurrence: 0.174320987654321
attach wall right | occurrence: 0.194320987654321
attach wall all | occurrence: 0.5708641975308641

attach nightstand top | occurrence: 0.30428441203281675
attach nightstand right | occurrence: 0.16226071103008205
attach nightstand left | occurrence: 0.1626253418413856
attach nightstand all | occurrence: 0.64102096627165

attach desk top | occurrence: 0.38023952095808383
attach desk all | occurrence: 0.45808383233532934
attach bed left | occurrence: 0.25
attach bed top | occurrence: 0.15
attach bed all | occurrence: 0.4

reachable chair bottom | occurrence: 0.1962457337883959
reachable chair right | occurrence: 0.19283276450511946
reachable chair left | occurrence: 0.21843003412969283
reachable chair all | occurrence: 0.7030716723549488

reachable bed top | occurrence: 0.25
reachable bed left | occurrence: 0.5
reachable bed all | occurrence: 0.8

align wall | occurrence: 0.5708641975308641
align nightstand | occurrence: 0.5965360072926162
```

## Chair
```
attach desk top | occurrence: 0.49528301886792453
attach desk all | occurrence: 0.6650943396226415

reachable bed right | occurrence: 0.2440273037542662
reachable bed top | occurrence: 0.2235494880546075
reachable bed left | occurrence: 0.2235494880546075
reachable bed all | occurrence: 0.7030716723549488

reachable chair left | occurrence: 0.12903225806451613
reachable chair top | occurrence: 0.14516129032258066
reachable chair right | occurrence: 0.11290322580645161
reachable chair all | occurrence: 0.45161290322580644

face desk | occurrence: 0.5047169811320755
```

## Wardrobe 
```
attach wall right | occurrence: 0.1541318477251625
attach wall left | occurrence: 0.15134633240482823
attach wall top | occurrence: 0.24233983286908078
attach wall bottom | occurrence: 0.17517796347879913
attach wall all | occurrence: 0.7229959764778706

attach wardrobe right | occurrence: 0.2932551319648094
attach wardrobe left | occurrence: 0.30058651026392963
attach wardrobe all | occurrence: 0.6744868035190615

reachable bed left | occurrence: 0.21231816774992263
reachable bed right | occurrence: 0.2135561745589601
reachable bed all | occurrence: 0.4818941504178273

align wall | occurrence: 0.7229959764778706
align wardrobe | occurrence: 0.6187683284457478
```

## Nightstand 
```
attach wall left | occurrence: 0.1601605253557096
attach wall top | occurrence: 0.19153593578985773
attach wall right | occurrence: 0.21141919007661436
attach wall all | occurrence: 0.6497628602699744

attach bed left | occurrence: 0.295897903372835
attach bed right | occurrence: 0.2999088422971741
attach bed all | occurrence: 0.6408386508659982

reachable bed left | occurrence: 0.47675478577939834
reachable bed right | occurrence: 0.46873290793072014
reachable bed all | occurrence: 0.9941659070191431

align wall | occurrence: 0.6497628602699744
align bed | occurrence: 0.9371011850501367
```

## Desk
```
attach wall right | occurrence: 0.18674698795180722
attach wall bottom | occurrence: 0.1897590361445783
attach wall left | occurrence: 0.1897590361445783
attach wall all | occurrence: 0.7018072289156626

attach bed left | occurrence: 0.16766467065868262
attach bed right | occurrence: 0.16167664670658682
attach bed all | occurrence: 0.4431137724550898

attach chair top | occurrence: 0.41037735849056606
attach chair all | occurrence: 0.660377358490566

attach desk right | occurrence: 0.2
attach desk top | occurrence: 0.3
attach desk all | occurrence: 0.6

reachable bed left | occurrence: 0.23952095808383234
reachable bed top | occurrence: 0.20958083832335328
reachable bed right | occurrence: 0.25449101796407186

reachable bed all | occurrence: 0.7305389221556886
reachable chair top | occurrence: 0.660377358490566
reachable chair all | occurrence: 0.9386792452830188

align wall | occurrence: 0.7018072289156626
face chair | occurrence: 0.7075471698113207
face desk | occurrence: 0.4
```