# iveiw Table 中checkbox填坑
```html
    <Table :columns="columns" :data="dataList" ref="selection" @on-row-click="handleClickRow"></Table>
```
# 通过给columns数据设置一项指定`type: 'selection'`即可自动开启多选功能
```js
{
    data(){
        return {
            columns:[
                {
                    title: '',
                    width: 50,
                    type: 'selection'
                },
                ……
            ]
        }
    }
}
```
## iview中的table组件点击一行中的任意一点选中本行，`toggleSelect`官方文档没有说明
```js
    handleClickRow(row, index){
        this.$refs.selection.toggleSelect(index)
    }
```
## 默认选中当前项
 给dataList 项中添加特殊属性 `_checked:true `
 ```js
{
    data(){
        return {
            dataList:[
                ……
                {
                  _checked:true 
                }
            ]
        }
    }
}
```
## 默认禁止选择当前项

 给dataList 项中添加特殊属性 `_disabled:true `
 ```js
{
    data(){
        return {
            dataList:[
                ……
                {
                  _disabled:true 
                }
            ]
        }
    }
}
```
### iview坑：
- 给data设置_check的属性。 _checked属性会影响checkbox的选中状态。但是checkbox的选中状态不会影响_check 属性
-  iview 官方文档说：
@on-selection-change，只要选中项发生变化时就会触发，返回值为 selection，已选项。

- 实现效果并不是这样的，而是：
用程序设置_checked=true后，并不会触发该事件。也不会触发on-select-cancel 和on-select。只有通过鼠标再次点击checkbox，才会触发上述三项事件。虽然最后参数中的selection是正确的。

- 用程序切换某一行的选中状态，需要调用函数this.$refs.xxx.toggleSelect(i)，调用该函数后，会触发on-select-cancel 、on-select、on-selection-change事件。但是在on-selection-change事件中得到的selection数据只有两条，第一下点击的元素和最后一下点击的元素。
- 所以可以推理iview的执行过程如下：触发选中事件时，先调用on-select, 再调用on-selection-change。