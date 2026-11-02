
[wifi_para]
wifi_used = 1
wifi_sdc_id = 1
wifi_usbc_id = 1
wifi_usbc_type = 1
wifi_mod_sel = 8
wifi_power = "axp22_dldo1"
wifi_power_ext1 = "axp22_dldo2"
wifi_power_ext2 = ""
rtl8189es_shdn = PL06
rtl8189es_host_wake = PL07rtl8189es_shdn = PL06<1><default><default><0>
rtl8189es_host_wake = PL07<4><default><default><1>Cihaz       : Navitech NeoTab HN115
İşlemci     : Allwinner A33
Android     : 5.1.1
Kernel      : Linux 3.4.39
Wi-Fi       : RTL8189ETV / RTL8189ES
Bağlantı    : SDIO
SDIO        : mmc1
Arayüz      : wlan0
Mod seçimi  : 8189es
Durum       : wlan.driver.status=okHN115 üzerinde daha önce yaptığımız çalışmalardan Wi-Fi için sys_config dosyalarında incelediğimiz ve korumaya aldığımız ayarları çıkardım.

Önemli nokta: Çalışan RTL8189ETV/RTL8189ES Wi-Fi yapılandırmasını bozmadan korumak temel kararımızdı. Bluetooth için yapılan değişiklikleri Wi-Fi ayarlarına karıştırmayacağız.

### 1\. Wi-Fi ile ilgili sys_config grupları

Önceki teknik kayıtlarda doğrulayabildiğimiz gruplar:

### [wifi_para]

| Parametre       | Kayıtlı değer          |
| --------------- | ---------------------- |
| wifi_used       | 1                      |
| Wi-Fi ailesi    | RTL8189ES / RTL8189ETV |
| SDIO bağlantısı | mmc1                   |
| Arayüz          | wlan0                  |

### [rf_para]

| Parametre         | Kayıtlı değer |
| ----------------- | ------------- |
| module_num        | 5             |
| module_power1     | axp22_dldo1   |
| module_power1_vol | 3300000       |

Bu bölüm önemli: Wi-Fi modülünün güç beslemesiyle ilişkili ayarlar burada bulunuyor.

### [wifi_para] – bulunan tam yapılandırma

[wifi_para]
wifi_used = 1
wifi_sdc_id = 1
wifi_usbc_id = 1
wifi_usbc_type = 1
wifi_mod_sel = 8
wifi_power = "axp22_dldo1"
wifi_power_ext1 = "axp22_dldo2"
wifi_power_ext2 = ""
rtl8189es_shdn = PL06
rtl8189es_host_wake = PL07
Pin yapılandırma kayıtları:

rtl8189es_shdn = PL06<1><default><default><0>
rtl8189es_host_wake = PL07<4><default><default><1>
Bunlar önceki sysconfig1.lhs referansında bulunan değerlerdir. HN115'teki çalışan yapılandırmayla eşleşmeleri ayrıca değerlendirilmelidir.

### 2\. Diğer önemli gruplar

| Grup        | İncelenen konu                | Durum                      |
| ----------- | ----------------------------- | -------------------------- |
| [wifi_para] | Wi-Fi modülü, SDIO, güç, GPIO | Değerleri bulundu          |
| [rf_para]   | Modül güç beslemesi           | Değerleri bulundu          |
| SDIO/MMC    | Wi-Fi veri bağlantısı         | mmc1 doğrulandı            |
| GPIO        | PL06, PL07                    | Referans değerleri bulundu |
| Kernel      | RTL8189ES sürücüsü            | Çalıştığı doğrulandı       |
| Firmware    | RTL8189ES firmware yüklemesi  | Loglarda görüldü           |

### 3\. Dosyalarda yaptığımız karşılaştırmalar

Önceki incelemelerde şu dosyaları karşılaştırmıştık:

| Dosya                                  | Boyut    | SHA-256           |
| -------------------------------------- | -------- | ----------------- |
| sys_config.bin                         | 41.972 B | dc35608f...523d0a |
| sys_config_RUNNING.bin                 | 41.824 B | 6f7289b7...e9a943 |
| sys_config_RUNNING_LPO_LOSC_REPACK.bin | 41.828 B | Şüpheli           |

Önemli bulgu:

* sys_config.fex ile iki Q10 referans FEX dosyasının byte-byte aynı olduğu kaydedilmişti.

* Paketlenmiş sys_config.bin ile çalışan sys_config_RUNNING.bin aynı değil.

* LPO_LOSC_REPACK dosyasında 4 baytlık kayma şüphesi vardı. Kullanılmaması kararlaştırılmıştı.

### 4\. HN115'te çalışan Wi-Fi'nin kesin referansı

Cihaz       : Navitech NeoTab HN115
İşlemci     : Allwinner A33
Android     : 5.1.1
Kernel      : Linux 3.4.39
Wi-Fi       : RTL8189ETV / RTL8189ES
Bağlantı    : SDIO
SDIO        : mmc1
Arayüz      : wlan0
Mod seçimi  : 8189es
Durum       : wlan.driver.status=ok
Korunacak ayarlar:

* [wifi_para]

* [rf_para]

* PL06 / PL07 GPIO bağlantıları

* Wi-Fi güç beslemesi

* SDIO bağlantısı

* RTL8189ES kernel sürücüsü ve firmware

### 5\. Henüz kesinleştirmediğimiz noktalar

Bunları yapılmış değişiklik gibi göstermeyeceğim:

* PL06 ve PL07'nin çalışan cihaz üzerindeki elektriksel davranışı.

* wifi_power_ext1 değerinin fiilen kullanılıp kullanılmadığı.

* [wifi_para] dışındaki tüm Wi-Fi ilişkili grupların tam içeriği.

* Çalışan sys_config_RUNNING.bin ile diğer dosyaların parametre bazında tam farkı.

Benim teknik sonucum: Wi-Fi için en önemli bulduğumuz grup [wifi_para], güç tarafındaki en önemli grup ise [rf_para]. Şu aşamada çalışan ayarları değiştirmek için gerekçemiz yok. Önce çalışan dosyayla referans dosyalarının farkını çıkarmalıyız.

build.prop içinde şu değerler var:
ro.inet.wlan.type=8189es
ro.product.8723b_bt.used=true
ro.product.device=astar-ibt
ro.product.model=NB102
