## Vue ElementUi 省市区三级联动

初衷: 网上找了好几个数据源，数据都不是最新的(比如没有深圳市龙华区，深圳市坪山区等)，后面自己又重新找了一个数据源，整理成适用于vue elementUI el-cascader组件的数据格式

原始数据 https://github.com/youzan/vant/blob/dev/src/area/demo/area.js

数据整理方法

```

import ads from "./ads";  // 引入原始数据 

/**
 * 数据源 https://github.com/youzan/vant/blob/dev/src/area/demo/area.js
 * @type {({children: [{children, label: string, value: string}], label: string, value: string}|{children: [{children, label: string, value: string}], label: string, value: string}|{children: ({children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string}|{children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string}|{children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string}|{children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string}|{children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string})[], label: string, value: string}|{children: ({children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string}|{children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string}|{children: [{label: string, value: string}, {label: string, value: string}, {label: string, value: string}, {label: string, value: string}, {label: string, value: string}], label: string, value: string}|{children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string}|{children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string})[], label: string, value: string}|{children: ({children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string}|{children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string}|{children: [{label: string, value: string}, {label: string, value: string}, {label: string, value: string}], label: string, value: string}|{children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string}|{children: ({label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string}|{label: string, value: string})[], label: string, value: string})[], label: string, value: string})[]}
 */
export const ADDRESS = () => {
  let address = [];
  let province = [];
  let city = [];
  let county = [];
  // 遍历省份
  for (let [key,val] of Object.entries(ads.province_list)) {
    province.push({
      label:val,
      value:key
    })
  }

  // 遍历城市
  for (let [key,val] of Object.entries(ads.city_list)) {
    city.push({
      label:val,
      value:key
    })
  }

  // 遍历区域
  for (let [key,val] of Object.entries(ads.county_list)) {
    county.push({
      label:val,
      value:key
    })
  }

  // 组合数据
  province.forEach((provinceItem)=>{
    let province_code = provinceItem.value.substr(0,2);
    let children = [];
    city.forEach((cityItem)=>{
      let city_code = cityItem.value.substr(0,2);
      let city_code_sub = cityItem.value.substr(0,4);
      let county_children = [];
      county.forEach((countyItem)=>{
        let county_code = countyItem.value.substr(0,4);
        if (city_code_sub == county_code) {
          county_children.push(countyItem)
        }
      })
      cityItem.children = county_children;
      if (province_code == city_code) {
        children.push(cityItem)
      }
    })
    provinceItem.children = children;
    address.push(provinceItem);
  })

  return address
}
````

数据整理成elementUI 的数据格式
```
export const adsTree = [{
  "label": "北京市",
  "value": "110000",
  "children": [{
    "label": "市辖区",
    "value": "110100",
    "children": [{"label": "东城区", "value": "110101"}, {"label": "西城区", "value": "110102"}, {
      "label": "朝阳区",
      "value": "110105"
    }, {"label": "丰台区", "value": "110106"}, {"label": "石景山区", "value": "110107"}, {
      "label": "海淀区",
      "value": "110108"
    }, {"label": "门头沟区", "value": "110109"}, {"label": "房山区", "value": "110111"}, {
      "label": "通州区",
      "value": "110112"
    }, {"label": "顺义区", "value": "110113"}, {"label": "昌平区", "value": "110114"}, {
      "label": "大兴区",
      "value": "110115"
    }, {"label": "怀柔区", "value": "110116"}, {"label": "平谷区", "value": "110117"}, {
      "label": "密云区",
      "value": "110118"
    }, {"label": "延庆区", "value": "110119"}]
  }]
    ...... 
}, 
}]
```

### 项目中使用
```
import {adsTree} from "../../assets/js/adsTree";

export default {
   data(){
    return {
      options:adsTree
    }
  }
}

// template
<el-cascader style="width: 100%" 
                            v-model="form.selectedOptions"
                             :options="options" :props="{value:'label'}"
                             clearable>
</el-cascader>

```


