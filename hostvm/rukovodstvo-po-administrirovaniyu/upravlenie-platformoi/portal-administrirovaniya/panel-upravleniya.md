# Панель управления

Панель управления предоставляет обзорную информацию по состоянию системы виртуализации, ее ресурсам и их утилизации. Информация по утилизации ресурсов обновляется один раз в 15 минут, перечень ресурсов – каждые 15 секунд или при повторном входе, или при обновлении страницы.

<figure><img src="../../../../.gitbook/assets/3 (2).png" alt=""><figcaption></figcaption></figure>

Верхняя секция панели управления отображает общий перечень ресурсов системы и включает в себя дата-центры, кластеры, хосты, хранилища данных, ВМ и события. В заголовке указывается общее количество ресурсов данного типа, их статус отображается ниже в виде соответствующих иконок, и количество элементов в этом статусе. Статус для кластеров всегда отображается как N/A.

<figure><img src="../../../../.gitbook/assets/4 (1).png" alt=""><figcaption></figcaption></figure>

<table><thead><tr><th width="106">Иконка</th><th width="539.3333333333334">Статус</th></tr></thead><tbody><tr><td><img src="../../../../.gitbook/assets/5.png" alt="" data-size="original"></td><td>Ресурсы данного типа не добавлены</td></tr><tr><td><img src="../../../../.gitbook/assets/6.png" alt="" data-size="original"></td><td><p>Количество ресурсов в статусе warning. Нажатие на иконку перенаправляет на соответствующую страницу с результатами поиска ресурсов в этом статусе. Условия поиска ресурсов:</p><p>• Data Centers – результатом поиска будут дата-центры в статусе not operational или non-responsive;</p><p>• Gluster Volumes – результатом поиска будут тома gluster в статусе powering up, paused, migrating, waiting, suspended, или powering down;</p><p>• Hosts – результатом поиска будут хосты, не добавленные в кластер, в режиме обслуживания, установки, перезапуска или подключения;</p><p>• Storage Domains – результатом поиска будут хранилища в режиме обслуживания, в статусе uninitialized, unattached, inactive, detaching, activating;</p><p>• Virtual Machines – результатом поиска будут ВМ в статусе powering up, paused, migrating, waiting, suspended, powering down;</p><p>• Events – результатом поиска будут события со статусом warning</p></td></tr><tr><td><img src="../../../../.gitbook/assets/7.png" alt="" data-size="original"></td><td>Количество ресурсов в статусе up. Нажатие на иконку перенаправляет на соответствующую страницу с результатами поиска ресурсов в этом статусе</td></tr><tr><td><img src="../../../../.gitbook/assets/8 (1).png" alt="" data-size="original"></td><td><p>Количество ресурсов в статусе down. Нажатие на иконку перенаправляет на соответствующую страницу с результатами поиска ресурсов в этом статусе. Условия поиска ресурсов:</p><p>• Data Centers – результатом поиска будут дата-центры в режиме обслуживания, в статусе uninitialized или в статусе down;</p><p>• Gluster Volumes – результатом поиска будут тома gluster в статусе detached или inactive;</p><p>• Hosts – результатом поиска будут хосты в статусе non-responsive, non-operational, initializing, down, а также имеющие ошибки;</p><p>• Storage Domains – результатом поиска будут хранилища в статусе detached или inactive;</p><p>• Virtual Machines – результатом поиска будут ВМ в статусе not responding или rebooting</p></td></tr><tr><td><img src="../../../../.gitbook/assets/9 (1).png" alt="" data-size="original"></td><td>Количество ресурсов в статусе alert. Нажатие на иконку перенаправляет на соответствующую страницу с результатами поиска ресурсов в этом статусе</td></tr><tr><td><img src="../../../../.gitbook/assets/10.png" alt="" data-size="original"></td><td>Количество ресурсов в статусе error. Нажатие на иконку перенаправляет на соответствующую страницу с результатами поиска ресурсов в этом статусе</td></tr></tbody></table>

Секция Global Utilization показывает общую утилизацию ресурсов системы (CPU, Memory, Storage).

<figure><img src="../../../../.gitbook/assets/11.png" alt=""><figcaption></figcaption></figure>

В верхней части отображается количество свободных ресурсов и коэффициент переподписки. Например, для CPU коэффициент рассчитывается делением доступных для ВМ виртуальных ядер на физические на основании последних данных из базы Data Warehouse (DWH).

Графики отображают среднее использование за последние 5 минут. При наведении курсора на секцию графика будет показано ее значение.

При нажатии на графики CPU и Memory будет отображен список из десяти хостов и ВМ с наивысшим потреблением. Для Storage будет отображен список из десяти хранилищ и ВМ. Стрелка справа показывает тренд утилизации ресурса за последнюю минуту.

<figure><img src="../../../../.gitbook/assets/12.png" alt=""><figcaption></figcaption></figure>

Секция Cluster Utilization показывает утилизацию кластерами CPU и памяти за последние 24 часа в виде блоков соответствующего цвета. При наведении курсора на блок отображается имя кластера. При нажатии на блок отображается страница с перечнем хостов кластера. Значение рассчитывается как общее среднее на основании средней утилизации ресурса каждым из хостов за последние 24 часа.

<figure><img src="../../../../.gitbook/assets/13.png" alt=""><figcaption></figcaption></figure>

Секция Storage Utilization показывает утилизацию хранилищ за последние 24 часа в виде блоков соответствующего цвета. При наведении курсора на блок отображается имя хранилища. При нажатии на блок отображается страница с результатом поиска хранилища. Значение рассчитывается как общее среднее на основании средней утилизации ресурса каждым из хостов за последние 24 часа.

<figure><img src="../../../../.gitbook/assets/14.png" alt=""><figcaption></figcaption></figure>