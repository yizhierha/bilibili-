# bilibili-视频信息爬取
## 爬取过程：因为哔哩哔哩是动态网页。所以需要爬取更精确的url；再进行视频信息爬取，包括标题，点赞数，硬币数，作者ID等信息；再将信息存放到csv文件或txt文件中。
### 1. 爬取url
      def professional_link(url,type):
        headers = {
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.25 Safari/537.36      Core/1.70.3754.400 QQBrowser/10.5.4020.400"
        }
        response = requests.get(url, headers=headers).content.decode('utf-8')
        links = re.findall(r'<li class="video-item matrix".*?>.*?<a href="//www.bilibili.com/video/(.*?)?from.*?" title.*?>.*?</a>.*?       </li>',response, re.DOTALL)
        for link in links:
          links_new = re.sub('\?', '', link)
          links_news = 'https://www.bilibili.com/video/' + links_new
          professional(links_new, headers,type)
### 2. 
