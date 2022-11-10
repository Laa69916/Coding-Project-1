# Coding-Project-1
For this project we are going to create a python code thaat reads information from a website and then obtains the information and puts it into an excel sheet.

We are also going to include a python program that can download images from websites. We were unable to download all images and put it into an excel sheet so its a work in progress.


import requests, openpyxl

from bs4 import BeautifulSoup

from selenium import webdriver

import io

from PIL import Image

excel = openpyxl.Workbook()

print(excel.sheetnames)

sheet = excel.active

sheet.title = 'Top Rated Fighters'

print(excel.sheetnames)

sheet.append(['Rank', 'Name'])

PATH = 'C://Users//xxxxx//Desktop//Python Codes//chromedriver.exe'

wd = webdriver.Chrome(PATH)

image_url = 'https://images.tapology.com/headshot_images/41705/icon/alexander-volkanovski.jpg?1567271247'

def download_image(download_path, url, file_name):

    image_content = requests.get(url).content
    
    image_file = io.BytesIO(image_content)
    
    image = Image.open(image_file)
    
    file_path = download_path + file_name

    with open(file_path, "wb") as f:
    
        image.save(f, "JPEG")

    print("success")

download_image('', image_url, "test.jpg")

try:
    source = requests.get('https://www.tapology.com/rankings/current-top-ten-best-pound-for-pound-mma-and-ufc-fighters')
    
    source.raise_for_status()

    soup = BeautifulSoup(source.text, 'html.parser')
    
#movies is assigned ; whenever specifing a class, make sure to include an underscore(_)

    movies = soup.find('ul', class_="rankingItemsList").find_all('li')

    for movie in movies:

        rank = movie.find('p', class_="rankingItemsItemRank").get_text(strip=True)
        
        name = movie.find('img', class_='countryFlag mini').get_text(strip=True)


        print(rank, name)
        
        sheet.append([rank, name])


except Exception as e:

    print(e)

excel.save('Fighters.xlsx')

