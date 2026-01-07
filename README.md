# Proxy Integrations


대부분의 웹 개발자 및 スクレイピング 전문가와 마찬가지로, 프로젝트 중 하나에서 <b>Selenium</B> 또는 동등한 브라우저 자동화 도구를 사용해 보셨을 것입니다. 이러한 도구는 강력한 테스트 및 웹 페이지 상호작용 도구이지만, 대부분의 경우 차단되지 않고 특정 작업 유형을 성공적으로 완료하려면 적절한 <b>プロキシ IP 통합</b>이 필요합니다.  
プロキシ 통합에는 2가지 옵션이 있습니다. 첫 번째는 [Bright Data super proxies](https://brightdata.co.kr/proxy-types/proxy-servers)와 직접 통합하는 것이고, 두 번째는 Bright Data [proxy manager](https://github.com/luminati-io/luminati-proxy)를 통해 통합하는 것입니다.

![Bright Data Proxy Manager](https://github.com/luminati-io/proxy-integrations/blob/main/Proxy%20Manager.png)

<h2>Selenium proxy integration</h2>

[Selenium](https://github.com/SeleniumHQ/selenium) 웹 드라이버는 Python 코더들 사이에서 인기 있는 브라우저 자동화 도구로, 가장 정밀한 웹사이트 테스트를 위한 현실적인 브라우징 상황을 만들고, 실제 사용자의 웹 페이지 상호작용을 에뮬레이션하는 Webスクレイピング에도 사용할 수 있습니다.
<h3>Selenium을 Bright Data super proxies와 통합하려면 다음 단계를 따르십시오:</h3>

* 먼저 Bright Data [control panel](https://brightdata.co.kr/cp/zones)에 접속한 후 ‘<b>add zone</b>’을 클릭합니다.
* 선호하는 네트워크 유형 - Datacenter, ISP, Residential, Mobile 등 - 을 선택한 다음 '<b>add zone</b>'을 다시 클릭합니다.
* Selenium 웹 드라이버에서 ```setProxy``` 함수에 HTTP 및 HTTPS 모두에 대해 ```Proxy IP:Port```를 입력합니다. 예: ```zproxy.lum-superproxy.io:22225```.
* ```sendKeys``` 아래에 Bright Data account ID와 proxy zone name을 입력합니다:```lum-customer-CUSTOMER-zone-YOURZONE``` 그리고 proxy zone 설정에서 찾을 수 있는 zone password를 입력합니다.
* 코드는 다음과 같은 형태여야 합니다:

```s
const {Builder, By, Key, until} = require('selenium-webdriver');
const proxy = require('selenium-webdriver/proxy');

(async function example(){
  let driver = await new Builder().forBrowser('firefox').setProxy(proxy.manual({
    http: 'zproxy.lum-superproxy.io:22225',
    https: 'zproxy.lum-superproxy.io:22225'
  })).build()

  try {
    await driver.get('http://lumtest.com/myip.json');
    driver.switchTo().alert()
      .sendKeys('lum-customer-USERNAME-zone-YOURZONE'+Key.TAB+'PASSWORD');
    driver.switchTo().alert().accept();
  } finally {
      await driver.quit();
  }
})();
```
<h3>Selenium을 Bright Data proxy manager와 통합하려면 다음 단계를 따르십시오:</h3>

* 필요한 네트워크, IP 유형, IP 수로 proxy zone을 생성합니다.
* 사용자 장치에 Bright Data Proxy Manager를 [Install](https://brightdata.co.kr/products/proxy-manager)하거나, Bright Data control panel에서 클라우드를 통해 액세스합니다.
* <b>‘add new proxy’</b> 옵션을 클릭하고 필요한 zone 및 설정을 선택한 다음 <b>‘save’</b>를 클릭합니다.
* Selenium 웹 드라이버로 이동합니다. setProxy에서 로컬 IP와 proxy manager port(예: 127.0.0.1:24000)를 입력합니다.
* 로컬 호스트 IP는 127.0.0.1입니다.
* Proxy Manager에서 생성되는 포트는 24XXX 형식이며, 예를 들어 24000입니다.
* 필드에 username과 password를 입력하지 마십시오 — Bright Data Proxy Manager는 Super Proxy server에 이미 인증되어 있습니다.
* Selenium 코드는 다음과 같은 형태여야 합니다:

```s
const {Builder, By, Key, until} = require('selenium-webdriver');
const proxy = require('selenium-webdriver/proxy');

(async function example(){
    let driver = await new Builder().forBrowser('firefox').setProxy(proxy.manual({
        http: '127.0.0.1:24000',
        https: '127.0.0.1:24000'
    })).build()

    try {
        await driver.get('http://lumtest.com/myip.json');
        driver.switchTo().alert().accept();
    } finally {
        await driver.quit();
    }
})();
```

<b>[여기에서 Selenium proxy integration을 시작하십시오](https://brightdata.co.kr/integration/selenium)</b>.


<h2>Puppeteer proxy integration</h2>

Puppeteer는 고수준 API를 통해 headless 및 non-headless Chrome과 Chromium 브라우저를 제어하고 자동화하기 위해 만들어진 Node 라이브러리입니다. 원래 테스트 플랫폼으로 설계되지는 않았지만, JavaScript 사용자들 사이에서 Selenium의 매우 인기 있는 대안이 되었으며 추가적인 [stealth extra plug-ins](https://github.com/berstend/puppeteer-extra) 기능도 제공합니다.

<h3>Puppeteer를 Bright Data super proxies와 통합하려면 다음 단계를 따르십시오:</h3>

* 먼저 Bright Data control panel에 접속한 후 <b>‘add zone’</b>을 클릭합니다.
* 선호하는 プロキシ 네트워크 유형 - Datacenter, ISP, Residential, Mobile 등 - 을 선택한 다음 <b>'add zone'</b>을 다시 클릭합니다.
* Puppeteer로 이동하여 ```proxy-server``` 값에 ```Proxy IP:Port```를 추가합니다(예: ```zproxy.lum-superproxy.io:22225```).
* ```page.authenticate``` 아래의 ```username``` 값에 Bright Data account ID와 proxy zone name을 다음과 같이 입력합니다: ```lum-customer-CUSTOMER-zone-YOURZONE``` 그리고 proxy zone settings에서 찾을 수 있는 proxy zone password를 입력합니다.
* Puppeteer 코드는 다음과 같은 형태여야 합니다:

```javascript
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch({
    headless: false,
    args: ['--proxy-server=zproxy.lum-superproxy.io:22225']
  });
  const page = await browser.newPage();
    await page.authenticate({
        username: 'lum-customer-USERNAME-zone-YOURZONE',
        password: 'PASSWORD'
    });
    await page.goto('http://lumtest.com/myip.json');
    await page.screenshot({path: 'example.png'});
    await browser.close();
})();
```

<h3>Puppeteer를 Bright Data proxy manager와 통합하려면 다음 단계를 따르십시오:</h3>

* Bright Data control panel에 접속하여 필요한 プロキシ 네트워크 유형, IP 유형, IP 수로 zone을 생성합니다.
* 장치에 Proxy Manager를 설치하거나 Bright Data control panel에서 클라우드를 통해 액세스합니다.
* <b>‘add new proxy’</b>를 클릭하고 필요한 zone과 설정을 선택한 다음 <b>‘save’</b>를 클릭합니다.
* Puppeteer에서 ```proxy-server``` 아래에 로컬 IP와 Bright Data Proxy Manager 포트(예: 127.0.0.1:24000)를 입력합니다.
* 로컬 호스트 IP는 127.0.0.1입니다.
* Bright Data Proxy Manager에서 기본으로 생성되는 포트는 24XXX 형식이며, 예: 24000입니다.
* 필드에 username과 password를 입력하지 마십시오 — Bright Data Proxy Manager는 Super Proxy server에 이미 인증되어 있습니다.
* Puppeteer 코드는 다음과 같은 형태여야 합니다:

```javascript
const puppeteer = require('puppeteer');

(async () => {
    const browser = await puppeteer.launch({
        headless: false,
        args: ['--proxy-server=127.0.0.1:24000']
    });
    const page = await browser.newPage();
    await page.authenticate();
    await page.goto('http://lumtest.com/myip.json');
    await page.screenshot({path: 'example.png'});
    await browser.close();
})();
```

Bright Data <b>[Puppeteer proxy integration을 여기에서 시작하십시오](https://brightdata.co.kr/integration/puppeteer)</b>.


<h2>Playwright proxy integration</h2>

Playwright는 단일 API를 사용하여 Chromium, Firefox, WebKit 자동화를 가능하게 하는 Node.js 라이브러리입니다. 다음은 Bright Data와 함께하는 빠른 Playwright proxy Integration 단계입니다. 

<h3>Playwright를 Bright Data super proxies와 통합하려면 다음 단계를 따르십시오:</h3>

* 먼저 Bright Data control panel에 접속한 후 <b>‘add a zone’</b>을 클릭합니다.
* 선호하는 プロキシ 네트워크 유형 - Datacenter, ISP, Residential, Mobile 등 - 을 선택한 다음 <b>'add zone'</b>을 다시 클릭합니다.
* Playwright로 이동하여 ```server``` 값에 ```Proxy IP:Port```를 입력합니다. 예: ```http://zproxy.lum-superproxy.io:22225``` .
* ```username``` 아래에 Bright Data account ID와 proxy zone name을 입력합니다(예:```lum-customer-CUSTOMER-zone-YOURZONE```). 그리고 ```password``` 아래에 Bright data proxy zone settings에서 찾을 수 있는 zone password를 입력합니다.
* Playwright 코드는 다음과 같은 형태여야 합니다:

```javascript
const playwright = require('playwright');

(async () => {
    for (const browserType of ['chromium', 'firefox', 'webkit']) {
        const browser = await playwright[browserType].launch({
            headless: false,
            proxy: {
                server: 'http://zproxy.lum-superproxy.io:22225',
                username: 'lum-customer-USERNAME-zone-YOURZONE',
                password: 'PASSWORD'
            },
        });
        const context = await browser.newContext();
        const page = await context.newPage();
        await page.goto('http://lumtest.com/myip.json');
        await page.screenshot({ path: 'example.png' });
        await browser.close();
    }
})();
```

<h3>Playwright를 Bright Data proxy manager와 통합하려면 다음 단계를 따르십시오:</h3>

* Bright Data control panel에 접속하여 필요한 プロキシ 네트워크 유형, IP 유형, IP 수로 zone을 생성합니다.
* 장치에 Proxy Manager를 설치하거나 Bright Data control panel에서 클라우드를 통해 액세스합니다.
* <b>‘add new proxy’</b>를 클릭하고 필요한 zone과 설정을 선택한 다음 <b>‘save’</b>를 클릭합니다.
* Playwright에서 ```server``` 아래에 로컬 IP와 Bright Data Proxy Manager 포트(예: 127.0.0.1:24000)를 입력합니다.
* 로컬 호스트 IP는 127.0.0.1입니다.
* Bright Data Proxy Manager에서 기본으로 생성되는 포트는 24XXX 형식이며, 예: 24000입니다.
* 필드에 username과 password를 입력하지 마십시오 — Bright Data Proxy Manager는 Super Proxy server에 이미 인증되어 있습니다.
* Playwright 코드는 다음과 같은 형태여야 합니다:

```javascript
const playwright = require('playwright');

(async () => {
    for (const browserType of ['chromium', 'firefox', 'webkit']) {
        const browser = await playwright[browserType].launch({
            headless: false,
            proxy: {
                server: '127.0.0.1:24000',
                username: '',
                password: ''
            },
        });
        const context = await browser.newContext();
        const page = await context.newPage();
        await page.goto('http://lumtest.com/myip.json');
        await page.screenshot({ path: 'example.png' });
        await browser.close();
    }
})();
```

Bright Data <b>[Playwright proxy integration을 여기에서 시작하십시오](https://brightdata.co.kr/integration/playwright)</b>.


<h2>Other useful Bright Data proxy integrations</h2>

* PhantomBuster - [YouTube](https://youtu.be/Tw68CHXs_jE)에서 proxy integration 튜토리얼 비디오를 시청하십시오.
* Apify
* SessionBox
* VMLogin
* AdsPower

새로운 소식을 확인하려면 Bright Data [proxy integrations](https://brightdata.co.kr/integration) 센터로 이동하십시오.