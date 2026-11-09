# 2. SExtractor: структура каталогов, конфигурация и запуск

Рекомендуется отделять исходные данные, конфигурационные файлы и результаты обработки. Примерная структура рабочего каталога:

```text
project/
├── data/
│   └── image.fits
├── config/
│   ├── sextractor/
│   │   ├── default.sex
│   │   └── default.param
│   └── scamp/
│       └── scamp.conf
└── output/
```

> ⚠️ **Внимание.** Все последующие команды будут выполняться из корня проекта.

## 3.1. Конфигурационный файл SExtractor

Конфигурационный файл SExtractor (`default.sex`) создается следующим образом:

```bash
sex -d > config/default.sex
```


Ниже приведен примерный вид файла конфигурации (`default.sex`):

<details>
<summary>Нажмите, чтобы развернуть/свернуть файл конфигурации default.sex</summary>

[default.sex](default.sex ':include :type=code')

</details>

> ⚠️ **Внимание.** Для последующей обработки выходного каталога SExtractor в Scamp необходимо поставить параметр `CATALOG_TYPE` в значение `FITS_LDAC`.


---
Ниже приведено описание параметров в конфигурационном файле и их влияние на результат 


[smth](./default_sex_desk.md ':include')



## 3.2. Параметрический файл SExtractor

Файл с выходными параметрами SExtractor (`default.param`) создается следующим образом:

```bash
sex -dp > config/default.param
```

Ниже приведен примерный вид файла параметров (`default.param`):

<details>
<summary>Нажмите, чтобы развернуть/свернуть файл конфигурации default.sex</summary>

[default.param](default.param ':include :type=code')

</details>

[smth](./default_param_desk.md ':include')


## 3.3. Запуск SExtractor

```bash
sex data/image.fits -c config/default.sex
```

При необходимости имя выходного каталога можно переопределить:

```bash
sex data/image.fits -c config/default.sex \
    -CATALOG_NAME catalogs/image.cat
```

Можно так же переопределить любые параметры:
```bash
sex data/image.fits -c config/default.sex \
    -CATALOG_NAME catalogs/image.cat \
    -ANALYSIS_THRESH 3.0 \
    -DETECT_MINAREA 5
```

В результате будет создан `catalogs/image.cat` в формате `ASCII_HEAD`.
