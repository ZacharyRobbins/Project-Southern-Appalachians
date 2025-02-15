Fire\_Analysis
================
Zjrobbin
12/22/2020

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(ggplot2)
library(ggpubr)
```

    ## Warning: package 'ggpubr' was built under R version 4.0.3

``` r
library(grid)
require(gridExtra)
```

    ## Loading required package: gridExtra

    ## 
    ## Attaching package: 'gridExtra'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     combine

``` r
library(raster)
```

    ## Loading required package: sp

    ## 
    ## Attaching package: 'raster'

    ## The following object is masked from 'package:ggpubr':
    ## 
    ##     rotate

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     select

``` r
library(RColorBrewer)

rdbu<-brewer.pal(10,"RdBu")
Greens<-brewer.pal(9,"Greens")
```

``` r
ProcessLAI<-function(t,LANDIS_Inputs){
  Fire1<-raster(paste0(LANDIS_Inputs,'scrapple-fire/ignition-type-',t,'.img'))
  Fire1[Fire1<=1]<-NA
  Fire1[Fire1>1]<-1
  LAITOF<-raster(paste0(LANDIS_Inputs,'NECN/LAI-',as.character(t-1),'.img'))*Fire1
  LAIAF<-raster(paste0(LANDIS_Inputs,'NECN/LAI-',t,'.img'))*Fire1
  LAI10YAF<-raster(paste0(LANDIS_Inputs,'NECN/LAI-',t+5,'.img'))*Fire1
  LAI15YAR<-raster(paste0(LANDIS_Inputs,'NECN/LAI-',t+10,'.img'))*Fire1
  
  values0<-as.data.frame(LAITOF$layer)%>%subset(!is.na(layer))
  values1<-as.data.frame(LAIAF$layer)%>%subset(!is.na(layer))
  values2<-as.data.frame(LAI10YAF$layer)%>%subset(!is.na(layer))
  values3<-as.data.frame(LAI15YAR$layer)%>%subset(!is.na(layer))
  #values4<-as.data.frame(LAI4$layer)%>%subset(!is.na(layer))
  #values5<-as.data.frame(LAI5$layer)%>%subset(!is.na(layer))
  delta<-LAIAF-LAITOF
  Fire1<-raster(paste0(LANDIS_Inputs,'scrapple-fire/ignition-type-',t,'.img'))
  #par(bg="grey")
  #plot(delta,ylim=c(0,400),xlim=c(0,400),col=rdbu,zlim=c(-8,8),colNA="darkgrey")
  data<-data.frame(name=c(rep("   Pre-burn",length(values0$layer)),rep("  Post-burn",length(values1$layer)),rep(" 5 yrs Post-burn",length(values2$layer)),rep("10 yrs Post-burn",length(values3$layer))),
                 value=c(values0$layer,values1$layer,values2$layer,values3$layer))
  return(data)
}

ProcessLAINF<-function(t,LANDIS_Inputs){
  #plot(Fire1,ylim=c(0,400),xlim=c(0,400))
  Fire1<-raster(paste0(LANDIS_Inputs,'scrapple-fire/ignition-type-',t,'.img'))
  Fire1[Fire1!=1]<-NA
  LAITOF<-raster(paste0(LANDIS_Inputs,'NECN/LAI-',as.character(t-1),'.img'))*Fire1
  LAIAF<-raster(paste0(LANDIS_Inputs,'NECN/LAI-',t+3,'.img'))*Fire1
  LAI10YAF<-raster(paste0(LANDIS_Inputs,'NECN/LAI-',t+5,'.img'))*Fire1
  LAI15YAR<-raster(paste0(LANDIS_Inputs,'NECN/LAI-',t+10,'.img'))*Fire1
  
  values0<-as.data.frame(LAITOF$layer)%>%subset(!is.na(layer))
  values1<-as.data.frame(LAIAF$layer)%>%subset(!is.na(layer))
  values2<-as.data.frame(LAI10YAF$layer)%>%subset(!is.na(layer))
  values3<-as.data.frame(LAI15YAR$layer)%>%subset(!is.na(layer))
  #values4<-as.data.frame(LAI4$layer)%>%subset(!is.na(layer))
  #values5<-as.data.frame(LAI5$layer)%>%subset(!is.na(layer))
  delta<-LAIAF-LAITOF
  #par(bg="grey")
  #plot(delta,ylim=c(0,400),xlim=c(0,400),col=rdbu,zlim=c(-8,8),colNA="darkgrey")
   data<-data.frame(name=c(rep("   Pre-burn",length(values0$layer)),rep("  Post-burn",length(values1$layer)),rep(" 5 yrs Post-burn",length(values2$layer)),rep("10 yrs Post-burn",length(values3$layer))),
                 value=c(values0$layer,values1$layer,values2$layer,values3$layer))
  return(data)
}


LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D1_R2/"
LANDIS_Inputs2<-"E:/DM_runs_4_4/GA_Model_D2_R2/"
LANDIS_Inputs3<-"E:/DM_runs_4_4/GA_Model_D3_R2/"
LANDIS_Inputs4<-"E:/DM_runs_4_4/GA_Model_D4_R2/"
LANDIS_Inputs5<-"E:/DM_runs_4_4/GA_Model_D5_R2/"

mod1Year2<-ProcessLAI(2,LANDIS_Inputs)
mod1Year3<-ProcessLAI(3,LANDIS_Inputs)
mod1Year4<-ProcessLAI(4,LANDIS_Inputs)
mod1Year5<-ProcessLAI(5,LANDIS_Inputs)
mod1Year6<-ProcessLAI(6,LANDIS_Inputs)
mod1Year7<-ProcessLAI(7,LANDIS_Inputs)
mod1Year8<-ProcessLAI(8,LANDIS_Inputs)
mod1Year9<-ProcessLAI(9,LANDIS_Inputs)
mod1Year10<-ProcessLAI(10,LANDIS_Inputs)
mod1Year11<-ProcessLAI(11,LANDIS_Inputs)
mod2Year2<-ProcessLAI(2,LANDIS_Inputs2)
mod2Year3<-ProcessLAI(3,LANDIS_Inputs2)
mod2Year4<-ProcessLAI(4,LANDIS_Inputs2)
mod2Year5<-ProcessLAI(5,LANDIS_Inputs2)
mod2Year6<-ProcessLAI(6,LANDIS_Inputs2)
mod2Year7<-ProcessLAI(7,LANDIS_Inputs2)
mod2Year8<-ProcessLAI(8,LANDIS_Inputs2)
mod2Year9<-ProcessLAI(9,LANDIS_Inputs2)
mod2Year10<-ProcessLAI(10,LANDIS_Inputs2)
mod2Year11<-ProcessLAI(11,LANDIS_Inputs2)
mod3Year2<-ProcessLAI(2,LANDIS_Inputs3)
mod3Year3<-ProcessLAI(3,LANDIS_Inputs3)
mod3Year4<-ProcessLAI(4,LANDIS_Inputs3)
mod3Year5<-ProcessLAI(5,LANDIS_Inputs3)
mod3Year6<-ProcessLAI(6,LANDIS_Inputs3)
mod3Year7<-ProcessLAI(7,LANDIS_Inputs3)
mod3Year8<-ProcessLAI(8,LANDIS_Inputs3)
mod3Year9<-ProcessLAI(9,LANDIS_Inputs3)
mod3Year10<-ProcessLAI(10,LANDIS_Inputs3)
mod3Year11<-ProcessLAI(11,LANDIS_Inputs3)
mod4Year2<-ProcessLAI(2,LANDIS_Inputs4)
mod4Year3<-ProcessLAI(3,LANDIS_Inputs4)
mod4Year4<-ProcessLAI(4,LANDIS_Inputs4)
mod4Year5<-ProcessLAI(5,LANDIS_Inputs4)
mod4Year6<-ProcessLAI(6,LANDIS_Inputs4)
mod4Year7<-ProcessLAI(7,LANDIS_Inputs4)
mod4Year8<-ProcessLAI(8,LANDIS_Inputs4)
mod4Year9<-ProcessLAI(9,LANDIS_Inputs4)
mod4Year10<-ProcessLAI(10,LANDIS_Inputs4)
mod4Year11<-ProcessLAI(11,LANDIS_Inputs4)
mod5Year2<-ProcessLAI(2,LANDIS_Inputs5)
mod5Year3<-ProcessLAI(3,LANDIS_Inputs5)
mod5Year4<-ProcessLAI(4,LANDIS_Inputs5)
mod5Year5<-ProcessLAI(5,LANDIS_Inputs5)
mod5Year6<-ProcessLAI(6,LANDIS_Inputs5)
mod5Year7<-ProcessLAI(7,LANDIS_Inputs5)
mod5Year8<-ProcessLAI(8,LANDIS_Inputs5)
mod5Year9<-ProcessLAI(9,LANDIS_Inputs5)
mod5Year10<-ProcessLAI(10,LANDIS_Inputs5)
mod5Year11<-ProcessLAI(11,LANDIS_Inputs5)
Stack_D<-rbind(mod1Year2,mod1Year3,mod1Year4,mod1Year5,mod1Year6,
                        mod1Year7,mod1Year8,mod1Year9,mod1Year10,mod1Year11,
                        mod2Year2,mod2Year3,mod2Year4,mod2Year5,mod2Year6,
                        mod2Year7,mod2Year8,mod2Year9,mod2Year10,mod2Year11,
                        mod3Year2,mod3Year3,mod3Year4,mod3Year5,mod3Year6,
                        mod3Year7,mod3Year8,mod3Year9,mod3Year10,mod3Year11,
                        mod4Year2,mod4Year3,mod4Year4,mod4Year5,mod4Year6,
                        mod4Year7,mod4Year8,mod4Year9,mod4Year10,mod4Year11,
                        mod5Year2,mod5Year3,mod5Year4,mod5Year5,mod5Year6,
                        mod5Year7,mod5Year8,mod5Year9,mod5Year10,mod5Year11)

Stack_D$model<-"Delay"

LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_ND1_R2/"
LANDIS_Inputs2<-"E:/DM_runs_4_4/GA_Model_ND2_R2/"
LANDIS_Inputs3<-"E:/DM_runs_4_4/GA_Model_ND3_R2/"
LANDIS_Inputs4<-"E:/DM_runs_4_4/GA_Model_ND4_R2/"
LANDIS_Inputs5<-"E:/DM_runs_4_4/GA_Model_ND5_R2/"
mod1Year2<-ProcessLAI(2,LANDIS_Inputs)
mod1Year3<-ProcessLAI(3,LANDIS_Inputs)
mod1Year4<-ProcessLAI(4,LANDIS_Inputs)
mod1Year5<-ProcessLAI(5,LANDIS_Inputs)
mod1Year6<-ProcessLAI(6,LANDIS_Inputs)
mod1Year7<-ProcessLAI(7,LANDIS_Inputs)
mod1Year8<-ProcessLAI(8,LANDIS_Inputs)
mod1Year9<-ProcessLAI(9,LANDIS_Inputs)
mod1Year10<-ProcessLAI(10,LANDIS_Inputs)
mod1Year11<-ProcessLAI(11,LANDIS_Inputs)
mod2Year2<-ProcessLAI(2,LANDIS_Inputs2)
mod2Year3<-ProcessLAI(3,LANDIS_Inputs2)
mod2Year4<-ProcessLAI(4,LANDIS_Inputs2)
mod2Year5<-ProcessLAI(5,LANDIS_Inputs2)
mod2Year6<-ProcessLAI(6,LANDIS_Inputs2)
mod2Year7<-ProcessLAI(7,LANDIS_Inputs2)
mod2Year8<-ProcessLAI(8,LANDIS_Inputs2)
mod2Year9<-ProcessLAI(9,LANDIS_Inputs2)
mod2Year10<-ProcessLAI(10,LANDIS_Inputs2)
mod2Year11<-ProcessLAI(11,LANDIS_Inputs2)
mod3Year2<-ProcessLAI(2,LANDIS_Inputs3)
mod3Year3<-ProcessLAI(3,LANDIS_Inputs3)
mod3Year4<-ProcessLAI(4,LANDIS_Inputs3)
mod3Year5<-ProcessLAI(5,LANDIS_Inputs3)
mod3Year6<-ProcessLAI(6,LANDIS_Inputs3)
mod3Year7<-ProcessLAI(7,LANDIS_Inputs3)
mod3Year8<-ProcessLAI(8,LANDIS_Inputs3)
mod3Year9<-ProcessLAI(9,LANDIS_Inputs3)
mod3Year10<-ProcessLAI(10,LANDIS_Inputs3)
mod3Year11<-ProcessLAI(11,LANDIS_Inputs3)
mod4Year2<-ProcessLAI(2,LANDIS_Inputs4)
mod4Year3<-ProcessLAI(3,LANDIS_Inputs4)
mod4Year4<-ProcessLAI(4,LANDIS_Inputs4)
mod4Year5<-ProcessLAI(5,LANDIS_Inputs4)
mod4Year6<-ProcessLAI(6,LANDIS_Inputs4)
mod4Year7<-ProcessLAI(7,LANDIS_Inputs4)
mod4Year8<-ProcessLAI(8,LANDIS_Inputs4)
mod4Year9<-ProcessLAI(9,LANDIS_Inputs4)
mod4Year10<-ProcessLAI(10,LANDIS_Inputs4)
mod4Year11<-ProcessLAI(11,LANDIS_Inputs4)
mod5Year2<-ProcessLAI(2,LANDIS_Inputs5)
mod5Year3<-ProcessLAI(3,LANDIS_Inputs5)
mod5Year4<-ProcessLAI(4,LANDIS_Inputs5)
mod5Year5<-ProcessLAI(5,LANDIS_Inputs5)
mod5Year6<-ProcessLAI(6,LANDIS_Inputs5)
mod5Year7<-ProcessLAI(7,LANDIS_Inputs5)
mod5Year8<-ProcessLAI(8,LANDIS_Inputs5)
mod5Year9<-ProcessLAI(9,LANDIS_Inputs5)
mod5Year10<-ProcessLAI(10,LANDIS_Inputs5)
mod5Year11<-ProcessLAI(11,LANDIS_Inputs5)
Stack_ND<-rbind(mod1Year2,mod1Year3,mod1Year4,mod1Year5,mod1Year6,
                        mod1Year7,mod1Year8,mod1Year9,mod1Year10,mod1Year11,
                        mod2Year2,mod2Year3,mod2Year4,mod2Year5,mod2Year6,
                        mod2Year7,mod2Year8,mod2Year9,mod2Year10,mod2Year11,
                        mod3Year2,mod3Year3,mod3Year4,mod3Year5,mod3Year6,
                        mod3Year7,mod3Year8,mod3Year9,mod3Year10,mod3Year11,
                        mod4Year2,mod4Year3,mod4Year4,mod4Year5,mod4Year6,
                        mod4Year7,mod4Year8,mod4Year9,mod4Year10,mod4Year11,
                        mod5Year2,mod5Year3,mod5Year4,mod5Year5,mod5Year6,
                        mod5Year7,mod5Year8,mod5Year9,mod5Year10,mod5Year11)

Stack_ND$model<-"No Delay"
mod1Year2<-ProcessLAINF(2,LANDIS_Inputs)
mod1Year3<-ProcessLAINF(3,LANDIS_Inputs)
mod1Year4<-ProcessLAINF(4,LANDIS_Inputs)
mod1Year5<-ProcessLAINF(5,LANDIS_Inputs)
mod1Year6<-ProcessLAINF(6,LANDIS_Inputs)
mod1Year7<-ProcessLAINF(7,LANDIS_Inputs)
mod1Year8<-ProcessLAINF(8,LANDIS_Inputs)
mod1Year9<-ProcessLAINF(9,LANDIS_Inputs)
mod1Year10<-ProcessLAINF(10,LANDIS_Inputs)
mod1Year11<-ProcessLAINF(11,LANDIS_Inputs)
mod2Year2<-ProcessLAINF(2,LANDIS_Inputs2)
mod2Year3<-ProcessLAINF(3,LANDIS_Inputs2)
mod2Year4<-ProcessLAINF(4,LANDIS_Inputs2)
mod2Year5<-ProcessLAINF(5,LANDIS_Inputs2)
mod2Year6<-ProcessLAINF(6,LANDIS_Inputs2)
mod2Year7<-ProcessLAINF(7,LANDIS_Inputs2)
mod2Year8<-ProcessLAINF(8,LANDIS_Inputs2)
mod2Year9<-ProcessLAINF(9,LANDIS_Inputs2)
mod2Year10<-ProcessLAINF(10,LANDIS_Inputs2)
mod2Year11<-ProcessLAINF(11,LANDIS_Inputs2)
mod3Year2<-ProcessLAINF(2,LANDIS_Inputs3)
mod3Year3<-ProcessLAINF(3,LANDIS_Inputs3)
mod3Year4<-ProcessLAINF(4,LANDIS_Inputs3)
mod3Year5<-ProcessLAINF(5,LANDIS_Inputs3)
mod3Year6<-ProcessLAINF(6,LANDIS_Inputs3)
mod3Year7<-ProcessLAINF(7,LANDIS_Inputs3)
mod3Year8<-ProcessLAINF(8,LANDIS_Inputs3)
mod3Year9<-ProcessLAINF(9,LANDIS_Inputs3)
mod3Year10<-ProcessLAINF(10,LANDIS_Inputs3)
mod3Year11<-ProcessLAINF(11,LANDIS_Inputs3)
mod4Year2<-ProcessLAINF(2,LANDIS_Inputs4)
mod4Year3<-ProcessLAINF(3,LANDIS_Inputs4)
mod4Year4<-ProcessLAINF(4,LANDIS_Inputs4)
mod4Year5<-ProcessLAINF(5,LANDIS_Inputs4)
mod4Year6<-ProcessLAINF(6,LANDIS_Inputs4)
mod4Year7<-ProcessLAINF(7,LANDIS_Inputs4)
mod4Year8<-ProcessLAINF(8,LANDIS_Inputs4)
mod4Year9<-ProcessLAINF(9,LANDIS_Inputs4)
mod4Year10<-ProcessLAINF(10,LANDIS_Inputs4)
mod4Year11<-ProcessLAINF(11,LANDIS_Inputs4)
mod5Year2<-ProcessLAINF(2,LANDIS_Inputs5)
mod5Year3<-ProcessLAINF(3,LANDIS_Inputs5)
mod5Year4<-ProcessLAINF(4,LANDIS_Inputs5)
mod5Year5<-ProcessLAINF(5,LANDIS_Inputs5)
mod5Year6<-ProcessLAINF(6,LANDIS_Inputs5)
mod5Year7<-ProcessLAINF(7,LANDIS_Inputs5)
mod5Year8<-ProcessLAINF(8,LANDIS_Inputs5)
mod5Year9<-ProcessLAINF(9,LANDIS_Inputs5)
mod5Year10<-ProcessLAINF(10,LANDIS_Inputs5)
mod5Year11<-ProcessLAINF(11,LANDIS_Inputs5)
Stack_C<-rbind(mod1Year2,mod1Year3,mod1Year4,mod1Year5,mod1Year6,
                        mod1Year7,mod1Year8,mod1Year9,mod1Year10,mod1Year11,
                        mod2Year2,mod2Year3,mod2Year4,mod2Year5,mod2Year6,
                        mod2Year7,mod2Year8,mod2Year9,mod2Year10,mod2Year11,
                        mod3Year2,mod3Year3,mod3Year4,mod3Year5,mod3Year6,
                        mod3Year7,mod3Year8,mod3Year9,mod3Year10,mod3Year11,
                        mod4Year2,mod4Year3,mod4Year4,mod4Year5,mod4Year6,
                        mod4Year7,mod4Year8,mod4Year9,mod4Year10,mod4Year11,
                        mod5Year2,mod5Year3,mod5Year4,mod5Year5,mod5Year6,
                        mod5Year7,mod5Year8,mod5Year9,mod5Year10,mod5Year11)


Stack_C$model<-"Undisturbed"




LAIStack<-rbind(Stack_D,Stack_ND,Stack_C)

write.csv(LAIStack,"LAIStack_4_28.csv")
```

``` r
LAIStack<-read.csv("LAIStack_4_28.csv")
# compare_means(value ~ model, data = LAIStack, 
#               group.by = "name")
# head(LAIStack)
# unique(LAIStack$name)
PB_D<-LAIStack$value[LAIStack$name=="   Pre-burn" & LAIStack$model=="Delay"]
PB_ND<-LAIStack$value[LAIStack$name=="   Pre-burn" & LAIStack$model=="No Delay"]
wilcox.test(PB_D,PB_ND)
```

    ## 
    ##  Wilcoxon rank sum test with continuity correction
    ## 
    ## data:  PB_D and PB_ND
    ## W = 86081198, p-value < 2.2e-16
    ## alternative hypothesis: true location shift is not equal to 0

``` r
B_D<-LAIStack$value[LAIStack$name=="  Post-burn" & LAIStack$model=="Delay"]
B_ND<-LAIStack$value[LAIStack$name=="  Post-burn" & LAIStack$model=="No Delay"]
wilcox.test(B_D,B_ND)
```

    ## 
    ##  Wilcoxon rank sum test with continuity correction
    ## 
    ## data:  B_D and B_ND
    ## W = 67962066, p-value < 2.2e-16
    ## alternative hypothesis: true location shift is not equal to 0

``` r
five_D<-LAIStack$value[LAIStack$name==" 5 yrs Post-burn" &LAIStack$model=="Delay"]
five_ND<-LAIStack$value[LAIStack$name==" 5 yrs Post-burn" & LAIStack$model=="No Delay"]
wilcox.test(five_D,five_ND)
```

    ## 
    ##  Wilcoxon rank sum test with continuity correction
    ## 
    ## data:  five_D and five_ND
    ## W = 67879249, p-value < 2.2e-16
    ## alternative hypothesis: true location shift is not equal to 0

``` r
ten_D<-LAIStack$value[LAIStack$name=="10 yrs Post-burn" &LAIStack$model=="Delay"]
ten_ND<-LAIStack$value[LAIStack$name=="10 yrs Post-burn" & LAIStack$model=="No Delay"]
wilcox.test(five_D,five_ND)
```

    ## 
    ##  Wilcoxon rank sum test with continuity correction
    ## 
    ## data:  five_D and five_ND
    ## W = 67879249, p-value < 2.2e-16
    ## alternative hypothesis: true location shift is not equal to 0

``` r
IntoClasses<-transform(LAIStack,group=cut(value,breaks=c(0,2,4,6,8,25),labels=c('0-2 ','2-4','4-6','6-8','>8')))
Delay<-IntoClasses[IntoClasses$model=="Delay",]
unique(IntoClasses$model)
```

    ## [1] "Delay"       "No Delay"    "Undisturbed"

``` r
D_Pre<-as.data.frame(table(Delay$group[Delay$name=="   Pre-burn"]))
s<-sum(D_Pre[,2])
D_Pre<-as.data.frame(table(Delay$group[Delay$name=="   Pre-burn"])/s)
D_Post<-as.data.frame(table(Delay$group[Delay$name=="  Post-burn"])/s)

D_5<-as.data.frame(table(Delay$group[Delay$name==" 5 yrs Post-burn"])/s)
D_10<-as.data.frame(table(Delay$group[Delay$name=="10 yrs Post-burn"])/s)
unique(IntoClasses$model)
```

    ## [1] "Delay"       "No Delay"    "Undisturbed"

``` r
Und<-IntoClasses[IntoClasses$model=="Undisturbed",]
#unique(Delay$name)
Und_Pre<-as.data.frame(table(Und$group[Und$name=="   Pre-burn"]))
s<-sum(Und_Pre[,2])
Und_Pre<-as.data.frame(table(Und$group[Und$name=="   Pre-burn"])/s)
Und_Post<-as.data.frame(table(Und$group[Und$name=="  Post-burn"])/s)
Und_5<-as.data.frame(table(Und$group[Und$name==" 5 yrs Post-burn"])/s)
Und_10<-as.data.frame(table(Und$group[Und$name=="10 yrs Post-burn"])/s)

ND<-IntoClasses[IntoClasses$model=="No Delay",]
unique(Delay$name)
```

    ## [1] "   Pre-burn"      "  Post-burn"      " 5 yrs Post-burn" "10 yrs Post-burn"

``` r
ND_Pre<-as.data.frame(table(ND$group[ND$name=="   Pre-burn"]))
s<-sum(ND_Pre[,2])
ND_Pre<-as.data.frame(table(ND$group[ND$name=="   Pre-burn"])/s)
ND_Post<-as.data.frame(table(ND$group[ND$name=="  Post-burn"])/s)
ND_5<-as.data.frame(table(ND$group[ND$name==" 5 yrs Post-burn"])/s)
ND_10<-as.data.frame(table(ND$group[ND$name=="10 yrs Post-burn"])/s)

Und<-IntoClasses[IntoClasses$model=="Undisturbed",]
unique(Delay$name)
```

    ## [1] "   Pre-burn"      "  Post-burn"      " 5 yrs Post-burn" "10 yrs Post-burn"

``` r
Und_Pre<-as.data.frame(table(Und$group[Und$name=="   Pre-burn"]))
s<-sum(Und_Pre[,2])
Und_Pre<-as.data.frame(table(Und$group[Und$name=="   Pre-burn"])/s)
Und_Post<-as.data.frame(table(Und$group[Und$name=="  Post-burn"])/s)
Und_5<-as.data.frame(table(Und$group[Und$name==" 5 yrs Post-burn"])/s)
Und_10<-as.data.frame(table(Und$group[Und$name=="10 yrs Post-burn"])/s)


print(cbind(D_Pre,ND_Pre,(D_Pre$Freq-ND_Pre$Freq)/(ND_Pre$Freq)))
```

    ##   Var1       Freq Var1       Freq (D_Pre$Freq - ND_Pre$Freq)/(ND_Pre$Freq)
    ## 1 0-2  0.08602404 0-2  0.09579158                              -0.10196662
    ## 2  2-4 0.11862676  2-4 0.13523046                              -0.12278079
    ## 3  4-6 0.22656925  4-6 0.23911824                              -0.05248025
    ## 4  6-8 0.35972975  6-8 0.36593186                              -0.01694882
    ## 5   >8 0.20905020   >8 0.16392786                               0.27525733

``` r
print(cbind(D_Post,ND_Post,(D_Post$Freq-ND_Post$Freq)/(ND_Post$Freq)))
```

    ##   Var1       Freq Var1       Freq (D_Post$Freq - ND_Post$Freq)/(ND_Post$Freq)
    ## 1 0-2  0.09395868 0-2  0.05883768                                 0.596913482
    ## 2  2-4 0.20661482  2-4 0.14949900                                 0.382048170
    ## 3  4-6 0.33537591  4-6 0.33458918                                 0.002351346
    ## 4  6-8 0.26820646  6-8 0.34412826                                -0.220620648
    ## 5   >8 0.05027889   >8 0.08577154                                -0.413804522

``` r
print(cbind(D_5,ND_5,(D_5$Freq-ND_5$Freq)/(ND_5$Freq)))
```

    ##   Var1      Freq Var1       Freq (D_5$Freq - ND_5$Freq)/(ND_5$Freq)
    ## 1 0-2  0.0346453 0-2  0.02276553                         0.52183132
    ## 2  2-4 0.0945086  2-4 0.05683367                         0.66289819
    ## 3  4-6 0.2644355  4-6 0.20913828                         0.26440528
    ## 4  6-8 0.4239139  6-8 0.44985972                        -0.05767536
    ## 5   >8 0.1670202   >8 0.25643287                        -0.34867869

``` r
print(cbind(D_10,ND_10,(D_10$Freq-ND_10$Freq)/(ND_10$Freq)))
```

    ##   Var1       Freq Var1       Freq (D_10$Freq - ND_10$Freq)/(ND_10$Freq)
    ## 1 0-2  0.01924739 0-2  0.01234469                            0.55916340
    ## 2  2-4 0.04556524  2-4 0.02557114                            0.78190103
    ## 3  4-6 0.15625737  4-6 0.10869739                            0.43754471
    ## 4  6-8 0.40985152  6-8 0.37931864                            0.08049402
    ## 5   >8 0.36695734   >8 0.47855711                           -0.23320053

``` r
par(mfrow=c(4,2))
par(mar=c(5.1, 7.1, 4.1, 6.1))
barplot(D_Pre$Freq*100,names.arg=D_Pre$Var1,main="\nDelayed Mortality \nPre Fire",ylim=c(0,50),col=Greens,cex.lab=3.0,cex.axis=2.5,cex.main=2.5,cex.names=2.5,
        ylab="Percent Sites")
#mtext('text is here', side=1, line=3.5, at=9)

barplot(ND_Pre$Freq*100,names.arg=D_Pre$Var1,main="\nImmediate Mortality \nPre Fire",ylim=c(0,50),col=Greens,cex.lab=3.0,cex.axis=2.5,cex.main=2.5,cex.names=2.5)
# barplot(Und_Pre$Freq,names.arg=D_Pre$Var1,main="\nLandscape  \nPre Fire",ylim=c(0,.5),col=Greens,cex.lab=1.7,cex.axis=1.7,cex.main=2.0,cex.names=1.7)


barplot(D_Post$Freq*100,names.arg=D_Pre$Var1,main="Post Fire",ylim=c(0,50),col=Greens,cex.lab=3.0,cex.axis=2.5,cex.main=2.5,cex.names=2.5, ylab="Percent Sites")

barplot(ND_Post$Freq*100,names.arg=D_Pre$Var1,main="Post Fire",ylim=c(0,50),col=Greens,cex.lab=3.0,cex.axis=2.5,cex.main=2.5,cex.names=2.5)
# barplot(Und_Post$Freq,names.arg=D_Pre$Var1,main="Post Fire",ylim=c(0,.5),col=Greens,cex.lab=1.7,cex.axis=1.7,cex.main=2.0,cex.names=1.7)
barplot(D_5$Freq*100,names.arg=D_Pre$Var1,main="5-Years Post Fire",ylim=c(0,50),col=Greens,cex.lab=3.0,cex.axis=2.5,cex.main=2.5,cex.names=2.5,
        ylab="Percent Sites")
barplot(ND_5$Freq*100,names.arg=D_Pre$Var1,main="5-Years Post Fire",ylim=c(0,50),col=Greens,cex.lab=3.0,cex.axis=2.5,cex.main=2.5,cex.names=2.5)
# barplot(Und_5$Freq,names.arg=D_Pre$Var1,main="5-Years Post Fire",ylim=c(0,.5),col=Greens,cex.lab=1.7,cex.axis=1.7,cex.main=2.0,cex.names=1.7)
barplot(D_10$Freq*100,names.arg=D_Pre$Var1,main="10-Years Post Fire",ylim=c(0,50),col=Greens,cex.lab=3.0,cex.axis=2.5,cex.main=2.5,cex.names=2.5,
        ylab="Percent Sites",xlab="LAI")
barplot(ND_10$Freq*100,names.arg=D_Pre$Var1,main="10-Years Post Fire",ylim=c(0,50),col=Greens,cex.lab=3.0,cex.axis=2.5,cex.main=2.5,cex.names=2.5,
        xlab="LAI")
```

![](Outputs_doc_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
# barplot(Und_10$Freq,names.arg=D_Pre$Var1,main="10-Years Post Fire",ylim=c(0,.5),col=Greens,cex.lab=1.7,cex.axis=1.7,cex.main=2.0,cex.names=1.7,
#         xlab="LAI")
```

### Calculating ANPP

``` r
ProcessAG_NPP<-function(t,LANDIS_Inputs){
  Fire1<-raster(paste0(LANDIS_Inputs,'scrapple-fire/ignition-type-',t,'.img'))
  Fire1[Fire1<=1]<-NA
  Fire1[Fire1>1]<-1
  AG_NPPTOF<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',as.character(t-1),'.img'))*Fire1
  #plot(AG_NPPTOF)
 AG_NPPTOF<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',as.character(t-1),'.img'))*Fire1
  AG_NPPAF<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',t,'.img'))*Fire1
  AG_NPP10YAF<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',as.character(t+10),'.img'))*Fire1
  AG_NPP20YAR<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',t+20,'.img'))*Fire1
  AG_NPP30YAR<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',t+30,'.img'))*Fire1
  AG_NPP40YAR<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',t+40,'.img'))*Fire1
  values0<-as.data.frame(AG_NPPTOF$layer)%>%subset(!is.na(layer))
  values1<-as.data.frame(AG_NPPAF$layer)%>%subset(!is.na(layer))
  values2<-as.data.frame(AG_NPP10YAF$layer)%>%subset(!is.na(layer))
  values3<-as.data.frame(AG_NPP20YAR$layer)%>%subset(!is.na(layer))
  values4<-as.data.frame(AG_NPP30YAR$layer)%>%subset(!is.na(layer))
  values5<-as.data.frame(AG_NPP40YAR$layer)%>%subset(!is.na(layer))
  #values4<-as.data.frame(AG_NPP4$layer)%>%subset(!is.na(layer))
  #values5<-as.data.frame(AG_NPP5$layer)%>%subset(!is.na(layer))
   delta<-AG_NPPAF-AG_NPPTOF
   #par(bg="grey")
 # plot(delta,ylim=c(0,400),xlim=c(0,400),col=rdbu,zlim=c(-3000,3000),colNA="darkgrey")
  data<-data.frame(name=c(rep("   Pre-burn",1),rep(" Post-burn",1),rep("10 yrs Post-burn",1),rep("20 yrs Post-burn",1),rep("30 yrs Post-burn",1),
                          rep("40 yrs Post-burn",1)),
                 value=c(mean(values0$layer),mean(values1$layer),mean(values2$layer),mean(values3$layer),mean(values4$layer),mean(values5$layer)))
  return(data)
}

ProcessAG_NPPNF<-function(t,LANDIS_Inputs){
  Fire1<-raster(paste0(LANDIS_Inputs,'scrapple-fire/ignition-type-',t,'.img'))
  #plot(Fire1,ylim=c(0,400),xlim=c(0,400))
  Fire1[Fire1!=1]<-NA
  AG_NPPTOF<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',as.character(t-1),'.img'))*Fire1
  AG_NPPAF<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',t,'.img'))*Fire1
  AG_NPP10YAF<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',t+10,'.img'))*Fire1
  AG_NPP20YAR<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',t+20,'.img'))*Fire1
  AG_NPP30YAR<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',t+30,'.img'))*Fire1
  AG_NPP40YAR<-raster(paste0(LANDIS_Inputs,'NECN/AG_NPP-',t+40,'.img'))*Fire1
  values0<-as.data.frame(AG_NPPTOF$layer)%>%subset(!is.na(layer))
  values1<-as.data.frame(AG_NPPAF$layer)%>%subset(!is.na(layer))
  values2<-as.data.frame(AG_NPP10YAF$layer)%>%subset(!is.na(layer))
  values3<-as.data.frame(AG_NPP20YAR$layer)%>%subset(!is.na(layer))
  values4<-as.data.frame(AG_NPP30YAR$layer)%>%subset(!is.na(layer))
  values5<-as.data.frame(AG_NPP40YAR$layer)%>%subset(!is.na(layer))
  #values5<-as.data.frame(AG_NPP5$layer)%>%subset(!is.na(layer))

  #par(bg="grey")
  #plot(delta,ylim=c(0,400),xlim=c(0,400),col=rdbu,zlim=c(-3000,3000),colNA="darkgrey")
  data<-data.frame(name=c(rep("   Pre-burn",1),rep(" Post-burn",1),rep("10 yrs Post-burn",1),rep("20 yrs Post-burn",1),rep("30 yrs Post-burn",1),
                          rep("40 yrs Post-burn",1)),
                 value=c(mean(values0$layer),mean(values1$layer),mean(values2$layer),mean(values3$layer),mean(values4$layer),mean(values5$layer)))
  return(data)
}


LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D1_R2/"
LANDIS_Inputs2<-"E:/DM_runs_4_4/GA_Model_D2_R2/"
LANDIS_Inputs3<-"E:/DM_runs_4_4/GA_Model_D3_R2/"
LANDIS_Inputs4<-"E:/DM_runs_4_4/GA_Model_D4_R2/"
LANDIS_Inputs5<-"E:/DM_runs_4_4/GA_Model_D5_R2/"

mod1Year2<-ProcessAG_NPP(2,LANDIS_Inputs)
mod1Year3<-ProcessAG_NPP(3,LANDIS_Inputs)
mod1Year4<-ProcessAG_NPP(4,LANDIS_Inputs)
mod1Year5<-ProcessAG_NPP(5,LANDIS_Inputs)
mod1Year6<-ProcessAG_NPP(6,LANDIS_Inputs)
mod1Year7<-ProcessAG_NPP(7,LANDIS_Inputs)
mod1Year8<-ProcessAG_NPP(8,LANDIS_Inputs)
mod1Year9<-ProcessAG_NPP(9,LANDIS_Inputs)
mod1Year10<-ProcessAG_NPP(10,LANDIS_Inputs)
mod1Year11<-ProcessAG_NPP(11,LANDIS_Inputs)
mod2Year2<-ProcessAG_NPP(2,LANDIS_Inputs2)
mod2Year3<-ProcessAG_NPP(3,LANDIS_Inputs2)
mod2Year4<-ProcessAG_NPP(4,LANDIS_Inputs2)
mod2Year5<-ProcessAG_NPP(5,LANDIS_Inputs2)
mod2Year6<-ProcessAG_NPP(6,LANDIS_Inputs2)
mod2Year7<-ProcessAG_NPP(7,LANDIS_Inputs2)
mod2Year8<-ProcessAG_NPP(8,LANDIS_Inputs2)
mod2Year9<-ProcessAG_NPP(9,LANDIS_Inputs2)
mod2Year10<-ProcessAG_NPP(10,LANDIS_Inputs2)
mod2Year11<-ProcessAG_NPP(11,LANDIS_Inputs2)
mod3Year2<-ProcessAG_NPP(2,LANDIS_Inputs3)
mod3Year3<-ProcessAG_NPP(3,LANDIS_Inputs3)
mod3Year4<-ProcessAG_NPP(4,LANDIS_Inputs3)
mod3Year5<-ProcessAG_NPP(5,LANDIS_Inputs3)
mod3Year6<-ProcessAG_NPP(6,LANDIS_Inputs3)
mod3Year7<-ProcessAG_NPP(7,LANDIS_Inputs3)
mod3Year8<-ProcessAG_NPP(8,LANDIS_Inputs3)
mod3Year9<-ProcessAG_NPP(9,LANDIS_Inputs3)
mod3Year10<-ProcessAG_NPP(10,LANDIS_Inputs3)
mod3Year11<-ProcessAG_NPP(11,LANDIS_Inputs3)
mod4Year2<-ProcessAG_NPP(2,LANDIS_Inputs4)
mod4Year3<-ProcessAG_NPP(3,LANDIS_Inputs4)
mod4Year4<-ProcessAG_NPP(4,LANDIS_Inputs4)
mod4Year5<-ProcessAG_NPP(5,LANDIS_Inputs4)
mod4Year6<-ProcessAG_NPP(6,LANDIS_Inputs4)
mod4Year7<-ProcessAG_NPP(7,LANDIS_Inputs4)
mod4Year8<-ProcessAG_NPP(8,LANDIS_Inputs4)
mod4Year9<-ProcessAG_NPP(9,LANDIS_Inputs4)
mod4Year10<-ProcessAG_NPP(10,LANDIS_Inputs4)
mod4Year11<-ProcessAG_NPP(11,LANDIS_Inputs4)
mod5Year2<-ProcessAG_NPP(2,LANDIS_Inputs5)
mod5Year3<-ProcessAG_NPP(3,LANDIS_Inputs5)
mod5Year4<-ProcessAG_NPP(4,LANDIS_Inputs5)
mod5Year5<-ProcessAG_NPP(5,LANDIS_Inputs5)
mod5Year6<-ProcessAG_NPP(6,LANDIS_Inputs5)
mod5Year7<-ProcessAG_NPP(7,LANDIS_Inputs5)
mod5Year8<-ProcessAG_NPP(8,LANDIS_Inputs5)
mod5Year9<-ProcessAG_NPP(9,LANDIS_Inputs5)
mod5Year10<-ProcessAG_NPP(10,LANDIS_Inputs5)
mod5Year11<-ProcessAG_NPP(11,LANDIS_Inputs5)
Stack_D<-rbind(mod1Year2,mod1Year3,mod1Year4,mod1Year5,mod1Year6,
                        mod1Year7,mod1Year8,mod1Year9,mod1Year10,mod1Year11,
                        mod2Year2,mod2Year3,mod2Year4,mod2Year5,mod2Year6,
                        mod2Year7,mod2Year8,mod2Year9,mod2Year10,mod2Year11,
                        mod3Year2,mod3Year3,mod3Year4,mod3Year5,mod3Year6,
                        mod3Year7,mod3Year8,mod3Year9,mod3Year10,mod3Year11,
                        mod4Year2,mod4Year3,mod4Year4,mod4Year5,mod4Year6,
                        mod4Year7,mod4Year8,mod4Year9,mod4Year10,mod4Year11,
                        mod5Year2,mod5Year3,mod5Year4,mod5Year5,mod5Year6,
                        mod5Year7,mod5Year8,mod5Year9,mod5Year10,mod5Year11)

Stack_D$model<-"Delay"

LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_ND1_R2/"
LANDIS_Inputs2<-"E:/DM_runs_4_4/GA_Model_ND2_R2/"
LANDIS_Inputs3<-"E:/DM_runs_4_4/GA_Model_ND3_R2/"
LANDIS_Inputs4<-"E:/DM_runs_4_4/GA_Model_ND4_R2/"
LANDIS_Inputs5<-"E:/DM_runs_4_4/GA_Model_ND5_R2/"
mod1Year2<-ProcessAG_NPP(2,LANDIS_Inputs)
mod1Year3<-ProcessAG_NPP(3,LANDIS_Inputs)
mod1Year4<-ProcessAG_NPP(4,LANDIS_Inputs)
mod1Year5<-ProcessAG_NPP(5,LANDIS_Inputs)
mod1Year6<-ProcessAG_NPP(6,LANDIS_Inputs)
mod1Year7<-ProcessAG_NPP(7,LANDIS_Inputs)
mod1Year8<-ProcessAG_NPP(8,LANDIS_Inputs)
mod1Year9<-ProcessAG_NPP(9,LANDIS_Inputs)
mod1Year10<-ProcessAG_NPP(10,LANDIS_Inputs)
mod1Year11<-ProcessAG_NPP(11,LANDIS_Inputs)
mod2Year2<-ProcessAG_NPP(2,LANDIS_Inputs2)
mod2Year3<-ProcessAG_NPP(3,LANDIS_Inputs2)
mod2Year4<-ProcessAG_NPP(4,LANDIS_Inputs2)
mod2Year5<-ProcessAG_NPP(5,LANDIS_Inputs2)
mod2Year6<-ProcessAG_NPP(6,LANDIS_Inputs2)
mod2Year7<-ProcessAG_NPP(7,LANDIS_Inputs2)
mod2Year8<-ProcessAG_NPP(8,LANDIS_Inputs2)
mod2Year9<-ProcessAG_NPP(9,LANDIS_Inputs2)
mod2Year10<-ProcessAG_NPP(10,LANDIS_Inputs2)
mod2Year11<-ProcessAG_NPP(11,LANDIS_Inputs2)
mod3Year2<-ProcessAG_NPP(2,LANDIS_Inputs3)
mod3Year3<-ProcessAG_NPP(3,LANDIS_Inputs3)
mod3Year4<-ProcessAG_NPP(4,LANDIS_Inputs3)
mod3Year5<-ProcessAG_NPP(5,LANDIS_Inputs3)
mod3Year6<-ProcessAG_NPP(6,LANDIS_Inputs3)
mod3Year7<-ProcessAG_NPP(7,LANDIS_Inputs3)
mod3Year8<-ProcessAG_NPP(8,LANDIS_Inputs3)
mod3Year9<-ProcessAG_NPP(9,LANDIS_Inputs3)
mod3Year10<-ProcessAG_NPP(10,LANDIS_Inputs3)
mod3Year11<-ProcessAG_NPP(11,LANDIS_Inputs3)
mod4Year2<-ProcessAG_NPP(2,LANDIS_Inputs4)
mod4Year3<-ProcessAG_NPP(3,LANDIS_Inputs4)
mod4Year4<-ProcessAG_NPP(4,LANDIS_Inputs4)
mod4Year5<-ProcessAG_NPP(5,LANDIS_Inputs4)
mod4Year6<-ProcessAG_NPP(6,LANDIS_Inputs4)
mod4Year7<-ProcessAG_NPP(7,LANDIS_Inputs4)
mod4Year8<-ProcessAG_NPP(8,LANDIS_Inputs4)
mod4Year9<-ProcessAG_NPP(9,LANDIS_Inputs4)
mod4Year10<-ProcessAG_NPP(10,LANDIS_Inputs4)
mod4Year11<-ProcessAG_NPP(11,LANDIS_Inputs4)
mod5Year2<-ProcessAG_NPP(2,LANDIS_Inputs5)
mod5Year3<-ProcessAG_NPP(3,LANDIS_Inputs5)
mod5Year4<-ProcessAG_NPP(4,LANDIS_Inputs5)
mod5Year5<-ProcessAG_NPP(5,LANDIS_Inputs5)
mod5Year6<-ProcessAG_NPP(6,LANDIS_Inputs5)
mod5Year7<-ProcessAG_NPP(7,LANDIS_Inputs5)
mod5Year8<-ProcessAG_NPP(8,LANDIS_Inputs5)
mod5Year9<-ProcessAG_NPP(9,LANDIS_Inputs5)
mod5Year10<-ProcessAG_NPP(10,LANDIS_Inputs5)
mod5Year11<-ProcessAG_NPP(11,LANDIS_Inputs5)
Stack_ND<-rbind(mod1Year2,mod1Year3,mod1Year4,mod1Year5,mod1Year6,
                        mod1Year7,mod1Year8,mod1Year9,mod1Year10,mod1Year11,
                        mod2Year2,mod2Year3,mod2Year4,mod2Year5,mod2Year6,
                        mod2Year7,mod2Year8,mod2Year9,mod2Year10,mod2Year11,
                        mod3Year2,mod3Year3,mod3Year4,mod3Year5,mod3Year6,
                        mod3Year7,mod3Year8,mod3Year9,mod3Year10,mod3Year11,
                        mod4Year2,mod4Year3,mod4Year4,mod4Year5,mod4Year6,
                        mod4Year7,mod4Year8,mod4Year9,mod4Year10,mod4Year11,
                        mod5Year2,mod5Year3,mod5Year4,mod5Year5,mod5Year6,
                        mod5Year7,mod5Year8,mod5Year9,mod5Year10,mod5Year11)


Stack_ND$model<-"No Delay"



mod1Year2<-ProcessAG_NPPNF(2,LANDIS_Inputs)
mod1Year3<-ProcessAG_NPPNF(3,LANDIS_Inputs)
mod1Year4<-ProcessAG_NPPNF(4,LANDIS_Inputs)
mod1Year5<-ProcessAG_NPPNF(5,LANDIS_Inputs)
mod1Year6<-ProcessAG_NPPNF(6,LANDIS_Inputs)
mod1Year7<-ProcessAG_NPPNF(7,LANDIS_Inputs)
mod1Year8<-ProcessAG_NPPNF(8,LANDIS_Inputs)
mod1Year9<-ProcessAG_NPPNF(9,LANDIS_Inputs)
mod1Year10<-ProcessAG_NPPNF(10,LANDIS_Inputs)
mod1Year11<-ProcessAG_NPPNF(11,LANDIS_Inputs)
mod2Year2<-ProcessAG_NPPNF(2,LANDIS_Inputs2)
mod2Year3<-ProcessAG_NPPNF(3,LANDIS_Inputs2)
mod2Year4<-ProcessAG_NPPNF(4,LANDIS_Inputs2)
mod2Year5<-ProcessAG_NPPNF(5,LANDIS_Inputs2)
mod2Year6<-ProcessAG_NPPNF(6,LANDIS_Inputs2)
mod2Year7<-ProcessAG_NPPNF(7,LANDIS_Inputs2)
mod2Year8<-ProcessAG_NPPNF(8,LANDIS_Inputs2)
mod2Year9<-ProcessAG_NPPNF(9,LANDIS_Inputs2)
mod2Year10<-ProcessAG_NPPNF(10,LANDIS_Inputs2)
mod2Year11<-ProcessAG_NPPNF(11,LANDIS_Inputs2)
mod3Year2<-ProcessAG_NPPNF(2,LANDIS_Inputs3)
mod3Year3<-ProcessAG_NPPNF(3,LANDIS_Inputs3)
mod3Year4<-ProcessAG_NPPNF(4,LANDIS_Inputs3)
mod3Year5<-ProcessAG_NPPNF(5,LANDIS_Inputs3)
mod3Year6<-ProcessAG_NPPNF(6,LANDIS_Inputs3)
mod3Year7<-ProcessAG_NPPNF(7,LANDIS_Inputs3)
mod3Year8<-ProcessAG_NPPNF(8,LANDIS_Inputs3)
mod3Year9<-ProcessAG_NPPNF(9,LANDIS_Inputs3)
mod3Year10<-ProcessAG_NPPNF(10,LANDIS_Inputs3)
mod3Year11<-ProcessAG_NPPNF(11,LANDIS_Inputs3)
mod4Year2<-ProcessAG_NPPNF(2,LANDIS_Inputs4)
mod4Year3<-ProcessAG_NPPNF(3,LANDIS_Inputs4)
mod4Year4<-ProcessAG_NPPNF(4,LANDIS_Inputs4)
mod4Year5<-ProcessAG_NPPNF(5,LANDIS_Inputs4)
mod4Year6<-ProcessAG_NPPNF(6,LANDIS_Inputs4)
mod4Year7<-ProcessAG_NPPNF(7,LANDIS_Inputs4)
mod4Year8<-ProcessAG_NPPNF(8,LANDIS_Inputs4)
mod4Year9<-ProcessAG_NPPNF(9,LANDIS_Inputs4)
mod4Year10<-ProcessAG_NPPNF(10,LANDIS_Inputs4)
mod4Year11<-ProcessAG_NPPNF(11,LANDIS_Inputs4)
mod5Year2<-ProcessAG_NPPNF(2,LANDIS_Inputs5)
mod5Year3<-ProcessAG_NPPNF(3,LANDIS_Inputs5)
mod5Year4<-ProcessAG_NPPNF(4,LANDIS_Inputs5)
mod5Year5<-ProcessAG_NPPNF(5,LANDIS_Inputs5)
mod5Year6<-ProcessAG_NPPNF(6,LANDIS_Inputs5)
mod5Year7<-ProcessAG_NPPNF(7,LANDIS_Inputs5)
mod5Year8<-ProcessAG_NPPNF(8,LANDIS_Inputs5)
mod5Year9<-ProcessAG_NPPNF(9,LANDIS_Inputs5)
mod5Year10<-ProcessAG_NPPNF(10,LANDIS_Inputs5)
mod5Year11<-ProcessAG_NPPNF(11,LANDIS_Inputs5)
Stack_C<-rbind(mod1Year2,mod1Year3,mod1Year4,mod1Year5,mod1Year6,
                        mod1Year7,mod1Year8,mod1Year9,mod1Year10,mod1Year11,
                        mod2Year2,mod2Year3,mod2Year4,mod2Year5,mod2Year6,
                        mod2Year7,mod2Year8,mod2Year9,mod2Year10,mod2Year11,
                        mod3Year2,mod3Year3,mod3Year4,mod3Year5,mod3Year6,
                        mod3Year7,mod3Year8,mod3Year9,mod3Year10,mod3Year11,
                        mod4Year2,mod4Year3,mod4Year4,mod4Year5,mod4Year6,
                        mod4Year7,mod4Year8,mod4Year9,mod4Year10,mod4Year11,
                        mod5Year2,mod5Year3,mod5Year4,mod5Year5,mod5Year6,
                        mod5Year7,mod5Year8,mod5Year9,mod5Year10,mod5Year11)


Stack_C$model<-"Undisturbed"



Stack_D$value[Stack_D$name=="10 yrs Post-burn"]
boxplot(Stack_D$value[Stack_D$name=="20 yrs Post-burn"],Stack_ND$value[Stack_ND$name=="20 yrs Post-burn"])


NPPStack<-rbind(Stack_D,Stack_ND,Stack_C)
write.csv(NPPStack,"NPPStack_4_28_years.csv")
```

``` r
NPPStack<-read.csv("NPPStack_4_28_years.csv")

unique(NPPStack$name)
```

    ## [1] "   Pre-burn"      " Post-burn"       "10 yrs Post-burn" "20 yrs Post-burn"
    ## [5] "30 yrs Post-burn" "40 yrs Post-burn"

``` r
summary(NPPStack$value[NPPStack$model=="Delay"& NPPStack$name==" Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   488.4   703.3   778.3   773.5   851.3   971.8

``` r
summary(NPPStack$value[NPPStack$model=="No Delay"& NPPStack$name==" Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   593.9   769.5   848.5   829.5   902.8  1082.2

``` r
summary(NPPStack$value[NPPStack$model=="Undisturbed"& NPPStack$name==" Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   761.7   889.7   960.3   944.4  1009.3  1093.0

``` r
summary(NPPStack$value[NPPStack$model=="Delay"& NPPStack$name=="10 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   550.2   728.7   782.4   783.3   840.3  1002.6

``` r
summary(NPPStack$value[NPPStack$model=="No Delay"& NPPStack$name=="10 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   645.9   736.4   844.3   817.1   882.7   941.6

``` r
summary(NPPStack$value[NPPStack$model=="Undisturbed"& NPPStack$name=="10 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   747.1   813.7   916.8   895.4   946.8  1031.1

``` r
summary(NPPStack$value[NPPStack$model=="Delay"& NPPStack$name=="20 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   610.1   732.2   814.8   808.8   888.7   936.4

``` r
summary(NPPStack$value[NPPStack$model=="No Delay"& NPPStack$name=="20 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   599.7   822.5   866.7   857.3   909.6   979.2

``` r
summary(NPPStack$value[NPPStack$model=="Undisturbed"& NPPStack$name=="20 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   751.1   908.5   939.5   931.7   969.2  1038.7

``` r
summary(NPPStack$value[NPPStack$model=="Delay"& NPPStack$name=="30 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   620.4   747.5   852.6   832.8   923.0  1046.6

``` r
summary(NPPStack$value[NPPStack$model=="No Delay"& NPPStack$name=="30 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   612.8   797.1   887.7   862.5   932.6  1088.5

``` r
summary(NPPStack$value[NPPStack$model=="Undisturbed"& NPPStack$name=="30 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   754.3   835.3   953.2   930.4  1005.9  1069.3

``` r
summary(NPPStack$value[NPPStack$model=="Delay"& NPPStack$name=="40 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   608.8   810.0   897.3   881.3   963.4  1053.1

``` r
summary(NPPStack$value[NPPStack$model=="No Delay"& NPPStack$name=="40 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   698.9   818.1   872.5   881.9   947.0  1134.5

``` r
summary(NPPStack$value[NPPStack$model=="Undisturbed"& NPPStack$name=="40 yrs Post-burn"])
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   778.6   897.0   984.7   962.4  1013.2  1124.3

``` r
NPPStack$model[NPPStack$model=="No Delay"]<-"Immediate"
NPPStack$model[NPPStack$model=="Undisturbed"]<-"Unburned"
NPPStack$model[NPPStack$model=="Delay"]<-"Delayed"
```

``` r
label.df <- data.frame(Group = c("Preburn"),
                       Value = c(1200))
#my_comparisons <- list( c("Delayed", "Immediate"))
unique(NPPStack$model)
```

    ## [1] "Delayed"   "Immediate" "Unburned"

``` r
NPPStack$value100<-NPPStack$value/100
p1 <- ggboxplot(NPPStack, x="model", y="value100", facet.by="name") + 
    stat_compare_means(
    comparisons = list(c("Delayed", "Immediate"),c("Delayed", "Unburned"),c("Immediate", "Unburned")
                       ), 
    label = "p.signif",size=5.0)+
  ylab("Above Ground NPP Mg/ha/year")+
  xlab("")+
  theme_classic(base_size = 16)
p1
```

![](Outputs_doc_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
?ggsave()
```

    ## starting httpd help server ... done

``` r
#getwd()
ggsave('ANPP_Fig.png', p1,width = 10.0,height=10.0,dpi=200)
```

``` r
label.df <- data.frame(Group = c("Preburn"),
                       Value = c(1200))
#my_comparisons <- list( c("Delayed", "Immediate"))
unique(NPPStack$model)
```

    ## [1] "Delayed"   "Immediate" "Unburned"

``` r
NPPStack$value100<-NPPStack$value/100
p1 <- ggboxplot(NPPStack, x="model", y="value100", facet.by="name") + 
    stat_compare_means(
    comparisons = list(c("Delayed", "Immediate"),c("Delayed", "Unburned"),c("Immediate", "Unburned")
                       ),size=5.0)+
  ylab("Above Ground NPP Mg/ha/year")+
  xlab("")+
  theme_classic(base_size = 16)
p1
```

![](Outputs_doc_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
Biomass<-"E:/DM_Runs_222/GA_Model_D1/Biomass/"
LANDIS_Inputs<-"E:/DM_Runs_222/GA_Model_D1/"

Firemaps<-function(LANDIS_Inputs,t){
  Fire1<-raster(paste0(LANDIS_Inputs,'scrapple-fire/ignition-type-',t,'.img'))
  FireOn<-Fire1
  FireOn[FireOn<=1]<-NA
  FireOn[FireOn>1]<-1
  plot(FireOn,ylim=c(0,400),xlim=c(0,400))
  FireOff<-Fire1
  FireOff[FireOff!=1]<-NA
  #plot(FireOff)
  return(list(FireOn,FireOff))
}
Fire2<-Firemaps(LANDIS_Inputs,2)
FireOn<-Fire2[[1]]
FireOff<-Fire2[[2]]
plot(FireOn,col=Greens,ylim=c(0,400),xlim=c(0,400))
# AllStack<-stack(paste0(Biomass,list.files(Biomass,pattern = paste0("\\-",(t+10),".img$"))))
Bio_sum<-calc(AllStack,sum)
# AcerRubr_perc<-AllStack$AcerRubr.ageclass.12/Bio_sum
#QuerPrin_perc<-AllStack$ /Bio_sum
#LiriTuli_perc<-AllStack$LiriTuli.ageclass.12/Bio_sum
#QuerRubr_perc<-AllStack$QuerRubr.ageclass.12/Bio_sum


#AcerRubr_percOn<-as.data.frame(AcerRubr_perc*FireOn)
#AcerRubr_percOff<-as.data.frame(AcerRubr_perc*FireOff)
#Species<-'AcerRubr'

CalculateFrame<-function(Species,perc,FireOn,FireOff){

    #perc<-AcerRubr_perc
    One_percOn<-mean((as.vector(perc*FireOn)),na.rm=T)
    One_percOff<-mean((as.vector(perc*FireOff)),na.rm=T)
    OneSpecies<-data.frame(Species=c(Species,Species),
               Fire=c(rep("Fire",length(One_percOn)),
                      rep("Landscape",length(One_percOff))),
               Value=c(One_percOn,One_percOff))
    OneSpecies<-na.omit(OneSpecies)
    return(OneSpecies)
  }

#hist(AcerFrame$Value[AcerFrame$Fire=="Fire"])
#ist(AcerFrame$Value[AcerFrame$Fire=="NoFire"])

# plot(Bio_sum,ylim=c(0,400),xlim=c(0,400))
# plot(AllStack$AcerRubr.ageclass.0/Bio_sum,ylim=c(0,400),xlim=c(0,400))
# plot(AllStack$QuerPrin.ageclass.0/Bio_sum,ylim=c(0,400),xlim=c(0,400))
# plot(AllStack$LiriTuli.ageclass.0/Bio_sum,ylim=c(0,400),xlim=c(0,400))
# plot(AllStack$QuerRubr.ageclass.0/Bio_sum,ylim=c(0,400),xlim=c(0,400))
```

``` r
# AllStack<-stack(paste0(Biomass,list.files(Biomass,pattern = paste0("\\-",(t+1),".img$"))))
# df<-NULL
# for(i in 1:length(names(AllStack))){
# 
# print(names(AllStack[[i]]))
# bio<-cellStats(AllStack[[i]],stat='sum')
# print(bio)
# row<-data.frame(name=names(AllStack[[i]]),bio=bio)
# df<-rbind(row,df)
# }
```

``` r
### AllFrame1
Sumstack<-function(Biomass,t,stagger,FireOn,FireOff){
 
  AllStack<-stack(paste0(Biomass,list.files(Biomass,pattern = paste0("\\-",(t+stagger),".img$"))))
  AllStack<-AllStack[[which(c(grepl("LiriTuli|AcerRubr|NyssSylv|QuerAlba|PinuTaed|QuerPrin|QuerCocc|PinuVirg|PinuStro|QuerRubr|QuerFalc",names(AllStack))))]]
  AllStack<-AllStack[[which(c(grepl("ageclass1",names(AllStack))))]]
  Bio_sum<-calc(AllStack,sum)
  LiriTuli_perc<-AllStack[[which(c(grepl("LiriTuli", names(AllStack), fixed = TRUE)))]]/Bio_sum
  LiriTuliFrame<-CalculateFrame("LiriTuli",LiriTuli_perc,FireOn,FireOff)
  AcerRubr_perc<-AllStack[[which(c(grepl("AcerRubr", names(AllStack), fixed = TRUE)))]]/Bio_sum
  AcerFrame<-CalculateFrame("AcerRubr",AcerRubr_perc,FireOn,FireOff)
  NyssSylv_perc<-AllStack[[which(c(grepl("NyssSylv", names(AllStack), fixed = TRUE)))]]/Bio_sum
  NyssSylvFrame<-CalculateFrame("NyssSylv",NyssSylv_perc,FireOn,FireOff)
  QuerAlba_perc<-AllStack[[which(c(grepl("QuerAlba", names(AllStack), fixed = TRUE)))]]/Bio_sum
  QuerAlbaFrame<-CalculateFrame("QuerAlba",QuerAlba_perc,FireOn,FireOff)
  PinuTaed_perc<-AllStack[[which(c(grepl("PinuTaed", names(AllStack), fixed = TRUE)))]]/Bio_sum
  PinuTaedFrame<-CalculateFrame("PinuTaed",PinuTaed_perc,FireOn,FireOff)
  QuerPrin_perc<-AllStack[[which(c(grepl("QuerPrin", names(AllStack), fixed = TRUE)))]]/Bio_sum
  QuerPrinFrame<-CalculateFrame("QuerPrin",QuerPrin_perc,FireOn,FireOff)
  QuerCocc_perc<-AllStack[[which(c(grepl("QuerCocc", names(AllStack), fixed = TRUE)))]]/Bio_sum
  QuerCoccFrame<-CalculateFrame("QuerCocc",QuerCocc_perc,FireOn,FireOff)
  PinuVirg_perc<-AllStack[[which(c(grepl("PinuVirg", names(AllStack), fixed = TRUE)))]]/Bio_sum
  PinuVirgFrame<-CalculateFrame("PinuVirg",PinuVirg_perc,FireOn,FireOff)
  PinuStro_perc<-AllStack[[which(c(grepl("PinuStro", names(AllStack), fixed = TRUE)))]]/Bio_sum
  PinuStroFrame<-CalculateFrame("PinuStro",PinuStro_perc,FireOn,FireOff)
  QuerRubr_perc<-AllStack[[which(c(grepl("QuerRubr", names(AllStack), fixed = TRUE)))]]/Bio_sum
  QuerRubrFrame<-CalculateFrame("QuerRubr",QuerRubr_perc,FireOn,FireOff)
  QuerFalc_perc<-AllStack[[which(c(grepl("QuerFalc", names(AllStack), fixed = TRUE)))]]/Bio_sum
  QuerFalcFrame<-CalculateFrame("QuerFalc",QuerFalc_perc,FireOn,FireOff)
  AllFrameThree<-rbind(LiriTuliFrame,AcerFrame,NyssSylvFrame,QuerAlbaFrame,
                     QuerAlbaFrame,PinuTaedFrame,QuerPrinFrame,QuerCoccFrame,
                     PinuVirgFrame,PinuStroFrame,QuerRubrFrame,QuerFalcFrame)
  AllFrameThree<-cbind(data.frame(Time=t,Stagger=stagger),AllFrameThree)
  return(AllFrameThree)}
# ptm <- proc.time()
# dfout<-Sumstack(Biomass,2,1)
# proc.time() - ptm
# 
# 
# dfout<-Sumstack(Biomass,2,1)
#### Stagger 1
Stagger<-function(LANDIS_Inputs,Biomass,Stagger){
  Fire2<-Firemaps(LANDIS_Inputs,2)
  FireOn<-Fire2[[1]]
  FireOff<-Fire2[[2]]
  Year2<-Sumstack(Biomass,2,Stagger,FireOn,FireOff)
  Fire3<-Firemaps(LANDIS_Inputs,3)
  FireOn<-Fire3[[1]]
  FireOff<-Fire3[[2]]
  Year3<-Sumstack(Biomass,3,Stagger,FireOn,FireOff)
  
  Fire4<-Firemaps(LANDIS_Inputs,4)
  FireOn<-Fire4[[1]]
  FireOff<-Fire4[[2]]
  Year4<-Sumstack(Biomass,4,Stagger,FireOn,FireOff)
  
  Fire5<-Firemaps(LANDIS_Inputs,5)
  FireOn<-Fire5[[1]]
  FireOff<-Fire5[[2]]
  Year5<-Sumstack(Biomass,5,Stagger,FireOn,FireOff)
  
  Fire6<-Firemaps(LANDIS_Inputs,6)
  FireOn<-Fire6[[1]]
  FireOff<-Fire6[[2]]
  Year6<-Sumstack(Biomass,6,Stagger,FireOn,FireOff)
  
  Fire7<-Firemaps(LANDIS_Inputs,7)
  FireOn<-Fire7[[1]]
  FireOff<-Fire7[[2]]
  Year7<-Sumstack(Biomass,7,Stagger,FireOn,FireOff)
  
  Fire8<-Firemaps(LANDIS_Inputs,8)
  FireOn<-Fire8[[1]]
  FireOff<-Fire8[[2]]
  Year8<-Sumstack(Biomass,8,Stagger,FireOn,FireOff)
  
  Fire9<-Firemaps(LANDIS_Inputs,9)
  FireOn<-Fire9[[1]]
  FireOff<-Fire9[[2]]
  Year9<-Sumstack(Biomass,9,Stagger,FireOn,FireOff)
  
  Fire10<-Firemaps(LANDIS_Inputs,10)
  FireOn<-Fire10[[1]]
  FireOff<-Fire10[[2]]
  Year10<-Sumstack(Biomass,10,Stagger,FireOn,FireOff)
  
  Fire11<-Firemaps(LANDIS_Inputs,11)
  FireOn<-Fire11[[1]]
  FireOff<-Fire11[[2]]
  Year11<-Sumstack(Biomass,11,Stagger,FireOn,FireOff)
  Stagger1<-rbind(Year2,Year3,Year4,Year5,Year6,Year7,Year8,
        Year9,Year10,Year11)
  return(Stagger1)
}
```

``` r
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D1_R2/"
LANDIS_Inputs2<-"E:/DM_runs_4_4/GA_Model_D2_R2/"
LANDIS_Inputs3<-"E:/DM_runs_4_4/GA_Model_D3_R2/"
LANDIS_Inputs4<-"E:/DM_runs_4_4/GA_Model_D4_R2/"
LANDIS_Inputs5<-"E:/DM_runs_4_4/GA_Model_D5_R2/"

Stagger1_1<-Stagger(LANDIS_Inputs,Biomass,1)
Stagger1_2<-Stagger(LANDIS_Inputs2,Biomass,1)
Stagger1_3<-Stagger(LANDIS_Inputs3,Biomass,1)
Stagger1_4<-Stagger(LANDIS_Inputs4,Biomass,1)
Stagger1_5<-Stagger(LANDIS_Inputs5,Biomass,1)
Stagger_yr1<-rbind(Stagger1_1,Stagger1_2,Stagger1_3,Stagger1_4,Stagger1_5)

write.csv(Stagger_yr1,"C:/Users/zacha/Desktop/Sapps_DM_paper/Stagger1_428.csv")

Stagger10_1<-Stagger(LANDIS_Inputs,Biomass,10)
Stagger10_2<-Stagger(LANDIS_Inputs2,Biomass,10)
Stagger10_3<-Stagger(LANDIS_Inputs3,Biomass,10)
Stagger10_4<-Stagger(LANDIS_Inputs4,Biomass,10)
Stagger10_5<-Stagger(LANDIS_Inputs5,Biomass,10)
Stagger_yr10<-rbind(Stagger10_1,Stagger10_2,Stagger10_3,Stagger10_4,Stagger10_5)
write.csv(Stagger_yr10,"C:/Users/zacha/Desktop/Sapps_DM_paper/Stagger10_428.csv")
###

# Stagger1<-write.csv(Stagger1,"C:/Users/zacha/Desktop/Sapps_DM_paper/Stagger1.csv")
# Stagger5<-write.csv(Stagger5,"C:/Users/zacha/Desktop/Sapps_DM_paper/Stagger5.csv")
# Stagger10<-write.csv(Stagger10,"C:/Users/zacha/Desktop/Sapps_DM_paper/Stagger10.csv")
```

``` r
#head(OneRun)

g_legend<-function(a.gplot){
  tmp <- ggplot_gtable(ggplot_build(a.gplot))
  leg <- which(sapply(tmp$grobs, function(x) x$name) == "guide-box")
  legend <- tmp$grobs[[leg]]
  return(legend)}


Stagger1<-read.csv("D:/Sapps_DM_paper/Stagger1_428.csv")
#Stagger5<-read.csv("C:/Users/zacha/Desktop/Sapps_DM_paper/Stagger5.csv")
Stagger10<-read.csv("D:/Sapps_DM_paper/Stagger10_428.csv")


Stagger1$Fire[Stagger1$Fire=="Fire"]<-"Burned Sites (Delayed Mortality)"
Stagger1$Fire[Stagger1$Fire=="Landscape"]<-"Landscape Average"

#Stagger5$Fire[Stagger5$Fire=="Fire"]<-"Burned Sites (Delayed Mortality)"
#Stagger5$Fire[Stagger5$Fire=="NoFire"]<-"Unburned Landscape"

Stagger10$Fire[Stagger10$Fire=="Fire"]<-"Burned Sites (Delayed Mortality)"
Stagger10$Fire[Stagger10$Fire=="NoFire"]<-"Unburned Landscape"
head(Stagger1)
```

    ##   X Time Stagger  Species                             Fire     Value
    ## 1 1    2       1 LiriTuli Burned Sites (Delayed Mortality) 0.1664392
    ## 2 2    2       1 LiriTuli                Landscape Average 0.1674065
    ## 3 3    2       1 AcerRubr Burned Sites (Delayed Mortality) 0.1107812
    ## 4 4    2       1 AcerRubr                Landscape Average 0.2468617
    ## 5 5    2       1 NyssSylv Burned Sites (Delayed Mortality) 0.1259665
    ## 6 6    2       1 NyssSylv                Landscape Average 0.2284488

``` r
compare_means(Value ~ Fire, data = Stagger1, 
              group.by = "Species")
```

    ## # A tibble: 11 x 9
    ##    Species  .y.   group1        group2          p p.adj p.format p.signif method
    ##    <chr>    <chr> <chr>         <chr>       <dbl> <dbl> <chr>    <chr>    <chr> 
    ##  1 LiriTuli Value Burned Sites~ Landscap~ 0.515   1     0.5147   ns       Wilco~
    ##  2 AcerRubr Value Burned Sites~ Landscap~ 0.866   1     0.8659   ns       Wilco~
    ##  3 NyssSylv Value Burned Sites~ Landscap~ 0.114   1     0.1136   ns       Wilco~
    ##  4 QuerAlba Value Burned Sites~ Landscap~ 0.0316  0.32  0.0316   *        Wilco~
    ##  5 PinuTaed Value Burned Sites~ Landscap~ 0.855   1     0.8554   ns       Wilco~
    ##  6 QuerPrin Value Burned Sites~ Landscap~ 0.319   1     0.3192   ns       Wilco~
    ##  7 QuerCocc Value Burned Sites~ Landscap~ 0.560   1     0.5602   ns       Wilco~
    ##  8 PinuVirg Value Burned Sites~ Landscap~ 0.528   1     0.5282   ns       Wilco~
    ##  9 PinuStro Value Burned Sites~ Landscap~ 0.00358 0.039 0.0036   **       Wilco~
    ## 10 QuerRubr Value Burned Sites~ Landscap~ 0.391   1     0.3907   ns       Wilco~
    ## 11 QuerFalc Value Burned Sites~ Landscap~ 0.839   1     0.8388   ns       Wilco~

``` r
# compare_means(Value ~ Fire, data = Stagger5, 
#               group.by = "Species")
compare_means(Value ~ Fire, data = Stagger10, 
              group.by = "Species")
```

    ## # A tibble: 11 x 9
    ##    Species  .y.   group1         group2        p  p.adj p.format p.signif method
    ##    <chr>    <chr> <chr>          <chr>     <dbl>  <dbl> <chr>    <chr>    <chr> 
    ##  1 LiriTuli Value Burned Sites ~ Landsc~ 8.88e-1 1      0.88761  ns       Wilco~
    ##  2 AcerRubr Value Burned Sites ~ Landsc~ 3.72e-1 1      0.37199  ns       Wilco~
    ##  3 NyssSylv Value Burned Sites ~ Landsc~ 6.32e-1 1      0.63185  ns       Wilco~
    ##  4 QuerAlba Value Burned Sites ~ Landsc~ 9.70e-1 1      0.96979  ns       Wilco~
    ##  5 PinuTaed Value Burned Sites ~ Landsc~ 3.85e-4 0.0042 0.00038  ***      Wilco~
    ##  6 QuerPrin Value Burned Sites ~ Landsc~ 1.06e-1 0.95   0.10596  ns       Wilco~
    ##  7 QuerCocc Value Burned Sites ~ Landsc~ 6.37e-1 1      0.63677  ns       Wilco~
    ##  8 PinuVirg Value Burned Sites ~ Landsc~ 8.39e-1 1      0.83885  ns       Wilco~
    ##  9 PinuStro Value Burned Sites ~ Landsc~ 1.90e-3 0.019  0.00190  **       Wilco~
    ## 10 QuerRubr Value Burned Sites ~ Landsc~ 7.85e-1 1      0.78539  ns       Wilco~
    ## 11 QuerFalc Value Burned Sites ~ Landsc~ 7.91e-1 1      0.79069  ns       Wilco~

``` r
#(OneSpecies$Value)
```

``` r
p1 <- ggplot(Stagger1, aes(x=Species, y=Value*100, fill=Fire)) +
  ggtitle("Immediately following fire") +
  stat_compare_means(method = "wilcox.test",label = "p.format",size =6 )+# fill=name allow to automatically dedicate a color for each group
  geom_boxplot(adjust=2, width=.7,outlier.shape = NA)+
  theme_classic(base_size = 24)+
  theme(legend.position="none")+
  xlab("") +
  ylab("Percent total biomass")
```

    ## Warning: Ignoring unknown parameters: adjust

``` r
# p2 <- ggplot(Stagger5, aes(x=Species, y=Value, fill=Fire)) + # fill=name allow to automatically dedicate a color for each group
#   ggtitle("Relative biomass 5 Years following fire") +
#   stat_compare_means(method = "wilcox.test",label = "p.format",size =6 )+
#   # fill=name allow to automatically dedicate a color for each group
#   geom_boxplot(adjust=2, width=.7,outlier.shape = NA)+
#   theme_classic(base_size = 24)+
#   theme(legend.position="none")+
#   xlab("") +
#   ylab("Portion of total biomass")
  
p3 <- ggplot(Stagger10, aes(x=Species, y=Value*100, fill=Fire)) + # fill=name allow to automatically dedicate a color for each group
  ggtitle("10 Years following fire") +
  stat_compare_means(method = "wilcox.test",label = "p.format",size =6 )+# fill=name allow to automatically dedicate a color for each group
  geom_boxplot(adjust=2, width=.7,outlier.shape = NA)+
  theme_classic(base_size = 24)+
  theme(legend.position="none")+
  xlab("Species") +
  ylab("Percent total biomass") 
```

    ## Warning: Ignoring unknown parameters: adjust

``` r
p4<-g_legend(ggplot(Stagger1, aes(x=Species, y=Value, fill=Fire)) +
  ggtitle("Relative biomass immediately following fire") +
  stat_compare_means(method = "wilcox.test",label = "p.format",size =6 )+# fill=name allow to automatically dedicate a color for each group
  geom_boxplot(adjust=2, width=.7,outlier.shape = NA)+
  theme_classic(base_size = 24)+
   theme(legend.title = element_blank(),axis.text=element_text(size=18),
        axis.title=element_text(size=18,face="bold")))
```

    ## Warning: Ignoring unknown parameters: adjust

``` r
grid.arrange(p1,p3,p4,nrow =3,top = textGrob("Trees under 20 years old",gp=gpar(fontsize=35,font=1)),heights=c(6,6,1))
```

![](Outputs_doc_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

``` r
# Folder1<-"E:/DM_Runs_222/"
# Csv<-read.csv(paste0(Folder1,"GA_Model_D1/scrapple-events-log.csv"))
# CohortsK<-Csv$CohortsKilled
# CohortsAv<-Csv$AvailableCohorts
CalculatePerc<-function(Csv){  
  CohortK_s<-sum(Csv$CohortsKilled)  
  CohortsAv_s<-sum(Csv$AvailableCohorts)
  #colnames(Csv)
  Perc<-CohortK_s/CohortsAv_s
  return(Perc)
}


Folder1<-"D:/Sapps_DM_paper/DM_runs_4_4/"
Csv<-read.csv(paste0(Folder1,"GA_Model_D2_R2/scrapple-events-log.csv"))
Delayed_1<-CalculatePerc(Csv)
D1_bio<-sum(Csv$TotalBiomassMortality)*(62500/(24*1e6))
Csv<-read.csv(paste0(Folder1,"GA_Model_D2_R2/scrapple-events-log.csv"))
Delayed_2<-CalculatePerc(Csv)
D2_bio<-sum(Csv$TotalBiomassMortality)*(62500/(24*1e6))
Csv<-read.csv(paste0(Folder1,"GA_Model_D3_R2/scrapple-events-log.csv"))
Delayed_3<-CalculatePerc(Csv)
D3_bio<-sum(Csv$TotalBiomassMortality)*(62500/(24*1e6))

Csv<-read.csv(paste0(Folder1,"GA_Model_D4_R2/scrapple-events-log.csv"))
Delayed_4<-CalculatePerc(Csv)
D4_bio<-sum(Csv$TotalBiomassMortality)*(62500/(24*1e6))
Csv<-read.csv(paste0(Folder1,"GA_Model_D5_R2/scrapple-events-log.csv"))
Delayed_5<-CalculatePerc(Csv)
D5_bio<-sum(Csv$TotalBiomassMortality)*(62500/(24*1e6))




Csv<-read.csv(paste0(Folder1,"GA_Model_ND1_R2/scrapple-events-log.csv"))
ND_1<-CalculatePerc(Csv)
ND1_bio<-sum(Csv$TotalBiomassMortality)*(62500/(24*1e6))
Csv<-read.csv(paste0(Folder1,"GA_Model_ND2_R2/scrapple-events-log.csv"))
ND_2<-CalculatePerc(Csv)
ND2_bio<-sum(Csv$TotalBiomassMortality)*(62500/(24*1e6))
Csv<-read.csv(paste0(Folder1,"GA_Model_ND3_R2/scrapple-events-log.csv"))
ND_3<-CalculatePerc(Csv)
ND3_bio<-sum(Csv$TotalBiomassMortality)*(62500/(24*1e6))
Csv<-read.csv(paste0(Folder1,"GA_Model_ND4_R2/scrapple-events-log.csv"))
ND_4<-CalculatePerc(Csv)
ND4_bio<-sum(Csv$TotalBiomassMortality)*(62500/(24*1e6))
Csv<-read.csv(paste0(Folder1,"GA_Model_ND5_R2/scrapple-events-log.csv"))
ND_5<-CalculatePerc(Csv)
ND5_bio<-sum(Csv$TotalBiomassMortality)*(62500/(24*1e6))

Runs_Bio<-data.frame(Run=c(rep("Delayed Mortality",5),rep("Immediate Mortality",5)),
                     Value=c(D1_bio,D2_bio,D3_bio,D4_bio,D5_bio,
                             ND1_bio,ND2_bio,ND3_bio,ND4_bio,ND5_bio))
 


Runs<-data.frame(Run=c(rep("Delayed Mortality",5),rep("Immediate Mortality",5)),
                 Value=c(Delayed_1,Delayed_2,Delayed_3,Delayed_4,Delayed_5,
                         ND_1,ND_2,ND_3,ND_4,ND_5))

#head(data)
min(Runs$Value[Runs$Run=="Delayed Mortality"])
```

    ## [1] 0.3775767

``` r
median(Runs$Value[Runs$Run=="Delayed Mortality"])
```

    ## [1] 0.3979959

``` r
max(Runs$Value[Runs$Run=="Delayed Mortality"])
```

    ## [1] 0.403935

``` r
min(Runs$Value[Runs$Run=="Immediate Mortality"])
```

    ## [1] 0.2238418

``` r
median(Runs$Value[Runs$Run=="Immediate Mortality"])
```

    ## [1] 0.2423879

``` r
max(Runs$Value[Runs$Run=="Immediate Mortality"])
```

    ## [1] 0.2457687

``` r
p1 <- ggplot(Runs_Bio, aes(x=Run, y=Value)) + # fill=name allow to automatically dedicate a color for each group
  geom_boxplot()+
  xlab("") +
  ggtitle("A )") +
  ylab("Biomass removed (Mg per year)") +
  theme_classic(base_size=16)


quantile(Runs_Bio$Value[Runs_Bio$Run=="Delayed Mortality"],.05)
```

    ##     5% 
    ## 212199

``` r
median(Runs_Bio$Value[Runs_Bio$Run=="Delayed Mortality"])
```

    ## [1] 212610.9

``` r
quantile(Runs_Bio$Value[Runs_Bio$Run=="Delayed Mortality"],.95)
```

    ##      95% 
    ## 263299.5

``` r
quantile(Runs_Bio$Value[Runs_Bio$Run=="Immediate Mortality"],.05)
```

    ##       5% 
    ## 107067.8

``` r
median(Runs_Bio$Value[Runs_Bio$Run=="Immediate Mortality"])
```

    ## [1] 132722.2

``` r
quantile(Runs_Bio$Value[Runs_Bio$Run=="Immediate Mortality"],.95)
```

    ##      95% 
    ## 163196.7

``` r
p2 <- ggplot(Runs, aes(x=Run, y=Value)) + # fill=name allow to automatically dedicate a color for each group
  geom_boxplot()+
  xlab("") +
  ylab("Percent mortality per simulation") +
  ggtitle("B )") +
  theme_classic(base_size=16)



#min(Runs$Value[Runs$Run=="Delayed"])
quantile(Runs$Value[Runs$Run=="Delayed Mortality"],.05)
```

    ##        5% 
    ## 0.3816605

``` r
median(Runs$Value[Runs$Run=="Delayed Mortality"])
```

    ## [1] 0.3979959

``` r
#max(Runs$Value[Runs$Run=="Delayed Mortality"])
quantile(Runs$Value[Runs$Run=="Delayed Mortality"],.95)
```

    ##      95% 
    ## 0.403034

``` r
#min(Runs$Value[Runs$Run=="Immediate Mortality"])
quantile(Runs$Value[Runs$Run=="Immediate Mortality"],.05)
```

    ##       5% 
    ## 0.224135

``` r
median(Runs$Value[Runs$Run=="Immediate Mortality"])
```

    ## [1] 0.2423879

``` r
quantile(Runs$Value[Runs$Run=="Immediate Mortality"],.95)
```

    ##       95% 
    ## 0.2454835

``` r
grid.arrange(p1,p2,nrow=1)
```

![](Outputs_doc_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

``` r
g <- arrangeGrob(p1,p2,nrow=1)
ggsave(file="Biomass_ForPub.png",g,height = 7.0, width = 10.0, dpi = 200)
getwd()
```

    ## [1] "D:/Sapps_DM_paper"

# Standing Biomass

``` r
#### Standing biomass. 

Biomass<-"E:/DM_runs_4_4/GA_Model_D1_R2/Biomass/"
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D1_R2/"
#t<-1
#stragger<-0
#AllStack<-stack(paste0(Biomass,list.files(Biomass,pattern = paste0("\\-",(t+stagger),".img$"))))
Firemaps<-function(LANDIS_Inputs,t){
  Fire1<-raster(paste0(LANDIS_Inputs,'scrapple-fire/ignition-type-',t,'.img'))
  FireOn<-Fire1
  FireOn[FireOn<=1]<-NA
  FireOn[FireOn>1]<-1
  plot(FireOn,ylim=c(0,400),xlim=c(0,400))
  FireOff<-Fire1
  FireOff[FireOff!=1]<-NA
  plot(FireOff)
  return(list(FireOn,FireOff))
}
Fire2<-Firemaps(LANDIS_Inputs,2)
FireOn<-Fire2[[1]]
FireOff<-Fire2[[2]]

# AllStack<-stack(paste0(Biomass,list.files(Biomass,pattern = paste0("\\-",(t+10),".img$"))))
#Bio_sum<-calc(AllStack,sum)


CalculateFrame<-function(Species,perc,FireOn,FireOff){
  #perc<-AcerRubr_perc

  One_percOn<-mean((as.vector(perc*FireOn)),na.rm=T)
  One_percOff<-mean((as.vector(perc*FireOff)),na.rm=T)
  OneSpecies<-data.frame(Species=c(Species,Species),
                         Fire=c(rep("Fire",length(One_percOn)),
                                rep("Landscape",length(One_percOff))),
                         Value=c(One_percOn,One_percOff))
  OneSpecies<-na.omit(OneSpecies)
  return(OneSpecies)
}

Sumstack2<-function(Biomass,t,stagger,FireOn,FireOff){

  AllStack<-stack(paste0(Biomass,list.files(Biomass,pattern = paste0("\\-",(t+stagger),".img$")))[c(FALSE,TRUE)])#
  LiriTuli_perc<-AllStack[[which(c(grepl("LiriTuli", names(AllStack), fixed = TRUE)))]]
  LiriTuliFrame<-CalculateFrame("LiriTuli",LiriTuli_perc,FireOn,FireOff)
  AcerRubr_perc<-AllStack[[which(c(grepl("AcerRubr", names(AllStack), fixed = TRUE)))]]
  AcerFrame<-CalculateFrame("AcerRubr",AcerRubr_perc,FireOn,FireOff)
  NyssSylv_perc<-AllStack[[which(c(grepl("NyssSylv", names(AllStack), fixed = TRUE)))]]
  NyssSylvFrame<-CalculateFrame("NyssSylv",NyssSylv_perc,FireOn,FireOff)
  QuerAlba_perc<-AllStack[[which(c(grepl("QuerAlba", names(AllStack), fixed = TRUE)))]]
  QuerAlbaFrame<-CalculateFrame("QuerAlba",QuerAlba_perc,FireOn,FireOff)
  PinuTaed_perc<-AllStack[[which(c(grepl("PinuTaed", names(AllStack), fixed = TRUE)))]]
  PinuTaedFrame<-CalculateFrame("PinuTaed",PinuTaed_perc,FireOn,FireOff)
  QuerPrin_perc<-AllStack[[which(c(grepl("QuerPrin", names(AllStack), fixed = TRUE)))]]
  QuerPrinFrame<-CalculateFrame("QuerPrin",QuerPrin_perc,FireOn,FireOff)
  QuerCocc_perc<-AllStack[[which(c(grepl("QuerCocc", names(AllStack), fixed = TRUE)))]]
  QuerCoccFrame<-CalculateFrame("QuerCocc",QuerCocc_perc,FireOn,FireOff)
  PinuVirg_perc<-AllStack[[which(c(grepl("PinuVirg", names(AllStack), fixed = TRUE)))]]
  PinuVirgFrame<-CalculateFrame("PinuVirg",PinuVirg_perc,FireOn,FireOff)
  PinuStro_perc<-AllStack[[which(c(grepl("PinuStro", names(AllStack), fixed = TRUE)))]]
  PinuStroFrame<-CalculateFrame("PinuStro",PinuStro_perc,FireOn,FireOff)
  QuerRubr_perc<-AllStack[[which(c(grepl("QuerRubr", names(AllStack), fixed = TRUE)))]]
  QuerRubrFrame<-CalculateFrame("QuerRubr",QuerRubr_perc,FireOn,FireOff)
  QuerFalc_perc<-AllStack[[which(c(grepl("QuerFalc", names(AllStack), fixed = TRUE)))]]
  QuerFalcFrame<-CalculateFrame("QuerFalc",QuerFalc_perc,FireOn,FireOff)
  AllFrameThree<-rbind(LiriTuliFrame,AcerFrame,NyssSylvFrame,
                       QuerAlbaFrame,PinuTaedFrame,QuerPrinFrame,QuerCoccFrame,
                       PinuVirgFrame,PinuStroFrame,QuerRubrFrame,QuerFalcFrame)
  AllFrameThree<-cbind(data.frame(Time=t,Stagger=stagger),AllFrameThree)
  return(AllFrameThree)}

Stagger<-function(LANDIS_Inputs,Biomass,Stagger){
  Fire2<-Firemaps(LANDIS_Inputs,2)
  FireOn<-Fire2[[1]]
  FireOff<-Fire2[[2]]
  Year2<-Sumstack2(Biomass,2,Stagger,FireOn,FireOff)
  
  Fire3<-Firemaps(LANDIS_Inputs,3)
  FireOn<-Fire3[[1]]
  FireOff<-Fire3[[2]]
  Year3<-Sumstack2(Biomass,3,Stagger,FireOn,FireOff)
  
  Fire4<-Firemaps(LANDIS_Inputs,4)
  FireOn<-Fire4[[1]]
  FireOff<-Fire4[[2]]
  Year4<-Sumstack2(Biomass,4,Stagger,FireOn,FireOff)
  
  Fire5<-Firemaps(LANDIS_Inputs,5)
  FireOn<-Fire5[[1]]
  FireOff<-Fire5[[2]]
  Year5<-Sumstack2(Biomass,5,Stagger,FireOn,FireOff)
  
  Fire6<-Firemaps(LANDIS_Inputs,6)
  FireOn<-Fire6[[1]]
  FireOff<-Fire6[[2]]
  Year6<-Sumstack2(Biomass,6,Stagger,FireOn,FireOff)
  
  Fire7<-Firemaps(LANDIS_Inputs,7)
  FireOn<-Fire7[[1]]
  FireOff<-Fire7[[2]]
  Year7<-Sumstack2(Biomass,7,Stagger,FireOn,FireOff)
  
  Fire8<-Firemaps(LANDIS_Inputs,8)
  FireOn<-Fire8[[1]]
  FireOff<-Fire8[[2]]
  Year8<-Sumstack2(Biomass,8,Stagger,FireOn,FireOff)
  
  Fire9<-Firemaps(LANDIS_Inputs,9)
  FireOn<-Fire9[[1]]
  FireOff<-Fire9[[2]]
  Year9<-Sumstack2(Biomass,9,Stagger,FireOn,FireOff)
  
  Fire10<-Firemaps(LANDIS_Inputs,10)
  FireOn<-Fire9[[1]]
  FireOff<-Fire9[[2]]
  Year10<-Sumstack2(Biomass,10,Stagger,FireOn,FireOff)
  
  Fire11<-Firemaps(LANDIS_Inputs,11)
  FireOn<-Fire11[[1]]
  FireOff<-Fire11[[2]]
  Year11<-Sumstack2(Biomass,11,Stagger,FireOn,FireOff)
  
  Stagger0<-rbind(Year2,Year3,Year4,Year5,Year6,Year7,Year8,
                  Year9,Year10,Year11)
  return(Stagger0)
}
```

``` r
###Testings
# 
# Fire2<-Firemaps(LANDIS_Inputs,3)
# FireOn<-Fire2[[1]]
# FireOff<-Fire2[[2]]
# Year2a<-Sumstack2(Biomass,3,0,FireOn,FireOff)
# sum(Year2a$Value)
# Year2<-Sumstack2(Biomass,3,-1,FireOn,FireOff)
# sum(Year2$Value)



#### Stagger 0

#### Year_of
Biomass<-"E:/DM_runs_4_4/GA_Model_D1_R2/Biomass/"
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D1_R2/"
rep1<-Stagger(LANDIS_Inputs = LANDIS_Inputs,Biomass,0)
Biomass<-"E:/DM_runs_4_4/GA_Model_D2_R2/Biomass/"
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D2_R2/"
rep2<-Stagger(LANDIS_Inputs = LANDIS_Inputs,Biomass,0)
Biomass<-"E:/DM_runs_4_4/GA_Model_D3_R2/Biomass/"
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D3_R2/"
rep3<-Stagger(LANDIS_Inputs = LANDIS_Inputs,Biomass,0)
Biomass<-"E:/DM_runs_4_4/GA_Model_D4_R2/Biomass/"
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D4_R2/"
rep4<-Stagger(LANDIS_Inputs = LANDIS_Inputs,Biomass,0)
Biomass<-"E:/DM_runs_4_4/GA_Model_D5_R2/Biomass/"
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D5_R2/"
rep5<-Stagger(LANDIS_Inputs = LANDIS_Inputs,Biomass,0)




StaggerAfter<-rbind(rep1,rep2,rep3,rep4,rep5)
#### Stagger 1
Biomass<-"E:/DM_runs_4_4/GA_Model_D1_R2/Biomass/"
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D1_R2/"
rep1<-Stagger(LANDIS_Inputs = LANDIS_Inputs,Biomass,-1)
Biomass<-"E:/DM_runs_4_4/GA_Model_D2_R2/Biomass/"
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D2_R2/"
rep2<-Stagger(LANDIS_Inputs = LANDIS_Inputs,Biomass,-1)
Biomass<-"E:/DM_runs_4_4/GA_Model_D3_R2/Biomass/"
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D3_R2/"
rep3<-Stagger(LANDIS_Inputs = LANDIS_Inputs,Biomass,-1)
Biomass<-"E:/DM_runs_4_4/GA_Model_D4_R2/Biomass/"
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D4_R2/"
rep4<-Stagger(LANDIS_Inputs = LANDIS_Inputs,Biomass,-1)
Biomass<-"E:/DM_runs_4_4/GA_Model_D5_R2/Biomass/"
LANDIS_Inputs<-"E:/DM_runs_4_4/GA_Model_D5_R2/"
rep5<-Stagger(LANDIS_Inputs = LANDIS_Inputs,Biomass,-1)
Staggerbefore<-rbind(rep1,rep2,rep3,rep4,rep5)


#write.csv(StaggerAfter,"StaggerAfter_428.csv")
#write.csv(Staggerbefore,"Staggerbefore_428.csv")
```

``` r
StaggerAfter<-read.csv('StaggerAfter_428.csv')
Staggerbefore<-read.csv('StaggerBefore_428.csv')
StaggerAfter$Fire[StaggerAfter$Fire=="Fire"]<-"Burned Sites (Delayed Mortality)"
#Stagger0$Fire[Stagger0$Fire=="NoFire"]<-"Unburned Landscape"

Fire<-StaggerAfter[StaggerAfter$Fire=="Burned Sites (Delayed Mortality)",]
#Fire0<-Staggeneg1[Staggeneg1$Fire=="Fire",]

Fire$Prior<-Staggerbefore$Value[Staggerbefore$Fire=="Fire"]
#print(Fire)

(sum(Fire$Value)-sum(Fire$Prior))/sum(Fire$Prior)
```

    ## [1] -0.1770432

``` r
Fire$Delta<-(Fire$Value-Fire$Prior)/Fire$Prior

Bars<-aggregate(x=list(Before=Fire$Prior,After=Fire$Value),by=list(Fire$Species),FUN=sum)
Bars$Dif<-(Bars$After-Bars$Before)/Bars$Before
#Bars

### Comparing total
#sum(Fire$Prior)

par(mar=c(8,8,8,8),bg="white")
barplot(Bars$Dif[order(Bars$Dif)]*100,names.arg=Bars$Group.1[order(Bars$Dif)],
        las=1,col="grey",xlab="Mean percent decrease biomass",horiz=T)
```

![](Outputs_doc_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

``` r
Before<-Bars[1:2]
Before$Model<-"Before"
After<-Bars[c(1,3)]
After$Model<-"After"
colnames(After)<-colnames(Before)
Data2<-rbind(Before,After)



g_legend<-function(a.gplot){
  tmp <- ggplot_gtable(ggplot_build(a.gplot))
  leg <- which(sapply(tmp$grobs, function(x) x$name) == "guide-box")
  legend <- tmp$grobs[[leg]]
  return(legend)}


  p1 <- ggplot(Data2, aes(x=Group.1, y=Before,fill=Model)) +
  ggtitle("mean biomass before/after fire") +
  #stat_compare_means(method = "wilcox.test",label = "p.format",size =6 )+# fill=name allow to automatically dedicate a color for each group
  geom_bar(stat="identity", position=position_dodge())+
  theme_classic(base_size =16 )+
  theme(legend.position="none")+
  xlab("") +
  ylab("Biomass g/m2")
p1
```

![](Outputs_doc_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->
