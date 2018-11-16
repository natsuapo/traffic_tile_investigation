# Comparing AutoNavi and Bing Map for traffic volume tile acquisition

## Solution investigation of AutoNavi map (Amap)

### Traffic tile representation in Amap:
- Two kinds of tiles: real-time and predicted one(based on history I guess). 
1. For real-time data, the map is refreshed by the callback functions: 
```
https://vdata.amap.com/traffic?t=16,53695,28462&w=1542357596854&callback=jsonp_906400_
```
- The time (w I guess) and callback parameter changes in the url while the tile itself is refreshed by the function. 
- There is no tile image for the traffic in Amap. Which make it difficult to acquire the data. 
 
2. For predicted result, it is represented the iamge with the following url example:
``` 
traffic?type=2&day=5&hh=14&mm=0&x=53351&y=28598&z=16  
```

- Parameters: type, day, hh, mm, x, y, z
- type: unknown parameter, seems to be the file format. 
- hh: hour, mm: minute, day: (day of a week) ,x: x index, y: y index, z: zoom size.
- It is rather easy to get the predicted results. 

### Problems of using Amap: 
- Difficult to get the realtime traffic tile. 
  -  Of course we can write some script to automatcally download the screen, but it might need more works for data preprocessing. 
- History data seems to be available, but it just a one week estimated result but not the true value. 


## Solution investigation of Bing map: 

### Traffic tile representation in Bing map: 
- Bing map seems to only have real time data, but the traffic information is represented by the tiles. 
```
https://t1-traffic.tiles.virtualearth.net/comp/ch/1321222320323123?it=Z,TF&L&n=z&key=AiuRTvFmAH1iHs-VO8Xn4R2AEnBaXiIPq25HNYCoceLUzducYTOCSfcOlWM1uIy1&c4w=1
```
- Note that key is required here. However, removing the key we can also get the result? (I guess the time of request will be limited with no keys.)

- `1321222320323123` represent the tile code. We can easily get the network tile with the same tile:

```
https://t.ssl.ak.dynamic.tiles.virtualearth.net/comp/ch/1321222320323123?mkt=zh-CN&it=G,OBX,L,LA&shading=hill&og=341&n=z&c4w=1&osm=1
```

On the other hand, for larger scale, the bit of mesh code will get larger(similar to meshcode in Japan), e.g. 1321222320323310. This is different from Amap utilizing zoom parameter. 


## Conclusion: 
- Get trajectory of Bing map seems to be more feasible than Amap. 
- The time, frequency of request limitation in Bing map need to be further investigated.








