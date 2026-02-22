<div align="center">
  <img alt="SC Hotspot Header" src="https://github.com/user-attachments/assets/d5d8806b-b251-4ddd-9306-2c4b9b798af2" width="100%">
</div>

<br>

<p align="center">
  <img src="https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white" alt="Bash">
  <img src="https://img.shields.io/badge/Arch_Linux-1793D1?style=for-the-badge&logo=arch-linux&logoColor=white" alt="Arch Linux">
  <img src="https://img.shields.io/badge/Linux-Supported-FCC624?style=for-the-badge&logo=linux&logoColor=black" alt="Linux">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="License">
</p>

<p align="center">
  <a href="./README.md">
    <img src="https://cdnjs.cloudflare.com/ajax/libs/flag-icon-css/3.5.0/flags/4x3/gb.svg" alt="English" width="40">
  </a>
  &nbsp;&nbsp;|&nbsp;&nbsp;
  <a href="./README_tr.md">
    <img src="https://cdnjs.cloudflare.com/ajax/libs/flag-icon-css/3.5.0/flags/4x3/tr.svg" alt="Türkçe" width="40">
  </a>
</p>

# SC Hotspot - Linux İçin Akıllı Wi-Fi Köprüsü ve Kurtarma Modu

SC Hotspot, Linux için gelişmiş ve tamamen otomatik bir Wi-Fi hotspot (erişim noktası) aracıdır. Donanım çakışmalarını (Tek Radyo/Single Radio kısıtlamaları) önlemek için aktif ağ frekansınızı dinamik olarak hesaplar ve askeri/radar (DFS) kanal kısıtlamalarını aşmak için akıllı bir **Hata Ayıklama ve Kurtarma Modu (Debug & Recovery)** sunar. Tüm bu özellikler, şık ve çift temalı bir Tux ASCII Terminal Arayüzü (TUI) ile paketlenmiştir.

KYK gibi büyük yurtlarda, şirket binalarında, havaalanlarında veya ağ sistemlerinin agresif yük dengeleme (load balancing) ya da kısıtlı 5 GHz bantları kullandığı otellerde internet paylaşmaya çalışan Linux kullanıcıları için idealdir.

---

## Neden SC Hotspot?

Kurumsal ağlar genellikle Yük Dengeleme (Load Balancing) kullanır. Bu, cihazınızın sürekli olarak 2.4 GHz ve 5 GHz bantları arasında geçiş yapabileceği veya kısıtlı DFS (Dinamik Frekans Seçimi) kanallarına zorlanabileceği anlamına gelir. Bu koşullarda standart bir hotspot açmaya çalışmak Wi-Fi kartınızın çökmesine neden olur. 

SC Hotspot bu sorunu şu şekilde çözer:

1. İşlemi başlatmadan hemen önce, tam olarak eşleşen kanalı anında hesaplar.

2. Bağlantı kopmalarını önlemek için güç tasarrufu (power-save) modlarını kapatır.

3. DFS kanal hatalarını (örneğin 116. Kanal) anında yakalar ve ağ yöneticinizi (NetworkManager) boş, sivil bir frekansa zorlamak için **SC Debug (Hata Ayıklama) Modu**nu başlatır.

---

## Gereksinimler

Betiği kullanmadan önce, aşağıdaki temel ağ araçlarının sisteminizde yüklü olduğundan emin olun.

*(Arch Linux için örnek kurulum)*

```bash
sudo pacman -S iw dnsmasq haveged
yay -S linux-wifi-hotspot
```

*Not: WPA2 el sıkışması sırasında ağın "Low entropy" (Düşük entropi) hatası vererek donmasını önlemek için `haveged` paketinin kurulması şiddetle tavsiye edilir.*

---

## Kurulum

1. Depoyu (repository) klonlayın ve dizine gidin:

   ```bash
   git clone https://github.com/SametCirik/SC-Hotspot.git
   cd SC-Hotspot
   ```
   
2. Hem ana betiği hem de hata ayıklama (debug) betiğini çalıştırılabilir hale getirin, ardından sistemin her yerinden erişilebilmesi için bin dizinine taşıyın:

   ```bash
   chmod +x sc-hotspot sc-debug
   sudo cp sc-hotspot /usr/local/bin/
   sudo cp sc-debug /usr/local/bin/
   ```

3. Arayüzünüzü yapılandırın (İsteğe bağlı):
Varsayılan ağ arayüzü `wlo1` olarak ayarlanmıştır. Sisteminiz farklı bir arayüz (örneğin `wlan0` veya `wlp3s0`) kullanıyorsa, terminalde şu komutu çalıştırarak `INTERFACE` değişkenini düzenleyebilirsiniz: 
`sudo nano /usr/local/bin/sc-hotspot` (aynı işlemi `sc-debug` için de yapın).

---

## Kullanım

Global kurulumdan sonra, interaktif arayüzü terminalinizde herhangi bir dizindeyken başlatabilirsiniz:

```bash
sc-hotspot
```

* **Özelleştirme:** İstediğiniz Ağ Adını (SSID) ve Şifreyi (en az 8 karakter) doğrudan Tux arayüzüne yazabilirsiniz. Boş bırakıp "Enter" tuşuna basarsanız varsayılan değerler uygulanacaktır.
* **Güvenlik ve Temizlik:** Betik otomatik bir yakalama (trap) mekanizmasına sahiptir. Hotspot'u kapattığınızda (`Ctrl+C`), arka planda oluşturduğu tüm ağ kilitlerini otomatik olarak temizleyerek sisteminizi normal kullanım için tertemiz bırakır.

---

## Öne Çıkan Özellik: SC Debug & DFS Kurtarma Modu

Wi-Fi kartınız havaalanlarında veya büyük yurtlarda kullanılan askeri kısıtlamalı bir DFS kanalına (örn. Kanal 116) bağlıysa, standart betikler donanımsal NO-IR kilitleri nedeniyle *"Your adapter cannot transmit to channel 116"* hatası verip çöker.

SC Hotspot bu durumu zekice yönetir:
1. Çökmeyi anında yakalar.
2. Ekran temizlenir ve **Kırmızı Sinirli Tux (Kurtarma Modu)** belirir.
3. Mevcut kurumsal ağınızı tarar ve çevrenizdeki tüm serbest/sivil BSSID'leri (MAC Adresleri) listeler.
4. Listeden bir numara seçmeniz yeterlidir (1, 6 veya 11 gibi 2.4 GHz'lik bir kanal önerilir).
5. Hata ayıklama betiği, NetworkManager'ı o spesifik serbest kanala kilitler, bağlantıyı yeniler, şifrelerinizi geri aktarır ve ana betik üzerinden hotspot'u otomatik olarak yeniden ateşler!

---

## Nasıl Çalışır? (Kaputun Altında)

* **Frekans Matematiği:** Fiziksel frekansları `iw` aracılığıyla okur ve bunları IEEE 802.11 matematiksel formüllerini kullanarak dönüştürür (örneğin, 2.4 GHz için `(Frekans - 2407) / 5`).
* **Stabilite Ezmesi:** `create_ap` mimarisini bozan anlık kopmaları önlemek için global olarak `iw dev wlo1 set power_save off` komutunu çalıştırır.
* **BSSID ve Bant Kilitleme:** DFS kısıtlamalarını atlamak için `nmcli` profillerini dinamik olarak manipüle eder ve çıkışta bu kilitleri otomatik olarak kaldırır.

---

## Katkıda Bulunma ve Lisans
Bu proje **MIT Lisansı** ile açık kaynak olarak sunulmuştur. 

Bu lisansın şartlarına göre; orijinal telif hakkı bildirimini ve lisans metnini kendi kopyalarınızda veya projelerinizde bulundurduğunuz sürece bu kodları dilediğiniz gibi **çatallayabilir (fork), değiştirebilir, dağıtabilir ve hatta ticari amaçlarla bile kullanabilirsiniz.** Özgür yazılım felsefesine uygun olarak kodlar tamamen sizin kontrolünüzdedir.

Projeyi geliştirmek, yeni özellikler eklemek veya hataları çözmek isterseniz her zaman **Pull Request (Çekme İsteği)** gönderebilir veya **Issue (Hata Bildirimi)** açabilirsiniz. Katkılarınız memnuniyetle karşılanacaktır!

---

## Geliştirici
Bu proje, [Samet Cırık](https://github.com/SametCirik) tarafından geliştirilmiştir.
