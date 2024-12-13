# Отчет по практической работе 2

Задание: 

1. Создать собственную тему на основе HTML, CSS, JS с использованием или без использования CSS-библиотек таких как Bootstrap, Bulma, фреймворков (например, Tailwind), JS-библиотек для разработки фронтэнда (например, React). 
2. На основе репозитория разработать пайплайн или набор пайплайнов (yml-файл) для тестирования и  сборки статики (HTML, CSS, JS) — фронтэнда сайта, а затем построения собственно самого сайта (интеграции контента в разметке Markdown в шаблон сайта) и деплоя его на GitHub Pages. 
3. Необходимо учесть, что HTML файлы должны валидироваться на корректность, минифицироваться, должна быть предусмотрена сборка с помощью PostCSS (см. репозиторий).
4. Дополнительным этапом (необязательным) реализуйте улучшение типографики содержимого сайта (например, используя этот инструмент). 
Требования: 

Минимальные требования к теме: 
1. наличие кастомного header, footer, а также стилизованной страницы для одной из секций (например, главной страницы). 
2. Обязательно добавьте метаданные сайта (название, описание, автор) (тег meta).
3. Сайт должен быть доступен по адресу GitHub Pages. Убедитесь, что все страницы и стили корректно отображаются.

## Ход работы

### Задание 1
Для создния собственной темы была создана директория statics со следующей структурой:
```bash
│   footer.html
│   header.html
│   main.html
│
└───css
        styles.css
```

Для корректного отображения темы были добавлены следующие параметры в файл-конфиг *mkdocs.yml*  
```yml
site_author: VLADIMIR
site_description: PRAC2 custom theme

theme:
  name: null
  custom_dir: 'statics'

extra_css:
  - css/stlyles.css 
```  
### Задание 2

Для  
>тестирования и  сборки статики (HTML, CSS, JS)  

Были добавлены следующие два шага в пайплайн *actions.yml*  
```yml
      - name: Run j2lint
        uses: ansible-actions/j2lint-action@v0.0.1
        with:
          target: "./statics"
      
      - name: Build
        run: mkdocs build
```
`ansible-actions/j2lint-action@v0.0.1` - данный этап проверяет jinja шаблоны на корректность, а `run: mkdocs build` собирает статику

### Задание 3 
Для того, чтобы  
>HTML файлы должны валидироваться на корректность  

Был добавлен следующий шаг в пайплайн *actions.yml*  
```yml
      - name: HTML5Validator
        uses: Cyb3r-Jak3/html5validator-action@v7.2.0
        with:
          root: ./site
```
Данный шаг проверит корректность статики в директории *./site*, куда билдится статика, созданная командой  
```bash
mkdocs build
```

## Вывод
В результате работы была добавлена кастомная тема для mkdocs на основе шаблонизатора jinja2, а также было добавлена два этапа в сборке:
1. Проверка jinja шаблонов
2. Проверка итоговой сборки статики