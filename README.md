# IDLE'ın Kısayolu Neden Python.exe Olarak Açılır?

## Kısa Cevap

**IDLE, ayrı bir program (`idle.exe`) değildir.** IDLE, Python diliyle yazılmış ve `tkinter` kütüphanesiyle arayüzü oluşturulmuş bir Python programıdır. Bir Python programı da tek başına çalışamaz; mutlaka Python yorumlayıcısı (`python.exe` / `pythonw.exe`) tarafından çalıştırılır. Bu yüzden IDLE'ı açtığınızda işletim sistemi aslında Python yorumlayıcısını başlatır.

## Detaylı Açıklama

### 1. Kısayolun Hedefi

Windows'ta Başlat menüsündeki **IDLE (Python 3.x)** kısayoluna sağ tıklayıp *Özellikler* dediğinizde "Hedef" alanında şuna benzer bir komut görürsünüz:

```
C:\Users\KullaniciAdi\AppData\Local\Programs\Python\Python3x\pythonw.exe "C:\Users\KullaniciAdi\AppData\Local\Programs\Python\Python3x\Lib\idlelib\idle.pyw"
```

Yani kısayol şunu der: *"`pythonw.exe` ile `idle.pyw` dosyasını çalıştır."*

| Parça | Görevi |
|-------|--------|
| `pythonw.exe` | Python yorumlayıcısı (konsol penceresi açmayan sürüm) |
| `Lib\idlelib\idle.pyw` | IDLE'ın kendi Python kaynak kodu (başlangıç dosyası) |

### 2. `python.exe` ile `pythonw.exe` Farkı

- **`python.exe`**: Konsol (siyah komut penceresi) açar. Terminalde çalışan programlar için kullanılır.
- **`pythonw.exe`**: Konsol açmaz. Arayüzlü (GUI) programlar için uygundur. Dosya uzantısı `.pyw` olan dosyalar bununla çalışır.

IDLE grafik arayüzlü olduğu için `pythonw.exe` kullanır. Görev Yöneticisi'nde ya da süreç listesinde de bu nedenle "Python" ismiyle görünür.

### 3. IDLE Çalışırken İki Ayrı Süreç Vardır

IDLE'ı açıp bir kod çalıştırdığınızda aslında iki süreç oluşur:

1. **Arayüz süreci:** Editörü ve Shell penceresini gösterir (`pythonw.exe`).
2. **Çalıştırma süreci:** Sizin yazdığınız kodu çalıştıran ayrı bir Python süreci.

Bu iki süreç birbiriyle yerel soket (socket) üzerinden haberleşir. Böylece yazdığınız kodda bir hata olsa veya sonsuz döngü çalışsa bile IDLE'ın arayüzü çökmez.

```
+-----------------------+   soket   +-----------------------+
|  IDLE arayüzü         | <-------> |  Kodunuzun çalıştığı  |
|  (pythonw.exe)        |           |  Python süreci        |
+-----------------------+           +-----------------------+
```

### 4. Bunu Kendiniz Doğrulayın

1. IDLE'ı açın.
2. **Görev Yöneticisi**'ni açın (`Ctrl + Shift + Esc`).
3. "Python" adlı süreçleri inceleyin; sağ tık > *Dosya konumunu aç* ile `python.exe`/`pythonw.exe` klasörüne gittiğinizi görebilirsiniz.

IDLE içinde şu kodu çalıştırarak da hangi yorumlayıcının kullanıldığını görebilirsiniz:

```python
import sys
print(sys.executable)
```

Çıktı, Python kurulum klasöründeki bir `python.exe` / `pythonw.exe` yolunu gösterecektir.

## Sonuç

IDLE'ın kısayolunun Python.exe (daha doğrusu `pythonw.exe`) olarak görünmesinin sebebi, IDLE'ın bağımsız bir uygulama olmayıp **Python'la yazılmış bir Python programı** olmasıdır. Kısayol, yorumlayıcıya "şu IDLE dosyasını çalıştır" talimatı verir.

## Kaynaklar

- [Python Docs: IDLE](https://docs.python.org/3/library/idle.html)
- [Python Docs: Using Python on Windows](https://docs.python.org/3/using/windows.html)
