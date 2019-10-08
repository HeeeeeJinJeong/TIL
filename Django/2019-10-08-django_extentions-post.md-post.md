# django extentions

- pip install django-extentions
- settings.py
```python
INSTALLED_APPS = [
    ...
    'django_extentions',
]

GRAPH_MODELS = {
    'all_applications': True,
    'group_models': True,
}
```
## 추가 모듈 설치
### MAC
- xcode-select --install
- brew install graphviz
- pip install --install-option="--include-path=/usr/local/include/" --install-option="--library-path=/usr/local/lib/" pygraphviz

### Ubuntu
- sudo apt-get install python-dev graphviz libgraphviz-dev pkg-config
- pip install pygraphviz

### 그래프 생성 명령어
- 전체 모델에 대한 그래프 출력<br>
python manage.py graph_models -a -g -o model_graph.png

- 특정 앱에 대한 그래프 출력<br>
python manage.py graph_models [앱 이름] -o model_graph.png
