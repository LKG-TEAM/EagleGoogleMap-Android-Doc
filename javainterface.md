## 相关接口
 
 地图api相关接口主要有两个`IMapDelegate`和`OnMapOperationListener`
 
## IMapDelegate的使用
 
 该接口为地图相关操作类，地图相关类必须实现这个，已实现`GoogleMapDelegate`  后续可能有BaiduMap等
 
 该接口需要外部使用的时候指明使用的是哪个地图
 >如果外部通过继承`BaseMapActivity`来使用地图，则需要实现`createMapDelegate`来创建实际的地图操作类
 
 ```java
  @Override
    public IMapDelegate createMapDelegate() {
        return new GoogleMapDelegate();//使用谷歌地图
    }
 ```
 
 >如果外部activity通过`BaseMapFragment`来使用地图，则需要在`BaseMapFragment.newInstance`中指明地图类型
 
 ```java
 BaseMapFragment.newInstance(MapSelector.MAP_TYPE.GOOGLE_MAP.ordinal());//使用谷歌地图
 ```
 
 
 >外部使用`IMapDelegate`相关api
 
 继承`BaseMapActivity`的activity中使用
 
 ```java
   getMapDelegate()
 ```
 
 使用`BaseMapFragment`的activity中使用
 
 ```java
   mMapFragment.getMapDelegate()
 ```
 
 ## IMapDelegate方法
 
 > 设置地图操作监听
 
```java
 /**
     * 设置地图操作监听
     *
     * @param listener
     */
    void setOnMapOperationListener(OnMapOperationListener listener);
```
 
 > BaseMapActivity中 初始化地图

```java
    /**
     * 在activity中使用的时候必须调用这个来初始化
     *
     * @param baseMapActivity
     */
    void initInActivity(@NonNull BaseMapActivity baseMapActivity);
```


 > BaseMapFragment中 初始化地图


```java

    /**
     * 在fragment中使用的时候必须调用这个来初始化
     *
     * @param fragment
     */
    void initInFragment(@NonNull BaseMapFragment fragment);
```

 > 更新地图设置

```java

    /**
     * 更新地图设置
     *
     * @param permissionGranted
     */
    void updateLocationUI(boolean permissionGranted);
```


 > 移动地图中心点

```java

    /**
     * 移动地图中心点
     *
     * @param location
     */
    void moveMapCenterTo(Location location);
```


 > 画地点

```java

    /**
     * 画地点
     *
     * @param location
     */
    void drawMarker(Location location);
```



 > 画圆
 
```java

    /**
     * 画圆
     *
     * @param centerLoc 中心点location
     * @param radius    半径  单位m
     */
    void drawCircle(Location centerLoc, int radius);
```


 > 获取两个地点间的路径

```java

    /**
     * 获取两个地点间的路径
     *
     * @param origin
     * @param dest
     */
    void getDirections(Location origin, Location dest);
```

> 打开地图导航

```java

    /**
     * 打开地图导航
     *
     * @param lat
     * @param lon
     * @param address
     */
    void navigateByMap(String lat, String lon, String address);
```

## OnMapOperationListener

地图相关操作功能，使用谷歌地图的activity必须实现该类

 > 地图初始化完成回调
 
 地图初始化完成后会调用该回调，地图相关操作必须在回调之后才能使用，否则可能会有些问题

```java
 /**
     * 地图准备好了才可以开始操作
     */
    void onMapReady();
```



 > 地点改变回调
 
 地点改变回调，这里可以针对改变后的地点进行一些操作
 
```java
   
    /**
     * 地点改变
     * @param location
     */
    void onLocationChange(Location location);
```

 

 
 