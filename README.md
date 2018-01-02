# elasticsearchpluginsample

### elasticsearch 插件demo

### 部署方法
```
mvn clean install 
cp target/releases/firestEsPlugin-5.6.4.zip elasticsearch-5.6.4/plugins
cd elasticsearch-5.6.4/plugins
unzip firestEsPlugin-5.6.4.zip
mv elasticsearch firestEsPlugin
rm -fr firestEsPlugin-5.6.4.zip
```

### 启动es后输出
```
[2018-01-02T21:01:23,710][WARN ][r.s.f.MyFirstPlugin      ] This is MyNativeScriptPlugin
```

### 插件测试方法

* 导入测试数据
```
PUT hockey/player/_bulk?refresh
{"index":{"_id":1}}
{"first":"johnny","last":"gaudreau","goals":[9,27,1],"assists":[17,46,0],"gp":[26,82,1],"born":"1993/08/13"}
{"index":{"_id":2}}
{"first":"sean","last":"monohan","goals":[7,54,26],"assists":[11,26,13],"gp":[26,82,82],"born":"1994/10/12"}
{"index":{"_id":3}}
{"first":"jiri","last":"hudler","goals":[5,34,36],"assists":[11,62,42],"gp":[24,80,79],"born":"1984/01/04"}
{"index":{"_id":4}}
{"first":"micheal","last":"frolik","goals":[4,6,15],"assists":[8,23,15],"gp":[26,82,82],"born":"1988/02/17"}
{"index":{"_id":5}}
{"first":"sam","last":"bennett","goals":[5,0,0],"assists":[8,1,0],"gp":[26,1,0],"born":"1996/06/20"}
{"index":{"_id":6}}
{"first":"dennis","last":"wideman","goals":[0,26,15],"assists":[11,30,24],"gp":[26,81,82],"born":"1983/03/20"}
{"index":{"_id":7}}
{"first":"david","last":"jones","goals":[7,19,5],"assists":[3,17,4],"gp":[26,45,34],"born":"1984/08/10"}
{"index":{"_id":8}}
{"first":"tj","last":"brodie","goals":[2,14,7],"assists":[8,42,30],"gp":[26,82,82],"born":"1990/06/07"}
{"index":{"_id":39}}
{"first":"mark","last":"giordano","goals":[6,30,15],"assists":[3,30,24],"gp":[26,60,63],"born":"1983/10/03"}
{"index":{"_id":10}}
{"first":"mikael","last":"backlund","goals":[3,15,13],"assists":[6,24,18],"gp":[26,82,82],"born":"1989/03/17"}
{"index":{"_id":11}}
{"first":"joe","last":"colborne","goals":[3,18,13],"assists":[6,20,24],"gp":[26,67,82],"born":"1990/01/30"}
```

* 查询语句
```
GET hockey/_search
{
  "query": {
    
    "function_score": {

      "functions": [
        {
          "script_score": {
            "script": {
                "source": "my_script",
                "lang" : "native",
                "params": {
                    "add": 3
                }
            }
          }
        }
      ]
    }
  }
}
```

修改"params"中"add"的值，观察返回结果中"_score"的变化
