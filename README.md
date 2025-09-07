# Traffic Counter (YOLOv8 + BoT-SORT) — Jupyter + Poetry

Этот проект считает автомобили, пересекающие диагональную линию, с использованием **Ultralytics YOLOv8** и трекера **BoT‑SORT**. Также есть модуль OCR (EasyOCR) для распознавания номерных знаков и выгрузка результатов в CSV.

## 1) Системные зависимости

### macOS (Homebrew)
```bash
brew update
brew install ffmpeg
```

### Ubuntu/Debian
```bash
sudo apt-get update
sudo apt-get install -y ffmpeg libsm6 libxext6 libgl1 libglib2.0-0
```

> Эти пакеты нужны OpenCV для работы с видео и отображением.

## 2) Установка Poetry (если ещё нет)
Следуй инструкциям с официального сайта, либо:
```bash
curl -sSL https://install.python-poetry.org | python3 -
# затем добавь poetry в PATH (см. вывод инсталлятора)
```


## 3) Создание и активация окружения

Убедись, что стоит нужная версия Python (например, 3.11). Затем:
```bash
poetry env use python3.11   # или свою версию
poetry shell                # опционально, можно и без этого
```

## 4) Установка зависимостей

### Основные
```bash
poetry add ultralytics opencv-python numpy pillow
poetry add filterpy lapx scipy
poetry add easyocr
```

Для CUDA (GPU, Linux/Windows:
```bash
poetry run pip install --index-url https://download.pytorch.org/whl/cu121 torch torchvision torchaudio
```

### Зависимости для ноутбуков (dev-группа)
```bash
poetry add --group dev jupyter ipykernel matplotlib pandas
```

## 5) Регистрация ядра Jupyter для этого окружения
```bash
poetry run python -m ipykernel install --user --name computer-vision-pet
```

## 6) Структура каталогов (минимум)

```
computer-vision-pet/
  data/
    Автотрафик.mp4           # входное видео
  notebooks/
    main_notebook.ipynb   # ноутбук с кодом
  pyproject.toml
  README.md
```

## 7) Запуск Jupyter

```bash
poetry run jupyter lab
# или
poetry run jupyter notebook
```

В интерфейсе выбери ядро **computer-vision-pet** (или то имя, которое указано в шаге 6).

## 8) Ключевые настройки кода

- **Диагональная линия** задаётся пропорциями от размеров кадра:
  ```python
  LINE_START_RATIO = (0.10, 0.05)  # (x_ratio, y_ratio)
  LINE_END_RATIO   = (0.95, 0.95)
  ```
  Эта линия хороша для конкретно взятого видео.


- **Классы детекции**: по умолчанию только `car` (COCO id=2).

- **Направление подсчёта**: берётся относительно вектора `line_start -> line_end`.
  ```python
  COUNT_DIRECTION_LEFT_TO_RIGHT = True  # считать только переход слева-направо
  ```

- **Выходы**: скрипт создаёт **новый** видеофайл `*_counted.mp4` и CSV в каталоге `data/`. Исходный файл не изменяется.

Удачи!
