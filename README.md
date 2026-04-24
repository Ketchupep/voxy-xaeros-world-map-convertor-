# Voxy → Xaero's World Map Converter

**[English](#english) | [Русский](#русский)**

---

<a id="english"></a>
## 🇬🇧 English

### Description

A tool for transferring explored world data from the [Voxy](https://modrinth.com/mod/voxy) mod (LoD renderer) to [Xaero's World Map](https://modrinth.com/mod/xaeros-world-map) format. Designed for multiplayer players who want to switch map mods without losing their explored territory.

Voxy stores 3D voxel data in a RocksDB database (ZSTD-compressed). This tool reads that data, extracts the top visible block for each column (X, Z), and generates Xaero-compatible region files — essentially converting a 3D voxel cache into a 2D map.

### Features

- **GUI & CLI** — graphical interface with progress bar, or command-line mode
- **Fast numpy processing** — fully vectorized section processing, zero Python loops per 32³ section
- **Auto-detection** — automatically finds Voxy saves and Xaero directories (supports `.minecraft` and Modrinth App profiles)
- **Minecraft detection** — checks if Minecraft is running before accessing the database
- **Safe read-only access** — never modifies Voxy data

### Requirements

```
Python 3.10+
pip install rocksdict zstandard numpy
```

### Usage

**GUI** (double-click or run without arguments):
```bash
python voxy_to_xaero_v3.py
```

**CLI:**
```bash
python voxy_to_xaero_v3.py "path/to/.voxy/saves/<server>/<hash>/storage" "path/to/xaero/world-map/Multiplayer_<server>/null/mw$default"
```

**Test run (limited sections):**
```bash
python voxy_to_xaero_v3.py "path/to/storage" "path/to/xaero" --max-sections 500
```

### How it works

1. Opens the Voxy RocksDB database (read-only)
2. Reads block/biome ID mappings from compressed NBT entries
3. Iterates all level-0 sections (32×32×32 voxels, full resolution)
4. For each X/Z column, finds the topmost non-air block using numpy vectorization
5. Maps modern block names to legacy numeric IDs (1.12.2 format)
6. Generates `.zip` region files compatible with Xaero's World Map (format version 4)

### After conversion

1. Launch Minecraft with Xaero's World Map installed
2. Connect to the server
3. Open the full-screen map (default key: `` ` ``)
4. Map settings → **Reload All Regions**

### Limitations

- Block colors may slightly differ from native Xaero rendering (legacy ID mapping)
- Modded blocks are mapped to the closest vanilla equivalent
- Only the surface layer is converted (top-down view)
- Xaero's cave mode is not supported

---

<a id="русский"></a>
## 🇷🇺 Русский

### Описание

Инструмент для переноса данных об исследованном мире из мода [Voxy](https://modrinth.com/mod/voxy) (LoD-рендерер) в формат [Xaero's World Map](https://modrinth.com/mod/xaeros-world-map). Создан для игроков на серверах, которые хотят сменить мод карты, не теряя исследованную территорию.

Voxy хранит 3D-воксельные данные в базе RocksDB (сжатие ZSTD). Этот инструмент читает эти данные, извлекает верхний видимый блок для каждой колонки (X, Z) и генерирует файлы регионов, совместимые с Xaero — по сути конвертирует 3D-воксельный кэш в 2D-карту.

### Возможности

- **GUI и CLI** — графический интерфейс с прогресс-баром или режим командной строки
- **Быстрая обработка через numpy** — полностью векторизованная обработка секций, ноль Python-циклов на секцию 32³
- **Автопоиск** — автоматически находит сохранения Voxy и папки Xaero (поддержка `.minecraft` и профилей Modrinth App)
- **Детект Minecraft** — проверяет, запущен ли Minecraft перед доступом к базе данных
- **Безопасный доступ** — база Voxy открывается только на чтение, данные не модифицируются

### Требования

```
Python 3.10+
pip install rocksdict zstandard numpy
```

### Использование

**GUI** (двойной клик или запуск без аргументов):
```bash
python voxy_to_xaero_v3.py
```

**CLI:**
```bash
python voxy_to_xaero_v3.py "путь/к/.voxy/saves/<сервер>/<хэш>/storage" "путь/к/xaero/world-map/Multiplayer_<сервер>/null/mw$default"
```

**Тестовый запуск (ограниченное число секций):**
```bash
python voxy_to_xaero_v3.py "путь/к/storage" "путь/к/xaero" --max-sections 500
```

### Как это работает

1. Открывает базу RocksDB мода Voxy (только чтение)
2. Читает маппинги ID блоков/биомов из сжатых NBT-записей
3. Итерирует все секции уровня 0 (32×32×32 вокселей, полное разрешение)
4. Для каждой колонки X/Z находит верхний непрозрачный блок через numpy-векторизацию
5. Конвертирует строковые имена блоков в числовые ID (формат 1.12.2)
6. Генерирует `.zip` файлы регионов, совместимые с Xaero's World Map (формат версии 4)

### После конвертации

1. Запустите Minecraft с модом Xaero's World Map
2. Подключитесь к серверу
3. Откройте полноэкранную карту (клавиша `` ` `` по умолчанию)
4. Настройки карты → **Reload All Regions**

### Ограничения

- Цвета блоков могут немного отличаться от нативного отображения Xaero (маппинг на старые ID)
- Блоки из модов отображаются как ближайший ванильный аналог
- Конвертируется только поверхностный слой (вид сверху)
- Пещерный режим Xaero не поддерживается

---

### License / Лицензия

MIT
