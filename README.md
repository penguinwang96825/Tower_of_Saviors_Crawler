# Tower of Saviors Crawler
A note to build a web crawler for TOS by using beautifulsoup and selenium.
This [website](https://towerofsaviors.fandom.com/wiki/Maximum_Stats) shows the stats for a TOS card at its maximum level, including any bonuses gained from Amelioration, Dual Max, and/or Awakening Recall. The final dataframe, which is crawled from this website, can be used to compare cards.

## Import packages
```python
import pandas as pd
import numpy as np
import time
import requests
import bs4
from selenium import webdriver
from bs4 import BeautifulSoup
from tqdm.notebook import tqdm
```

## Main Code
```python
def get_tos_statistics_dataframe():
    driver_path = r"C:\Users\YangWang\Desktop\machineLearning\indiaNewsClassification\chromedriver.exe"
    driver = webdriver.Chrome(driver_path)
    time.sleep(2)
    url = "https://towerofsaviors.fandom.com/wiki/Maximum_Stats"
    driver.get(url)
    time.sleep(10)
    soup = BeautifulSoup(driver.page_source, "html.parser")
    table = soup.find(name="table", attrs={"class": "wikitable sortable jquery-tablesorter"})
    tbody = table.find(name="tbody")
    driver.quit()

    tos_stat = []
    for tr in tbody.find_all(name="tr"):
        tds = tr.find_all(name="td")
        index = tds[0].string
        card_name = tds[1].find(name="a").string
        star = tds[2].string
        attribute = tds[3].string
        race = tds[4].string
        level_max = tds[5].string
        exp_tier = tds[6].string
        exp_max = tds[7].string
        hp_max = tds[8].string
        attack_max = tds[9].string
        rcr_max = tds[10].string
        total_max = tds[11].string
        tos_stat.append([index, card_name, star, attribute, race, level_max, 
                         exp_tier, exp_max, hp_max, attack_max, rcr_max, total_max])
        
    df = pd.DataFrame(tos_stat)
    df.columns = [
        "index", "card_name", "star", "attribute", "race", "level_max", 
        "exp_tier", "exp_max", "hp_max", "attack_max", "rcr_max", "total_max"]

    return df
```

