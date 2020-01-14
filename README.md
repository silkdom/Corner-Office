# Corner-Office

## Motivation
The MMA program at Rotman is intensive, and thus I spend most of my day every day at uni. When thinking about what made my time at Rotman better/worse, the study room situation stood out! The rooms are often completely booked especially during peak times, which is frustrating for our perpetual groupwork assignments. Rooms can be booked two-days in advance, starting from 12:00AM. In order to stand a chance of booking a room, especially the 'best' rooms, you currently have to wait for the clock to strike midnight. 

What are the 'best' rooms you may say? In my opinion they are those with windows, are close to the water fountain/bathroom, and are spacious. Of all the rooms at Rotman, the corner room on the 3rd floor (3064) can be seen as the holy grail in this regard. I created this project to alleviate my room booking issues and lay claim to the corner office!

# How it works

The web-based automation package selenium was chosen alongside a WebDriver to do the heavy work. Once the ChromeDriver is initialized, the login website is input and the login credentials are filled. The element names are found via the inspect feature of the Chrome browser.
```python
# Initialize the driver
wd = webdriver.Chrome('chromedriver',options=options)
# Go to room booking website
wd.get('https://apps.rotman.utoronto.ca/rBookOSM/')

# Select the id box
id_box = wd.find_element_by_name('ctl00$ctl00$TemplateContent$mainbody$ui_UName')
# Send id information
id_box.send_keys('Dominic.Silk20@Rotman.Utoronto.Ca')
# Find password box
pass_box = wd.find_element_by_name('ctl00$ctl00$TemplateContent$mainbody$ui_PWord')
# Send password information
pass_box.send_keys('********')
```
![Image of login](https://github.com/silkdom/Corner-Office/blob/master/img/git1.png)

## Installation

Both the selenium package and ChromeDriver must be installed into the Colab notebook. 

```bash
!apt install chromium-chromedriver
!cp /usr/lib/chromium-browser/chromedriver /usr/bin
!pip install selenium

```

```python

from selenium import webdriver
options = webdriver.ChromeOptions()
options.add_argument('-headless')
options.add_argument('-no-sandbox')
options.add_argument('-disable-dev-shm-usage')

```

# Usage

Google sheets


The Rotman booking system is fairly simple but heavily constrained. 

