####数字类型
```
$width:300px;
$height:200px;
$zoomValue: 2;

.div{
  width: $width;
  height: $height;
  zoom:$zoomValue;
}

```
####字符类型
####数组类型()
下标位置从1开始
```
$list:(100px,200px,'avatar',2);

.div{
  width: nth($list,1);
  height: nth($list,2);
}

```
####map
```
$map:(top:1px,left:2px,bottom:3px,right:4px);
.div{
  top:map_get($map,top);
  left: map_get($map,left);
  bottom: map_get($map,bottom);
  right: map_get($map,right);
}
```
遍历赋值方式
```
$map:(top:1px,left:2px,bottom:3px,right:4px);
.div{
  @each $key,$value in $map{
    #{$key}:#{$value}
  }
}
```
