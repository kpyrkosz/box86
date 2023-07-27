![Official logo](img/Box86Logo.png "Official Logo")

Емулятор простіра користувача x86_64 із родзинкою

[Дивитись журнал змін](https://github.com/ptitSeb/box86/blob/master/docs/CHANGELOG.md) | [中文](https://github.com/ptitSeb/box86/blob/master/docs/README_CN.md) | [Українська](https://github.com/ptitSeb/box86/blob/master/docs/README_UK.md) | [Повідомити про помилку](https://github.com/ptitSeb/box86/issues/new)

![build](https://app.travis-ci.com/ptitSeb/box86.svg?branch=main) ![stars](https://img.shields.io/github/stars/ptitSeb/box86) ![forks](https://img.shields.io/github/forks/ptitSeb/box86) ![contributors](https://img.shields.io/github/contributors/ptitSeb/box86) ![prs](https://img.shields.io/github/issues-pr/ptitSeb/box86) ![issues](https://img.shields.io/github/issues/ptitSeb/box86)

----

Box86 дозволяє запускати x86 Linux програми (наприклад, ігри) на системах Linux відмінних від x86, наприклад ARM (хост-система має мати 32-розрядний порядок редагування).

Вам *ПОТРІБНА* 32-розрядна підсистема для запуску та створення Box86. Box86 марний лише на 64-розрядних системах. Крім того, вам *ПОТРІБЕН* 32-розрядний інструментарій для створення Box86. Ланцюжок інструментів, який підтримує лише 64-розрядні версії, не компілює Box86, і ви отримаєте помилки (зазвичай на aarch64 ви отримуєте що «-marm» не розпізнається, і вам знадобиться середовище з мультиархітектурою або chroot).

Оскільки Box86 використовує рідні версії деяких «системних» бібліотек, таких як libc, libm, SDL і OpenGL, його легко інтегрувати та використовувати з більшістю програм, а продуктивність у багатьох випадках може бути напрочуд високою. Подивіться на цей стендовий аналіз для прикладу [тут](https://box86.org/index.php/2021/06/game-performances/). Це також означає, що вам знадобиться 32-бітний простір користувача на 64-бітній ОС, наприклад `armhf` поверх `aarch64` 64-бітної ОС.

Більшість x86-ігор потребують OpenGL, тому на платформах ARM може знадобитися таке рішення, як [gl4es](https://github.com/ptitSeb/gl4es). (Деякі платформи ARM підтримують лише OpenGL ES і/або їх реалізація OpenGL хитра. (див. OpenGL на Android))

Box86 інтегрується з DynaRec (динамічний рекомпілятор) для платформ ARM64 і RV64 забезпечуючи підвищення швидкості в 5-10 разів, ніж використання лише інтерпретатора. Деякі високорівневі відомості про те, як працює DynaRec, можна знайти [тут](https://box86.org/2021/07/inner-workings-a-high%e2%80%91level-view-of-box86-and-a-low%e2%80%91level-view-of-the-dynarec/).

Багато ігор просто працюють без особливих налаштувань, наприклад: WorldOfGoo, Airline Tycoon Deluxe і FTL. Багато ігор GameMaker Linux також працюють нормально. (є довгий список, серед них UNDERTALE, A Risk of Rain і Cook Serve Delicious). Ігри Unity3D також працюють добре, але вимога OpenGL може бути проблемою на деяких платформах ARM.

Якщо ви серйозно ставитеся до розробки Box86, вам слід встановити ccache та створити Box86 з ним. (Використовуйте, наприклад, ccmake.)
Щоб увімкнути TRACE (тобто скидання в stdout усіх окремих виконаних інструкцій x86 із дампом регістрів), вам також знадобиться [бібліотека Zydis](https://github.com/zyantific/zydis), доступна у вашій системі.

Деякі внутрішні коди операцій x86 використовують частини «Бібліотеки емулятора Realmode X86», дивіться [x86primop.c](../src/emu/x86primop.c) для детальної інформації про авторські права.

----
Компіляція/Встановлення
----
> Інструкцію зі складання Box86 можна знайти [тут](COMPILE.md).\
> Інструкцію з встановлення Wine для Box86 можна знайти [тут](X86WINE.md).

----

Ось 6 відео, перші 2 відео – це відео "Airline Tycoon Deluxe" і "Heretic 2", які працюють на GigaHertz OpenPandora (друге використовує Dynarec), а наступні 2 відео – це відео "Bit.Trip.Runner" і "Neverwinter Night", які працюють на ODroid XU4 (без dynarec), а останні 2 відео на Pi4: Shovel Knight (відео від @ITotalJustice) і Freedom Planet (відео від @djazz), також без Dynarec.


[![Play on Youtube](https://img.youtube.com/vi/bLt0hMoFDLk/3.jpg)](https://www.youtube.com/watch?v=bLt0hMoFDLk) [![Play on Youtube](https://img.youtube.com/vi/MM7kWYts7IA/3.jpg)](https://www.youtube.com/watch?v=MM7kWYts7IA) [![Play on Youtube](https://img.youtube.com/vi/8hr71S029Hg/1.jpg)](https://www.youtube.com/watch?v=8hr71S029Hg) [![Play on Youtube](https://img.youtube.com/vi/B4YN37z3-ws/1.jpg)](https://www.youtube.com/watch?v=B4YN37z3-ws) [![Play on Youtube](https://img.youtube.com/vi/xk8Q30mxqPg/1.jpg)](https://www.youtube.com/watch?v=xk8Q30mxqPg) [![Play on Youtube](https://img.youtube.com/vi/_QMRMVvYrqU/1.jpg)](https://www.youtube.com/watch?v=_QMRMVvYrqU)

Ви можете знайти багато інших відео про Box86 у YouTube каналах [MicroLinux](https://www.youtube.com/channel/UCwFQAEj1lp3out4n7BeBatQ), [Pi Labs](https://www.youtube.com/channel/UCgfQjdc5RceRlTGfuthBs7g) або [The Byteman](https://www.youtube.com/channel/UCEr8lpIJ3B5Ctc5BvcOHSnA).

Список сумісності тут: https://github.com/ptitSeb/box86-compatibility-list/issues

<img src="img/Box86Icon.png" width="96" height="96">

Логотип і піктограма зроблені @grayduck, дякую!

Зауважте, що цей проект не слід плутати з [86box](https://github.com/86Box/86Box), гарним емулятором "повної системи", що спеціалізується на ранньому (до відносно недавньому) апаратному забезпеченні ПК.

----

Використання
----

Є кілька змінних середовища для керування поведінкою Box64.

Дивіться [тут](USAGE.md) для перегляду всіх змінних середовища та того, що вони роблять.

Примітка: Dynarec від Box86 використовує механізм із захистом пам’яті та обробником сигналу помилки сегментів для обробки JIT коду. Простіше кажучи, якщо ви хочете використовувати GDB для налагодження запущеної програми, яка використовує JIT-код (наприклад, mono/Unity3D), ви все одно матимете багато «звичайних» помилок сегментів. Пропонується використовувати щось на зразок `handle SIGSEGV nostop` в GDB, щоб не зупинятися на кожній помилці сегмента, і, можливо, поставити точку зупину всередині `my_memprotectionhandler` в `signals.c`, якщо ви хочете перехопити помилки сегментів.

----

Історія версій/Журнал змін
----

Історія версій доступна [тут](docs/CHANGELOG.md).

----

Примітки щодо 64-розрядних платформ
----

Оскільки Box86 працює шляхом прямого перекладу викликів функцій із x86 на хост-систему, хост-система (на якій працює Box86) повинна мати 32-розрядні бібліотеки. Box86 не містить 32-біт <-> 64-біт перекладу. Отже, щоб запустити Box86, наприклад, на платформі ARM64, вам потрібно буде зібрати Box86 для 32-бітної ARM, а також потрібно мати chroot з 32-бітними бібліотеками.

Якщо ви дивитесь на 64-розрядну версію box86, подивіться на [Box64](https://github.com/ptitSeb/box64): він може запускати двійкові файли x86_64 на 64-розрядних платформах. Але зауважте, що вам все одно потрібен Box86 (і 32-розрядний chroot) для запуску бінарних файлів x86 (як це також трапляється в реальному Linux x86_64, для запуску якого потрібні бібліотеки x86 і двійковий файл в multiarch).

----

Примітки щодо конфігурації Box86
----

Box86 тепер має конфігураційні файли. Завантажуються 2 файли: `/etc/box4.box86rc` і `~/.box86rc`. Обидва файли мають однаковий синтаксис і в основному є файлами ini. Розділ у квадратних дужках визначає назву процесу, а решта — встановлють змінну середовища. Подивіться на [використання](docs/USAGE.md), щоб дізнатися, які параметри можна використовувати. Box86 поставляється з файлом за замовчуванням, який слід інсталювати для кращої стабільності. Файл знаходиться в `system/box86.box86rc` і має бути встановлено в `/etc/box86.box86rc`. Якщо з певних причин ви не хочете встановлювати цей файл у `/etc`, принаймні скопіюйте його в `~/.box86rc` або гра може не працювати належним чином. Зауважте, що пріоритетом є: `~/.bashrc` > `/etc/box86.box86rc` > командний рядок
Таким чином, ваші налаштування в `~/.bashrc` можуть замінити налаштування з вашого командного рядка...

----

Примітки щодо емуляції ігор Unity
----

Запуск ігор Unity має працювати, але ви також повинні зауважити, що для багатьох ігор Unity3D потрібен OpenGL 3+, який може бути складно забезпечити на ARM SBC (одноплатних комп’ютерах). Крім того, багато нових ігор Unity3D (наприклад, KSP) використовують стиснуті текстури BC7, які не підтримуються багатьма інтегрованими GPU ARM.
Підказка: в Pi4 використовуйте `MESA_GL_VERSION_OVERRIDE=3.2`, а в Panfrost — `PAN_MESA_DEBUG=gl3`, щоб використовувати вищий профіль, якщо гра запускається, а потім завершується, перш ніж щось показує.

----

Примітки щодо Steam
----

Linux версія Steam тепер може працювати з Box86, але вам також потрібен Box64, щоб його можна було повністю використовувати.
Рекомендується запускати Steam у small режимі, оскільки він використовує менше пам’яті, але steamwebhelper (64-бітний процес) усе одно завантажуватиметься, навіть якщо він не використовується.
Екран входу не може працювати без steamwebhelper, і буде показувати лише порожні вікна без належного налаштування box64 у системі.
Зауважте, що Steam використовуватиме багато пам’яті та ледве поміститься в систему з 4 ГБ оперативної пам’яті. Він більше не працюватиме в системі з меншим об’ємом пам’яті (як обхідний шлях, створіть файл підкачки, увійдіть і поставте прапорець «запам’ятати мене», використовуйте box64rc, щоб вимкнути steamwebhelper і запускати лише в малому режимі без підкачки після першого входу в систему)
Останнє зауваження: Steam BigPicture працюватиме, але також потрібен steamwebhelper (і box64) і багато пам’яті. Він не запуститься в системі лише з 4 ГБ оперативної пам’яті без файлу підкачки.
- Якщо у вас виникли проблеми зі встановленням Steam, ви можете знайти `install_steam.sh` у кореневій папці сховища box86. Цей простий скрипт завантажить і встановить Steam у вашу домашню папку, а потім створить ярлик steam у `/usr/local/bin` (для цього він запитає дозвіл sudo). Просто використовуйте `steam` для запуску після встановлення. Зауважте, що інсталяція, яка знаходиться в папці Home, працюватиме лише для одного користувача. Не використовуйте цей скрипт, якщо вам потрібна багатокористувацька інсталяція.
- Щоб уникнути повідомлення «libc.so.6 відсутній», ви можете використовувати `STEAMOS=1` і `STEAM_RUNTIME=1` як змінні середовища (воно автоматично з’являється, якщо ви використовували сценарій `install_steam.sh`)

Щоб вимкнути `steamwebhelper`, коли Box64 встановлений і працює, створіть (або відредагуйте) `~/.box64rc` і додайте у ньому:
```
[steamwebhelper]
BOX64_EXIT=1
```
І тоді `steamwebhelper` буде вимкнено. Прокоментуйте `#` ці 2 рядки, щоб увімкнути його знову.

----

Примітки щодо Wine
----

Wine тепер підтримується. Інтегровани програми Wine працює як і багато програм та ігор для Windows. Не забувайте, що більшість ігор для Windows використовують Direct3D, для цього може знадобитися повний драйвер OpenGL і якомога більш високий профіль (і gl4es з бекендом ES2 наразі мають проблеми з Wine).
Примітка: якщо ви плануєте використовувати box86 з Wine на Raspberry Pi 3 або ранішої версії, ці моделі використовують ОС за замовчуванням, яка має ядро ​​з розділенням 2/2 (тобто 2G місця для програми користувача та 2 Г місця для ядра). Це несумісно з програмами Wine, яким потрібен доступ до пам’яті > 2 Гб адреси. Тож вам потрібно буде переналаштувати ядро ​​для розділення 3G/1G.

----

Примітки щодо Vulkan
----

Box86 вже використовує Vulkan. Якщо у вашій системі є 32-розрядний драйвер Vulkan, Box86 використовуватиме його за потреби. Профіль 1.0, 1.1, 1.2 і 1.3, з деякими розширеннями, має бути в порядку. DXVK, включаючи 2.0, працює. Я знаю, що деякі демонстраційні версії працюють на Pi4 (демонстраційні версії Sascha Willems для x86 працюють так само, як якщо б створювати безпосередньо на armhf). Зауважте, що драйвер Vulkan для Pi4 наразі НЕ підтримує DXVK (обгортка Wine DirectX->Vulkan). Це не проблема Box86, у драйвері відсутні розширення (підтримка апаратного забезпечення) та кілька інших речей, через які DXVK не працює на pi4. З боку Panfrost, PanVK трохи молодий, і я ще не тестував DXVK з ним.

----
Останнє слово
----

Я хочу подякувати всім, хто зробив внесок у розвиток box86.
Є багато способів зробити свій внесок: код, фінанси, апаратне забезпечення та реклама!
Отже, без особливого порядку, я хочу подякувати:
 * За основний внесок у код: rajdakin, icecream95, M-HT
 * За великий фінансовий внесок: FlyingFathead, stormchaser3000, dennis1248, sll00, [libre-computer-project](https://libre.computer/)
 * За внесок у апаратне забезпечення: [Radxa](https://rockpi.org/), [Pine64](https://www.pine64.org/), [DragonBox](https://pyra-handheld.com/), [Novaspirit](https://www.youtube.com/channel/UCrjKdwxaQMSV_NDywgKXVmw), [HardKernel](https://www.hardkernel.com/), [Команда TwisterOS](https://twisteros.com/)
 * За постійну рекламу Box86: salva ([microLinux](https://www.youtube.com/channel/UCwFQAEj1lp3out4n7BeBatQ)), [PILab](https://www.youtube.com/channel/UCgfQjdc5RceRlTGfuthBs7g)/[Команда TwisterOS](https://twisteros.com/), [The Byteman](https://www.youtube.com/channel/UCEr8lpIJ3B5Ctc5BvcOHSnA), [NicoD](https://www.youtube.com/channel/UCpv7NFr0-9AB5xoklh3Snhg)

І я також дякую багатьом іншим людям, які хоч раз брали участь у проекті.

(Якщо ви використовуєте Box86 у своєму проекті, будь ласка, не забудьте згадати про це!)