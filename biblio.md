# Web Scraping API for Bibliotheque de la Ville de Québec

[https://www.bibliothequedequebec.qc.ca/](https://www.bibliothequedequebec.qc.ca/)

This RESTful API work with Python and Selenium, the requests are handled with Flask.

## Hosting the API
To self-host the API do the following steps, I consider that you already have PIP and Python.
1. Install flask with PIP  `pip install flask`
2. Install the requirements  with PIP `pip install -r requirements.txt`
3. Download [firefox](https://www.mozilla.org/en-US/firefox/all/#product-desktop-release) and [geckodriver](https://github.com/mozilla/geckodriver/releases), this API works with the Firefox browser.
4. Download my Python script (app.py)
5. Unzip the gecko driver folder and specify its directory in the 'app.py' file, specifically at line 29.
6. Run this command in the directory of the app.py `flask run` so it is now running on port 5000

## Requests

For a request you will need to POST json data, here an example in python
```
api_url = 'http://127.0.0.1:5000/login'

data_to_extract = ['user_name']

# JSON data containing the username, password, and data_to_extract
data = {'username': username, 'password': password, 'data_to_extract': data_to_extract}

# Send a POST request to the API
response = requests.post(api_url, json=data)
```


The **api_url** is the one of the flask app which is by default localhost:5000 .
In **data_to_extract** you can enter the you wish to get. We recommend only one field at the time because I can't guarantee that it will output in order.
Replace **username** with the library card code which is almost always a 6 digit number. 
Replace the **password** with the pin of the card. For the ones I’ve seen it was 4 digit pin, but I can’t guarantee it always that.

See rqts.py for a complete example of the a request.

## Selenium 
There are two ways to use selenium and Firefox, both methods work well, but your choice may depend on your specific requirements.
#### Local Firefox with gecko driver
To use this method, just take the default script in the GitHub repository and add you gecko driver path to the script at line **29** 
#### Selenium grid standalone Firefox
You can use selenium grid if you think having big traffic. This way you just need to host the Docker image on a server. Use this image for Firefox standalone: https://hub.docker.com/r/selenium/standalone-firefox 
You will also need to modify the code (replace the part under "Local or Grid") and firefox options. To handle the grid browser you will need to add the url of the server which is usually on port 4444. 

```
    # Set up Firefox options
    options = FirefoxOptions()
    options.add_argument('--ignore-ssl-errors=yes')
    options.add_argument('--ignore-certificate-errors')
    options.add_argument("--headless")

	# Local or Grid
    driver = webdriver.Remote(
    command_executor='http://localhost:4444/wd/hub',
    options=options
   
```


## Troubleshooting 
The majority of problems are cause by not enough time given to load the web pages. I don’t know the exact cause but I know adding delay solve some problems.

## Data

Here you have all the possible requests you can do and information you will get from it.

| Value Name            | Meaning                                                                    | Response                                         |
| --------------------- | -------------------------------------------------------------------------- | ------------------------------------------------ |
| total_prets           | Total of actual books borrowed                                             | Numerical value: 0, 1, 2, ...                    |
| docs_retard           | Total of actual books borrowed overdue                                     | Numerical value: 0, 1, 2, ...                    |
| email                 | Get the email associaoted to the account if exist                          | example@example.com                              |
| user_name             | Get last and first name of the user                                        | Smith, Jack                                      |
| biblio_pref           | The library set as preference by the user (most of the time the nearest)   | MAIS    --  4 letter code see [[#Library Table]] |
| frais_table           | Table in html of all the things the user should pay                        | Raw html table                                   |
| payment_history_table | Table of the history of what the user already paid. (not always available) | Raw html table                                   |




## Library Table

| Code | Library                  |
| ---- | ------------------------ |
| ALIE | Aliette-Marchand         |
| BONP | Bon-Pasteur              |
| CHAM | Champigny                |
| CHAH | Charles-H.-Blais         |
| CHRY | Chrystine-Brouillet      |
| CLAI | Claire-Martin            |
| COLL | Collège-des-Jésuites     |
| DUCH | Du-Chemin-Royal          |
| ETIE | Etienne-Parent           |
| FELI | Félix-Leclerc            |
| FERN | Fernand-Dumont           |
| JEAN | Jean-Baptiste-Duberger   |
| LEBO | Lebourgneuf              |
| LETO | Le Tournesol             |
| MAIS | Maison de la littérature |
| MARI | Marie-Claire-Blais       |
| MONI | Monique-Corriveau        |
| NEUF | Neufchâtel               |
| PAUL | Paul-Aimé-Paiement       |
| ROGE | Roger-Lemelin            |
| ROMA | Romain-Langlois          |
| STAL | Saint-Albert             |
| STAN | Saint-André              |
| STCH | Saint-Charles            |
| STSA | Saint-Sauveur            |
