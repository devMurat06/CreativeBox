# CreativeBox
An imaginative engineer who redesigns the world while writing code

import math
import random
import pygame

# --- PYGAME AYARLARI ---
# Pencere boyutları
GENISLIK, YUKSEKLIK = 1000, 700
BEYAZ = (255, 255, 255)
SIYAH = (0, 0, 0)
KIRMIZI = (255, 0, 0)
YESIL = (0, 255, 0)
MAVI = (0, 0, 255)
MOR = (128, 0, 128)
SARI = (255, 255, 0)
TURUNCU = (255, 165, 0)
GRI = (150, 150, 150)
AQUA = (0, 255, 255)


# Pygame başlatma ve temel ekran ayarları
def init_pygame_screen(caption):
    pygame.init()
    # Farklı yazı tipleri için başlatma
    pygame.font.init()
    ekran = pygame.display.set_mode((GENISLIK, YUKSEKLIK))
    pygame.display.set_caption(caption)
    return ekran


# Ekrana metin yazma yardımcı fonksiyonu
def text_to_screen(surface, text, pos, color=SIYAH, font_size=24, center_x=False):
    font = pygame.font.Font(None, font_size)
    text_surface = font.render(text, True, color)
    text_rect = text_surface.get_rect()
    if center_x:
        text_rect.centerx = pos[0]
        text_rect.top = pos[1]
    else:
        text_rect.topleft = pos
    surface.blit(text_surface, text_rect)
    return text_rect.height  # Yazılan metnin yüksekliğini döndürür


# Pygame döngüsünü yöneten temel fonksiyon
def pygame_loop(draw_function):
    ekran = init_pygame_screen("Matematiksel Gorsellestirme")
    clock = pygame.time.Clock()

    calisiyor = True
    while calisiyor:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                calisiyor = False
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_ESCAPE:  # ESC tuşu ile çıkış
                    calisiyor = False

        ekran.fill(BEYAZ)  # Ekranı temizle
        draw_function(ekran)  # Görselleştirme fonksiyonunu çağır
        text_to_screen(ekran, "Esc ile Cikis", (GENISLIK - 150, YUKSEKLIK - 30), GRI, 18)
        pygame.display.flip()  # Ekranı güncelle
        clock.tick(30)  # Saniyede 30 kare

    pygame.quit()


# --- 1. Sayı Teorisi Fonksiyonları ---

def asal_sayi_mi(sayi):
    """Bir sayının asal olup olmadığını kontrol eder."""
    if sayi < 2:
        return False
    for i in range(2, int(math.sqrt(sayi)) + 1):
        if sayi % i == 0:
            return False
    return True


def carpanlari_bul(sayi):
    """Bir sayının tüm pozitif çarpanlarını (bölenlerini) bulur."""
    carpanlar = set()  # Tekrar edenleri engellemek için set kullan
    for i in range(1, int(math.sqrt(sayi)) + 1):
        if sayi % i == 0:
            carpanlar.add(i)
            carpanlar.add(sayi // i)
    return sorted(list(carpanlar))


def ebob_bul(a, b):
    """İki sayının En Büyük Ortak Bölenini (EBOB) bulur (Öklid Algoritması)."""
    while b:
        a, b = b, a % b
    return a


def ekok_bul(a, b):
    """İki sayının En Küçük Ortak Katını (EKOK) bulur."""
    if a == 0 or b == 0:
        return 0
    return abs(a * b) // ebob_bul(a, b)


# --- 2. Cebir Fonksiyonları ---

def dogrusal_denklem_coz(a, b):
    """ax + b = 0 şeklindeki doğrusal denklemi çözer."""
    if a == 0:
        if b == 0:
            return "Sonsuz çözüm var (0 = 0)."
        else:
            return "Çözüm yok (0 = -b, b sıfırdan farklı)."
    else:
        x = -b / a
        return f"Denklemin çözümü: x = {x:.2f}"


def ikinci_dereceden_denklem_coz(a, b, c):
    """ax^2 + bx + c = 0 şeklindeki ikinci dereceden denklemi çözer."""
    delta = b ** 2 - 4 * a * c

    if a == 0:
        return "Bu bir ikinci dereceden denklem değil. Doğrusal denklem çözücüyü kullanın."

    if delta > 0:
        x1 = (-b - math.sqrt(delta)) / (2 * a)
        x2 = (-b + math.sqrt(delta)) / (2 * a)
        return f"İki farklı reel kök var: x1 = {x1:.2f}, x2 = {x2:.2f}"
    elif delta == 0:
        x = -b / (2 * a)
        return f"Tek reel kök var (çakışık kökler): x = {x:.2f}"
    else:
        reel_kisim = -b / (2 * a)
        sanal_kisim = math.sqrt(abs(delta)) / (2 * a)
        return f"İki karmaşık kök var: x1 = {reel_kisim:.2f} - {sanal_kisim:.2f}i, x2 = {reel_kisim:.2f} + {sanal_kisim:.2f}i"


# --- 3. Geometri Fonksiyonları ---

def dikdortgen_alan_cevre(uzunluk, genislik):
    """Dikdörtgenin alanını ve çevresini hesaplar."""
    alan = uzunluk * genislik
    cevre = 2 * (uzunluk + genislik)
    return f"Alan: {alan:.2f}, Cevre: {cevre:.2f}"


def daire_alan_cevre(yaricap):
    """Dairenin alanını ve çevresini (çevresi) hesaplar."""
    alan = math.pi * (yaricap ** 2)
    cevre = 2 * math.pi * yaricap
    return f"Alan: {alan:.2f}, Cevre: {cevre:.2f}"


# --- 4. Kesirler Fonksiyonları ---

def kesir_sadelestir(pay, payda):
    """Bir kesri sadeleştirir."""
    ortak_bolen = ebob_bul(pay, payda)
    return pay // ortak_bolen, payda // ortak_bolen


def kesir_topla(pay1, payda1, pay2, payda2):
    """İki kesri toplar."""
    yeni_payda = ekok_bul(payda1, payda2)
    yeni_pay1 = pay1 * (yeni_payda // payda1)
    yeni_pay2 = pay2 * (yeni_payda // payda2)
    toplam_pay = yeni_pay1 + yeni_pay2
    return kesir_sadelestir(toplam_pay, yeni_payda)


def kesir_cikar(pay1, payda1, pay2, payda2):
    """İki kesri birbirinden çıkarır."""
    yeni_payda = ekok_bul(payda1, payda2)
    yeni_pay1 = pay1 * (yeni_payda // payda1)
    yeni_pay2 = pay2 * (yeni_payda // payda2)
    fark_pay = yeni_pay1 - yeni_pay2
    return kesir_sadelestir(fark_pay, yeni_payda)


def kesir_carp(pay1, payda1, pay2, payda2):
    """İki kesri çarpar."""
    carpim_pay = pay1 * pay2
    carpim_payda = payda1 * payda2
    return kesir_sadelestir(carpim_pay, carpim_payda)


def kesir_bol(pay1, payda1, pay2, payda2):
    """İki kesri böler."""
    if pay2 == 0:
        return "Tanımsız (Sıfıra bölme hatası)."
    bolum_pay = pay1 * payda2
    bolum_payda = payda1 * pay2
    return kesir_sadelestir(bolum_pay, bolum_payda)


# --- 5. Ondalık Sayılar Fonksiyonları ---

def ondalik_yuvarla(sayi, basamak):
    """Ondalık sayıyı belirtilen basamağa yuvarlar."""
    return round(sayi, basamak)


# --- 6. Yüzdeler Fonksiyonları ---

def yuzde_hesapla(ana_deger, yuzde_orani):
    """Bir sayının belirli bir yüzdesini hesaplar."""
    return ana_deger * (yuzde_orani / 100)


def yuzde_farki(deger1, deger2):
    """İki değer arasındaki yüzde farkını hesaplar."""
    if deger1 == 0:
        return "Hesaplama için ilk değer sıfır olamaz."
    fark = ((deger2 - deger1) / deger1) * 100
    return f"{fark:.2f}%"


# --- 7. Problem / Veri Üretimi ---

def rastgele_tam_sayi_uret(min_deger, max_deger, miktar=1):
    """Belirtilen aralıkta rastgele tam sayılar üretir."""
    if miktar == 1:
        return random.randint(min_deger, max_deger)
    else:
        return [random.randint(min_deger, max_deger) for _ in range(miktar)]


def rastgele_ondalikli_sayi_uret(min_deger, max_deger, ondalik_basamak=2, miktar=1):
    """Belirtilen aralıkta rastgele ondalıklı sayılar üretir."""
    aralik = max_deger - min_deger

    if miktar == 1:
        return round(min_deger + random.random() * aralik, ondalik_basamak)
    else:
        return [round(min_deger + random.random() * aralik, ondalik_basamak) for _ in range(miktar)]


# --- PYGAME GÖRSELLEŞTİRMELERİ ---

def asal_carpan_agaci_gorsellestir(sayi):
    """
    Bir sayının asal çarpan ağacını Pygame ile adım adım görselleştirir.
    """
    if sayi < 2:
        print("Asal çarpan ağacı için sayı 1'den büyük olmalıdır.")
        return

    ekran = init_pygame_screen(f"{sayi} Sayısının Asal Çarpan Ağacı")
    clock = pygame.time.Clock()

    # Ağaç düğümleri için bir sınıf (node) tanımlayalım
    class Node:
        def __init__(self, value, x, y, parent=None):
            self.value = value
            self.x = x
            self.y = y
            self.children = []
            self.parent = parent
            self.is_prime = asal_sayi_mi(value)

        def add_child(self, child_node):
            self.children.append(child_node)

    # Ağaç oluşturma ve görselleştirme fonksiyonu
    def build_and_draw_tree(current_num, x, y, parent_node=None):
        node = Node(current_num, x, y, parent_node)

        if parent_node:
            parent_node.add_child(node)

        if node.is_prime and current_num > 1:  # Asal ve 1'den büyükse dallanmayı durdur
            return node

        if current_num == 1:  # Sayı 1'e ulaştığında dur
            return node

        # Bölen bulma
        divisor = 2
        while divisor * divisor <= current_num:
            if current_num % divisor == 0:
                child_num1 = divisor
                child_num2 = current_num // divisor

                # Çocukları oluştur ve ağaca ekle
                child_node1 = build_and_draw_tree(child_num1, x - 50, y + 80, node)  # Örnek konumlandırma
                child_node2 = build_and_draw_tree(child_num2, x + 50, y + 80, node)  # Örnek konumlandırma
                node.add_child(child_node1)
                node.add_child(child_node2)
                return node  # Dallanma tamamlandı, ana düğümü döndür
            divisor += 1

        # Eğer yukarıdaki döngüde bölen bulunamadıysa ve sayı 1'den büyükse, sayı asal demektir.
        if current_num > 1:
            node.is_prime = True  # Düğümü asal olarak işaretle
            return node
        return node  # Sayı 1 ise doğrudan döndür

    # Ağacı çizen ana fonksiyon
    def draw_all_nodes(screen, node_list):
        for node in node_list:
            if node.parent:
                # Ebeveyn ve çocuk arasına çizgi çek
                pygame.draw.line(screen, GRI, (node.parent.x, node.parent.y + 20), (node.x, node.y - 20), 2)

            # Düğümün değerini çiz
            color = MAVI if not node.is_prime else KIRMIZI  # Asal ise kırmızı, değilse mavi
            text_rect = text_to_screen(screen, str(node.value), (node.x, node.y), color, 30, center_x=True)

            # Düğüm etrafına daire çiz (görselleştirme için)
            pygame.draw.circle(screen, color, (node.x, node.y + text_rect // 2), text_rect // 2 + 10, 2)

            # Çocukları da çiz
            for child in node.children:
                draw_all_nodes(screen, [child])  # Özyinelemeli çizim

    # Ağacı oluşturma ve animasyon için düğüm listesi
    root_node = Node(sayi, GENISLIK // 2, 50)
    current_step_nodes = [root_node]
    all_nodes = [root_node]

    processing_queue = [root_node]
    step_delay = 1000  # Her adım arasında 1 saniye bekleme

    calisiyor = True
    while calisiyor:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                calisiyor = False
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_ESCAPE:
                    calisiyor = False
                elif event.key == pygame.K_SPACE:  # Boşluk tuşu ile bir sonraki adımı göster
                    if processing_queue:
                        node_to_process = processing_queue.pop(0)

                        if node_to_process.is_prime or node_to_process.value == 1:
                            # Asal veya 1 ise dallanma yok
                            pass
                        else:
                            divisor = 2
                            found_divisor = False
                            while divisor * divisor <= node_to_process.value:
                                if node_to_process.value % divisor == 0:
                                    child_val1 = divisor
                                    child_val2 = node_to_process.value // divisor

                                    # Çocuk düğümlerini oluştur ve konumlandır
                                    # Basit bir yatay yayılım algoritması
                                    child_x1 = node_to_process.x - 70
                                    child_x2 = node_to_process.x + 70

                                    child_node1 = Node(child_val1, child_x1, node_to_process.y + 100, node_to_process)
                                    child_node2 = Node(child_val2, child_x2, node_to_process.y + 100, node_to_process)

                                    node_to_process.add_child(child_node1)
                                    node_to_process.add_child(child_node2)

                                    all_nodes.extend([child_node1, child_node2])
                                    processing_queue.append(child_node1)
                                    processing_queue.append(child_node2)
                                    found_divisor = True
                                    break
                                divisor += 1

                            # Eğer sayı asal çıktıysa ve 1'den büyükse, onu asal olarak işaretle
                            if not found_divisor and node_to_process.value > 1:
                                node_to_process.is_prime = True

        ekran.fill(BEYAZ)
        text_to_screen(ekran, f"{sayi} Sayısının Asal Çarpan Ağacı", (GENISLIK // 2, 20), SIYAH, 40, center_x=True)
        text_to_screen(ekran, "Bosluk (Space) ile Adim Adim Olustur, Esc ile Cikis", (GENISLIK // 2, YUKSEKLIK - 50),
                       GRI, 20, center_x=True)

        # Tüm düğümleri çiz
        for node in all_nodes:
            if node.parent:
                pygame.draw.line(ekran, GRI, (node.parent.x, node.parent.y + 20), (node.x, node.y - 20), 2)

            color = KIRMIZI if node.is_prime else MAVI
            text_rect_height = text_to_screen(ekran, str(node.value), (node.x, node.y), color, 30, center_x=True)
            pygame.draw.circle(ekran, color, (node.x, node.y + text_rect_height // 2), text_rect_height // 2 + 10, 2)

        pygame.display.flip()
        clock.tick(30)

    pygame.quit()
    print(f"\n{sayi} sayısının asal çarpan ağacı görselleştirmesi tamamlandı.")


def kesir_gorsellestir(pay, payda):
    """
    Bir kesri dikdörtgen şeklinde görselleştirir.
    """
    if payda == 0:
        print("Payda sıfır olamaz.")
        return
    if pay > payda:
        print("Basit kesir görselleştirme, pay paydadan küçük olmalıdır.")
        return

    ekran = init_pygame_screen(f"{pay}/{payda} Kesri Görselleştirme")

    def draw_fraction(screen):
        rect_width = GENISLIK * 0.8
        rect_height = 100
        start_x = (GENISLIK - rect_width) // 2
        start_y = (YUKSEKLIK - rect_height) // 2 - 50

        # Bütün dikdörtgeni çiz
        pygame.draw.rect(screen, SIYAH, (start_x, start_y, rect_width, rect_height), 3)

        # Parçaları çiz
        segment_width = rect_width / payda
        for i in range(payda):
            x = start_x + i * segment_width
            # Dolu kısımları farklı renkte çiz
            if i < pay:
                pygame.draw.rect(screen, MAVI, (x, start_y, segment_width, rect_height))
            # Her bir parçanın ayırıcısını çiz
            pygame.draw.line(screen, SIYAH, (x, start_y), (x, start_y + rect_height), 2)

        # Kesir metnini yaz
        text_to_screen(screen, f"Kesir: {pay}/{payda}", (GENISLIK // 2, start_y + rect_height + 50), SIYAH, 36,
                       center_x=True)
        text_to_screen(screen, f"Ondalik: {pay / payda:.2f}", (GENISLIK // 2, start_y + rect_height + 100), SIYAH, 30,
                       center_x=True)
        text_to_screen(screen, f"Yuzde: {(pay / payda) * 100:.2f}%", (GENISLIK // 2, start_y + rect_height + 150),
                       SIYAH, 30, center_x=True)

    pygame_loop(draw_fraction)


def daire_gorsellestir(yaricap):
    """
    Daireyi Pygame ile çizer ve alan/çevre bilgisini gösterir.
    """
    ekran = init_pygame_screen("Daire Çizimi")

    def draw_circle(screen):
        # Çizilecek dairenin yarıçapı (ekranı aşmayacak şekilde ölçekle)
        cizim_yaricap = min(yaricap * 8, min(GENISLIK, YUKSEKLIK) // 2 - 50)
        merkez_x, merkez_y = GENISLIK // 2, YUKSEKLIK // 2

        pygame.draw.circle(screen, YESIL, (merkez_x, merkez_y), int(cizim_yaricap), 3)

        # Yarıçapı göstermek için çizgi
        pygame.draw.line(screen, MOR, (merkez_x, merkez_y), (merkez_x + int(cizim_yaricap), merkez_y), 2)
        text_to_screen(screen, f"r = {yaricap}", (merkez_x + int(cizim_yaricap) // 2, merkez_y - 20), MOR, 20)

        # Alan ve çevre bilgilerini göster
        alan_cevre_bilgisi = daire_alan_cevre(yaricap)
        text_to_screen(screen, alan_cevre_bilgisi, (50, 50), SIYAH)
        text_to_screen(screen, f"Yaricap: {yaricap}", (50, 80), SIYAH)

    pygame_loop(draw_circle)


def dikdortgen_gorsellestir(uzunluk, genislik):
    """
    Dikdörtgeni Pygame ile çizer ve alan/çevre bilgisini gösterir.
    """
    ekran = init_pygame_screen("Dikdörtgen Çizimi")

    def draw_rectangle(screen):
        # Çizilecek dikdörtgenin boyutları (ekran ortalaması için)
        scale_factor = min((GENISLIK - 100) / uzunluk, (YUKSEKLIK - 150) / genislik, 10)  # Maksimum ölçek 10
        rect_width = uzunluk * scale_factor
        rect_height = genislik * scale_factor

        rect_x = (GENISLIK - rect_width) // 2
        rect_y = (YUKSEKLIK - rect_height) // 2 - 50

        pygame.draw.rect(screen, MAVI, (rect_x, rect_y, rect_width, rect_height), 3)

        # Uzunluk ve genişlik metinlerini çiz
        text_to_screen(screen, f"U: {uzunluk}", (rect_x + rect_width / 2, rect_y - 30), SIYAH, 24, center_x=True)
        text_to_screen(screen, f"G: {genislik}", (rect_x - 30, rect_y + rect_height / 2), SIYAH, 24)

        # Alan ve çevre bilgilerini göster
        alan_cevre_bilgisi = dikdortgen_alan_cevre(uzunluk, genislik)
        text_to_screen(screen, alan_cevre_bilgisi, (50, 50), SIYAH)
        text_to_screen(screen, f"Uzunluk: {uzunluk}, Genislik: {genislik}", (50, 80), SIYAH)

    pygame_loop(draw_rectangle)


def koordinat_duzlemi_gorsellestir(noktalar, cizgiler):
    """
    Koordinat düzlemini çizer ve verilen noktaları/çizgileri görselleştirir.
    Noktalar: [(x, y), ...]
    Çizgiler: [(x1, y1, x2, y2), ...]
    """
    ekran = init_pygame_screen("Koordinat Duzlemi")

    # Ölçekleme faktörleri ve merkez noktası
    max_abs_coord = 0
    if noktalar:
        for p in noktalar:
            max_abs_coord = max(max_abs_coord, abs(p[0]), abs(p[1]))
    if cizgiler:
        for l in cizgiler:
            max_abs_coord = max(max_abs_coord, abs(l[0]), abs(l[1]), abs(l[2]), abs(l[3]))

    # Minimum 10 birim varsayalım
    if max_abs_coord == 0:
        max_abs_coord = 10

        # Eksenleri çizmek için ekranın ortası
    merkez_x, merkez_y = GENISLIK // 2, YUKSEKLIK // 2

    # Ölçekleme faktörü, koordinatları ekran boyutlarına sığdırmak için
    # Kenarlarda boşluk bırakmak için -100 kullanılır
    scale_factor = min((GENISLIK - 100) / (2 * max_abs_coord + 1), (YUKSEKLIK - 100) / (2 * max_abs_coord + 1),
                       20)  # Max 20 birim

    def coord_to_pixel(x, y):
        px = merkez_x + x * scale_factor
        py = merkez_y - y * scale_factor  # Pygame'de Y ekseni aşağı doğru artar
        return int(px), int(py)

    def draw_grid(screen):
        # X ekseni
        pygame.draw.line(screen, SIYAH, (0, merkez_y), (GENISLIK, merkez_y), 2)
        # Y ekseni
        pygame.draw.line(screen, SIYAH, (merkez_x, 0), (merkez_x, YUKSEKLIK), 2)

        # Izgara çizgileri ve etiketler
        for i in range(int(-GENISLIK / (2 * scale_factor)), int(GENISLIK / (2 * scale_factor)) + 1):
            x_px, _ = coord_to_pixel(i, 0)
            if i != 0:
                pygame.draw.line(screen, GRI, (x_px, 0), (x_px, YUKSEKLIK), 1)
                text_to_screen(screen, str(i), (x_px + 5, merkez_y + 5), GRI, 18)

        for i in range(int(-YUKSEKLIK / (2 * scale_factor)), int(YUKSEKLIK / (2 * scale_factor)) + 1):
            _, y_px = coord_to_pixel(0, i)
            if i != 0:
                pygame.draw.line(screen, GRI, (0, y_px), (GENISLIK, y_px), 1)
                text_to_screen(screen, str(i), (merkez_x + 5, y_px + 5), GRI, 18)

        # Eksen etiketleri
        text_to_screen(screen, "X", (GENISLIK - 20, merkez_y - 30), SIYAH, 24)
        text_to_screen(screen, "Y", (merkez_x + 10, 10), SIYAH, 24)
        text_to_screen(screen, "0", (merkez_x + 5, merkez_y + 5), SIYAH, 18)

        # Noktaları çiz
        for i, p in enumerate(noktalar):
            px, py = coord_to_pixel(p[0], p[1])
            pygame.draw.circle(screen, KIRMIZI, (px, py), 5)
            text_to_screen(screen, f"({p[0]}, {p[1]})", (px + 10, py - 20), KIRMIZI, 20)

        # Çizgileri çiz
        for i, l in enumerate(cizgiler):
            px1, py1 = coord_to_pixel(l[0], l[1])
            px2, py2 = coord_to_pixel(l[2], l[3])
            pygame.draw.line(screen, MAVI, (px1, py1), (px2, py2), 3)

    pygame_loop(draw_grid)


def yuzde_pasta_grafik_gorsellestir(veriler, baslik="Yuzdelik Dagilim"):
    """
    Yüzdelik verileri pasta grafiği ile görselleştirir.
    Veriler: [yuzde1, yuzde2, ...] veya [deger1, deger2, ...]
    """
    ekran = init_pygame_screen(baslik)

    # Veriler yüzde değilse, yüzdeye çevir
    toplam_deger = sum(veriler)
    if toplam_deger == 0:
        print("Veri toplami sıfır olamaz.")
        return
    yuzdeler = [(v / toplam_deger) * 100 for v in veriler]

    renkler = [KIRMIZI, YESIL, MAVI, SARI, MOR, TURUNCU, AQUA, GRI]  # Daha fazla renk eklenebilir

    def draw_pie_chart(screen):
        center_x, center_y = GENISLIK // 2, YUKSEKLIK // 2 + 50
        radius = min(GENISLIK, YUKSEKLIK) // 3  # Çap

        start_angle = 0  # Başlangıç açısı

        for i, yuzde in enumerate(yuzdeler):
            end_angle = start_angle + (yuzde / 100) * 360  # Bitiş açısı (derece cinsinden)

            # Dilimi çiz
            # Pygame'de derece radyana çevrilmeli
            start_rad = math.radians(start_angle)
            end_rad = math.radians(end_angle)

            # Dilimi çizmek için polygon kullan
            noktalar = [(center_x, center_y)]
            num_segments = int(360 * (yuzde / 100))  # Dilimin yuvarlaklığı için segment sayısı
            for angle in range(int(start_angle), int(end_angle) + 1):
                rad = math.radians(angle)
                x = center_x + radius * math.cos(rad)
                y = center_y + radius * math.sin(rad)
                noktalar.append((x, y))

            if len(noktalar) > 2:
                pygame.draw.polygon(screen, renkler[i % len(renkler)], noktalar)
                pygame.draw.aaline(screen, SIYAH, (center_x, center_y), (noktalar[-1]))  # Son kenarı çiz

            # Dilimlerin kenarlarını çiz (daha belirgin olması için)
            pygame.draw.arc(screen, SIYAH, (center_x - radius, center_y - radius, radius * 2, radius * 2), start_rad,
                            end_rad, 2)

            # Yüzde etiketlerini çiz (dilimin ortasına yakın)
            mid_angle = math.radians((start_angle + end_angle) / 2)
            label_x = center_x + (radius * 0.7) * math.cos(mid_angle)
            label_y = center_y + (radius * 0.7) * math.sin(mid_angle)

            text_to_screen(screen, f"{yuzde:.1f}%", (label_x, label_y), SIYAH, 20, center_x=True)

            start_angle = end_angle  # Bir sonraki dilim için başlangıç açısını güncelle

        # Başlık ve legend
        text_to_screen(screen, baslik, (GENISLIK // 2, 20), SIYAH, 40, center_x=True)

        # Legend (Açıklamalar)
        legend_y = 60
        for i, val in enumerate(veriler):
            text_to_screen(screen, f"Dilim {i + 1}: {val}", (50, legend_y + i * 25), renkler[i % len(renkler)], 20)

    pygame_loop(draw_pie_chart)


# --- ANA MENÜ ---

def kullanici_girdi_al(soru, tip=int, min_val=None, max_val=None):
    """Kullanıcıdan geçerli giriş alan yardımcı fonksiyon."""
    while True:
        try:
            girdi = input(soru)
            deger = tip(girdi)
            if min_val is not None and deger < min_val:
                print(f"Hata: Değer {min_val}'den küçük olamaz.")
            elif max_val is not None and deger > max_val:
                print(f"Hata: Değer {max_val}'den büyük olamaz.")
            else:
                return deger
        except ValueError:
            print("Hata: Geçersiz giriş. Lütfen doğru tipi girin.")


def ana_menu():
    while True:
        print("\n--- Ortaokul Matematik Uygulaması ---")
        print("1. Sayı Teorisi İşlemleri")
        print("2. Cebir İşlemleri")
        print("3. Geometri İşlemleri")
        print("4. Kesir İşlemleri")
        print("5. Ondalık Sayılar İşlemleri")
        print("6. Yüzdeler İşlemleri")
        print("7. Rastgele Problem / Veri Üretimi")
        print("8. GÖRSELLEŞTİRMELER (Pygame)")
        print("0. Çıkış")

        secim = kullanici_girdi_al("Lütfen bir seçenek girin: ", tip=int, min_val=0, max_val=8)

        if secim == 1:
            print("\n--- Sayı Teorisi ---")
            sub_secim = kullanici_girdi_al(
                "1. Asal Sayı Kontrolü\n2. Çarpanları Bulma\n3. EBOB Bulma\n4. EKOK Bulma\nSeçiminiz: ", tip=int,
                min_val=1, max_val=4)
            if sub_secim == 1:
                sayi = kullanici_girdi_al("Bir sayı girin: ", tip=int, min_val=0)
                print(f"{sayi} asal mı? {asal_sayi_mi(sayi)}")
            elif sub_secim == 2:
                sayi = kullanici_girdi_al("Çarpanlarını bulmak için bir sayı girin: ", tip=int, min_val=1)
                print(f"{sayi}'nin çarpanları: {carpanlari_bul(sayi)}")
            elif sub_secim == 3:
                a = kullanici_girdi_al("Birinci sayıyı girin: ", tip=int, min_val=0)
                b = kullanici_girdi_al("İkinci sayıyı girin: ", tip=int, min_val=0)
                print(f"EBOB({a}, {b}): {ebob_bul(a, b)}")
            elif sub_secim == 4:
                a = kullanici_girdi_al("Birinci sayıyı girin: ", tip=int, min_val=0)
                b = kullanici_girdi_al("İkinci sayıyı girin: ", tip=int, min_val=0)
                print(f"EKOK({a}, {b}): {ekok_bul(a, b)}")

        elif secim == 2:
            print("\n--- Cebir ---")
            sub_secim = kullanici_girdi_al(
                "1. Doğrusal Denklem Çözme (ax + b = 0)\n2. İkinci Dereceden Denklem Çözme (ax^2 + bx + c = 0)\nSeçiminiz: ",
                tip=int, min_val=1, max_val=2)
            if sub_secim == 1:
                a = kullanici_girdi_al("a değerini girin: ", tip=float)
                b = kullanici_girdi_al("b değerini girin: ", tip=float)
                print(dogrusal_denklem_coz(a, b))
            elif sub_secim == 2:
                a = kullanici_girdi_al("a değerini girin: ", tip=float)
                b = kullanici_girdi_al("b değerini girin: ", tip=float)
                c = kullanici_girdi_al("c değerini girin: ", tip=float)
                print(ikinci_dereceden_denklem_coz(a, b, c))

        elif secim == 3:
            print("\n--- Geometri ---")
            sub_secim = kullanici_girdi_al("1. Dikdörtgen Alan/Çevre\n2. Daire Alan/Çevre\nSeçiminiz: ", tip=int,
                                           min_val=1, max_val=2)
            if sub_secim == 1:
                uzunluk = kullanici_girdi_al("Uzunluğu girin: ", tip=float, min_val=0)
                genislik = kullanici_girdi_al("Genişliği girin: ", tip=float, min_val=0)
                print(dikdortgen_alan_cevre(uzunluk, genislik))
            elif sub_secim == 2:
                yaricap = kullanici_girdi_al("Yarıçapı girin: ", tip=float, min_val=0)
                print(daire_alan_cevre(yaricap))

        elif secim == 4:
            print("\n--- Kesir İşlemleri ---")
            sub_secim = kullanici_girdi_al(
                "1. Kesir Sadeleştir\n2. Kesirleri Topla\n3. Kesirleri Çıkar\n4. Kesirleri Çarp\n5. Kesirleri Böl\nSeçiminiz: ",
                tip=int, min_val=1, max_val=5)
            if sub_secim == 1:
                pay = kullanici_girdi_al("Payı girin: ", tip=int)
                payda = kullanici_girdi_al("Paydayı girin: ", tip=int, min_val=1)
                sade_pay, sade_payda = kesir_sadelestir(pay, payda)
                print(f"{pay}/{payda} kesrinin sadeleşmiş hali: {sade_pay}/{sade_payda}")
            elif sub_secim >= 2 and sub_secim <= 5:
                print("Birinci kesir:")
                pay1 = kullanici_girdi_al("Payı girin: ", tip=int)
                payda1 = kullanici_girdi_al("Paydayı girin: ", tip=int, min_val=1)
                print("İkinci kesir:")
                pay2 = kullanici_girdi_al("Payı girin: ", tip=int)
                payda2 = kullanici_girdi_al("Paydayı girin: ", tip=int, min_val=1)

                if sub_secim == 2:
                    sonuc_pay, sonuc_payda = kesir_topla(pay1, payda1, pay2, payda2)
                    print(f"Toplam: {sonuc_pay}/{sonuc_payda}")
                elif sub_secim == 3:
                    sonuc_pay, sonuc_payda = kesir_cikar(pay1, payda1, pay2, payda2)
                    print(f"Fark: {sonuc_pay}/{sonuc_payda}")
                elif sub_secim == 4:
                    sonuc_pay, sonuc_payda = kesir_carp(pay1, payda1, pay2, payda2)
                    print(f"Çarpım: {sonuc_pay}/{sonuc_payda}")
                elif sub_secim == 5:
                    sonuc = kesir_bol(pay1, payda1, pay2, payda2)
                    if isinstance(sonuc, tuple):
                        print(f"Bölüm: {sonuc[0]}/{sonuc[1]}")
                    else:
                        print(sonuc)  # Tanımsız mesajı

        elif secim == 5:
            print("\n--- Ondalık Sayılar ---")
            sub_secim = kullanici_girdi_al("1. Ondalık Sayıyı Yuvarla\nSeçiminiz: ", tip=int, min_val=1, max_val=1)
            if sub_secim == 1:
                sayi = kullanici_girdi_al("Yuvarlanacak ondalık sayıyı girin: ", tip=float)
                basamak = kullanici_girdi_al("Kaç ondalık basamağa yuvarlansın? ", tip=int, min_val=0)
                print(f"Yuvarlanmış sayı: {ondalik_yuvarla(sayi, basamak)}")

        elif secim == 6:
            print("\n--- Yüzdeler ---")
            sub_secim = kullanici_girdi_al("1. Bir Sayının Yüzdesini Hesapla\n2. Yüzde Farkını Hesapla\nSeçiminiz: ",
                                           tip=int, min_val=1, max_val=2)
            if sub_secim == 1:
                ana_deger = kullanici_girdi_al("Ana değeri girin: ", tip=float)
                yuzde_orani = kullanici_girdi_al("Yüzde oranını girin (örn. 25): ", tip=float)
                print(f"Sonuç: {yuzde_hesapla(ana_deger, yuzde_orani):.2f}")
            elif sub_secim == 2:
                deger1 = kullanici_girdi_al("İlk değeri girin: ", tip=float)
                deger2 = kullanici_girdi_al("İkinci değeri girin: ", tip=float)
                print(f"Yüzde Farkı: {yuzde_farki(deger1, deger2)}")

        elif secim == 7:
            print("\n--- Rastgele Problem / Veri Üretimi ---")
            sub_secim = kullanici_girdi_al("1. Rastgele Tam Sayı Üret\n2. Rastgele Ondalıklı Sayı Üret\nSeçiminiz: ",
                                           tip=int, min_val=1, max_val=2)
            if sub_secim == 1:
                min_val = kullanici_girdi_al("Minimum değeri girin: ", tip=int)
                max_val = kullanici_girdi_al("Maksimum değeri girin: ", tip=int)
                miktar = kullanici_girdi_al("Kaç adet sayı üretilsin? (Tek sayı için 1 girin): ", tip=int, min_val=1)
                print(f"Üretilen tam sayılar: {rastgele_tam_sayi_uret(min_val, max_val, miktar)}")
            elif sub_secim == 2:
                min_val = kullanici_girdi_al("Minimum değeri girin: ", tip=float)
                max_val = kullanici_girdi_al("Maksimum değeri girin: ", tip=float)
                ondalik_basamak = kullanici_girdi_al("Ondalık basamak sayısı girin: ", tip=int, min_val=0)
                miktar = kullanici_girdi_al("Kaç adet sayı üretilsin? (Tek sayı için 1 girin): ", tip=int, min_val=1)
                print(
                    f"Üretilen ondalıklı sayılar: {rastgele_ondalikli_sayi_uret(min_val, max_val, ondalik_basamak, miktar)}")

        elif secim == 8:
            print("\n--- GÖRSELLEŞTİRMELER (Pygame) ---")
            sub_secim = kullanici_girdi_al(
                "1. Asal Çarpan Ağacı\n2. Kesir Görselleştirme\n3. Dikdörtgen Çizimi\n4. Daire Çizimi\n5. Koordinat Düzlemi\n6. Yüzde Pasta Grafik\nSeçiminiz: ",
                tip=int, min_val=1, max_val=6)
            if sub_secim == 1:
                sayi = kullanici_girdi_al("Asal çarpan ağacı için bir sayı girin (örn. 72): ", tip=int, min_val=2)
                asal_carpan_agaci_gorsellestir(sayi)
            elif sub_secim == 2:
                pay = kullanici_girdi_al("Kesir için payı girin: ", tip=int, min_val=1)
                payda = kullanici_girdi_al("Kesir için paydayı girin (paydan büyük olmalı): ", tip=int, min_val=pay)
                kesir_gorsellestir(pay, payda)
            elif sub_secim == 3:
                uzunluk = kullanici_girdi_al("Dikdörtgen uzunluğunu girin: ", tip=float, min_val=0.1)
                genislik = kullanici_girdi_al("Dikdörtgen genişliğini girin: ", tip=float, min_val=0.1)
                dikdortgen_gorsellestir(uzunluk, genislik)
            elif sub_secim == 4:
                yaricap = kullanici_girdi_al("Daire yarıçapını girin: ", tip=float, min_val=0.1)
                daire_gorsellestir(yaricap)
            elif sub_secim == 5:
                nokta_sayisi = kullanici_girdi_al("Kaç nokta çizmek istersiniz? (örn. 2): ", tip=int, min_val=0)
                noktalar_listesi = []
                for i in range(nokta_sayisi):
                    x = kullanici_girdi_al(f"{i + 1}. nokta için X koordinatını girin: ", tip=float)
                    y = kullanici_girdi_al(f"{i + 1}. nokta için Y koordinatını girin: ", tip=float)
                    noktalar_listesi.append((x, y))

                cizgi_sayisi = kullanici_girdi_al("Kaç çizgi çizmek istersiniz? (örn. 1): ", tip=int, min_val=0)
                cizgiler_listesi = []
                for i in range(cizgi_sayisi):
                    print(f"{i + 1}. çizgi için başlangıç ve bitiş noktalarını girin:")
                    x1 = kullanici_girdi_al("Başlangıç X: ", tip=float)
                    y1 = kullanici_girdi_al("Başlangıç Y: ", tip=float)
                    x2 = kullanici_girdi_al("Bitiş X: ", tip=float)
                    y2 = kullanici_girdi_al("Bitiş Y: ", tip=float)
                    cizgiler_listesi.append((x1, y1, x2, y2))

                koordinat_duzlemi_gorsellestir(noktalar_listesi, cizgiler_listesi)
            elif sub_secim == 6:
                veri_str = input("Pasta grafiği için verileri virgülle ayırarak girin (örn: 25,15,30,30): ")
                try:
                    veriler = [float(x.strip()) for x in veri_str.split(',')]
                    if not veriler or any(v < 0 for v in veriler):
                        print("Hata: Geçerli pozitif sayılar girin.")
                        continue
                    yuzde_pasta_grafik_gorsellestir(veriler, "Veri Dagilimi")
                except ValueError:
                    print("Hata: Geçersiz veri formatı. Lütfen sayıları virgülle ayırın.")

 
        elif secim == 0:
            print("Uygulamadan çıkılıyor. Güle güle!")
            break
        else:
            print("Geçersiz seçenek. Lütfen tekrar deneyin.")


# Ana menüyü başlat
if __name__ == "__main__":
    ana_menu()

