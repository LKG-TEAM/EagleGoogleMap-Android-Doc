## 使用方式

>该模块暂时没有直接使用navigateTo可以使用的内容，外部Activity需要使用地图的话可以通过两种方式来使用地图相关Api。1.继承BaseMapActivity。2.直接使用BaseMapFragment.

!>该模块目前设计为Delegate方式来实现地图的api，可以方便的切换不同的地图来展示内容，目前仅实现了GoogleMap，其他地图看情况再接入

## 如何使用BaseMapActivity

外部activity要使用`BaseMapActivity`
首先继承`BaseMapActivity`

```java 
public class MapsActivity extends BaseMapActivity {}
```

然后 实现地图操作相关的方法

```java
 @Override
    public IMapDelegate createMapDelegate() {
        return new GoogleMapDelegate();//使用谷歌地图
    }

    @Override
    public void onMapReady() {//这里做些地图初始化成功之后的操作
    }

    @Override
    public void onLocationChange(Location location) {//这里做些Location 改变之后的操作
    }

```

!>目前仅实现了谷歌地图相关的api 所以`createMapDelegate`目前只有使用`GoogleMapDelegate`

## 如何使用BaseMapFragment

!>`BaseMapFragment`目前仅支持在`Activity`中使用，暂不支持在别的`Fragment`中使用（用也可以用，只是无法响应`OnMapOperationListener`中的事件）

`BaseMapFragment` 继承于`BaseFragment` 使用和其他fragment类似。

首先需要在使用的Activity的layout中添加控件

```xml
    <FrameLayout
        android:id="@+id/map_container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
       />

```
 
 然后在Activity中调用以下代码来加载`BaseMapFragment`
 
 ```java
  mMapFragment = BaseMapFragment.newInstance(MapSelector.MAP_TYPE.GOOGLE_MAP.ordinal());//使用谷歌地图

  FragmentManager fragmentManager = this.getSupportFragmentManager();
  FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
  fragmentTransaction.replace(R.id.map_container, mMapFragment);
  fragmentTransaction.commit();
 
 ```
 
 >MapSelector.MAP_TYPE.GOOGLE_MAP.ordinal()代表选择谷歌地图作为地图加载者
 
 最后 使用地图的Actvitiy 实现`OnMapOperationListener`即可响应地图加载相关事件
 
 ```java
public class TestMapActivity extends BaseActivity implements OnMapOperationListener {}
 ```
 