# 4. SCAMP: структура каталогов, конфигурация и запуск

SCAMP использует каталоги `FITS_LDAC`, созданные SExtractor. Рабочая структура каталогов может выглядеть так:

```text
project/
├── data/
│   └── image.fits
├── config/
│   └── scamp.conf
├── catalogs/
│   └── image.cat
├── headers/
└── src/
    └── scamp/          # клон репозитория SCAMP, если сборка велась вручную
```

Каталог `src/scamp` содержит исходники и используется только для сборки. Для запуска вызывается установленный бинарник `scamp` из `$PATH`, а не файл из `src/scamp`.

## 4.1. Базовый конфигурационный файл SCAMP

Пример `config/scamp.conf`:

```text
GROUP_RADIUS     1.0

ASTREF_CATALOG   2MASS
ASTREF_BAND      DEFAULT

MATCH            Y
PIXSCALE_MAXERR  1.2
POSANGLE_MAXERR  5.0
POSITION_MAXERR  1.0

SOLVE_ASTROM     Y
DISTORT_DEGREES  3
CENTROID_KEYS    XWIN_IMAGE,YWIN_IMAGE
CENTROIDERR_KEYS ERRAWIN_IMAGE,ERRBWIN_IMAGE,ERRTHETAWIN_IMAGE
PHOTFLUX_KEY     FLUX_AUTO
PHOTFLUXERR_KEY  FLUXERR_AUTO

MERGEDOUTCAT_TYPE  FITS_LDAC
MERGEDOUTCAT_NAME  catalogs/scamp_merged.cat
```

## 4.2. Запуск SCAMP

Для одного каталога:

```bash
scamp catalogs/image.cat -c config/scamp.conf
```

Для нескольких каталогов сформируйте список путей (по одному на строку) и передайте его через `@`:

```bash
scamp @catalog_list.txt -c config/scamp.conf
```

В результате будут созданы `.head`-файлы с обновлёнными WCS-ключами. Их можно применить к исходным FITS-файлам через MissFITS или передать в SWarp для сложения.

> 💡 **Справочный каталог.** По умолчанию SCAMP использует USNO-B1. При отсутствии сетевого доступа укажите локальный каталог: `ASTREF_CATALOG FILE` и `ASTREFCAT_NAME /path/to/catalog`.
