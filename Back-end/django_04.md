# Django_04

## Form

-   Form은 Django의 유효성 검사 도구 중 하나로 **외부의 악의적 공격 및 데이터 손상에 대한 중요한 방어 수단**
-   Django는 Form과 관련한 **유효성 검사를 단순화하고 자동화할 수 있는 기능을 제공하여** 개발자로 하여금 직접 작성하는 코드보다 더 안전하고 빠르게 수행하는 코드를 작성할 수 있게 함
-   Django는 form에 관련된 작업의 아래 세 부분을 처리
    -   렌더링을 위한 데이터 준비 및 재구성
    -   데이터에 대한 HTML form 생성
    -   클라이언트로부터 받은 데이터 수신 및 처리

### Form Class

-   우리는 지금까지 HTML form, input을 통해서 사용자로부터 데이터를 받음
-   이렇게 직접 사용자의 데이터를 받으면 입력된 데이터의 유효성을 검증하고, 필요시에 입력된 데이터를 검증 결과와 함께 다시 표시해야 함
    -   **사용자가 입력한 데이터는 개발자가 요구한 형식이 아닐 수 있음을 항상 생각해야 함**
-   이렇게 사용자가 입력한 데이터를 검증하는 것을 '유효성 검증'이라고 하는데, 이 과정을 코드로 모두 구현하는 것은 많은 노력이 필요한 작업임
-   Django는 이러한 과중한 작업과 반복 코드를 줄여줌으로써 이 작업을 훨씬 쉽게 만들어 줌
    -   Django Form

#### 사용방법

-   Django Form 관리 시스템의 핵심
-   Form 내 field, field 배치, 디스플레이 widget, label, 초기값, 유효하지 않은 field에 관련된 에러 메시지를 결정

```python
# articles/forms.py

from django import forms

class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField()
```



-   Model을 선언하는 것과 유사하며 같은 필드타입을 사용 (또한, 일부 매개변수도 유사)
-   forms 라이브러리에서 파생된 Form 클래스를 상속받음

```python
# articles/views.py

from .forms import Articleform

def new(requests):
    form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/new.html', context)
```



#### Form rendering options

1.   `as_p()`
     -   각 필드가 `<p>`태그로 감싸져서 렌더링 됨
2.   `as_ul()`
     -   각 필드가 `<li>`태그로 감싸져서 렌더링 됨
     -   `<ul>`태그는 직접 작성해야 함
3.   `as_table()`
     -   각 필드가 `<tr>`태그로 감싸져서 렌더링 됨
     -   `<table>`태그는 직접 작성해야 함

```django
<!-- new.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>NEW</h1>
  <hr>
  {{ form.as_p }}
  <a href="{% url 'articles:index' %}">back</a>
```



#### HTML input 표현 방법

1.   Form fields
     -   input에 대한 유효성 검사 로직을 처리하며 템플릿에서 직접 사용됨
2.   Widgets
     -   웹 페이지의 HTML input 요소 렌더링
     -   GET/POST 딕셔너리에서 데이터 추출
     -   widgets은 반드시 Form fields에 할당됨
     -   주의사항
         -   Form fields와 혼동되어서는 안됨
         -   Form fields는 유효성 검사를 처리
         -   Widgets은 웹페이지에서 input element의 단순한 렌더링 처리

```python
# articles/forms.py

from django import forms

class ArticleForm(forms.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField(widget=forms.Textarea)
```

```python
from django import forms

class ArticleForm(forms.Form):
    REGION_A = 'sl'
    REGION_B = 'pg'
    REGIONS_CHOICES = [
        (REGION_A, '서울'),
        (REGION_B, '판교'),
    ]
    
    title = forms.CharField(max_length=10)
    content = forms.CharField(widget=forms.Textarea)
    region = forms.ChoiceField(widget=forms.Select, choices=REGIONS_CHOICES)
```



## ModelForm

### ModelForm Class

-   Model을 통해 Form Class를 만들 수 있는 Helper
-   일반 Form Class와 완전히 같은 방식(객체 생성)으로 view에서 사용 가능

#### 사용방법

```python
# articles/forms.py

from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    
    class Meta:
        model = Article
        fields = '__all__'
        # exclude = ('title',)
```

-   forms 라이브러리에서 파생된 ModelForm 클래스를 상속
-   정의한 클래스 안에 Meta 클래스를 선언하고, 어떤 모델을 기반으로 Form을 작성할 것인지에 대한 정보를 Meta 클래스에 지정

#### Meta class

-   Model의 정보를 작성하는 곳
-   ModelForm을 사용할 경우 사용할 모델이 있어야 하는데 **Meta class가** 이를 구성함
    -   해당 Model에 정의한 field 정보를 Form에 적용하기 위함
-   [참고] Inner class (Nested class)
    -   클래스 내에 선언된 다른 클래스
    -   관련 클래스를 함께 그룹화하여 가독성 및 프로그램 유지관리를 지원 (논리적으로 묶어서 표현)
    -   외부에서 내부 클래스에 접근할 수 없으므로 코드의 복잡성을 줄일 수 있음
-   [참고] Meta data
    -   "데이터를 위한 데이터"
    -   ex. 사진 데이터 - 사진의 메타 데이터 (촬영시각, 렌즈, 조리개 값 등)

```python
# articles/views.py

from django.shortcuts import render, redirect
from .models import Article
from .forms import ArticleForm

def create(request):
    form = ArticleForm(request.POST)
    
    # 유효성 검사
    if form.is_valid():
        article = form.save()
        return redirect('articles:detail', article.pk)
        
    return redirect('articles:new')
```



#### is_valid() method

-   유효성 검사를 실행하고, 데이터가 유효한지 여부를 boolean으로 반환
-   데이터 유효성 검사를 보장하기 위한 많은 테스트에 `is_valid()`를 제공
-   [참고] 유효성 검사
    -   요청한 데이터가 특정 조건에 충족하는지 확인하는 작업
    -   데이터베이스 각 필드 조건에 올바르지 않은 데이터가 서버로 전송되거나 저장되지 않도록 하는 것

#### The save() method

-   Form에 바인딩 된 데이터에서 데이터베이스 객체를 만들고 저장
-   ModelForm의 하위(sub) 클래스는 기존 모델 인스턴스를 키워드 인자 **instance로** 받아들일 수 있음
    -   이것이 제공되면 `save()`는 해당 인스턴스를 수정 (update)
    -   제공되지 않은 경우 `save()`는 지정된 모델의 새 인스턴스를 만듦 (create)
-   Form의 유효성이 확인되지 않은 경우 (hasn't been validated) `save()`를 호출하면 `form.errors`를 통해 에러 확인 가능 

```python
# articles/views.py

from django.shortcuts import render, redirect
from .models import Article
from .forms import ArticleForm


def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST)
    
        # 유효성 검사
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
        
    else:
        form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/create.html', context)


def update(request, pk):
    if request.method == 'POST':
        article = Article.objects.get(pk=pk)
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
    else:
        article = Article.objects.get(pk=pk)
        form = ArticleForm(instance=article)
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/update.html', context)
```



## Form & ModelForm 비교

|                             Form                             |                          ModelForm                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| 어떤 Model에 저장해야 하는지 알 수 없으므로 유효성 검사 이후 `cleaned_data` 딕셔너리 생성 | Django가 해당 model에서 양식에 필요한 대부분의 정보를 이미 정의함 |
| `cleaned_data` 딕셔너리에서 데이터를 가져온 후 `.save()` 호출해야 함 | 어떤 레코드를 만들어야 할 지 알고 있으므로 바로 `.save()` 호출 가능 |
|         Model에 연관되지 않은 데이터를 받을 때 사용          |                                                              |



## Widget

```python
# articles/forms.py

from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        widget=forms.TextInput(
        	attrs={
                'class': 'my-title',
                'placeholder': 'Enter the title',
            }
        ),
        error_messages={
            'required': 'Please fill your content',
        },
    )
    
    class Meta:
        model = Article
        fields = '__all__'
        # exclude = ('title',)
```



## Rendering fields manually

### Rendering fields manually

```django
<!-- create.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>NEW</h1>
  <hr>
  <form action="" method="POST">
      {% csrf_token %}
      <div>
          {{ form.title.errors }}
          {{ form.title.label_tag }}
          {{ form.title }}
      </div>
      <div>
          {{ form.content.errors }}
          {{ form.content.label_tag }}
          {{ form.content }}
      </div>
  <input type="submit">
  </form>
{% endblock content %}
```



### Looping over the form's fields

```django
<!-- create.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>NEW</h1>
  <hr>
  <form action="" method="POST">
      {% csrf_token %}
      {% for field in form %}
        {{ field.errors }}
      	{{ field.label_tag}}
      	{{ field }}
      {% endfor %}
  <input type="submit">
  </form>
{% endblock content %}
```

