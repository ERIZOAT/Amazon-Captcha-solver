# Best Amazon Captcha Solver when Scraping Amazon

Amazon is a prominent e-commerce platform that offers a vast amount of valuable data. This data encompasses various aspects, including product details, pricing information, customer feedback, and emerging trends. Accessing and analyzing this data can be beneficial for sellers monitoring competitors, researchers studying market trends, and consumers making informed purchasing decisions. In this tutorial, we will introduce CapSolver's solution, which can significantly expedite the scraping process.  
  
## Consider Using CapSolver to Scrape Amazon

When it comes to scraping large amounts of data from Amazon, manual web scraping can be time-consuming and inefficient, especially as the scale of the task increases. To streamline the process and extract extensive data from Amazon, CapSolver is a recommended solution.

CapSolver offers an efficient and effective way to scrape Amazon by solving CAPTCHAs and automating the data extraction process. With its advanced capabilities, CapSolver can navigate through dynamic sites like Amazon, effortlessly handling JavaScript, AJAX requests, and other complexities that arise during scraping.

Alternatively, if you require structured Amazon data promptly, such as product listings, reviews, or seller profiles, CapSolver provides access to its curated Amazon dataset. This dataset allows you to download and access pre-extracted, high-quality data directly from the CapSolver platform.

By leveraging CapSolver for scraping Amazon, you can significantly enhance the efficiency and accuracy of your data collection efforts. Whether you choose to automate the scraping process or utilize the curated dataset, CapSolver offers a comprehensive solution tailored to your specific needs.

When scraping Amazon, you may encounter CAPTCHA challenges that need to be resolved before you can access further.Amazon currently has 2 main scenarios for captcha, one is FunCaptcha and one is Imagetotext  
 

**Funcaptcha** is a type of CAPTCHA technology that was developed by a company called Arkose Labs. Unlike traditional CAPTCHAs, Funcaptcha uses interactive puzzles and games to differentiate between humans and bots. These puzzles are designed to be engaging and fun for humans, but difficult for bots to solve.
  
**Image-to-text**, also known as optical character recognition (OCR), is a technology that converts printed or handwritten text within an image into machine-readable text. It involves using algorithms and computer vision techniques to analyze the visual patterns and structures of characters in an image and translate them into editable and searchable text.


## Solving Amazon Funcaptcha

### Create Task

Create a task with the [createTask](../api-createtask.md) to create a task.

### Task Object Structure

| Properties               | Type   | Required | Description                                                                                                                                                                                                                                                                                                                |
|--------------------------|--------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| type                     | String | Required | `FunCaptchaTaskProxyLess`                                                                                                                                                                                                                                                                                                  |
| websiteURL               | String | Required | Web address of the website using funcaptcha, generally it's fixed value. (Ex: https://google.com)                                                                                                                                                                                                                          |
| websitePublicKey         | String | Required | The domain public key, rarely updated. (Ex: E8A75615-1CBA-5DFF-8031-D16BCF234E10)                                                                                                                                                                                                                                          |
| funcaptchaApiJSSubdomain | String | Optional | A special subdomain of [funcaptcha.com](http://funcaptcha.com/), from which the JS captcha widget should be loaded. Most FunCaptcha installations work from shared domains.                                                                                                                                                |
| data                     | String | Optional | Additional parameter that may be required by FunCaptcha implementation. Use this property to send "blob" value as a stringified array. See example how it may look like. {"\blob\":\"HERE_COMES_THE_blob_VALUE\"}  Learn [how to get FunCaptcha blob data](https://www.capsolver.com/blog/FunCaptcha/funcaptcha-data-blob) |
| proxy                    | String | Optional | Learn [Using proxies](../api-how-to-use-proxy)                                                                                                                                                                                                                                                                             |

### Example Request

``` json
POST https://api.capsolver.com/createTask
Host: api.capsolver.com
Content-Type: application/json

{
    "clientKey": "YOUR_API_KEY_HERE",
    "task": {
        "type":"FunCaptchaTaskProxyLess", //Required
        "websiteURL":"", //Required
        "websitePublicKey":"", //Required
        "data": "{\"blob\": \"flaR60YY3tnRXv6w.l32U2KgdgEUCbyoSPI4jOxU...\"}" // Optional
    }
}
```


After you submit the task to us, you should receive in the response a 'Task id' if it's successfull. Please
read [errorCode: full list of errors](https://captchaai.atlassian.net/wiki/spaces/CAPTCHAAI/pages/394062/FuncaptchaTask+solving+FunCaptcha#)
if you didn't receive the task id.

### Example Response

``` json
{
    "errorId": 0,
    "status": "idle",
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006"
}

```

### **Getting Result**

Use the [getTaskResult](../api-gettaskresult.md) method to get the recognition results

Depending on the system load, you will get the results within the interval of `1s` to `20s`

### Example Request

``` json
POST https://api.capsolver.com/getTaskResult
Host: api.capsolver.com
Content-Type: application/json

{
    "clientKey": "YOUR_API_KEY",
    "taskId": "61138bb6-19fb-11ec-a9c8-0242ac110006"
}
```

### Example Response

``` json
{
    "errorId": 0,
    "solution": {
        "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36",
        "token": "3AHJ_q25SxXT-pmSeBXjzScW-EiocHwwpwqtk1QXlJnGnU......"
    },
    "status": "ready"
}
```


## Solving Amazon Imagetotext



:::

### Create Task

Create the task with the [createTask](../api-createtask.md).

**Task Object Structure**

Note that this type of task returns the task execution result directly after createTask, rather than getting it
asynchronously through getTaskResult.

| Properties | Type    | Required | Description                                                                                            |
|------------|---------|----------|--------------------------------------------------------------------------------------------------------|
| type       | String  | Required | ImageToTextTask                                                                                        |
| websiteURL | String  | Optional | Page source url to improve accuracy                                                                    |
| body       | String  | Required | base64 encoded content of the image (no newlines) (no data:image/****\*****; base64, content           |
| module     | String  | Optional | Specifies the module. Currently, the supported modules are common and queueit                          |
| score      | Float   | Optional | `0.8 ~ 1`, Identify the matching degree. If the recognition rate is not within the range, no deduction |
| case       | Boolean | Optional | Case sensitive or not                                                                                  |

### Example Request

```text
POST https://api.capsolver.com/createTask
Host: api.capsolver.com
Content-Type: application/json
```

```json lines
{
  "clientKey": "YOUR_API_KEY",
  "task": {
    "type": "ImageToTextTask",
    "websiteURL": "https://xxxx.com",
    // You can choose the module you need to use
    // ocr single image model, default common
    "module": "queueit",
    // base64 encoded image
    "body": "/9j/4AAQSkZJRgABA......"
  }
}
```

### Example Response

```json lines
{
  "errorId": 0,
  "errorCode": "",
  "errorDescription": "",
  "status": "ready",
  "solution": {
    "text": "44795sds"
  },
  "taskId": "2376919c-1863-11ec-a012-94e6f7355a0b"
}
```
