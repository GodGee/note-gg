InputMask使用

1、引入<script src="<?= assetJs('jquery.inputmask.min.js'); ?>"></script>

2、加载

```js
<script>
    $(document).ready(function() {
            $(":input").inputmask();
        });
</script>
```

3、标签

  <input id="tax_id" name="tax_id" class="user-setting-form-group-item js_changeTaxId" type="text">

4、触发

```js
 let code = getCountryCode(".js_changeAddrCountry");
            let countryCode = countryCodes.map((item) => {
                return item.country_code;
            });
            if (countryCode && countryCode.includes(code)) {
                $("#tax_form_group").css("display", "block");
                if (code === "BR") {
                    $("#tax_id_label").html(jsLg.cpf_number);
                    $("#tax_id").inputmask({
                        mask: function () {
                            return ["999.999.999-99", "99.999.999/9999-99"];
                        },
                    });
                } else if (code === "CL") {
                    $("#tax_id_label").html(jsLg.rut_number);
                    $("#tax_id").inputmask({ mask: "99999999-9" });
                } else {
                }
            } else {
                $("#tax_form_group").css("display", "none");
            }

```

