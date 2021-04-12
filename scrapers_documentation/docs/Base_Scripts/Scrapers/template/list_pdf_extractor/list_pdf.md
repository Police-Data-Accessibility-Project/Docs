# Setup

1. Clone the repo, either via the command line, `git clone https://github.com/Police-Data-Accessibility-Project/Scrapers.git` or from the website.
2. `CD` into the `Scrapers` folder, and type `pip install -r requirements.txt` 
3. Copy the extractor version you need, and the `configs.py` to the `COUNTRY/STATE/COUNTY` that you created for the precinct.
4. For example, Alameda county, California, would be placed into the folder `Scrapers/USA/CA/alameda/`. 
   
   This **MUST** be placed within the `Scrapers` folder that you downloaded. See [here](https://github.com/Police-Data-Accessibility-Project/Scrapers/tree/master/USA/CA/alameda) for the example.

Open the `configs.py` file that you copied:
1. Set `webpage` to the page with the pdf lists

2. Open a few pdfs and get the common file path for them, and set that as `web_path`

3. Set the `domain` to the beginning of the document host.

4. On the page you want to scrape, open inspect element and using "Select an element", click the link to the pdf (once), and look at the element pane.

   If the `href` tag looks like the following, (without the domain, just a path), add the common portion of the path. In this case, it's `/Portals/24/Booking Log/` (Spaces *should* be properly dealt with in the script, but if not, just replace it with `%20`)


![image](https://user-images.githubusercontent.com/40151222/113303191-d5093200-92ce-11eb-8e42-0c23f70d9f47.png)

Also, if the `href` tag does not have a slash in front of it, like the following picture, please add one.

![image](https://user-images.githubusercontent.com/40151222/113487408-ffe9b680-9485-11eb-8942-b08fa7c1e528.png)

Make sure to add a slash to the end of the `domain`.
For example, `domain = "https://www.website.com"` would become `domain = "https://www.website.com/"`




 If the site has a set crawler time under it's `robots.txt`, set `sleep_time` to it's value. Otherwise, just leave it at `5`

If this does not make sense, try checking the comments within the code.
 Working example can be found [here](https://github.com/CaptainStabs/Scrapers/blob/master/USA/CA/alameda/alameda_scraper.py)

# Versions:
`list_pdf_extractor.py` : most basic of the scripts, mostly used for reference

`list_pdf_extractor_v2.py` : Uses imported `get_files` function. Useful for cases where a custom `get_files` is **not** needed. Function can be found [here](https://github.com/CaptainStabs/Scrapers/blob/master/common/bs_scrapers/get_files.py)

`list_pdf_extractor_v3.py` : Built off of V2, Allows for filtering links by common unwanted words. See [golden_west_scraper.py](https://github.com/CaptainStabs/Scrapers/blob/master/USA/CA/golden_west_college/golden_west_scraper.py) for working example.


This script has two functions, the first, `extract_info`, extracts the links containing documents, and saves the url and the document name to a file called `links.txt`
The second function, `get_files`, reads the link and name from `links.txt` and downloads the files.  


# More in depth explanations (Poorly explained, nerdy stuff)
 `extract_info` uses `urllib` to open the webpage, and then `BeautifulSoup4` to parse it. It then uses regex to find all links that end with pdf or doc. It needs a few lines to be replaced with regex.
