# Indeed Helper

This is a project running Selenium with Python that is intended to help make your job applications
on indeed a little bit quicker. It will automatically fill out the fields for questions that it 
recognizes, alert you if there's an unknown field, and then finish the rest of the submission
process on it's own. Basically, once you start the program up you just have to listen for a beep
that will let you know it encountered a situation that it needs human help with - whether it be
a captcha or an unknown question. 

## Current stage
Currently working on getting it to wait after detecting an unknown question so that you have time
to answer it. After you click continue it will pick up right where it left off. 

This functionality is not yet working.

## Getting started

**Full Disclosure:** This application is still in DEVELOPMENT. It may not work correctly 95% of
the time, or even 50%.

- Make sure that you have Selenium installed (included in the requirements)
- Change the information in config.py to yours (there is a warning not to change below a certain line)
- Keep in mind this is not 100% automatic, it may require your assistance from time to time

## Setting it up

- In order to get this up and running with your information you will want to edit the config.py file
to be tailored for yourself.
- I've included a lot of the predefined settings commented next to the code lines to help get you started.

```python
account_email = ''
account_password = ''
```
These are your indeed account information to login

```python
job_titles = ['python developer',] 
job_location = '30340'
```
You can enter different job titles, and it will apply to everything that is an easy apply on indeed
that matches the criteria. Job titles are entered into a list variable. The zip code is for the
location that you want it to originate its search in.

```python
job_posting_age = '1'  # all, 1, 3, 7, 14 (these are days)
job_posting_type = 'all'  # all, full-time, contract, part-time, internship, temporary
job_posting_experience = 'all'  # all, entry, mid, senior
job_posting_distance = '25'  # default 5, 10, 15, 25, 50, 100 (these are miles)
```
These are the filters that indeed provides to narrow down your job search. Commented next to
the variables are the choices that you can use to tailor your search criteria. Be warned, if you
use an option that is not listed, it will not work.

```python
apply_linkedin = 'www.linkedin.com'
apply_personalwebsite = 'www.me.com'
apply_stateresident = 'yes'  # yes, no -- are you a state resident
apply_sponsorship = 'no'  # yes, no -- will you need work sponsorship
apply_relocate = 'yes'  # yes, no -- resident of ga or willing to relocate
apply_workauthorized = 'yes'  # yes, no -- authorized to work in us
apply_citizen = 'yes'  # yes, no -- us citizen or green card holder
apply_education = 'bachelor'  # other, highschool, associate, bachelor, master, doctorate
apply_leadershipdevelopment = '5'  # years of leadership development experience
apply_salary = '55k'  # what you want your salary to be
apply_gender = 'male' # male, female, decline
apply_veteran = 'no' # yes, no, decline -- veteran status
apply_disability = 'no' # yes, no, decline -- disability status
```
This is a list of questions encountered that required answers. Next to them in the commments are
the specific options that you can choose from to have the bot auto answer. If you see a -- in the
comment portion, that will explain exactly what the field is asking you for.

```python
apply_java = '1'
apply_aws = '0.5'
apply_python = '1'
apply_django = '1'
apply_php = '5'
apply_react = '0'
apply_node = '0'
apply_angular = '0'
apply_javascript = '5'
apply_orm = '0'
apply_sdet = '0'
apply_selenium = '0'
apply_testautomation = '0'
apply_webdevyears = '10'
```
Since I developed this bot and tailored it towards a software engineer position there were a lot of questions
regarding years of experience. That's what these variables are. Change as needed to fit you!

## Add new question recognition
**(WARNING: this involves core code changes / additions)**

If you encounter an unknown question you can add it so that if you re-encounter it the bot will automatically
answer it for you.

```python
if 'linkedin profile' == question_text.lower():
    question.find_element_by_css_selector('[id^="input-q"]').send_keys(apply_linkedin)
    time.sleep(questions_sleep)
```
This code is for a text box question. What you will have to do is change where it says 'linkedin profile' to
(a) keyword(s) that are included in the question. MAKE SURE that what you type in is all lowercase letters - if
it is not, it won't work. Where you see apply_linkedin - that is a variable that is found on config.py
so if you would like to set up another variable feel free to add it there. If you want your answer to be
hard coded just replace apply_linkedin with 'here is your answer' -- basically put your answer in quotes in 
place of the variable name.
You can copy, edit, and then pase this code as you see fit into the big if statement where the rest of the 
questions are.

```python
elif 'sponsorship' in question_text.lower():
    question.find_element_by_xpath(".//label/div[contains(text(), '" + apply_sponsorship.title() + "')]").click()
    time.sleep(questions_sleep)
```
This is the code for selecting a question that has options for its answer. Replace 'sponsorship' with your
question's keyword(s) making sure they are all lowercase. If you want to hardcode your answer, remove the 
" + apply_sponsorship.title() + " and replace it with 'your answer here'. **KEEP IN MIND** your answer MUST 
have the same capatilzation as the options on the website. You can copy, edit, and then pase this code as you see fit into the big if statement where the rest of the 
questions are.

```python
elif 'ethnicity' in question_text.lower():
    question.find_element_by_xpath(".//div/select/option[contains(text(), 'Two or More')]").click()
    time.sleep(questions_sleep)
```
This is the code for handling a question that has a select element as its answer. Replace 'ethnicity' with
your questions keyword(s) making sure they are all lowercase. In order to change the select option you will
change 'Two or More' to be what you need it to be. **KEEP IN MIND** your answer MUST have the same 
capatilzation as the options on the website.
You can copy, edit, and then pase this code as you see fit into the big if statement where the rest of the 
questions are.

---

## Authors

  - **Jon Robertson** -
    [JRRobertson](https://github.com/jrrobertson)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for
details
