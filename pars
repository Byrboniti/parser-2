import requests
from bs4 import BeautifulSoup
import lxml
from proxy import proxies
import json
import time
import random

headers = {
    "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.5060.134 Safari/537.36"
}
fests_url_list = []
fest_list_resalt = []
count = 0
for i in range(0,120,24):
    url = f"https://www.skiddle.com/festivals/search/?ajaxing=1&sort=0&fest_name=&from_date=4%20Aug%202022&to_date=&maxprice=500&o={i}&bannertitle=August"

    req = requests.get(url=url, headers=headers, )
    json_data = json.loads(req.text)
    html_response = json_data["html"]

    with open(f"data/index_{i}.html", "w", encoding="UTF-8-sig") as file:
        file.write(html_response)

    with open(f"data/index_{i}.html",  encoding="UTF-8-sig") as file:
        src = file.read()

    soup = BeautifulSoup(src,"lxml")

    cards = soup.find_all( class_= "btn card-btn ripple-btn lvl-2 block bold bg-darkgreen tc-white")

    for item in cards:
        fest_url = "https://www.skiddle.com/"+ item.get("href")
        fests_url_list.append(fest_url)

# collect fest info
for url in fests_url_list:
    count +=1
    req = requests.get(url=url, headers=headers)
    print(count)
    try:
        soup = BeautifulSoup(req.text,"lxml")
        fest_info_block = soup.find(class_="top-info-cont span span9 no-clear")
        fest_name = fest_info_block.find("h1").text

        # fest_date_block = soup.find(class_="MuiGrid-root MuiGrid-item MuiGrid-grid-xs-11 css-twt0ol")
        fest_data = fest_info_block.find("h3").text.strip()

        # print(fest_name)
        # print(fest_data)
        time.sleep(random.randrange(2, 4))

    except Exception as ex:
        print(ex)
    fest_list_resalt.append(
        {
            "Fest name: ": fest_name,
            "Fest data: ": fest_data
        }
    )
with open("data/Fest_list_resalt.json", "a",encoding="UTF-8-sig") as file:
    json.dump(fest_list_resalt, file, indent=4, ensure_ascii=False)
