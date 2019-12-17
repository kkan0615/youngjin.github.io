---
layout: post
title:  "[Javascript] 자바스크립트 웹 크롤링 Puppeteer 사용법"
image: ''
date:   2019-12-16 00:06:31
tags:
- javascript
- node js
- WebScraping
description: 'Learn Web Scraping with Javascript'
categories:
- javascript
- node js
- Web Scraping
---

## 웹크롤링이란?
주로 웹 크롤링 이라고 불리우나 Web crawling 보다는 Web Scraping이 더 맞는 표현입니다.
사전의 의미를 간추리자면 얻고자하는 웹사이트의 자료를 자동으로 수집하는 행위를 의미합니다.

## 자바스크립트의 웹 크롤링
많은 회사에서 python을 이용하여 웹 크롤링을 진행하지만 Node Js 역시 좋은 기술이 있고 사용 회사는 늘어가고 있습니다.
대표적인 기술로는 Puppeteer와 cheerio가 있습니다. 이번 게시물에서는 Chrome 베이스인 구글에서만든 Puppeteer을 사용해보도록 하겠습니다.
혹시나 둘의 차이점을 알고 싶은 분들은 아래에 기제해 두었습니다.

### Puppeteer
Puppetter은 크롬 Headless 웹 브라우저입니다.
<a href="https://pptr.dev/" target="_blank">공식 사이트</a>

### Cheerio
Cheerio는 DOM Parser에 더 가깝습니다. Request와 jQuery를 사용합니다.
<a href="https://cheerio.js.org/" target="_blank">공식 사이트</a>

## 간단 시작하기

### 설치
{% highlight bash %}
npm i puppeteer # 일반적으로 사용하는 것
npm i puppeteer-core # 조금 더 가벼운 버전이라 생각하시면됩니다.
{% endhighlight %}

### 공식 사이트 예제
파일명은 example.js입니다.
{% highlight javascript %}
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  await page.goto('https://example.com');
  await page.screenshot({path: 'example.png'});

  await browser.close();
})();
{% endhighlight %}

### 실행
Command line 에서
{% highlight bash %}
node example.js
{% endhighlight %}

## 코드 분석
{% highlight javascript %}
const puppeteer = require('puppeteer');
{% endhighlight %}
puppeteer을 가져옵니다.

{% highlight javascript %}
const browser = await puppeteer.launch();
{% endhighlight %}
Brower을 open합시다 여기에는 옵션을 넣을 수 있습니다.
주로 사용 되는 옵션은 아래와 같습니다.
puppeteer.launch([options])
1. headless: 설정해 두지 않으면 Default 값은 true입니다.
2. devtools: Dev모드 설정
3. executablePath: 다른 버전의 크롬 사용시
4. ignoreHTTPSErrors: Https 에러들을 무시합니다. Default 값은 false입니다.
5. slowMo: 작동시간을 늦춥니다. 이를 통해 파싱되는 과정을 천천히 볼 수 있습니다.
6. defaultViewport: 창의 크기를 설정합니다.
이 외에도 <a href="https://github.com/puppeteer/puppeteer/blob/v2.0.0/docs/api.md#puppeteerlaunchoptions" target="_blank">여기</a>에서 추가 적으로 확인 할 수 있습니다.

{% highlight javascript %}
  const page = await browser.newPage();
{% endhighlight %}
새 페이지를 오픈합니다.

{% highlight javascript %}
  await page.goto('https://example.com');
{% endhighlight %}
새로 열린 페이지를 다음 주소로 이동합니다.

{% highlight javascript %}
  await page.screenshot({path: 'example.png'});
{% endhighlight %}
현재 페이지를 이미지로 저장합니다.

{% highlight javascript %}
  await browser.close();
{% endhighlight %}
브라우저를 종료합니다.

### 간단 예제외 페이지 주요 함수
1. content() : Html을 파싱후 글자로써 return 합니다.
2. on('action', function): action이 진행 될때 function을 실행합니다.
3. type('Selctor', 'action', {option}): 검색창을 작성하거나 버튼을 누를 때 사용됩니다.
4. click(): 버튼을 누를때 사용합니다.
5. evaluate(): 데이터를 가공할 때 주로 사용합니다.
6. $(selector): Selector를 가져옵니다.

브라우저랑 페이지에 관해서는 여러 함수들이 있습니다. 아래에 나올 활용 예제를 이해하시면 다른 함수의 작동 원리 역시 이해하기 쉽습니다.
<a href="https://github.com/puppeteer/puppeteer/blob/v2.0.0/docs/api.md#puppeteerlaunchoptions" target="_blank">여기</a>에서 더 많은 함수를 확인가능합니다.

## 활용 예제

### 옥션 크롤링
{% highlight javascript %}
const puppeteer = require('puppeteer');

(async () => {
    try {
         /* Open the Browser with some options
        headless:
        devtools: dev mode 일때 사용합니다.
        */
        const browser = await puppeteer.launch({
            headless: false,
            devtools: true,
        });
        const page = await browser.newPage(); // Open the page
        await page.goto('http://www.auction.co.kr/'); // Page goto the Url
        await page.type('input[class="search_input_keyword"]', '마이크', { delay: 100 });
        await page.type('input[class="search_input_keyword"]', String.fromCharCode(13), { delay: 100 });
        await page.waitForSelector('ul');
        await page.screenshot({ path: 'example.png' }); // Screen Shot
        /* Get the values and create as data */
        const hotdeal = await page.evaluate(() => {
            const list = document.getElementsByClassName('itemcard');
            const length = (list.length < 10) ? list.length : 10;
            const arr = [];
            for (let i = 0; i < length; i++) {
                // Get name
                const name = list[i].querySelector('.text--title').innerHTML;
                // Get link
                const link = list[i].querySelector('.link--itemcard').getAttribute('href');
                // Get price
                const price = list[i].querySelector('.text--price_seller').innerHTML;
                const item = {
                    name,
                    link,
                    price,
                }
                arr.push(item);
            }
            return arr;
        });
        /* LOG */
        console.log(hotdeal);

        await browser.close(); // Close Browser
    } catch (error) {
        console.error(error);
    }
})();
{% endhighlight %}

### 유튜브
{% highlight javascript %}
const puppeteer = require('puppeteer');

(async () => {
    try {
        /* Open the Browser with some options
        headless:
        devtools: dev mode 일때 사용합니다.
        */
        const browser = await puppeteer.launch({
            headless: false,
            devtools: true,
        });
        const page = await browser.newPage(); // Open the page
        await page.goto('https://www.youtube.com/'); // Page goto the Url
        // Input id: search
        await page.type('#search', '겨울왕국', { delay: 100 });
        // Button click with id
        await page.click('button#search-icon-legacy');
        await page.screenshot({ path: 'example.png' }); // Screen Shot
        await page.waitForSelector('ytd-thumbnail.ytd-video-renderer');

        const videoList = await page.evaluate(() => {
            const list = document.querySelectorAll('#contents ytd-video-renderer, #contents ytd-grid-video-renderer'); // or .style-scope ytd-item-section-renderer OR #contents
            console.log(list[0].innerHTML);

            const length = (list.length < 10) ? list.length : 10;
            const arr = [];
            for (let i = 0; i < length; i++) {
                // Get name
                const title = list[i].querySelector('#video-title').innerHTML;
                // Get link
                const link = list[i].querySelector('#video-title').getAttribute('href');
                const snippet = list[i].querySelector('#description-text').textContent;
                const channel = list[i].querySelector('#text').textContent;
                const channelLink = list[i].querySelector('#text a').getAttribute('href');
                const views = list[i].querySelector('#metadata-line span:nth-child(1)').textContent;
                const date = list[i].querySelector('#metadata-line span:nth-child(2)').textContent;
                const video = {
                    title,
                    link,
                    snippet,
                    channel,
                    channelLink,
                    views,
                    date,
                }
                arr.push(video);
            }
            return arr;
        });
        /* LOG */
        console.log(videoList);

        /* Go to video */
        const videos = await page.$$('ytd-thumbnail.ytd-video-renderer');
        await videos[0].click();
        await page.waitForSelector('.html5-video-container');
        await page.waitFor(5000);
        await page.screenshot({ path: 'youtube_search_first.png' });
        //await browser.close(); // Close Browser
    } catch (error) {
        console.error(error);
    }
})();
{% endhighlight %}

## 끝으로
댓글 혹은 이메일을 통해 피드백 받습니다. 혹시 모르는 함수들은 댓글로 남겨주시면 답변드리고 게시글에 추가하도록 하겠습니다.