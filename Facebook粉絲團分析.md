##讀取蔡英文粉絲團資料
```{r}
if (!require('Rfacebook')){
  install.packages("Rfacebook")
  library(Rfacebook)
}

token<-'CAACEdEose0cBAPZAxGRol9DFzsTbmk2CVqSZAGnnFPlhnQMgzZA2ChvW2FCvch29YVp8jM3ZBAif6OlZAcRFVFZAy8uZC4HggS9NnBIStkiWlZB5bGPEUeoedX7p6ppoZCJ96OcZCHZBsbXR5RQnqKsMbBPOn6poWHwCQaHbYVKzF2qQtv7R3XKkxxlZCwz3tAFZANKN0ediJXoY1IHt0L0rTrM2E'
totalPage<-NULL
lastDate<-Sys.Date()
DateVectorStr<-as.character(seq(as.Date("2016-01-01"),lastDate,by="5 days"))
for(i in 1:(length(DateVectorStr)-1)){
  tempPage<-getPage("tsaiingwen", token,
                    since = DateVectorStr[i],until = DateVectorStr[i+1])
  totalPage<-rbind(totalPage,tempPage)
}
nrow(totalPage)
```
2016/01/01至2016/04/07蔡英文粉絲團一共有206篇文章
##每日發文數分析
```{r}
totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                 format = "%Y-%m-%dT%H:%M:%S+0000", 
                                 tz = "GMT") #2016-01-16T15:05:36+0000
totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                            tz = "Asia/Taipei") #2016-01-16
totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
PostCount<-aggregate(id~dateTPE,totalPage,length)
library(knitr)
kable(head(PostCount[order(PostCount$id,decreasing = T),]))
```
2016/01/15（週五）的發文數最多，一共有八篇，2016/01/11和2016/01/14居次，有可能是因為隔天就是投票日了，所以蔡英文才會多發幾篇文拉票。
##每日likes數分析
```{r}
totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                 format = "%Y-%m-%dT%H:%M:%S+0000", 
                                 tz = "GMT") #2016-01-16T15:05:36+0000
totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                            tz = "Asia/Taipei") #2016-01-16
totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
PostCount<-aggregate(likes_count~dateTPE,totalPage,mean)
library(knitr)
kable(head(PostCount[order(PostCount$likes_count,decreasing = T),]))
```
2016/01/17（週日）的按讚數最多，一共有26萬多人，2016/01/16和2016/3/29居次，由此可知自從她當選總統後，人氣因此而暴增。

##每日comments數分析
```{r}
totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                 format = "%Y-%m-%dT%H:%M:%S+0000", 
                                 tz = "GMT") #2016-01-16T15:05:36+0000
totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                            tz = "Asia/Taipei") #2016-01-16
totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
CommentCount<-aggregate(comments_count~dateTPE,totalPage,mean)
library(knitr)
kable(head(CommentCount[order(CommentCount$comments_count,decreasing = T),]))
```
2016/01/20（週三）的留言數最多，一共有2萬七千多筆，2016/01/21和2016/01/17居次，由此表可推測自從當選總統後，蔡英文發文的留言評論突然暴增。
##每日shares數分析
```{r}
totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                 format = "%Y-%m-%dT%H:%M:%S+0000", 
                                 tz = "GMT") #2016-01-16T15:05:36+0000
totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                            tz = "Asia/Taipei") #2016-01-16
totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
ShareCount<-aggregate(shares_count~dateTPE,totalPage,mean)
library(knitr)
kable(head(ShareCount[order(ShareCount$shares_count,decreasing = T),]))
```
2016/03/29（週四）的分享數最多，一共有1千八百多筆，2016/01/16和2016/01/17居次，主要是因為3月29號當天，小英PO了一篇緬懷小燈泡斷頭慘案的文章，而有不少的回響。