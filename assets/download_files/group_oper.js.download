// JavaScript Document
/*群发*/
document.write("<script language=javascript src='/assets/js/rsa/jsencrypt.min.js'></script>");
rsa_key='-----BEGIN PUBLIC KEY-----' +
    'MIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEA7QBkq8b32I1QzcLZzu1h' +
    '/cV7DQu2ZfTz+wq8rusrBAELYWeECKshuiRlik3Tjy1M49pXT5GK2MWGe1LnaYIE' +
    '8tkcIsRB56xl0IzI1rl4TfeTimVdtXc/RFDf/3ZcLBD76s/kjJ73pQxV4vF201FU' +
    'qrVkjxe2w3R0amgE/lZqKQVP+s1w1Otn6dUdX8ZUnKxYMn3jEeQxS2SVR3bqc7je' +
    'CTIH2O7+MO49XwlXBfvYYowPSIu38WflZki9W3elPJej7GD4PWZoICbgiDCSDPbE' +
    '/RN2zYylVR9TCSrOSsOid/yVGD/rFKmDn4eqfWOIIvwvckTKsTBLEZSw7SguscVJ' +
    'm9EHcJ4bFPGTc7KTiVZRJdKBp/sfslyaVoijBDNLEVUYI1MLL4N43P4pr6aB84tj' +
    'M3XiPEe5s2yT4pt/bwvcPiHDCRjIZkI+XP3S1lgmd3TzuF/2aJ6b1ahBMUu5PI2M' +
    'bMH+iCD7wafo2mrgciWEd/JtBzM1Dmjn8pkAXeddxuNdyUEw8mfriCh8kqJrMII1' +
    'E7pjsPyWPVZYFi8VvGFTs14O7sELiV5QZQvJ51m4JD4lypRUglDnsWMiZ0SpKXC8' +
    'b7evV+sG6gpUCJoX0JcYSp1j/ILiTcLxFXrSEl2R60Ebdal4L1AF9YpcDGE7ok5B' +
    'WZB82Vo0qG8oxC2hXw8kXGcCAwEAAQ==' +
    '-----END PUBLIC KEY-----'
/**
 * 模仿form表单提交
 * @param url 提交地址
 * @param arr 提交参数 数组
 * @returns {boolean}
 */
function postSubmit(url, arr) {
    var loadIndex = layer.load(2)
    var input = [];
    input = arrToInnput(arr);
    var inputs = input.join(" ");
    // console.log(inputs);
    exportForm = $('<form action="' + url + '" method="post">'
        + inputs
        + '<input type="hidden" name="_token" value="' + $('meta[name="csrf-token"]').attr('content') + '"/>'
        + '</form>');
    $(document.body).append(exportForm);
    exportForm.submit(); // 表单提交
    exportForm.remove();
    layer.close(loadIndex);
    return true;
}

/**
 * 配合上 postSubmit 使用
 * @param arr
 * @param num
 * @returns {*[]}
 */
function arrToInnput(arr, num) {
    var por = [];
    for (var i in arr) {
        if (typeof arr[i] == "string" || typeof arr[i] == "number") {
            if (arr[i] != '') {
                if (!isNull(num)) {
                    por.push("<input type='hidden' name='" + i + "' value='" + arr[i] + "'/>")
                } else {
                    por.push("<input type='hidden' name='" + num + "[" + i + "]" + "' value='" + arr[i] + "'/>")
                }
            }
        } else {
            var my_arr = [];
            if (!isNull(num)) {
                my_arr = arrToInnput(arr[i], i);
            } else {
                my_arr = arrToInnput(arr[i], num + "[" + i + "]");
            }
            var sumData = [];
            sumData = sumData.concat(por).concat(my_arr);
            por = sumData;
        }
    }
    return por;
}

// var fun = postAjax($(this), '/phantomIn/getLinkedinEnterpriseUrl', params, 'post');
// $.when(fun)
//     .done(function (data) {
//         console.log(data);
//         console.log('加载成功!');
//     })
//     .fail(function (data) {
//         console.log(data);
//         console.log('加载失败!');
//     })
/**
 * ajax 提交
 * @param _this 提交按钮
 * @param url 提交地址
 * @param data 提交数据或 form  #id
 * @param type 提交方式
 * @param num 是否返回数据  获取返回数据配合上面注释使用
 * @param sue_url 请求成功跳转地址  默认刷新当前页面
 * @returns {*}
 */
function postAjax(_this, url, data, type, num, sue_url) {
    if (!isNull(type)) type = "post";
    let my_data;
    if (typeof data == 'string' && data.indexOf("#") !== false) {
        my_data = $(data).serialize();
        var sta = VerificationNull(data);
        if (!isNull(sta)) return false;
        if (!isNull(url)) url = $(data).attr("action");
    } else {
        my_data = data;
    }
    var index;
    let row = '';
    var dtd = $.Deferred();
    var encrypt = new JSEncrypt();
    encrypt.setPublicKey(rsa_key);
    var d = encrypt.encrypt($("input[name=key]").val());
    $.ajax({
        type: type,
        url: url,
        headers: {
            'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content'),
            'FORM-TOKEN': d,
        },
        data: my_data,
        dataType: "json",
        beforeSend: function () {
            if (isNull(_this)) _this.attr("disabled", "disabled");
            index = layer.load(1);
        },
        success: function (data) {
            var _data = data;
            layer.close(index);
            if (isNull(_this)) _this.removeAttr('disabled');

            if (data.status === 200) {
                if (num === 1) dtd.resolve(_data);
                else
                    layer.msg(data.message, {
                        icon: 1,
                        time: 2000 //2秒关闭（如果不配置，默认是3秒）
                    }, function () {
                        if (sue_url=='9') return false;
                        else if (isNull(sue_url)) window.open(sue_url);
                        else location.reload();
                    });
            } else {
                 if (num === 1) {
                     dtd.resolve(_data);
                 }else{
                      layer.msg(data.message, {
                            icon: 2,
                            time: 2000 //2秒关闭（如果不配置，默认是3秒）
                        });
                 }

            }
        }, error: function (data) {
            console.log(data);
            layer.close(index);
            if (isNull(_this)) _this.removeAttr('disabled');
             layer.msg('网络错误', {
                icon: 2,
                time: 2000 //2秒关闭（如果不配置，默认是3秒）
            });
             if (num === 1) {
                 dtd.resolve(data);
             }
        }
    });
    return dtd.promise();
}

/**
 * 空判断
 * @param str 字符串
 * @returns {boolean|*}
 */
function isNull(str) {
    switch (str) {
        case null:
            return false;
        case undefined:
            return false;
        case "":
            return false;
        case "NaN":
            return false;
        case "null":
            return false;
        case "undefined":
            return false;
    }
    return str;
}

/**
 * 表单验证 表单元素 需要含有title属性  class或id 需要包含 "verification-" 字符串
 * @param class_or_id 属性下的表单
 * @returns {boolean}
 * @constructor
 */
function VerificationNull(class_or_id) {
    var int = $("input,select,textarea ");
    if (isNull(class_or_id)) int = $(class_or_id + " input,select,textarea ");
    var _h = '';
    var m = '';
    int.each(function () {
        var _this = $(this);
        var nodeName = _this.prop("tagName");
        var disabled = _this.prop("disabled");
        if (disabled) return true;
        var typeName = _this.prop("type");
        if (typeName == 'hidden') return true;
        var msg = _this.attr('title');
        if (!isNull(msg)) msg = _this.attr("placeholder");
        if (!isNull(msg)) msg = _this.attr("name");
        var verification = _this.attr("class");
        if (!isNull(verification)) verification = _this.attr("id");
        if (!isNull(verification)) return true;
        if (verification.indexOf("verification-") < 0) return true;
        var val = _this.val();
        if (isNull(class_or_id)) nodeName = class_or_id + " " + nodeName;

        if (typeName == 'radio' || typeName == 'checkbox') {
            val = $(nodeName + "[name='" + _this.attr("name") + "']").is(":checked")
        } else {
            var val1 = $(nodeName + "[name='" + _this.attr("name") + "']").val();
            if (isNull(val1)) val = val1;
        }
        if (!isNull(val)) {
            _h = _this;
            m = "“" + msg + "”不能为空！";
            return false;
        }
        if (verification.indexOf("verification-nan-") === 0) {
            if (isNaN(val)) {
                _h = _this;
                m = "“" + msg + "”必须是数字！";
                return false;
            }
        }
    })
    if (_h) {
        layer.msg(m, {
            icon: 2,
            time: 2000 //2秒关闭（如果不配置，默认是3秒）
        });

        _h.focus();
        return false;
    }
    var param = {};
    if (isNull(class_or_id)) {
        var fm = $(class_or_id).serializeArray();
        for (var i in fm) {
            if (isNull(fm[i]['value'])) {
                if (fm[i]['name'].indexOf("[]") > 0) {
                    if (!isNull(param[fm[i]['name']])) param[fm[i]['name']] = [];
                    param[fm[i]['name']].push(fm[i]['value']);
                } else param[fm[i]['name']] = fm[i]['value'];
            }
        }
    }
    return param;
}

$(".content-wrap table tr td.td-hide").each(function () {
    var h = $(this).innerHeight();
    var text = $(this).html();
    if (h > 100) {
        $(this).html("<div style=\"width:100%;height:60px;overflow: hidden\">" + text + "</div>");
        $(this).height("60px");
    }
})

item_tips();

function item_tips (id) {
    var cls = ".item-tips";
    if (isNull(id)) cls = "#" + id + " " + cls;
    $(cls + '-div').each(function () {
        var h = $(this).height();
        var text = $(this).html();
        if (h > 60 && !isNull($(this).find("div svg").length) && isNull(text)) {
            var div = "<div style=';margin: 0 auto;display: flex;height: 100%'>" + "<div style='height: 55px;width: calc(100% - 18px) ; float: left;margin: auto;overflow: hidden'>" + text + "</div>" + "<svg style='float: right;background: #0d8ddb;color: #fff;margin: auto' viewBox=\"0 0 24 24\" width=\"14\" height=\"14\" stroke=\"currentColor\" stroke-width=\"2\" fill=\"none\" stroke-linecap=\"round\" stroke-linejoin=\"round\" class=\"css-i6dzq1\">\n" + "                                                                        <polyline points=\"6 9 12 15 18 9\"></polyline>\n" + "                                                                    </svg></div>";
            $(this).html(div);
            $(this).attr("atl", text);
        }
    })
    $("html body").on('click', cls + '-div svg', function () {
        var h = Math.round($(this).prev().height());
        if (h == '55') {
            $(this).prev().css("height", 'auto');
            $(this).css('transform', 'rotate(180deg)');
        }
        else {
            $(this).prev().css("height", '55px');
            $(this).css('transform', 'rotate(0deg)');
        }
    })
    $(cls).dblclick(function () {
        $(cls).css('color', '')
        $(this).css('color', '#3595CC')
        var flag = copyText($(this).attr("atl"));
        if(!isNull())flag = copyText($(this).text());
        layer.msg(flag ? "复制成功！" : "复制失败！");
    })
    $(cls).each(function () {
        var text = $(this).text();
        var len = $(this).attr("len");
        if (len > 0 && isNull(text)) {
            if (isNaN(parseInt(len))) len = 50;
            $(this).attr("atl", text);
            var txt = text.length <= len ? text : text.substring(0, len) + "...";
            $(this).text(txt);
        }
    })
    $(cls).mouseenter(function () {
        var id = $(this).attr("id");
        var text = $(this).attr("atl");
        if(!isNull($("#"+id).length))id=$(this);
        else id=$("#"+id);
        if (isNull(text)) {
            layer.tips(text,id, {
                tips: [1, '#3595CC'], time: 0, area: ['30%', 'auto'],
            });
            // $(this).css('color', '#000');
            $(this).css('font-weight', 'bold');
        }
    }).mouseleave(function () {
        // $(this).css('color', '')
        $(this).css('font-weight', '');
        layer.closeAll();
    })
}
function txet_delete_label (str) {
    str = str.replace(/<\/?(\s\S)*.*?>/g,'\r\n'); //去除HTML tag
    str = str.replace(/[ | ]*\n/g,'|'); //去除行尾空白
    str = str.replace(/\n[\s| | ]*\r/g,'|'); //去除多余空行
    str=str.replace(/ /ig,'');//去掉
    str=str.replace(/(\|)+/ig,'\r\n');//去掉
    return str;
}
function copyText (text) {
    if(!isNull(text))return false;
    text = txet_delete_label (text)
    var textarea = document.createElement("textarea");//创建input对象
    var currentFocus = document.activeElement;//当前获得焦点的元素
    document.body.appendChild(textarea);//添加元素
    textarea.value = text;
    textarea.style.zIndex = '-999999';
    textarea.style.position = 'fixed';
    textarea.style.top = '50%';
    textarea.style.left = '50%';
    textarea.style.transform = 'translate(-50%,-50%)';
    textarea.focus();
    if (textarea.setSelectionRange) textarea.setSelectionRange(0, textarea.value.length);//获取光标起始位置到结束位置
    else textarea.select();
    try {
        var flag = document.execCommand("copy");//执行复制
    } catch (eo) {
        var flag = false;
    }
    document.body.removeChild(textarea);//删除元素
    currentFocus.focus();
    return flag;
}
//判断文件是否存在
function isExistFile(yourFileURL) {
    var flag;
    var xmlhttp;
    if (window.XMLHttpRequest) {
        xmlhttp = new XMLHttpRequest();//其他浏览器
    } else if (window.ActiveXObject) {
        try {
            xmlhttp = new ActiveXObject("Msxml2.XMLHTTP");//旧版IE
        } catch (e) {
        }
        try {
            xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");//新版IE
        } catch (e) {
        }
        if (!xmlhttp) {
            window.alert("不能创建XMLHttpRequest对象");
        }
    }
    try {
        xmlhttp.open("GET", yourFileURL.replace(/https\:|http\:/g, ""), false);
        xmlhttp.send();
    }catch (e) {
        console.log(e)
    }
    if (xmlhttp.readyState == 4) {
        if (xmlhttp.status == 200)
            flag = true;
        else
            flag = false;
    }
    return flag;
}
/***************获取当前时间*******************/
function getRecentDay (day, num,sum) {
    var today = new Date();
    var targetday_milliseconds = today.getTime() + 1000 * 60 * 60 * 24 * day;
    if (isNull(num)) targetday_milliseconds = targetday_milliseconds + (num * 1000)
    today.setTime(targetday_milliseconds);
    var tYear = today.getFullYear();
    var tMonth = doHandleMonth(today.getMonth() + 1);
    var tDate = doHandleMonth(today.getDate());
    var tHours = doHandleMonth(today.getHours());
    var tMinutes = doHandleMonth(today.getMinutes());
    var tSeconds = doHandleMonth(today.getSeconds());
    if(!isNull(sum)) return tYear + "-" + tMonth + "-" + tDate + " " + tHours + ":" + tMinutes + ":" + tSeconds;
    if(sum===1)return tYear + "-" + tMonth + "-" + tDate;
}

function doHandleMonth (month) {
    var m = month;
    if (month.toString().length == 1) {
        m = "0" + month;
    }
    return m;
}
/***************获取当前时间*******************/
