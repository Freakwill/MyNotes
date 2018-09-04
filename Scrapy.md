# Scrapy

## directory structure


```tutorial/
    scrapy.cfg
    tutorial/
        __init__.py
        items.py
        pipelines.py
        settings.py
        spiders/
            __init__.py
# deploy configuration file
# project's Python module, you'll import your code from here
# project items definition file
# project pipelines file
# project settings file
# a directory where you'll later put your spiders
```

## process

* Request: client -> spider -> request (urls) -> internet
* Response: internet -> response -> spider -> parse -> data / urls -> client

## items

```python
name = scrapy.Field()
```

## selector
scrapy shell

```python
sel.xpath('//div[class="class-name"]') # sel -> sel
sel.xpath('//div[class="class-name"]/text()').extrac() # sel -> [str]
```
