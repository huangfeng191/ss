# MDParam

## 代码逻辑:
1. 将MD title 行 解析出 数组参数 以及 字段参数 ,最后一个参数是 field 参数 
    1. #   param1   ?param:default? 
    2. method: disposeBefore
2. 将每个数据行的参数值解析出来
    1. ?ColSpan?br    : 保存在  MDParamO:{"数据行":{值}}
    2. 删除 ? 以后的数据
    3. method: protoToArray 
3. 处理正常的行数据，处理完后，按参数替换行数据
4. 如果没用输入值的参数 设置成默认值



``` js
 protoParam:{ // 提供全局参数, 供后续调用
        "MDTitle":[], // #  1 2 3 , 可以有多个，如果最后一个 是 ?param1?... 方式 那么 可以理解为全局的，建议用在第一个
        "MDParam":[], // title 的 最后一个值  放在 markdown title 中的定义，目前支持用(?param1) 模式 取值是 ?  ?ColSpan?br  
        "MDParamO":{},// 值 按行绑定的参数， {0:{ColSpan:"","br":"" },1:{} }
 }



```

## 应用
 <!-- 将 数组行 前添加MD 参数  -->
    protoRowTranslate: [
            {
              k: "fun",
              v: function(arr, index, self) {
                let MDParam = self.protoParam.MDTitle;
                if (MDParam.length > 0) {
                  if (MDParam[index] && MDParam[index][0]) {
                    arr[1] = MDParam[index][0] + "." + (arr[1] || "");
                  }
                }

                return arr;
              }
            }
          ]