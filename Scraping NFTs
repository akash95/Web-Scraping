from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from bs4 import BeautifulSoup as bs
import time
import os
import requests
   
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
url = 'https://opensea.io/collection/boredapeyachtclub?search[sortAscending]=false&search[stringTraits][0][name]=Fur&search[stringTraits][0][values][0]=Solid%20Gold'


def p2():
    
    #Part 1
    driver = webdriver.Chrome(executable_path='chromedriver.exe') #Need to make this path global instead of absolute before submission
    # driver = webdriver.Edge(executable_path='C:/Users/akash/Downloads/msedgedriver.exe')
    
    driver.get(url);
           
    time.sleep(5)
    for n in range(0,8):
        
        WebDriverWait(driver, 20).until(EC.visibility_of_element_located((By.XPATH, "//div[@class='sc-29427738-0 sc-e7851b23-1 dVNeWL hfa-DJE Asset--loaded']")))

        d = driver.find_elements_by_xpath("//a[@class='sc-1f719d57-0 fKAlPV Asset--anchor']")
        driver.execute_script("arguments[0].scrollIntoView(true);", d[n])

        d[n].click()
        
        time.sleep(10)

  
        # Creating a new file to store the contents of the page
        review_file = open('bayc_'+str(n+1)+'.htm', 'wb')
        page = driver.page_source.encode('utf-8')

        review_file.write(page)
 
        # Closing the file
        review_file.close()

        driver.back()
        time.sleep(5)
    
    driver.close()                                        



def p3():

    dic = {}
    actual_attributes = ['Background','Clothes','Earring','Eyes','Fur','Hat','Mouth'] #Total list of attributes
    final_list = []
    
    
    #Read from the html files downloaded in part 2
    for i in range(8):     

        file = open('bayc_'+str(i+1)+'.htm',"rb").read()
       
        # file = open('bayc_'+str(2)+'.htm',"rb").read() #Debug
        
        t = bs(file,'html.parser')
        attribute_key = [i.text for i in t.find_all('div',{'class':'Property--type'})]
        
        # print(len(attribute_key)) # For checking count of attributes
        
        
        attribute_value = [i.text for i in t.find_all('div',{'class':'Property--value'})]
        
        dic = {attribute_key[i]: attribute_value[i] for i in range(len(attribute_key))}
        
        for att in actual_attributes:
            if att not in dic.keys():
                dic[att] = 'None'
        
        dic['Name'] = t.find('h1').text.strip('#')
        
        final_list.append(dic) 
    
    
    #Insert the values to mongoDB collection
    import pymongo

    myclient = pymongo.MongoClient("mongodb://localhost:27017/")
    
    mydb = myclient['project_2']
    mycol = mydb['bayc']
    
    # filter_query =  filter_q
    # columns_query = col_q
    
    mydoc = mycol.insert_many(final_list)

#Function call 
p2()      
p3()
