## BOSS直聘自动投递插件
***
### 自动翻页插件
```js
// ==UserScript==
// @name         页面跳转
// @namespace    http://tampermonkey.net/
// @version      2024-12-16
// @description  try to take over the world!
// @author       You
// @include     https://www.zhipin.com/web/geek/job*
// @icon         https://www.google.com/s2/favicons?sz=64&domain=zhipin.com
// @grant        none
// ==/UserScript==

window.addEventListener('load', function () {
    function delay(ms) {
        return new Promise((resolve) => {
            setTimeout(resolve, ms);
        });
    }

    var currentUrl = window.location.href;
    var targetUrlPrefix = "https://www.zhipin.com/web/geek/job?";
    if (currentUrl.startsWith(targetUrlPrefix) && !currentUrl.includes('page=')) {
        var newUrl = currentUrl + '&page=1';
        window.location.href = newUrl;
    }

    // 创建按钮元素
    var myButton = document.createElement('button');
    myButton.textContent = '开始投递';
    var input1 = document.createElement('input');
    input1.type = 'number';
    input1.placeholder = '开始页数';

    // 创建第二个输入框
    var input2 = document.createElement('input');
    input2.type = 'number';
    input2.placeholder = '结束页数';
    myButton.style.marginTop = '70px';

    // 为按钮添加点击事件监听器，点击时执行主要功能函数
    myButton.onclick = async function () {
        var startPage = parseInt(input1.value); // 获取起始页并转换为整数
        var endPage = parseInt(input2.value);
        console.log("起始页：" + startPage + "，结束页：" + endPage); // 获取结束页并转换为整数

        if (isNaN(startPage) || isNaN(endPage) || startPage < 1 || endPage < startPage) {
            alert('请输入合法的页码范围，起始页要大于等于1且结束页要大于等于起始页');
            return;
        }

        // 定义每次打开页面的间隔时间（单位：毫秒）
        const intervalMs = 1000*180; // 设置1秒间隔

        // 逐个页面打开
        for (let i = startPage; i <= endPage; i++) {
            var newUrl = currentUrl.replace(/page=\d+/, 'page=' + i);
            console.log('打开页面：', i);

            // 在新的标签页中打开页面
            window.open(newUrl, '_blank');

            // 延迟，确保每个页面打开之间有时间间隔
            await delay(intervalMs); // 每次间隔1秒
        }
    };

    // 获取页面的body元素
    var body = document.body;

    // 将按钮插入到body元素的第一个子节点之前，也就是页面最上方
    document.body.insertBefore(myButton, document.body.firstChild);
    document.body.insertBefore(input2, myButton);
    document.body.insertBefore(input1, input2);
});
```

### 自动投递插件
```js
// ==UserScript==
// @name         该页投递
// @namespace    http://tampermonkey.net/
// @version      2024-12-16
// @description  try to take over the world!
// @author       You
// @include     https://www.zhipin.com/web/geek/job*
// @icon         https://www.google.com/s2/favicons?sz=64&domain=zhipin.com
// @grant        none
// ==/UserScript==

window.addEventListener('load', function() {
		function delay(ms) {
            return new Promise((resolve) => {
                setTimeout(resolve, ms);
            });
        }
    console.log("开始自动投递");
        const baseUrl = "https://www.zhipin.com/wapi/zpgeek/friend/add.json";

        // 创建一个返回Promise的函数，用于实现延迟

        async function sendGetRequest(url) {
            try {
                const response = await fetch(url, {
                    method: 'GET'
                });
                const result = await response.json();
                console.log('请求响应数据:', result);
            } catch (error) {
                console.error('请求出现错误:', error);
            }
        }

        async function main() {
            await delay(Math.floor(Math.random() * 2000 + 3000));
            console.log("开始自动投递1");
            const elements = document.getElementsByClassName('job-card-left');
            for (let i = 0; i < elements.length; i++) {
                const pattern = /\/job_detail\/([^.]+)\.html\?lid=([^&]+)&securityId=([^&]+)&sessionId=/;
                const match = elements[i].href.match(pattern);

                const urlWithParams = `${baseUrl}?jobId=${match[1]}&lid=${match[2]}&securityId=${match[3]}`;
				console.log(match[2]);
                console.log(urlWithParams);

                await sendGetRequest(urlWithParams);
                // 调用delay函数实现暂停3 - 5秒，生成随机的3000 - 5000毫秒的延迟时间
                await delay(Math.floor(Math.random() * 2000 + 3000));
            }
            console.log("本页投递完成");
        }

        main();
    });
```

### 使用教程
首先下载篡改猴
![下载篡改猴](/下载篡改猴.png)


添加脚本
![添加脚本](/添加脚本.png)


打开投递页，输入开始页码，结束页码，然后点击开始投递
![打开投递页](/打开投递页.png)
