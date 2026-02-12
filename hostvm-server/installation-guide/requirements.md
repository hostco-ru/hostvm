# Системные требования

## Системные требования для сервера виртуализации:

### Требования к CPU

1. Используемый процессор должен поддерживать технологию AMD-V или Intel VT.&#x20;
2. Аппаратная виртуализация на сервере должна быть включена.

Перечень поддерживаемых моделей CPU:

<table data-header-hidden><thead><tr><th width="249.33333333333331" align="center">AMD</th><th align="center">Intel</th><th align="center">IBM Power</th></tr></thead><tbody><tr><td align="center"><strong>AMD</strong></td><td align="center">Intel</td><td align="center">IBM POWER8</td></tr><tr><td align="center">Opteron G4</td><td align="center">Nehalem</td><td align="center">IBM POWER9</td></tr><tr><td align="center">Opteron G5</td><td align="center">Westmere</td><td align="center"></td></tr><tr><td align="center">EPYC</td><td align="center">Sandybridge</td><td align="center"></td></tr><tr><td align="center"></td><td align="center">Haswell</td><td align="center"></td></tr><tr><td align="center"></td><td align="center">Haswell-noTSX</td><td align="center"></td></tr><tr><td align="center"></td><td align="center">Broadwell</td><td align="center"></td></tr><tr><td align="center"></td><td align="center">Broadwell-noTSX</td><td align="center"></td></tr><tr><td align="center"></td><td align="center">Skylake (client)</td><td align="center"></td></tr><tr><td align="center"></td><td align="center">Skylake (server)</td><td align="center"></td></tr><tr><td align="center"></td><td align="center">Cascadelake</td><td align="center"></td></tr><tr><td align="center"></td><td align="center">Icelake</td><td align="center"></td></tr></tbody></table>

### Требования к дисковой подсистеме

Для следующих разделов требуется дисковое пространство в объеме:

| Наименование раздела | Требуемый объем |
| -------------------- | --------------- |
| /                    | 8 GB            |
| /boot                | 1 GB            |
| /var                 | 15 GB           |
| swap                 | 1 GB            |

Таким образом для установки сервера виртуализации потребуется минимум 25 GB дискового пространства.

Для установки сервера виртуализации и виртуальной машины Engine дисковая подсистема должна обеспечивать минимум 115 GB дисковой памяти.

## Системные требования для виртуальной машины Engine (управление системой виртуализации):

Количество vCPU: 4\
Объем RAM: 16384 MB

### Требования к дисковой подсистеме

Диск: 90 GB (размер раздела для установки виртуальной машины)

| Наименование раздела | Требуемый объем |
| -------------------- | --------------- |
| /data                | 90 GB           |
| /                    | 8 GB\*          |
| /boot                | 1 GB            |
| /var                 | 15 GB           |
| swap                 | 1 GB            |

\*В случае, если установка производится с offline дистрибутива, то минимальный раздел корневого раздела / должен быть увеличен до 10 GB.
