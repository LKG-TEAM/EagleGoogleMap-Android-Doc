## 说明
 
 `GoogleMapDelegate`是`IMapDelegate`的实现，与其他可能存在的地图是平级关系，可以在使用的时候指定是哪一个地图
 
## 权限相关
地图初始化完成，由于定位等功能需要权限 ，需要在`AndroidMenifest.xml`中添加权限

```xml
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

!>由于是运行时权限，所以在外部回调的`onMapReady`方法中需要请求权限，或者在加载地图之前的任何时间请求权限都可以

## GoogleMapDelegate加载 
 
!> `GoogleMapDelegate`加载过程严格按照`IMapDelegate`中的流程来进行加载，和其他的地图实现应该类似

 `GoogleMapDelegate`实现了两个接口
 
 ```java
public class GoogleMapDelegate implements IMapDelegate, OnMapReadyCallback{}
 ```
 
1.首先外部创建`GoogleMapDelegate`

BaseMapFragment中通过`MapSelector`来创建

```java
/**
     * 创建地图delegate
     *
     * @return
     */
    private IMapDelegate createMapDelegate() {
        int mapType = getArguments().getInt(KEY_MAP_TYPE);
        return MapSelector.createMapDelegate(mapType);
    }
```

继承`BaseMapActivity`的类中直接创建

```java
 @Override
    public IMapDelegate createMapDelegate() {
        return new GoogleMapDelegate();//使用谷歌地图
    }
```

2.设置地图回调

```java
        mGoogleMapDelegate.setOnMapOperationListener(this);
```

3.初始化地图

```java
public void initInActivity(@NonNull BaseMapActivity baseMapActivity)
```

或者

```java

    void initInFragment(@NonNull BaseMapFragment fragment);
```
 
 
4.地图初始化完成会回调

```java
 onMapReady(GoogleMap googleMap);
```

 
## 地图相关api

> 地图画点

```java
mMap.addMarker(new MarkerOptions()
                    .position(new LatLng(location.getLatitude(),location.getLongitude())
                    .title("Marker " + i)
                    .icon(BitmapDescriptorFactory.defaultMarker())
                    .flat(false)
                    .rotation(0));
```

 
> 画圆

```java
 mMap.addCircle(new CircleOptions()
                .center(center)
                .radius(radius)//单位m
                .strokeWidth(5)
                .strokeColor(strokeColor)
                .fillColor(fillColor)
                .clickable(true));
```

> 画路线
 
```java
  mMap.addPolyline(lineOptions);
```
 
> 获取两个地点之间的路线
 
 ```java
  LatLng originL = new LatLng(origin.getLatitude(), origin.getLongitude());

  LatLng desL = new LatLng(dest.getLatitude(), dest.getLongitude());
  new DirectionsHelper(new DirectionsHelper.OnDirectionsListener() {
    @Override
    public void onGetDirectionRoute(PolylineOptions lineOptions) {
      mMap.addPolyline(lineOptions);
    }

    @Override
    public void onFail(final String msg) {
     
   }
 }).requestDirection(originL, desL);
 ```