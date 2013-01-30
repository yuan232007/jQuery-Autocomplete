#Ajax AutoComplete for jQuery

jQuery的自动完成插件,方便创建input或者boxs类自动完成功能,只依赖于jQuery.

压缩过后文件大小只有2.7K(jquery.autocomplete.js)

##API

* `$(selector).autocomplete(options);`
    * 绑定插件到所选input.(可为多个)
    * `options`: 对于插件行为的逐条定义,等于是settings.
        * `serviceUrl`: 服务器请求地址,本地数据的设置lookup.
        * `lookup`:suggestion组成的数组,suggestion可以是字符串数组或对象数组.
            * `suggestion`: 单体数据字符串或后面这种 `{ 'value': 'string', 'data': 'any'}`
		* `lookupFilter`: `function (suggestion, query, queryLowerCase) {}` 数据为本地时对数据进行过滤的函数.默认只会进行很愚蠢的局部匹配.
        * `onSelect`: `function (suggestion) {}` 选择suggestions之后的回调函数,this对象和绑定的input关联
        * `minChars`: 做少显示多少个选项. Default: `1`.
        * `maxHeight`: suggestions container 的最大像素高度. Default: `300`.
        * `deferRequestBy`: keyUp之后发起请求的间隔时间. Default: `0`.
        * `width`: Suggestions 的样式宽度, e.g.: 300. Default:`auto`,默认等于input field 的宽度.
        * `params`: 额外添加的请求参数,可选属性.
        * `formatResult`: `function (suggestion, currentValue) {}` 格式化suggestions container中的suggestion entry, 可选属性.
        * `delimiter`: 字符串或者正则,splits字符串并把得出数组的最后一项作为query进行请求.当需要分别对两个或者以上的词汇进行自动完成时很有用.
        * `zIndex`: 'z-index' 默认 `9999`.
        * `type`: XHR类型. Default: `GET`.
        * `noCache`: 设置是否缓存ajax结果的布尔值. 默认 `true`.
        * `onSearchStart`: `function (query) {}` ajax请求之前被调用,可用于前端验证,(译者注) `this` 被绑定在input上.
        * `onSearchComplete`: `function (query) {}` 请求成功之后的回调.`this` 被绑定在input上.
        * `tabDisabled`: 默认`false`. 设置为true的话,用户选择结果之后,鼠标指针会blur,默认行为是focus.
        * `paramName`: 默认`query`. query词的键名.
        * `transformResult`: `function(response){}`Ajax请求完成后用于调节response.suggestions的格式.
		* `autoSelectFirst`: 如果设置为 `true`,将默认选择第一个结果. 默认 `false`.
		* `appendTo`: suggestions container 被插入的地方.默认是`body`.可以是jQuery对徐昂, 选择器或html元素.确定已经设置`position: absolute` or `position: relative` 在被绑定元素上.

##用例

HTML:

    <input type="text" name="country" id="autocomplete"/>

Ajax 请求:

    $('#autocomplete').autocomplete({
        serviceUrl: '/autocomplete/countries',
        onSelect: function (suggestion) {
            alert('You selected: ' + suggestion.value + ', ' + suggestion.data);
        }
    });

本地数据 (no ajax):

    var countries = [
       { value: 'Andorra', data: 'AD' },
       // ...
       { value: 'Zimbabwe', data: 'ZZ' }
    ];

    $('#autocomplete').autocomplete({
        lookup: countries,
        onSelect: function (suggestion) {
            alert('You selected: ' + suggestion.value + ', ' + suggestion.data);
        }
    });

##样式

自带样式是下面这样的,特殊样式请自行修改

html:

	<div class="autocomplete-suggestions">
        <div class="autocomplete-suggestion autocomplete-selected">...</div>
        <div class="autocomplete-suggestion">...</div>
        <div class="autocomplete-suggestion">...</div>
    </div>

css:

    .autocomplete-suggestions { border: 1px solid #999; background: #FFF; overflow: auto; }
    .autocomplete-suggestion { padding: 2px 5px; white-space: nowrap; overflow: hidden; }
    .autocomplete-selected { background: #F0F0F0; }
    .autocomplete-suggestions strong { font-weight: normal; color: #3399FF; }

##Response 数据结构

后台返回数据必须为JSON格式的JS对象.

    {
        query: "Unit",
        suggestions: [
            { value: "United Arab Emirates", data: "AE" },
            { value: "United Kingdom",       data: "UK" },
            { value: "United States",        data: "US" }
        ]
    }

数据可以是字符串或对象,会被传入格式化函数formatResults作为onSelect的callback.没有数据的情况下可以做一个默认的假数据作为候补.

    {
        query: "Unit",
        suggestions: ["United Arab Emirates", "United Kingdom", "United States"]
    }

## 不符合标准数据要求的情况

如果你的ajax服务器需要不同的query结构,返回的数据格式也不符合本插件要求的话,用一下接口修改.

    $('#autocomplete').autocomplete({
        paramName: 'searchString',
        transformResult: function(response) {
            return $.map(response.myData, function(dataItem) {
                return {value: dataItem.valueField, data: dataItem.dataField};
            });
        }
    })
        

注意:query必须和插件绑定的input的value相同,不然suggestions的数据不会被显示.

##协议

使用类MIT协议的组织或个人可以自由使用本插件,需要留版权信息.
MIT-style [license](https://github.com/devbridge/jQuery-Autocomplete/blob/master/dist/license.txt).

##作者

Tomas Kirda / [@tkirda](https://twitter.com/tkirda)

##文档翻译

Tiankui / [@github](https://github.com/Tiankui)
