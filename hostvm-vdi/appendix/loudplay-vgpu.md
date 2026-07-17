# Loudplay-vGPU

## Описание продукта <a href="#description" id="description"></a>

### Назначение и состав <a href="#features-and-components" id="features-and-components"></a>

#### Loudplay

Обеспечивает возможность:

1. передавать качественную картинку на низкоскоростных каналах передачи данных от 100 кбит/с с потерями до 30-40%
2. быстрый отклик (сервер <6мс, клиент <15мс)
3. проброс (USB, веб-камер, аудио, микрофона, принтеров, гарнитур, файлов, тестового буфера обмена, оптимиз. проброс сканеров
4. Мониторинг 16 показателей
5. управление сервисами Loudplay (Битрейт, FPS, разрешение, автобитрейт)

Состав:

* Клиент Loudplay - клиентское приложение
* Стриминг-сервер Loudplay – серверное приложение
* Стриминг-сервис Loudplay – приложение для ВМ
* Сервер лицензий Loudplay

#### Forsite vGate

Обеспечивает возможность:

1. использовать vGPU на территории РФ.
2. мониторинга состояния ВМ, статуса протокола доставки рабочего стола Loudplay.
3. управления сервисами Nvidia.
4. быстрого подключения через протокол Loudplay к ВМ из административного веб интерфейса.

Состав:

* vGate Agent – клиентское приложение
* vGate Server – сервер лицензий

### Характеристики <a href="#specs" id="specs"></a>

Заявлена работа на актуальных и последних поколениях GPU

**Nvidia GRID:**

* Ada Lovelace: L2, L4, L20, L20 liquid cooled, L40, L40S RTX 5000 Ada, RTX 5880 Ada, RTX 6000 Ada
* Blackwell: RTX PRO 6000 Blackwell Server Edition, RTX PRO 4500 Blackwell Server Edition
* Ampere: A2, A10, A16, A40, RTX A5000, RTX A5500, RTX A6000
* Maxwell: M10
* Turing: T4, RTX 6000, RTX 6000 passive, RTX 8000, RTX 8000 passive
* Volta: V100.

**AIE (Time-Slice/MIG backend):**

* Blackwell: NVIDIA HGX B200, NVIDIA RTX PRO 6000 Blackwell SE, NVIDIA RTX PRO 4500 Blackwell SE
* Hopper: H800, H200, H200 NVL, H100, H20
* Ada Lovelace: L40, L40s, L20, L4, L2, RTX 6000 Ada, RTX 5880 Ada, RTX 5000 Ada
* Ampere: A800, A100, A40, A30, A16, A10, RTX A6000, RTX A5500, RTX A5000
* Turing: T4, RTX 6000 passive, RTX 8000 passive
* Volta: V100

**Типы драйверов:** Nvidia GRID, Nvidia AI Enterprise

**Поддержка серий:** 535 – 595

**Типы профилей Nvidia vGPU:** vWS, vAPP, vPC, vCS

### Совместимость <a href="#compatibility" id="compatibility"></a>

**Гипервизоры:**

* Citrix XenServer
* Microsoft Hyper-V
* HOSTVM
* VMware vSphere
* ROSA VIRTUALIZATION
* zVirt
* ПК СВ «Брест»
* VMmanager
* РЕД Виртуализация
* Альт Виртуализация
* ГОРИЗОНТ-ВС
* oVirt
* OpenStack
* Proxmox VE

**Гостевые ОС (ВРМ):**

* Windows Server 2019 - 2022
* Windows 10, 11
* Astra Linux 1.7, 1.8
* Ubuntu 20, 22, 24 LTS
* РЕД ОС 7.3, 8
* Alt Linux 10

**Клиентские ОС (на устройствах доступа):**

* Windows 7 и выше
* Astra Linux 1.7, 1.8
* Ubuntu 20, 22, 24 LTS
* РЕД ОС 7.3, 8
* Alt Linux 10

## Профили (редакции) и назначение <a href="#profiles" id="profiles"></a>

### vApps (Virtual Applications)

**Семейство vGPU типов:** A‑series

**Назначение:** Виртуальные приложения и сессионные рабочие столы (RDSH и аналоги).

**Преимущества:** Оптимизация под доставку приложений/сессионные сценарии.

**Ограничения/условия:** Ограничения vGPU Software по матрице поддержки (64‑бит guest OS, Wayland не поддерживается и др.).

### vPC (Virtual PC)

**Семейство vGPU типов:** B‑series

**Назначение:** Виртуальные бизнес‑десктопы (офисные нагрузки, браузер, HD‑видео).

**Преимущества:** Баланс UX и плотности; подходит для массового VDI.

**Ограничения/условия:** Ограничения vGPU Software по матрице поддержки.

### vWS (RTX Virtual Workstation)

**Семейство vGPU типов:** Q‑series

**Назначение:** Профессиональная графика/3D и «виртуальная рабочая станция».

**Преимущества:** Расширенные графические возможности для workstation‑класса.

**Ограничения/условия:** Ограничения vGPU Software по матрице поддержки.

### vCS (Virtual Compute Server) / vGPU for Compute

**Семейство vGPU типов:** C‑series («-C»)

**Назначение:** Виртуализированные вычисления (AI/ML/HPC).

**Преимущества:** MIG-backed: изоляция; для поддерживаемых GPU — варианты time‑sliced/MIG‑backed.

**Ограничения/условия:** С vGPU Software 16.0 compute вынесен в AIE; в AIE P2P CUDA transfers не поддерживаются на Windows.

## Общие ограничения vGPU Software (влияют на vApps/vPC/vWS) <a href="#restrictions" id="restrictions"></a>

### Гостевые ОС <a href="#guest-os" id="guest-os"></a>

Поддерживаются только 64‑бит гостевые ОС.

Источник NVIDIA: Support Matrix

### Wayland

Wayland не поддерживается (в ряде случаев доступен только console/CLI).

Источник NVIDIA: Support Matrix

### Compute (C‑series)

С 16.0 C‑серия недоступна в vGPU Software; compute — через AIE.

Источник NVIDIA: What's New vGPU 16.0

## Матрица технологий виртуализации NVIDIA (Ampere → Blackwell) <a href="#matrix" id="matrix"></a>

GRID = NVIDIA vGPU Software; AIE = NVIDIA AI Enterprise; MIG = Multi‑Instance GPU.

### GPU и доступные технологии <a href="#gpu-supported-features" id="gpu-supported-features"></a>

#### Ampere

**GPU (пример моделей):**

A2; A10/A16/A40; RTX A5000/5500/6000; A100

**GRID (vGPU Software) в списке Supported GPUs:**

Да

**GRID профили A/B/Q (vApps/vPC/vWS):**

Да (A/B/Q доступны для VDI/графики — по GPU/релизу)

**AIE vGPU for Compute Time-Sliced (профили «-C»):**

Да (AIE vGPU for Compute)

**MIG (аппаратная поддержка):**

Да только на A100, A30

#### Ada Lovelace

**GPU (пример моделей):**

L2/L4/L20/L40/L40S (и др. по релизам)

**GRID (vGPU Software) в списке Supported GPUs:**

Да

**GRID профили A/B/Q (vApps/vPC/vWS):**

Да (A/B/Q — по GPU/релизу)

**AIE vGPU for Compute Time-Sliced (профили «-C»):**

Да (AIE vGPU for Compute)

**MIG (аппаратная поддержка):**

Нет

#### Hopper

**GPU (пример моделей):**

H100/H200

**GRID (vGPU Software) в списке Supported GPUs:**

Нет

**GRID профили A/B/Q (vApps/vPC/vWS):**

Нет

**AIE vGPU for Compute Time-Sliced (профили «-C»):**

Да (AIE vGPU for Compute)

**MIG (аппаратная поддержка):**

Да

#### Blackwell

**GPU (пример моделей):**

RTX PRO 6000 Blackwell Server Edition; HGX B200 180GB

**GRID (vGPU Software) в списке Supported GPUs:**

RTX PRO 6000 Blackwell SE — да.

**GRID профили A/B/Q (vApps/vPC/vWS):**

RTX PRO 6000 Blackwell SE — да (A/B/Q);

**AIE vGPU for Compute Time-Sliced (профили «-C»):**

Да (AIE vGPU for Compute)

**MIG (аппаратная поддержка):**

Да только на B200, B300, RTX Pro 6000 SE.

### Преимущества и ограничения <a href="#advantages-and-limitations" id="advantages-and-limitations"></a>

#### MIG-backed vGPU (AIE)

**Преимущества:** Аппаратная изоляция ресурсов (включая память) и гарантированная производительность.

**Ограничения/условия:** Доступно только на MIG‑capable GPU; Режимы определяются гипервизором и релизом AIE. Не поддерживает графическое ускорение за исключением RTX PRO 6000 Blackwell Server Edition.

#### time-sliced vGPU (AIE)

**Преимущества:** Повышенная плотность размещения за счёт планирования времени внутри. Динамическая производительность.

**Ограничения/условия:** Доступно только на GPU/релизах, где режим заявлен. Требуется включение и поддержка гипервизором. Не поддерживает графическое ускорение. Не поддерживает ОС Windows.

#### time-sliced vGPU (Grid)

**Преимущества:** Поддержка графического ускорения. Повышенная плотность размещения за счёт планирования времени внутри. Динамическая производительность.

**Ограничения/условия:** Поддерживает большинство карт за исключением х100/200/300.

