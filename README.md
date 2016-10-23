#Multimedia Datasets (Trade-Offs Optimisation in RTB)

This data description contains two parts:
* Overview of the dataset
* Design of data crawler

## Overview:
This dataset aims to investigate the trade-off among stakeholders in Real-Time bidding. It contains 7,273 unique webpages from YouTube and 5, 307 unique webpages from AOL, which we collected  over the period from Sep 6th, 2016 to Sep 9th, 2016 in Singapore. As we focus on single slot ad displaying, each webpage in our dataset contains only one ad slot. Since YouTube and AOL adopt different ad-networks, we store the collected data separately. 

Both Youtube dataset and AOL dataset have two folders and two .json files:
*       ./webpage_image/: contains all the webpage-images under the corresponding platform
*       ./ad_image/: contains all the ad-images in the displayed webpages. 
*       ./webpage.json: contains textual information of collected webpages
*       ./ad.json: contains textual information of collected ads within each displayed webpage

For ./webpage.json, the textual information of each webpage is:
*     webpage_ID:
*     webpage_url:
*     webpage_title:
*     webpage_keywords: which can be found from the source of the webpage
*     webpage_description: which can be found from the source of the webpage
*     webpage_text: we collect all the text within the webpage
*     webpage_ad_id: which can be used to find the displayed ad from ./ad.json file
*     ad_location: the location of displayed ads

For ./ad.json, the textual information of each ad is:
*     ad_ID:
*     ad_URL:
*     ad_title:
*     ad_keywords: which can be found from the source of the ad landing webpage
*     ad_description: which can be found from the source of the ad landing webpage
*     ad_text: we collect all the text within the ad landing page
*     ad_location: the location of this ad

*Note that*: 
* 1) We set the chromedriver in the privacy model, which disables browsing history, web cache and data storage in the cookies so that the collected ads are not affected by the pervious page views.
* 2) In ./ad_image/ and ./ad.json , you may notice that some ads with different ad_IDs share the same content, this is because: a) those ads appeared from time to time.  b) ./ad_image/ and ./ad.json store visual and textual information of each displayed ad. In other words, they store the information of original ads for webpages. We did not, and we did not have to distinguish repetitively ads when in our data crawler.
 
## Data Crawler:
We implement the crawler with Python2.7 and chromedriver, and the source code can be found in: 
* ./youtube/collect_ad_youtube.py
* ./aol/collected_ad_aol.py

To help you better understand our code, I will explain three important variables:
* collecting_webpage_urls: store the webpage URL candidates that is under collecting;
* to_be_colleccted_webpage_urls: store the new webpage URL candidates;
* collected_webapge_urls: store all the collected webpage URLs. 

We design our crawler as follows: 
* Step1: initialize some variables, such as starting chromedriver, 
**      setting collecting_webpage_urls as seed_url
**      setting to_be_colleccted_webpage_urls as empty, 
**      setting collected_webapge_urls as empty. 

* Step2:for a webpage URL in colleccting_webpage_urls, in order to remove repetitive webpages , we first check whether it is in collected_webpage_urls. If yes, select the next webpage URL candidate. If not, go to step 3. Once we have traversed colleccting_webpage_urls, we will set it as to_be_colleccted_webpage_urls and set to_be_colleccted_webpage_urls as empty. 

* Step3: for a new webpage URL, we first add it to collected_webapge_urls. And then we select all new URLs that belong to Youtube/AOL within the webpage and add them to  to_be_colleccted_webpage_urls. If there exist an ad in the webpage, go to Step 4. If not, go to step 2.

* Step4: For the detected ad, we collect the ad_image by cropping screen, and then we perform ‘clicking-ad’ action and transfer to the ad landing page. We collect textual information of the ads in the ad landing page, and save them in ./ad.json. Then, go to Step5.

* Step5: we return to the original webpage, and collect the visual and textual information of the webpage, and save it to ./webpage_image/ and ./webpage.json.   

Note that: to ensure the diversity of webpages and banner ads, our seed URL must contain multiple categories of content. We set the seed URL as follows:
* YouTube: https:www.youtube.com/channels
* AOL: http://www.aol.com

## Datasets Download
[https://www.dropbox.com/s/hbbinjgbpe8of5g/RTBTradeoffsMultimediaDataset.7z?dl=0](https://www.dropbox.com/s/hbbinjgbpe8of5g/RTBTradeoffsMultimediaDataset.7z?dl=0)


## Contact 
- Chen Xiang: [chxiang@comp.nus.edu.sg](mailto:chxiang@comp.nus.edu.sg)
- Bowei Chen: [bchen@lincoln.ac.uk](mailto:bchen@lincoln.ac.uk) 
