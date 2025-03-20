# Bypassing CAPTCHAs With Selenium in Python

[![Promo](https://github.com/luminati-io/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.com/)

This guide explains how to bypass CAPTCHAs in Selenium:

- [Common CAPTCHA Types](#common-captcha-types)
- [Selenium CAPTCHA Handling: Step-By-Step Tutorial](#selenium-captcha-handling-step-by-step-tutorial)
  - [Step #1: Create a New Python Project](#step-1-create-a-new-python-project)
  - [Step #2: Install Selenium](#step-2-install-selenium)
  - [Step #3: Set Up Your Selenium Script](#step-3-set-up-your-selenium-script)
  - [Step #4: Add the Browser Automation Logic](#step-4-add-the-browser-automation-logic)
  - [Step #5: Install the Selenium Stealth Plugin](#step-5-install-the-selenium-stealth-plugin)
  - [Step #6: Configure the Stealth Settings to Avoid CAPTCHAs](#step-6-configure-the-stealth-settings-to-avoid-captchas)
  - [Step #7: Repeat the Bot Detection Test](#step-7-repeat-the-bot-detection-test)
 - [What If the Above Solution Does Not Work](#what-if-the-above-solution-does-not-work)


## What Are CAPTCHAs?


A **CAPTCHA** (Completely Automated Public Turing test to tell Computers and Humans Apart) is used to distinguish human users from bots. It presents challenges that are easy for humans but difficult for machines. Common providers include Google reCAPTCHA, hCaptcha, and BotDetect.

## Common CAPTCHA Types

- **Text-based**: Enter distorted letters/numbers.  
- **Image-based**: Identify specific objects in a grid.  
- **Audio-based**: Type words from an audio clip.  
- **Puzzle-based**: Solve simple puzzles (e.g., mazes).  

![Text CAPTCHA example](https://brightdata.com/wp-content/uploads/2024/07/Text-CAPTCHA-example.png)

Often, CAPTCHAs appear at the final step of form submissions:

![CAPTCHA in form submission](https://brightdata.com/wp-content/uploads/2024/07/CAPTCHA-as-a-step-of-a-form-submission-process-example.png)

They prevent automated bots from completing actions. While CAPTCHA-solving services exist, hard-coded CAPTCHAs are rare due to **poor user experience**.

CAPTCHAs are part of broader security measures like **Web Application Firewalls (WAFs)**:

![Web Application Firewall](https://brightdata.com/wp-content/uploads/2024/07/Example-of-a-Web-Application-Firewall-1024x488.png)

These systems trigger CAPTCHAs when detecting suspicious activity. To bypass them, bots must **mimic human behavior**, which requires frequent script updates.

## Selenium CAPTCHA Handling: Step-By-Step Tutorial

One of the best tools to mimic human behavior while controlling a browser for is [Selenium](https://www.selenium.dev/), a popular browser automation library. Let's learn how to avoid CAPTCHAs in Selenium using a Python script.

### Step #1: Create a New Python Project

You will need Python 3 and Crhome installed locally to follow this guide.

If you already have a Selenium web scraping or testing script, skip the first three steps. Otherwise, create a folder for your Selenium CAPTCHA bypass demo project and navigate to it in the terminal window:

```bash
mkdir selenium_demo

cd selenium_demo
```

Next, add a new [Python virtual environment](https://docs.python.org/3/library/venv.html) inside it:

```bash
python -m venv venv
```

Open the project’s folder in your favorite Python IDE and create a new file named `script.py`.

### Step #2: Install Selenium

Activate the [Python virtual environment](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#activate-a-virtual-environment).

On Windows:

```powershell
venv\Scripts\activate
```

On Linux or macOS:

```bash
source venv/bin/activate
```

Install Selenium:

```bash
pip install selenium
```

### Step #3: Set Up Your Selenium Script

Import Selenium by adding the following line to `script.py`:

```python
from selenium import webdriver
```

Create a [`ChromeOptions`](https://selenium-python.readthedocs.io/api.html#module-selenium.webdriver.chrome.options) object to configure Chrome to start in headless mode:

```python
options = webdriver.ChromeOptions()

options.add_argument("--headless")
```

Initialize a [Chrome WebDriver](https://selenium-python.readthedocs.io/api.html#module-selenium.webdriver.chrome.webdriver) instance with these options, and finally close it with `quit()`. This is what your current `script.py` file should currently look like:

```python
from selenium import webdriver

# configure Chrome to start in headless mode

options = webdriver.ChromeOptions()

options.add_argument("--headless")

# start a Chrome instance

driver = webdriver.Chrome(options=options)

# browser automation logic...

# close the browser and release its resources

driver.quit()
```

The above script launches a new Chrome instance in headless mode before closing the browser.

### Step #4: Add the Browser Automation Logic

To test Selenium CAPTCHA bypass logic, the script will connect to **[bot.sannysoft.com](https://bot.sannysoft.com/)** and capture a screenshot. This page runs various tests to detect whether the visitor is a human or a bot. When accessed via a standard browser, all tests pass.

Use the [`get()`](https://selenium-python.readthedocs.io/api.html#selenium.webdriver.remote.webdriver.WebDriver.get) method to instruct Chrome to visit the target page:

```python
driver.get("https://bot.sannysoft.com/")
```

Selenium does not provide a built-in function to capture a full-page screenshot. As a workaround, adjust the browser window size to match the `<body>` dimensions before taking a screenshot.


```python
# get the body width and height

full_width = driver.execute_script("return document.body.parentNode.scrollWidth")

full_height = driver.execute_script("return document.body.parentNode.scrollHeight")

# set the browser window to the body width and height

driver.set_window_size(full_width, full_height)

# take a screenshot of the entire page

driver.save_screenshot("screenshot.png")

# restore the original window size

driver.set_window_size(original_size["width"], original_size["height"])
```

The above trick will do, and `screenshot.png` will contain the screenshot of the entire page.

Put it all together, and you will have the following logic:

```python
from selenium import webdriver

# configure Chrome to start in headless mode

options = webdriver.ChromeOptions()

options.add_argument("--headless")

# start a Chrome instance

driver = webdriver.Chrome(options=options)

# connect to the target page

driver.get("https://bot.sannysoft.com/")

# get the current window size

original_size = driver.get_window_size()

# get the body width and height

full_width = driver.execute_script("return document.body.parentNode.scrollWidth")

full_height = driver.execute_script("return document.body.parentNode.scrollHeight")

# set the browser window to the body width and height

driver.set_window_size(full_width, full_height)

# take a screenshot of the entire page

driver.save_screenshot("screenshot.png")

# restore the original window size

driver.set_window_size(original_size["width"], original_size["height"])

# close the browser and release its resources

driver.quit()
```

Launch the `script.py` file above with this command:

```bash
python script.py
```

The script launches a **Chromium instance in headless mode**, navigates to the target page, captures a screenshot, and then closes the browser. After execution, a `screenshot.png` file will appear in the project root folder.

![screenshot.png file example](https://brightdata.com/wp-content/uploads/2024/07/screenshot.png-file-example-206x1024.png)

As shown by the red boxes, Chrome in headless mode fails multiple detection tests. This means your script is likely flagged as a bot, leading to CAPTCHA challenges on protected sites.

To avoid detection and prevent CAPTCHAs, use the Stealth plugin.

### Step #5: Install the Selenium Stealth Plugin

[Selenium Stealth](https://github.com/diprajpatra/selenium-stealth) is a Python package that reduces the likelihood of Chrome/Chromium being detected as a bot when controlled by Selenium. It helps bypass anti-bot detection by modifying browser properties to prevent automation leaks.

Selenium Stealth alters various browser attributes to mimic human behavior and avoid detection. It functions similarly to Puppeteer Stealth but is designed for Selenium.

Install Selenium Stealth via `pip`:

```sh
pip install selenium-stealth
```

Then, import the library by adding this line to the `script.py` file:

```python
from selenium_stealth import stealth
```

### Step #6: Configure the Stealth Settings to Avoid CAPTCHAs

To register Selenium Stealth and configure the Chrome WebDriver to avoid CAPTCHAs, call the `stealth()` function:

```python
stealth(

driver,

user_agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36",

languages=["en-US", "en"],

vendor="Google Inc.",

platform="Win32",

webgl_vendor="Intel Inc.",

renderer="Intel Iris OpenGL Engine",

fix_hairline=True,

)
```

Set the function arguments as needed, but note that the values above are sufficient to bypass most anti-bot defenses.

With these settings, the Selenium-controlled browser will mimic a real user’s browser.

### Step #7: Repeat the Bot Detection Test

Below is the final `script.js` file:

```python
from selenium import webdriver

from selenium_stealth import stealth

# configure Chrome to start in headless mode

options = webdriver.ChromeOptions()

options.add_argument("--headless")

# start a Chrome instance

driver = webdriver.Chrome(options=options)

# configure the WebDriver to avoid bot detection

# with Selenium Stealth

stealth(

driver,

user_agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/126.0.0.0 Safari/537.36",

languages=["en-US", "en"],

vendor="Google Inc.",

platform="Win32",

webgl_vendor="Intel Inc.",

renderer="Intel Iris OpenGL Engine",

fix_hairline=True,

)

# connect to the target page

driver.get("https://bot.sannysoft.com/")

# get the current window size

original_size = driver.get_window_size()

# get the body width and height

full_width = driver.execute_script("return document.body.parentNode.scrollWidth")

full_height = driver.execute_script("return document.body.parentNode.scrollHeight")

# set the browser window to the body width and height

driver.set_window_size(full_width, full_height)

# take a screenshot of the entire page

driver.save_screenshot("screenshot.png")

# restore the original window size

driver.set_window_size(original_size["width"], original_size["height"])

# close the browser and release its resources

driver.quit()
```

Execute the bypass CAPTCHA Selenium Python script again:

```bash
python script.py
```

Take a look at `screenshot.png`, and you will notice that all bot detection tests have been passed:

![All bot detection tests passed on the new screenshot.png](https://brightdata.com/wp-content/uploads/2024/07/All-bot-detction-tests-passed-on-the-new-screenshot-249x1024.png)

## What If the Above Solution Does Not Work?

Anti-bot systems don’t just analyze browser settings—IP reputation is crucial. Simply switching IPs with a free library won’t work; you need Selenium proxy integration.

Even with an optimally configured browser, CAPTCHAs may still appear. For basic reCAPTCHA v2 challenges, you can try [selenium-recaptcha-solver](https://pypi.org/project/selenium-recaptcha/), but these libraries are outdated and limited.

Basic methods fail against complex anti-bot systems like Cloudflare. For a robust solution, use Bright Data’s web scraping tools, which support  reCAPTCHA, hCaptcha, px_captcha, SimpleCaptcha, GeeTest, FunCaptcha, Cloudflare Turnstile, AWS WAF Captcha, KeyCAPTCHA, and more.

Bright Data’s CAPTCHA Solver works with any HTTP client or browser automation tool, including Selenium.

## Conclusion

Thanks to the Selenium Stealth library, you can override the default configurations of Chrome to limit bot detection. However, this approach is not a definitive solution. Advanced bot detection tools will still be able to block you. The real solution is to connect to the target site via an unblocking API that can return the CAPTCHA-free HTML of any web page.

One of such solutions is [Web Unlocker](https://brightdata.com/products/web-unlocker), a scraping API that automatically rotates your exit IP with each request via proxy integration and handles browser fingerprinting, automatic retries, and CAPTCHA resolution for you. Dealing with CAPTCHAs has never been easier!

Sign up now and start your free trial.