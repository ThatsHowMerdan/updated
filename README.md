2. Derleme Talimatları

A. Kernel-Mode Driver’ı Derleme (Y4VIA_Spoofer_Driver.c)

	1.	Windows Driver Kit (WDK) yüklü olmalı. Eğer yüklü değilse, WDK’yi Microsoft’un sitesinden indir.
	2.	Visual Studio’yu aç ve “Yeni Proje” oluştur.
	3.	Proje türü olarak “Kernel Mode Driver” seç.
	4.	Kodları projeye ekle ve aşağıdaki ayarları yap:
	•	Çözüm Yapılandırması: Release
	•	Platform: x64
	5.	Projeyi derle ve “Y4VIADriver.sys” dosyasının oluştuğunu kontrol et.

B. EFI Bootkit’i Derleme (Y4VIA_Bootkit.c)

	1.	TianoCore EDK2 EFI geliştirme ortamını kur.
	•	EDK2’yi buradan indirebilirsin.
	2.	EFI Bootkit için derleme komutlarını çalıştır:

build -a X64 -t VS2019 -p MdeModulePkg.dsc


	3.	Derleme başarılı olursa Y4VIA_Bootkit.efi dosyası oluşacaktır.
	4.	Bu dosyayı C:\EFI\Boot\ klasörüne kopyala.

C. Kullanıcı Arayüzünü Derleme (Y4VIA_Spoofer_GUI.cpp)

	1.	Visual Studio’da yeni bir C++ projesi oluştur.
	2.	Projeye aşağıdaki kütüphaneleri ekle:

d3d11.lib
imgui.lib


	3.	Kodları ekleyip Release - x64 modunda derle.
	4.	Derleme tamamlandığında Y4VIA_Spoofer.exe oluşacak.

3. Çalıştırma Talimatları

📌 Spoofer’ı çalıştırmadan önce Windows’u “Test Mode”a al:

bcdedit /set testsigning on
bcdedit /set nointegritychecks on
shutdown /r /t 0

📌 Bilgisayar yeniden başladıktan sonra:

	1.	Önce Kernel-Mode Driver’ı yükle:

sc create Y4VIA_DRIVER binPath=C:\Windows\System32\drivers\Y4VIADriver.sys type= kernel start= demand
sc start Y4VIA_DRIVER


	2.	EFI Bootkit’i etkinleştir:

bcdedit /set {current} path \EFI\Y4VIA_Bootkit.efi
shutdown /r /t 0


	3.	Son olarak Y4VIA_Spoofer.exe çalıştır.
	4.	“Spoof+Bypass” butonuna bas ve spoof işlemi tamamlanınca bilgisayarı yeniden başlat.
