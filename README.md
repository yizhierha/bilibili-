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
### 2. 提取视频信息
      def professional(link, headers,type):
            url = "https://api.bilibili.com/x/web-interface/view?&bvid="+link
            response = requests.get(url, headers).content.decode('utf-8')
            content = json.loads(response)
            # 利用json提取视频信息
            title = content['data']['title']
            author = content['data']['owner']['name']
            info = content['data']['desc']
            info = re.sub(r'\n', '', info)
            aid = 'av' + str(content['data']['stat']['aid'])
            view = content['data']['stat']['view']
            coin = content['data']['stat']['coin']
            share = content['data']['stat']['share']
            like = content['data']['stat']['like']
### 3. 将视频信息存放到列表中，方便存放到csv文件中
       # 将信息存放到列表中
       bilibilis = list()
       values = {
           'title': title,
           'author': author,
           'info': info,
           'aid': aid,
           'view': view,
           'coin': coin,
           'share': share,
           'like': like
       }
       bilibilis.append(values)
       print(bilibilis)
       # 调用csv，将信息放入csv文件中
       bilibili_excel(type, bilibilis)
### 4. 将视频信息放入csv文件中
      def bilibili_excel(type, bilibilis):
          # 编辑csv文件标题
          H = "bilibili" + type + '信息.csv'
          # 编辑表头信息
          headers = ['title', 'author', 'info', 'aid', 'view', 'coin', 'share', 'like']
          # 存放到csv文件中
          with open(H, "a+", encoding="utf-8-sig", newline="")as f:
              writer = csv.DictWriter(f,headers)
              writer.writeheader()
              writer.writerows(bilibilis)
### 5. 可以使用多线程使爬取速度更快
      # 多线程爬取简历
      def jianli():
          for i in range(1, 40):
              url = 'https://search.bilibili.com/all?keyword=简历&page=%d' % i
              professional_link(url, '简历')

      # 多线程爬取简历模板
      def jianlimoban():
          for i in range(1, 40):
              url = 'https://search.bilibili.com/all?keyword=简历模板&page=%d' % i
              professional_link(url, '简历模板')

      # 多线程爬取面试
      def mianshi():
          for i in range(1, 40):
              url = 'https://search.bilibili.com/all?keyword=面试&page=%d' % i
              professional_link(url, '面试')

      # 多线程爬取实习
      def shixi():
          for i in range(1, 40):
              url = 'https://search.bilibili.com/all?keyword=实习&page=%d' % i
              professional_link(url, '实习')

      # 多线程爬取找工作
      def zhaogongzuo():
          for i in range(1, 40):
              url = 'https://search.bilibili.com/all?keyword=找工作&page=%d' % i
              professional_link(url, '找工作')

      # 多线程爬取笔试
      def bishi():
          for i in range(1, 40):
              url = 'https://search.bilibili.com/all?keyword=笔试&page=%d' % i
              professional_link(url, '笔试')

      # 多线程爬取职场
      def zhichang():
          for i in range(1, 40):
              url = 'https://search.bilibili.com/all?keyword=职场&page=%d' % i
              professional_link(url, '职场')

      # 多线程
      def main():
          t1 = threading.Thread(target=jianli)
          t2 = threading.Thread(target=jianlimoban)
          t3 = threading.Thread(target=mianshi)
          t4 = threading.Thread(target=shixi)
          t5 = threading.Thread(target=zhaogongzuo)
          t6 = threading.Thread(target=bishi)
          t7 = threading.Thread(target=zhichang)

          t1.start()
          t2.start()
          t3.start()
          t4.start()
          t5.start()
          t6.start()
          t7.start()
## 注：哔哩哔哩可以反爬，所以建议使用IP代理，否则可能会被禁止访问。

