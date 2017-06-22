# -
爬取豆瓣电影top250及评分
import re
import urllib

def linkDouban():
    url_site = 'https://movie.douban.com/top250'
    b=[]
    for pageIndex in range(10):
        url=url_site+"?start=" + str(25*pageIndex)+"&filter="
        try: 
            response = urllib.request.urlopen(url,timeout=3)
            html=response.read().decode('utf-8')     
        except urllib.request.URLError:
            print(u"网络地址错误")
            exit()
        name=[]
        movieList = re.findall(r'<span\sclass="title">(.*?)</span>',html)
        top_num=1
        a=[0]*25
        
        for index, item in enumerate(movieList):
            if item.find("&nbsp")==-1:
                name.append("Top"+str(top_num+25*pageIndex)+" "+item+" "+"评分:")
                top_num+=1
            
        score = re.findall( r'<span class="rating_num" property="v:average">(.*?)</span>',html)
        with open('douban.txt','w',errors='ignore') as fp:
            fp.write(html)   
        
        for i in range(25):
            a[i]=name[i]+score[i]
        b=b+a
    print(b)
if __name__ == '__main__':
    linkDouban()
