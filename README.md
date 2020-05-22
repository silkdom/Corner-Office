# Corner-Office

## Motivation
The MMA program at Rotman is intensive, and thus I spend most of my day every day at uni. When thinking about what made my time at Rotman better/worse, the study room situation stood out! The rooms are often completely booked especially during peak times, which is frustrating for our perpetual groupwork assignments. Rooms can be booked two-days in advance, starting from 12:00AM. In order to stand a chance of booking a room, especially the 'best' rooms, you currently have to wait for the clock to strike midnight. 

What are the 'best' rooms you may say? In my opinion they are those with windows, are close to the water fountain/bathroom, and are spacious. Of all the rooms at Rotman, the corner room on the 3rd floor (3064) can be seen as the holy grail in this regard. The superiority of this room is no secret accross Rotman, and is often booked by 12:01AM. Therefore, I created this project to alleviate my room booking issues and lay claim to the corner office!

## How it works

Code can be found **here!**

The web-based automation package selenium was chosen alongside a WebDriver to do the heavy work. Once the ChromeDriver is initialized, the login website is input and, login credentials are filled, and the login button clicked. The id and password element names are found via the inspect feature of the Chrome browser.
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

# Find login button
login_button = wd.find_element_by_id('ctl00_ctl00_TemplateContent_mainbody_ui_SubmitButton')
# Click login
login_button.click()
```
![Image of login](https://github.com/silkdom/silkdom.github.io/blob/master/assets/logo.JPG)

On the resultant page the desired booking date/time/duration is entered, and the reserve room button clicked. The inspect features XPath copy had to be applied for the button click.

```python
select_dates = Select(wd.find_element_by_name('ctl00$ctl00$body$mainbody$ui_DateList'))
select_dates.select_by_visible_text('date input')

select_time = Select(wd.find_element_by_name('ctl00$ctl00$body$mainbody$ui_TimeDropDownList'))
select_time.select_by_visible_text('time')

select_duration = Select(wd.find_element_by_name('ctl00$ctl00$body$mainbody$ui_DurationDropDownList'))
select_duration.select_by_visible_text('duration')

wd.find_element_by_xpath('//*[@id="body_mainbody_ui_RoomGridView"]/tbody/tr[33]/td[3]/a').click()
```

![Image of login](https://github.com/silkdom/Corner-Office/blob/master/img/git_2.png)

Finally, the emails of the two group members required to book a room are input and the submit request button is clicked.

```python
emails = wd.find_element_by_id('Members')
emails.send_keys('emails')

comment = wd.find_element_by_id('rComment')
comment.send_keys('test')

submit_request = wd.find_element_by_id('BookPanelOkButton')
submit_request.click()
```

![Image of login](https://github.com/silkdom/Corner-Office/blob/master/img/git_3.png)


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

## Usage

For this project to be worth anything, the process has to be both faster and more conveniant than manual entry. Two main features were added to ensure this; Google sheets integration and timing implementation. The combination of these features facilitates simple and efficien operation (can also be done on mobile). 

### Timing

So that I wouldn't have to wait until midnight each night (making the project redundant), timing implementation was vital. Although it is not the most elegant solution (versus cronjob or AWS deployment), it works well. The user can run the code prior to midmight, as it will not proceed until 12:00:01 on the specified date. 

```python
# Start at 1 second past midnight
target_time = datetime(2020, month, day, 7, 0, 1)
# +7 hours difference

# Don't proceed until 12:00:01
while datetime.now() < target_time:
  time.sleep(10)
print('continue with code')
```

### Google Sheets

Google sheets is important as it facilitates easier credential and date/time/duration entry. The user can input the required fields in the user friendly excel sheet. Rather that applying the Google Sheets API, a simpler approach was taken where the pandas read_csv function allows for easy connection.


```python
import pandas

googleSheetId = '1qhITsQa2TsThnGsW5EAdrIBT8rdrC20pndANeat04QY'
worksheetName = 'Sheet1'
URL = 'https://docs.google.com/spreadsheets/d/{0}/gviz/tq?tqx=out:csv&sheet={1}'.format(
	googleSheetId,
	worksheetName
)

df = pandas.read_csv(URL)
```
![Image of login](https://github.com/silkdom/Corner-Office/blob/master/img/git_4.png)

## Results

Picture of corner room

## Future work

Deployment on AWS Lambda or as a cronjob on Heroku
