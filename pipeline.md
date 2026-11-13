# 4. Сборка конвейера

Минимальная последовательность обработки одного изображения:

```bash
# 1. Формирование каталога источников
sex data/image.fits -c config/default.sex

# 2. Вычисление астрометрического решения
scamp catalogs/image.cat -c config/scamp.conf

# 3. Применение заголовков (пример с MissFITS)
missfits data/image.fits image.head
```

После выполнения в заголовке `data/image.fits` будут обновлены ключи `CRVAL1`, `CRVAL2`, `CD1_1` и другие параметры WCS.

При неудовлетворительном результате на этапе SCAMP в первую очередь проверьте значения `FLAGS` в каталоге SExtractor. Объекты с сомнительными флагами (слившиеся, обрезанные краем кадра, содержащие битые пиксели) рекомендуется отфильтровать до передачи в SCAMP.

---

## Ссылки

- Документация SExtractor: [sextractor.readthedocs.io](https://sextractor.readthedocs.io/)
- Документация SCAMP: [scamp.readthedocs.io](https://scamp.readthedocs.io/)
- Исходный код SExtractor: [github.com/astromatic/sextractor](https://github.com/astromatic/sextractor)
- Исходный код SCAMP: [github.com/astromatic/scamp](https://github.com/astromatic/scamp)
- Homebrew: [brew.sh](https://brew.sh/)
