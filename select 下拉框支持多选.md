select 下拉框支持多选

1.第三方库 select2

2.后端数据传输可以是 pay_id[]:'123'  pay_id[]:'321' 多个，无需自己把数据组装成pay_id[]:['123','321']

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta name="renderer" content="webkit|ie-comp|ie-stand">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
    <meta http-equiv="Cache-Control" content="no-siteapp" />
    
    
    <link type="text/css" rel="stylesheet" href="./dist/css/select2.min.css" />
    <script type="text/javascript" src="./jquery-3.2.1/jquery-3.2.1.js"></script>
    <script type="text/javascript" src="./dist/js/select2.min.js"></script>
</head>

<body>
    <div id="body">
        <table class="table table-bordered table-striped" width="800" border="none" cellspacing="0" cellpadding="0">
            <tbody>
                <tr>
                    <td align="right">多选标签：</td>
                    <td>
                        <select class="combox" id="sel_productTag" name="tagId" multiple>
                            <optgroup label="子辈">
                                <option value="kakaxi">卡卡西</option>
                                <option value="mingren">鸣人</option>
                                <option value="zuozhu">佐助</option>
                                <option value="xiaoying">小樱</option>
                            </optgroup>
                            <optgroup label="父辈">
                                <option value="bofengshuimen">波风水门</option>
                                <option value="dashewan">大蛇丸</option>
                                <option value="gangshou">纲手</option>
                                <option value="自来也">自来也</option>
                            </optgroup>
                        </select>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <script type="text/javascript">
    $(function() {
        $('#sel_productTag').select2({
            placeholder: "请至少选择一个人名",
            tags: true,
            createTag: function(decorated, params) {
                return null;
            },
            width: '256px'
        });

      $("select#sel_productTag").change(function(){
        console.log($(this).val())
        });
    });
 
    </script>
</body>

</html>
```

