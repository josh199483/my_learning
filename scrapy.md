# 開始scrapy
scrapy startproject myScrapy
cd myScrapy 
tree 可看專案結構
```bash
scrapy shell <url> #可產生一個python互動式介面，並且可爬取該網頁
scrapy parse --spider=basic(爬蟲的名字) <url> #debug時很好用，可以看到爬取的結果，類似log檔
scrapy crawl basic -o basic.json或csv #把爬取的結果存入檔案
scrapy genspider -t crawl <新建的爬蟲名稱> <domain name> #會產生一個爬蟲py檔
scrapy crawl basic -s CLOSESPIDER_ITEMCOUNT=90 #有時為了快速測試爬蟲，可用此參數就只會爬到90筆items
scrapy crawl basic -a file=another_todo.csv -o out.csv #可在要爬蟲時決定用哪個csv的資料來爬取
# 常用的一些settings command
scrapy settings --get CONCURRENT_REQUESTS
```