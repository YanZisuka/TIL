# Python_04

## Python을 활용한 데이터 수집

>   Requests

-   Requests is an elegant and simple HTTP library for Python, built for human beings.
-   https://docs.python-requests.org/en/latest/#

>   BeautifulSoup

-   BeautifulSoup is a Python library for pulling data out of HTML and XML files.
-   https://www.crummy.com/software/BeautifulSoup/bs4/doc/

```python
import requests
from bs4 import BeautifulSoup

URL = 'https://finance.naver.com/sise/'

response = requests.get(URL).text
data = BeautifulSoup(response, 'html.parser')

print(type(response), type(data))

kospi = data.select_one('#KOSPI_now')

print(kospi.text)
```

>   API(Application Programming Interface)

-   **컴퓨터나 컴퓨터 프로그램 사이의 연결**
-   일종의 소프트웨어 인터페이스이며 다른 종류의 소프트웨어에 서비스를 제공
-   사용하는 방법을 기술하는 문서나 표준은 API 사양/명세 (specification)